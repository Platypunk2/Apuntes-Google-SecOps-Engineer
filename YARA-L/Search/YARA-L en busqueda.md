YARA-L tambien se ocupa como el modo de exploración/hunting: se hace una consulta a los logs normalizados en UDM para encontrar eventos específicos, sin necesidad de que "dispare" una alerta como una regla.

# Estructura

| Orden | Sección     | ¿Obligatoria?                                            |
| ----- | ----------- | -------------------------------------------------------- |
| 1     | `meta`      | No                                                       |
| 2     | `events`    | Sí                                                       |
| 3     | `match`     | No (solo si agrupás/correlacionás)                       |
| 4     | `outcome`   | No (permitido sin agregados y sin `match`)               |
| 5     | `condition` | No                                                       |
| 6     | `dedup`     | No (exclusiva de Search, sirve para eliminar duplicados) |
> [!NOTE] Nota:
> `options` **no** apica en Search, solo en Rules

```
events:
   // filtros de campos UDM

match:
   // (opcional) agrupar por campos + ventana de tiempo

outcome:
   // (opcional) cálculos/agregaciones

dedup:
   // (opcional) deduplicar resultados por campo
```

# Ejemplos

Buscar todos los procesos que ejecutaron `powershell.exe`:

```
events:
   $e.metadata.event_type = "PROCESS_LAUNCH"
   $e.target.process.file.full_path = /powershell\.exe/ nocase
```

Solo tiene `events`. No hace falta nada más — es una consulta directa.

---

Ver conexiones de red entre una IP interna y una IP externa, sin resultados repetidos:

```
events:
   metadata.event_type = "NETWORK_CONNECTION"
   target.ip != ""
   principal.ip != ""
match:
   target.ip, principal.ip
dedup:
   principal.ip
```

Acá `match` agrupa por las dos IPs, y `dedup` elimina duplicados basándose en `principal.ip`. No hay `meta` ni `condition` porque no es una regla de alerta, es una vista de datos.

Acá no hay ninguna necesidad de "etiquetar" ese evento con un nombre (`$e`, `$conn`, etc.) porque **no hay un segundo evento con el cual correlacionarlo** ni un placeholder que reutilizar en otra sección aparte de `match`/`dedup` (que referencian directamente el nombre del campo, no una variable).

### ¿Cuándo SÍ es obligatorio el `$`?

- Cuando hacés **correlación multi-evento** (necesitás distinguir `$login` de `$file_op`, por ejemplo)
- Cuando necesitás usar ese valor en **`outcome`** con funciones de agregación referenciando el evento (`count($e.metadata.id)`)
- Cuando estás en una **Rule** con `condition` que usa el símbolo `#` (`#e > 5`) — ahí sí o sí necesitás la variable declarada