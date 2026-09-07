![[Pasted image 20260906194104.png]]

La respuesta correcta es la C porque Google SecOps SOAR tiene un mecanismo nativo diseñado exactamente para este propósito: el campo **"Root Cause"** dentro del diálogo de cierre de caso (**Close Case dialog**). Este campo es configurable en la configuración del entorno, y puedes definir una lista personalizada de opciones (en este caso, los cinco tipos de eventos DLP) para que el analista **seleccione obligatoriamente** una de ellas al cerrar el caso.

---

**Por qué no es A**

- Los **case tags** son etiquetas de metadatos genéricas, pensadas para búsqueda, filtrado o categorización libre — no son el campo estructurado de "root cause" que el sistema reconoce específicamente al cerrar un caso.

- Aunque se "manual" (igual que la C que requiere selección del analista), el problema es que **no está vinculado al proceso de cierre.** Nada garantiza que el analista asigne el tag antes de cerrar el caso — podría cerrarse sin tag, o con múltiples tags contradictorios, sin ninguna validación.

- No cumple el requisito de que la clasificación quede **obligatoriamente** asociada al evento de cierre del caso.

**Por qué no es B**

- El **nombre del caso** es un campo de texto libre pensado para identificación rápida (ej: "DLP-2024-0091 - Marketing Dept"), no un campo estructurado y filtrable como el root cause.

- Meter el tipo de evento DLP dentro del nombre del caso es una solución "hacky": no se puede validar contra una lista fija, en propenso a errores de tipeo, inconsistencias de formato, y no sirve para generar reportes/métricas confiables de forma nativa.

- Confunde el propósito de "identificar" el caso con el de "clasificar la causa raíz del caso".

**Por qué no es D**

- Usa **tags** en vez del campo Root Cause — mismo problema de fondo que A.

- Además, "automáticamente" implica que el playbook necesitaría lógica para inferir cuál de los cinco tipos de DLP aplica a cada caso. El enunciado no menciona ningún dato estructurado previo del que el playbook pueda derivar esa clasificación de forma confiable.

- La determinación de la causa raíz de un incidente DLP normalmente requiere el **juicio del analista** tras investigar el caso — no es algo que un playbook pueda automatizar sin una fuente de verdad clara.