 
> [!IMPORTANT] Las variables siempre son referenciadas como `$<variable name>`

Se pueden definir distintos tipos de variables en una regla:

## Event (Variables de evento)

- **Todo campo UDM necesita una variable delante.** No se puede escribir `target.user.userid` simplemente, ==se necesita estar atado a una variable de evento.==

- Estos campos son o de **UDM Event** (eventos normalizados) o de **Entity** (Entidades, como contexto de activos/usuarios). Si no se especifica que es una entidad, YARA-L asume que es un campo UDM normal
	
	- **Ejemplo 1 — Solo Evento (UDM Event, el caso "default")**
		
		`$e1.principal.hostname != ""`
		`$e1.principal.hostname = $hostname`
		
		Acá `$e1` es un evento común, `principal.hostname` es un campo UDM estándar, no hace falta indicar nada especial, se asume que es Event.
		
	- **Ejemplo 2 — Entity (contexto adicional sobre ese host)**
		
		`$context.graph.entity.hostname = $host`
		`$context.graph.metadata.entity_type = "ASSET"`
		
		Acá `$context` es una variable **Entity**, y se nota porque el path para por `graph.entity` y `graph.metadata`, esa es la "marca" de que estás mirando datos de contexto (activos, usuarios, riesgo), no un evento de actividad
	
- Los Campos se referencian encadenados (como un path), por ejemplo: `$event.target.user.userid`

- El nombre de la variable es arbitrario, no se tiene que usar `$e` o `$event`, se puede (y conviene) ponerle nombres descriptivos.

- **Ejemplo clave:** cuando se trabaja con múltiples eventos en la misma regla, es mucho más claro usar nombres descriptivos que te digan que es cada evento. Por ejemplo, si se esta comparando logins fallidos contra logins existoso: 
	
	- `$failure.target.user.userid` -> identifica al usuario en el evento de login fallido
	  
	- `$success.target.user.userid` ->identifica al usuario en el vento de login existoso
	  
	Esto te permite después, en `match` o `condition`, agrupar y comparar ambos tipos de evento sin confundirte.

# Variables Placeholder (marcador de posición)

- Estas variables se definen en la sección `events` y sirven para dos cosas:
	
	1. **Asociar eventos entre sí** (por ejemplo, decir "el usuario del evento de fallo es el mismo que el usuario del evento de éxito")
	
	2. **Comparar un tipo de enveto con otro**
	
- Ejemplo: `$success.target.user.userid = $target_userid`. Acá `$target_userid` es el placeholder: está diciendo que "el userid del evento exitoso se guarda en la variable `$target_userid`". Si en otra línea se tiene `$failure.target.user.userid = $target_userid`, se esta forzando que ambos eventos (éxito y fallo) compartan el mismo usuario.

- Una vez definido en `events`, ese placeholder **se puede reutilizar en otras secciones de la regla**, como en `outcome`.

# Variables en Match

- Una o más variables se usan para **agrupar y agregar resultados dentro de una ventana de tiempo.**

- Se definen primero como placeholder en `events`, y **después se aplican en `match`**.

- Ejemplo: `$target_userid over 5m` -> agrupa los resultados por `target_userid`, evaluando esa agrupación en ventanas de 5 minutos.

- **Analogía con SQL:** esto es básicamente el equivalente a un `GROUP BY`, agregando de que también define la ventana temporal.

# El símbolo espacial `#`

- `#` es un carácter especial que se usa en la sección `condition`.

- Se coloca antes del nombre de una variable de evento o placeholder.

- **Representa el número de eventos distintos o valores distintos** que cumplen todas las condiciones de la sección `events`.

- Dato importante: usar `$event` solo (sin `#`) en una condición es **equivalente** a escribir `#event > 0` (es decir, "existe al menos un evento que cumple la condición").

- Esto es lo que te permite crear **umbrales (thresholds)** en la condición de la regla.

**Ejemplo**

```
condition:
	#event > 5 and #target_userid > 2
```

Eso significa: "quiero que haya **más de 5 eventos distintos** Y **más de 2 valores distintos de `target_userid`**". Es decir, no alcanza con que un solo usuario dispare muchos eventos — necesitás que haya al menos 3 usuarios diferentes involucrados, y en total más de 5 eventos.

# Ejemplos usando todo lo anterior

Esta regla busca hosts con `USER_LOGIN` (evento) y cruza esa info con el **contexto de entidad** (¿es un activo conocido?) y el **risk score** de esa entidad:

```
rule EntityContextAndRiskScore {
  meta:
    author = "Security Team"
    description = "Flags logins on high-risk hosts"

  events:
    // EVENTO normal: un login
    $log_in.metadata.event_type = "USER_LOGIN"
    $log_in.principal.hostname = $host

    // ENTITY: contexto — ¿esta hostname es un activo conocido?
    $context.graph.entity.hostname = $host
    $context.graph.metadata.entity_type = "ASSET"

    // ENTITY: risk score — ¿qué tan riesgosa es esta entidad?
    $risk_score.graph.entity.hostname = $host
    $risk_score.graph.risk_score.risk_window_size.seconds = 604800

  match:
    $host over 2m

  outcome:
    $entity_risk_score = max($risk_score.graph.risk_score.normalized_risk_score)

  condition:
    $log_in and $context and $risk_score and $entity_risk_score > 100
}
```

- `$log_in.metadata.event_type` y `$log_in.principal.hostname` → **campos UDM Event** (sin nada especial, van directo).
- `$context.graph.entity.hostname` y `$risk_score.graph.risk_score...` → **campos Entity**, siempre pasando por el prefijo `graph`.

---

Regla que detecta cuando el mismo usuario tiene un login fallido seguido de un login exitoso poco después (indicio de fuerza bruta exitosa):

```
rule failed_then_success_login
{
  meta:
    author = "Security Team"
    description = "Detects a failed login followed by a successful login for the same user"
    severity = "HIGH"

  events:
    $failure.metadata.event_type = "USER_LOGIN"
    $failure.security_result.action = "FAIL"
    $failure.target.user.userid = $target_userid

    $success.metadata.event_type = "USER_LOGIN"
    $success.security_result.action = "SUCCESS"
    $success.target.user.userid = $target_userid

  match:
    $target_userid over 10m

  condition:
    $failure and $success
}
```

- `$failure` y `$success` son **variables de evento** con nombres descriptivos (en vez de `$e1`, `$e2`).
- `$target_userid` es el **placeholder** que conecta ambos eventos (mismo usuario en ambos).
- `$target_userid over 10m` en `match` agrupa (como un `GROUP BY`) por usuario en ventanas de 10 minutos.
- En `condition`, `$failure and $success` es shorthand de `#failure > 0 and #success > 0` — es decir, "que exista al menos un evento de fallo Y al menos uno de éxito" para ese usuario en esa ventana.


# Variables de los Data Tables (`%`)

## ¿Qué son las Data Tables?

Son construcciones **multicolumna** que te permiten subir tus propios datos a Google SecOps. Funcionan como **tablas de lookup** con columnas definidas y datos en filas. Podés crearlas/importarlas vía la UI de SecOps, la API de data tables, o directamente desde una query YARA-L en las reglas.

## Diferencia clave con Reference Lists

|                        | Reference List                           | Data Table                                                                     |
| ---------------------- | ---------------------------------------- | ------------------------------------------------------------------------------ |
| **Estructura**         | Una sola columna/lista plana de valores  | Múltiples columnas, estructura de fila/columna                                 |
| **Símbolo**            | `%nombre_lista`                          | `%nombre_tabla.nombre_columna`                                                 |
| **Caso de uso típico** | Lista simple de IPs maliciosas conocidas | Datos con múltiples atributos relacionados (ej. hostname + owner + criticidad) |
## Sintaxis para referenciar una Data Table en una regla

Se usa el operador `in`, igual que con reference lists, pero apuntando a `%tabla.columna`:

**STRING** (coincidencia exacta de texto):

```
$e.target.hostname in %table_name.column_name
```

**REGEX** (coincidencia por expresión regular):

```
$e.target.hostname in regex %table_name.column_name
```

**CIDR** (coincidencia de rango de IP):

```
$e.principal.ip in cidr %table_name.column_name
```

### ¿Dónde se puede usar dentro de la regla?

Tanto reference lists como data tables se pueden usar en la sección **`events`** o en la sección **`outcome`** de una regla.

## Ejemplo práctico

Supongamos que se tiene una data table llamada `critical_assets` con una columna `hostname` que contiene tus servidores críticos, y querés generar una alerta de alta prioridad cuando se detecte actividad sospechosa en esos hosts específicos:

```
rule suspicious_activity_on_critical_asset
{
  meta:
    author = "Security Team"
    description = "Detects suspicious process launches on critical assets"
    severity = "HIGH"

  events:
    $e.metadata.event_type = "PROCESS_LAUNCH"
    $e.principal.hostname = $hostname
    $hostname in %critical_assets.hostname

  match:
    $hostname over 5m

  condition:
    $e
}
```

Acá, `$hostname in %critical_assets.hostname` es el join: filtra los eventos de proceso solo para hostnames que **existen como fila** en la columna `hostname` de la tabla `critical_assets`.