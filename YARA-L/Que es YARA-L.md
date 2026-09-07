YARA-L Es el lenguaje especializado de Google Security Operations que trabaja sobre datos de logs empresariales, permitiendo explorar esos datos, investigar amenazas y construir reglas de detección. Es el lenguaje unificado que se usa tanto para búsquedas (search), dashboard como para reglas de detección (rules)

# Estructura de un regla

| **Orden** | **Sección** | **¿Obligatoria en Rules?** | **¿Obligatoria en Search/Dashboards?**        |
| ------------- | --------------- | ------------------------------ | ------------------------------------------------- |
| 1             | `meta`          | Sí                             | No aplica                                         |
| 2             | `events`        | Sí                             | Sí                                                |
| 3             | `match`         | No                             | No (pero requerida para correlación multi-evento) |
| 4             | `outcome`       | No                             | No                                                |
| 5             | `condition`     | Sí                             | No                                                |
| 6             | `options`       | No                             | No                                                |

# Variables en Yara-L

Las variables en Yara-L nos son tan simples como creación o definición de un objetos, dependiendo de la sección en que este, la variable pasa a definir o identificar una idea distinta.

[[Variables]]

# Operadores y modificadores en variables Event

[[Operadores y Modificadores en YARA-L]]

# Uso de Yara-L en SecOps

[[YARA-L en reglas de detección]]
[[YARA-L en dashboards]]
[[YARA-L en busqueda]]