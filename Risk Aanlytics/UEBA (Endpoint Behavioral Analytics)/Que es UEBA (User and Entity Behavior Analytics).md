UEBA no es una herramienta independiente, es **una capacidad integrada dentro de Google Security Operations (Google SecOps).**

**UEBA = User and Entity Behavior Analytic = Análisis del comportamiento de usuarios y entidades.**

La idea es que Google SecOps observe el comportamiento de usuarios y otras entidades — por ejemplo, dispositivos, cuentas o recursos — y pueda detectar cuando algo se sale de lo habitual. Esta capacidad de Goggle SecOps ocupa análisis estadístico y machine learning para detectar comportamientos anómalos de usuarios y entidades (equipos, routers, servidores, endpoints) dentro del entorno de una organización.

---

# ¿Para que sirve?

Está incluida en el paquete Enterprise de Google SecOps, junto con inteligencia de amenazas mejorada y asistencia de IA generativa de Google Cloud. Su objetivo es identificar automáticamente comportamientos anómalos que podrían indicar **credenciales comprometidas** o **amenazas internas** (insider threats).

---

# Cómo funciona en Google SecOps

UEBA emprea un motor analítico de múltiples capas que combina dos enfoques complementarios: por un lado, análisis estadístico a gran escala para definir qué es "normal" y detectar valores atípicos entre miles de millones de eventos; por otro, reglas basadas en inteligencia de amenazas que buscan comportamientos específicos vinculados a Tácticas, Técnicas y Procedimientos (TTPs) conocidos de adversarios.

Dentro de la plataforma, esto se traduce en la categoría de reglas **"Risk Analytics for UEBA"**, que agrupa detecciones como:

- Nuevo inicio de sesión de un usuario en un dispositivo (New Login by User to Device)
- Eventos de autenticación anómalos de un usuario, comparados con su uso histórico (Anomalous Authentication Events by User) 