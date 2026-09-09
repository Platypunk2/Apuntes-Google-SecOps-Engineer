![[Pasted image 20260909001136.png]]

### Qué pide el escenario

Tienes datos altamente sensibles en BigQuery (correos electrónicos, nombres). El equipo de operaciones necesita usar la tabla, pero **no debe poder leer** esos campos sensibles. Solo el equipo de Recursos Humanos (HR) debe poder verlos cuando realmente lo necesite ("need-to-know").

Entonces necesitas una técnica que:

1. Oculte/transforme los datos sensibles para la mayoría de los usuarios.
2. Permita que un grupo autorizado (HR) pueda **revertir** la transformación y ver el dato original cuando lo necesite.

Eso es la clave: **reversibilidad controlada**.

---

### Por qué D es correcta: Tokenización/Pseudonimización

La **pseudonimización con tokenización** (usando Cloud DLP, específicamente FPE - Format Preserving Encryption, o Crypto-based tokenization) reemplaza el valor real (ej. "[juan@correo.com](mailto:juan@correo.com)") por un token (ej. "[a3F9x2Qz@domain.com](mailto:a3F9x2Qz@domain.com)"). Este token:

- No revela el dato original a quien lo ve.
- **Es reversible**: con la clave criptográfica correcta, el equipo de HR puede "detokenizar" y recuperar el valor original.
- Operaciones ve solo el token (dato inútil sin la clave), cumpliendo la restricción de privacidad.
- HR, al tener acceso a la clave/servicio de detokenización, puede recuperar el dato real cuando lo necesite.

Esto encaja perfectamente con el requisito de "disponible solo en base a necesidad de saber" — porque el acceso al dato real se controla mediante quién tiene permiso de detokenizar, no mediante quién ve la tabla.


---

### Por qué las otras opciones NO sirven

**A. Data Masking (enmascaramiento)**

- El masking generalmente es **irreversible o parcial** (ej. mostrar solo los últimos 4 dígitos, o reemplazar con "XXXX").
- El problema: si el dato queda enmascarado permanentemente al guardarse, **nadie** (ni siquiera HR) puede recuperar el valor original desde esa copia. No cumple la necesidad de que HR sí pueda ver el dato real.

**B. Data Redaction (redacción)**

- La redacción **elimina completamente** el dato sensible (lo borra o lo sustituye por nada, ej. "[REDACTED]").
- Es aún más destructivo que el masking: el dato original se pierde para siempre. HR tampoco podría recuperarlo. No cumple el requisito de acceso condicional.

**C. Data Inspection (inspección)**

- La inspección de DLP solo **identifica/clasifica** qué datos son sensibles (detecta que un campo es un email o nombre), pero **no los transforma ni los protege**.
- Es un paso de descubrimiento, no de protección. Si solo inspeccionas, el dato sensible sigue expuesto tal cual en BigQuery.