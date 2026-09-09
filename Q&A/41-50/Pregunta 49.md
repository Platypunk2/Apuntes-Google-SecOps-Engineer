![[Pasted image 20260908234557.png]]

#### Qué pide el escenario

- Antes: proceso **manual** — exportar datos de contexto de Active Directory (AD) e importarlos como una "watchlist" en el SIEM anterior cada vez que había cambios
- Ahora: quieres **mejorar/automatizar** ese proceso usando las capacidades nativas de Google SecOps

---

#### Por qué D es correcta

> _"Ingest AD organizational context data as user/asset context to enrich user/asset information in your security events."_

- Google SecOps tiene un **modelo de datos de entidades (entity data model)** diseñado específicamente para este propósito: ingerir **contexto de usuario/activo** desde fuentes autoritativas como **Active Directory** (usando el parser nativo **"Microsoft AD"** o **"Azure AD Organizational Context"**), directamente hacia el **Entity Graph** de SecOps.
- Una vez ingerido, este contexto **enriquece automáticamente los eventos UDM**: campos como departamento, título, estado del empleado (activo/terminado), etc., quedan disponibles directamente en los eventos, sin que tengas que hacer ningún join manual — la correlación entre "usuario en el evento" y "contexto de ese usuario en AD" ocurre de forma **nativa y automática**.
- Esto **automatiza completamente** el proceso que antes hacías manualmente (exportar/importar cada vez que había cambios): SecOps **actualiza el contexto periódicamente** de forma nativa a través del feed configurado, sin intervención manual continua.

---

#### Por qué no las demás

**A** (Configurar una integración SOAR de AD para enriquecer en las alertas):

- Esto enriquecería datos **solo en el contexto de SOAR/playbooks** (durante la respuesta a un caso específico), no de forma **automática y continua para todos los eventos UDM** en el SIEM. Es un enfoque más limitado, reactivo (ocurre cuando se ejecuta un playbook), en lugar de una enriquecimiento **nativo y constante** a nivel de la plataforma SIEM.

**B** (Crear una reference list y usarla en reglas YARA-L):

- Una **reference list** es una simple lista de valores (strings) para hacer matching directo — es útil para IOCs simples (una lista de hashes o dominios), pero **no está diseñada para modelar contexto rico y relacional** como el de usuarios/activos de AD (departamento, gerente, estado, grupos, etc.). Además, seguiría requiriendo que **tú mantengas manualmente** esa lista actualizada, sin aprovechar la ingesta automática nativa de contexto.

**C** (Crear una data table con datos de AD, y usarla en la regla YARA-L para correlacionar):

- Las **data tables** son más flexibles que las reference lists (permiten estructuras tabulares), pero de nuevo, esto significa que **tú tendrías que construir y mantener manualmente** esa tabla — no aprovecha el **feed automático nativo** de contexto de AD que SecOps ya ofrece a través del entity data model. Sería reinventar con más esfuerzo manual algo que la plataforma ya resuelve automáticamente.

---

#### La diferencia clave para el examen

Cuando el escenario mencione **fuentes de contexto "autoritativas" y ya soportadas nativamente** (Active Directory, Workspace, Okta, ServiceNow CMDB, etc.), la respuesta correcta casi siempre es **ingerir esos datos como "user/asset context"** a través de los **feeds/parsers nativos del entity data model** de SecOps — esto habilita el **enriquecimiento automático de UDM**, eliminando la necesidad de mantener manualmente reference lists, data tables, o depender de integraciones de SOAR para lo que debería ser un enriquecimiento continuo a nivel de plataforma SIEM.
