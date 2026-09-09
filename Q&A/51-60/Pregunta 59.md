![[Pasted image 20260909090732.png]]

#### Qué pide el escenario

- Automatizar la investigación de alertas de phishing usando un **playbook de SOAR**
- Necesitas que los **resultados de una query de SIEM** se incluyan **automáticamente** en el caso
- Restricción clave: **"without writing any new code"**

---

#### Por qué D es correcta

> _"Add an action to the playbook that runs the SIEM query and returns the results."_

- Google SecOps SOAR tiene **integraciones nativas ya construidas** (out-of-the-box) para conectarse con su propio SIEM — es decir, existen **acciones predefinidas y listas para usar** dentro del catálogo de integraciones de SOAR que permiten **ejecutar una búsqueda UDM/SIEM** y **traer los resultados de vuelta al playbook**, sin necesitar programar nada nuevo.
- Simplemente **arrastras esa acción existente** dentro del diseñador visual del playbook, la configuras con los parámetros de búsqueda necesarios, y los resultados se integran automáticamente al **caso** — cumpliendo exactamente el requisito de "sin escribir código nuevo."

---

#### Por qué no A (la opción más tentadora)

> _"Create a custom action in Google SecOps IDE that runs the SIEM query from a playbook through an API call and returns the results."_

- El problema clave está en la palabra **"custom action"**: crear una **acción personalizada** en el IDE de Google SecOps **requiere programar** — necesitas escribir código (típicamente Python) para definir esa integración/acción desde cero, usando el SDK del IDE.
- Esto **viola directamente** el requisito explícito del enunciado: **"without writing any new code."** Es una solución técnicamente viable, pero **innecesaria**, porque ya existe una acción nativa (opción D) que hace exactamente esto sin necesidad de desarrollo.

#### Por qué no B

> _"Modify the detection rule in the SIEM to include the query results as part of the detection."_

- Esto ataca la **capa equivocada**: modificar la **regla de detección** afecta cómo se genera la alerta inicial (en el SIEM), no cómo se **enriquece un caso ya creado** dentro de SOAR con información adicional bajo demanda durante la investigación. El enunciado habla de un proceso de **investigación** posterior a que la alerta ya llegó al caso — no de cambiar la lógica de detección misma.

#### Por qué no C

> _"Add a widget to the Default Case View that allows the analyst team to query directly from the widget."_

- Esto seguiría dependiendo de que un **analista ejecute manualmente** la consulta cada vez desde el widget — no es una automatización real. El enunciado pide específicamente **automatizar** el proceso ("automate the investigation"), y que los resultados se incluyan **automáticamente**, no que se facilite hacerlo manualmente de forma más cómoda.

---

#### La idea clave para el examen

Cuando el enunciado pida automatizar algo dentro de un **playbook de SOAR** con la restricción explícita de **"sin escribir código nuevo"**, la señal apunta a usar **acciones/integraciones nativas ya existentes** en el catálogo de SOAR — evita las opciones que mencionen **"custom action"**, **"IDE"**, o **desarrollo desde cero**, ya que esas siempre implican programación, sin importar cuán automatizado parezca el resultado final.