![[Pasted image 20260908222515.png]]

#### Qué pide el escenario

- Un reporte específico sobre **ROI (Return on Investment)** atribuido a la actividad de los analistas
- Debe incluir **tiempo ahorrado** y **ganancias de eficiencia** por usar las funciones de SOAR
- Debe generarse con el enfoque **más eficiente y preciso**, con el **nivel de detalle requerido**

---

#### Por qué B es correcta

> _"Use the ROI - Analysts Benchmark report in SOAR Reports. Configure the report to display data for the desired time period, and filter by individual analysts."_

- Google SecOps SOAR tiene un **reporte nativo y predefinido** llamado específicamente **"ROI - Analysts Benchmark"** dentro de la sección de **SOAR Reports** — diseñado exactamente para calcular y mostrar el **retorno de inversión atribuido a la actividad de los analistas**, incluyendo métricas de **tiempo ahorrado** y **eficiencia** gracias a la automatización.
- Como es un reporte **ya construido y mantenido por Google**, simplemente necesitas **configurarlo** (ajustar el rango de fechas al mes anterior, y filtrar por analistas específicos si se requiere) — esto es el enfoque de **menor esfuerzo, mayor precisión** posible, ya que aprovecha una funcionalidad nativa diseñada exactamente para este caso de uso.

---

#### Por qué no las demás

**A** (Query personalizado + exportar a spreadsheet para calcular ROI manualmente):

- Esto requiere **construir manualmente** la lógica de cálculo de ROI en una hoja de cálculo externa — mucho más **esfuerzo**, propenso a errores humanos, y **menos preciso** que usar un reporte nativo ya diseñado y validado específicamente para esta métrica.

**C** (Reporte "Management - SOC Status"):

- Este reporte se enfoca en el **estado operativo general del SOC** (volumen de casos, tiempos de resolución, estado de casos abiertos/cerrados, etc.) — es útil para una vista de **salud operativa**, pero **no está diseñado específicamente para calcular ROI** ni para mostrar el ahorro de tiempo/eficiencia atribuido a la automatización de SOAR. Es la herramienta equivocada para esta métrica en particular.

**D** (Playbook personalizado que agrega métricas, aplica factores ponderados, calcula ROI con fórmulas predefinidas, y genera un PDF):

- Aunque suena sofisticado, esto implica **construir desde cero** una solución de reporting compleja (definir factores de ponderación, fórmulas de cálculo, lógica de agregación) cuando **ya existe un reporte nativo (B)** que resuelve exactamente esta necesidad. Es una solución de **mucho mayor esfuerzo de desarrollo y mantenimiento**, contradiciendo el requisito de usar el enfoque **más eficiente**.