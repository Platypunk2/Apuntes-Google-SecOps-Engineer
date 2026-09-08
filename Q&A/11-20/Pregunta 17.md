![[Pasted image 20260908110854.png]]

**Qué pide el escenario**

- Malware nuevo y hecho a medida (custom-developed) específicamente para atacar a tu organización, por un grupo de amenaza avanzado (APT)

- Necesitas **analizarlo rápido** para obtener IOCs

- **Condición crítica**: sin **alertar al grupo de amenaza** de que lo detectaste

---

**Por qué C es correcta**

- El punto clave aquí es entender qué pasa cuando subes un archivo a **VirusTotal** en su modo normal: los resultados y (en muchos casos) **el propio archivo puede quedar visible/consultable por otros usuarios** de la plataforma, incluyendo por los múltiples motores antivirus y por cualquier persona con acceso a VT Intelligence que busque ese hash.

- Como este malware es **custom y dirigido específicamente a tu organización**, si el grupo de amenaza (o alguien que colabora con ellos) **monitorea VirusTotal** buscando su propio malware (una práctica común entre atacantes avanzados para saber si fueron detectados), verían que su sample fue subido — y sabrían que **los descubriste**. Esto podría hacer que cambien de tácticas, borren infraestructura, o aceleren el ataque.

- **Private Scanning** (Análisis Privado) es una función diseñada exactamente para este caso: te permite subir el archivo y obtener el análisis (comportamiento, IOCs, detecciones de motores AV) **sin que el archivo ni sus metadatos se compartan públicamente** ni se hagan visibles para otros usuarios de la plataforma. Obtienes la inteligencia que necesitas, **sin alertar al adversario**.

---

**Por qué no las demás**

**B**
- Esta es la opción "trampa" — funcionalmente hace lo mismo (analiza el malware y da IOCs), pero **sin la protección de privacidad**. Al subirlo de forma normal, corres el riesgo de que el sample se vuelva visible/buscable por terceros, incluyendo potencialmente el propio grupo de amenaza. Contradice directamente el requisito de **"without alerting the threat group."**

**A**
- Esto solo te daría **información ya existente** sobre el grupo de amenaza (TTPs conocidos, campañas previas) — pero **no analiza tu sample específico**. Como el enunciado dice que el malware fue **desarrollado específicamente para este ataque**, es probable que sea **nuevo y no tenga huellas previas** en la base de datos de threat intel. No obtienes los IOCs específicos de esta muestra.

**D**
- Esta opción es más segura en cuanto a privacidad (buscar un hash no revela el archivo en sí), pero tiene un problema práctico fundamental: si el malware es custom-developed y nuevo, es muy probable que su hash no exista todavía en ninguna base de datos de VirusTotal — nadie lo ha visto antes. La búsqueda no arrojaría ningún resultado útil, y no obtendrías el análisis de comportamiento ni los IOCs que necesitas.

- Esta opción funciona bien para malware **conocido/genérico**, pero no para el escenario descrito de una muestra **nueva y dirigida**.

---

**La idea clave para el examen**

Cuando el enunciado combine **"malware nuevo/custom desarrollado por un grupo de amenaza avanzado"** + **"sin alertar al atacante"**, la respuesta señala hacia **Private Scanning** en Google Threat Intelligence/VirusTotal — porque es la única opción que te permite obtener un análisis completo y IOCs de una muestra **desconocida hasta ahora**, sin exponer públicamente que la detectaste.
