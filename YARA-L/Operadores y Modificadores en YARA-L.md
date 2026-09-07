
# Operadores de Eventos (Event Operators)

- Dentro de la sección `events` se pueden usar los operadores lógicos clásicos: `AND`, `OR`, `NOT`.

- También se pueden usar **paréntesis** `( )` para controlar el orden de precedencia (igual que en matemática o en cualquier lenguaje de programanción).

- `AND` **se asume por defecto** cuando no se pone ningún operador explícito entre líneas. Por eso en todos los ejemplos anteriores, aunque nunca escribimos la palabra `AND`, todas esas condiciones línea por línea se estaban combinando con AND implícito.

- **Dentro de un paréntesis, el operador es obligatorio** — no se puede simplemente poner varias condiciones separadas por salto de línea dentro de un `( )` y esperar que se asuma `AND`; se tiene que escribir explícitamente.

---

## Ejemplo

```
events:
   $e.metadata.event_type = "USER_LOGIN"
   ($e.principal.hostname = "server-01" OR $e.principal.hostname = "server-02")
   NOT $e.security_result.action = "ALLOW"
```

Esto dice: *"eventos de login, Y (el host es server-01 O server-02), Y el resultado de seguridad NO fue ALLOW"*

> [!OBS] Ojo
> la primera línea y la línea con paréntesis se combinan con `AND` implícito, pero **dentro** del paréntesis se tiene que escribir `OR` explícitamente.


# Modificadores de Eventos (Event Modifiers)

`nocase`

- Se usa para **ignorar mayúsculas/minúsculas** al comparar un campo.

- Se agrega **al final** de la condición.

- **No se puede usar con campos enumerados** (enums) como `metadata.event_type` o `network.ip_protocl` — porque esos campos tienen un set fijo y predefinido de valores posibles (no tiene sentido ignorar mayúsculas ahí, ya que Google los normaliza a un valor exacto).

- Aplica a comparaciones de **strings** y de **regex**

## Sintaxis

```
$event.principal.hostname != "http-server" nocase
```

> [!IMPORTANT] Dato importante
> Si un campo contiene valores que fueron generados/ingresados por un usuario humano (por ejemplo, un hostname que alguien tipeó a mano, un nombre de archivo, un comentario), **siempre conviene usar `nocase`** — porque la gente no es consistente con mayúsculas/minúsculas al escribir. Ejemplo típico: alguien puede loguearse desde `WORKSTATION-01` o `workstation-01` y sin `nocase` tu regla podría no matchear.


# Repeated Fields (Campos Repetidos)

- Hay campos UDM que **pueden tener más de un valor** dentro del mismo evento — el ejemplo clásico es `mac` (una máquina puede tener varias interfaces de red, cada una con su MAC) o `ip` (un mismo dispositivo puede reportar varias IPs).

- Para identificar cuáles campos son "repeated", se tiene que fiar en la columna **"Label"** del UDM fields list (documentación oficial) — ahí dice explícitamente si el campo es "repeated".

- Como un campo repetido tiene **una lista de valores**, no un solo valor, necesitás decirle a YARA-L **cómo evaluar esa lista:** ¿alcanza con que **uno** de los valores cumpla la condición, o se necesita que **todos** los valores la cumplan? Para eso están los modificadores `ANY` y `ALL`.

---

`ANY` — con que **UN** valor cumpla la condición, alcanza

```
any $event.target.ip = "127.0.0.1"
```

Si **alguno** de los valores dentro de `target.ip` (recordá que es un campo repetido, puede tener varias IPs) es igual a `127.0.0.1`, la condición se cumple.

`ANY` es un operador de tipo **OR implícito** sobre todos los elementos de la lista.

---

`ALL` — **TODOS los valores deben cumplir la condición**

```
all $event.target.ip != "127.0.0.1"
```

Revisa **todos** los valores dentro de `target.ip`, y si **ninguno** de ellos es `127.0.0.1` (es decir, todos son distintos de esa IP), la regla continúa evaluándose.

`ALL` es un operador tipo **AND implícito** sobre todos los elementos de la lista — la condición solo es verdadera si se cumple para cada elemento sin excepción.

---

## Tabla resumen

|Modificador|Se comporta como|Se cumple cuando...|
|---|---|---|
|`any`|OR implícito sobre la lista|Al menos **un** valor de la lista cumple la condición|
|`all`|AND implícito sobre la lista|**Todos** los valores de la lista cumplen la condición|

---

# Ejemplo integrador (aplicando todo)

Regla que detecta tráfico saliente hacia loopback en **cualquiera** de las IPs target, excluyendo hostnames de servidores conocidos (ignorando mayúsculas), y usando paréntesis para agrupar la lógica:

```
rule loopback_traffic_detection
{
  meta:
    author = "Security Team"
    description = "Flags any target IP resolving to loopback, excluding known servers"
    severity = "MEDIUM"

  events:
    $e.metadata.event_type = "NETWORK_CONNECTION"
    any $e.target.ip = "127.0.0.1"
    NOT ($e.principal.hostname = "trusted-server-01" nocase OR $e.principal.hostname = "trusted-server-02" nocase)

  condition:
    $e
}
```

Puntos claves:

1. `AND` es implícito fuera de paréntesis, pero **obligatorio explícito dentro** de paréntesis.

2. `nocase` **nunca** es usa con campos enumerados (`event_type`, `ip_protocol`, etc.) — solo con strings/regex "libres".

3. Para campos "repeated" (mac, ip, etc.) tenés que elegir explícitamente entre `any` (alcanza con uno) y `all` (deben cumplir todos) — si no lo hacés, YARA-L puede no evaluar la lista como esperás.