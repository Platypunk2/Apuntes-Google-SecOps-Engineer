YARA-L tambien se ocupara para construir paneles visuales (gráficos, tablas, métricas) que muestran tendencias de seguridad a lo largo del tiempo. Se apoya mucho en `outcome` (para calcular métricas) y `match` (para definir cómo se agrupan y en qué ventana temporal).

# Estructura

|Orden|Sección|¿Obligatoria?|
|---|---|---|
|1|`meta`|No|
|2|`events`|Sí|
|3|`match`|No (pero prácticamente siempre se usa, porque define la ventana temporal del gráfico)|
|4|`outcome`|No (pero es la sección clave — acá van tus funciones de agregación: `count()`, `avg()`, `sum()`, etc.)|
|5|`condition`|No|
> [!Note] Nota:
> Acá tampoco aplica `options`, y `dedup` normalmente no se combina con agregaciones de dashboard (son enfoques distintos: dedup = quitar duplicados, outcome = calcular métricas).

```
events:
   // filtros de campos UDM

match:
   // agrupar por campo(s) + ventana de tiempo (ej. over 10m)

outcome:
   // funciones de agregación: count(), sum(), avg(), max(), min()
```

# Ejemplos

Panel que muestra cuántos logins fallidos tuvo cada usuario en ventanas de 10 minutos (ideal para graficar como serie temporal o tabla):

```
events:
   $e.metadata.event_type = "USER_LOGIN"
   $e.security_result.action = "FAIL"
   $user = $e.target.user.userid

match:
   $user over 10m

outcome:
   $failed_login_count = count($e.metadata.id)
```

Esto te da, por cada usuario, un valor `failerd_login_count` calculado cada 10 minutos — justo lo que necesitás para poner en un widget de dashboard tipo "Top usuarios con más fallos de login".

---
Dashboard de volumen de tráfico por IP externa:

```
events:
   $e.metadata.event_type = "NETWORK_CONNECTION"
   $e.target.ip = $external_ip

match:
   $external_ip over 1h

outcome:
   $connection_count = count($e.metadata.id)
   $total_bytes = sum($e.network.sent_bytes)
```

Panel que agrupa por IP externa cada hora, mostrando cantidad de conexiones y bytes enviados — típico gráfico de "top destinos externos por volumen".