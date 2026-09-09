![[Pasted image 20260908200615.png]]

#### Qué pide el escenario

- Identificar **todos los activos** (endpoints, cuentas de servicio, recursos cloud) con los que un usuario específico **interactuó** en los últimos **7 días**
- Entender las **relaciones usuario-activo** para evaluar el impacto potencial de una actividad sospechosa

---
#### Por qué B es correcta

> _"Query for hostnames in UDM Search and filter the results by user."_

- El **UDM (Unified Data Model)** normaliza todos los eventos de Google SecOps en un esquema común, donde cada evento contiene campos como `principal.hostname`, `principal.user.userid`, `target.asset.asset_id`, etc. — es decir, **la relación usuario-activo ya está codificada en cada evento normalizado**.
- Al hacer una **búsqueda UDM** filtrando por el `userid` del usuario en cuestión, y extrayendo/agrupando los distintos `hostname` (o `asset_id`) que aparecen asociados a ese usuario a lo largo del período de 7 días, obtienes exactamente el mapa de **con qué endpoints, cuentas de servicio o recursos cloud interactuó** ese usuario.
- Es el método **directo y flexible**: puedes ajustar la ventana de tiempo (7 días), y el UDM Search te permite ver la actividad tal cual quedó registrada, cruzando el campo de usuario contra los distintos tipos de activos (hostnames, asset IDs, recursos cloud) que aparecen en los mismos eventos.
---

#### Por qué no las demás

**A** (Raw Log Scan agrupando por asset ID):

- Trabajar con **logs crudos (raw)** implica que los datos **aún no están normalizados** al UDM — no tendrías un campo estandarizado y consistente de "usuario" para cruzar contra "asset ID" de forma confiable across múltiples fuentes de log heterogéneas. Es un enfoque mucho menos eficiente y más propenso a inconsistencias que trabajar con datos ya normalizados en UDM.

**C** (Generar un reporte de ingesta para identificar fuentes donde apareció el usuario):

- Los **reportes de ingesta** están diseñados para dar visibilidad sobre **qué fuentes de log están enviando datos** (volumen, salud de la ingesta, tipos de log) — no están diseñados para rastrear la actividad específica de un usuario individual ni sus relaciones con activos. Es la herramienta equivocada para este objetivo.

**D** (Ejecutar un retrohunt para encontrar matches de reglas activadas por el usuario):

- Un **retrohunt** aplica una **regla de detección específica** contra datos históricos para ver si esa regla hubiera generado alertas en el pasado. Esto solo te mostraría actividad que **coincide con una regla ya existente** — no te da una vista **completa** de todos los activos con los que el usuario interactuó, solo lo que esa regla en particular detectaría. Es demasiado limitado para el objetivo de "identificar todos los activos" de forma exhaustiva.

---

#### La idea clave para el examen

Cuando necesites **mapear relaciones entre una entidad (usuario) y los activos con los que interactuó** durante un período de tiempo específico, la vía más directa y flexible en Google SecOps es consultar los **datos ya normalizados en UDM** (vía UDM Search), filtrando por el campo de usuario y extrayendo los campos de activo relacionados (hostname, asset ID, etc.) — en lugar de depender de logs crudos sin normalizar, reportes de salud de ingesta, o herramientas de detección que solo capturan lo que una regla específica ya cubre.