# SISTEMA DE ACONDICIONAMIENTO TÉRMICO

Sistema de procesamiento automatizado de estudios de acondicionamiento térmico, operado mediante Agentes de IA bajo supervisión del Arquitecto.

## Flujo de trabajo

1. **INPUT** → Arquitecto sube planos y EETT a  2 - INPUT PROYECTOS / \[ID\_PROYECTO]
2. **EXTRACCIÓN** → Agente Extractor genera datos\_extraidos.json en  3
3. **CÁLCULO** → Agente Calculista copia plantillas y ejecuta cálculos en  3
4. **REDACCIÓN** → Agente Redactor genera borradores en  4 / 01\_Borradores\_Parciales
5. **REVISIÓN** → Arquitecto aprueba y deposita PDF final en  4 / 02\_Informe\_Final\_Unificado

## Regla de IDs

El ID PRJ\_NNN\_Nombre debe ser **idéntico** en carpetas  2,  3 y  4.


TERMICO/SISTEMA\_ACONDICIONAMIENTO\_TERMICO/

├── README.md

├── 00 - BASE IA/

│   ├── Prompts\_y\_Roles/README.md

│   ├── Checkpoints\_Sistema/README.md

│   └── Archivos\_Configuracion/README.md

├── 01 - BASE INFORMACION/

│   ├── Normativa\_Legal/README.md

│   ├── Plantillas\_Excel\_Motor/README.md

│   └── Plantillas\_Informe\_Base/README.md

├── 02 - INPUT PROYECTOS/

│   ├── README.md

│   └── \_PLANTILLA\_ID\_PROYECTO/

│       ├── 01\_Arquitectura/README.md

│       └── 02\_Especificaciones/README.md

├── 03 - DATA Y CALCULOS/

│   ├── README.md

│   └── \_PLANTILLA\_ID\_PROYECTO/

│       ├── 01\_Data\_Extraida/README.md

│       ├── 02\_Excel\_Calculados/README.md

│       └── 03\_Resultados\_Estudios/README.md

└── 04 - OUTPUT INFORMES/

&#x20;   ├── README.md

&#x20;   └── \_PLANTILLA\_ID\_PROYECTO/

&#x20;       ├── 01\_Borradores\_Parciales/README.md

&#x20;       └── 02\_Informe\_Final\_Unificado/README.md

