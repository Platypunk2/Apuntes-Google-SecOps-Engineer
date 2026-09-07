
El uso de Yara-L en la creación tiene las siguientes secciones:

| Orden | Sección     | ¿Obligatoria?                                            |
| ----- | ----------- | -------------------------------------------------------- |
| 1     | `meta`      | Sí                                                       |
| 2     | `events`    | Sí                                                       |
| 3     | `match`     | No (solo si agrupás/correlacionás)                       |
| 4     | `outcome`   | No                                                       |
| 5     | `condition` | Sí                                                       |
| 6     | `dedup`     | No (exclusiva de Search, sirve para eliminar duplicados) |
## Estructura genérica

```Yara-L
rule <nombre_regla>
{
  meta:
    // pares clave-valor: autor, severidad, descripción, etc.

  events:
    // filtra eventos y define relaciones (joins) entre ellos

  match:
    // agrupa resultados (group by) + ventana de tiempo

  outcome:
    // qué datos extra devolver cuando se dispara la regla

  condition:
    // lógica que determina si la regla se dispara

  options:
    // prender/apagar comportamientos específicos
}
```

## Detalles de cada sección

- `meta`— se compone de líneas clave-valor, donde la clave va sin comillas y el valor entre comillas `<key> = "<value>"` . Ejemplo `author = "Google"`, `severity = "HIGH"`.

- `events`— acá defines: declaración de variables, filtros de eventos, y joins entre eventos. Para declarar variables se usa `<EVENT_FILED> = <VAR>` o `<VAR> = <EVENT_FIELD>` (son equivalentes), por ejemplo `$e.source.hostname = $hostname`

- `match` — agrupa  por campos + define la ventana temporal (ej. `user over 10m`). Es lo que te permite decir "quiero agrupar por usuario en ventanas de 10 minutos".

- `outcome` — calcula métricas con funciones de agregaciones como `count()`, `avg()`, etc.

- `condition` — la lógica final que decide si se dispara la alerta.

## Ejemplo de una regla

```Yara-L
rule failed_logins
{
  meta:
    author = "Security Team"
    description = "Detects multiple failed user logins within 10-minute windows."
    severity = "HIGH"

  events:
    $e.metadata.event_type = "USER_LOGIN"
    $e.security_result.action = "FAIL"
    $user = $e.target.user.userid

  match:
    $user over 10m

  outcome:
    $failed_login_count = count($e.metadata.id)

  condition:
    $failed_login_count >= 5
}
```

Esta regla detecta cuando un mismo usuario ($user) acumula 5 o más logins fallidos en una ventana de 10 minutos.

