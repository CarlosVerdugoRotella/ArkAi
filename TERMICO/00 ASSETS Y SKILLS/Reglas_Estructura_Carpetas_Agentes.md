# MAPA DE DIRECTORIOS Y REGLAS DE ENRUTAMIENTO (AGENTE ORQUESTADOR)

Este documento define la estructura de carpetas del sistema de Acondicionamiento Térmico y establece las **reglas obligatorias de lectura y escritura** para cualquier Agente de IA o script que opere en el repositorio.

---

## ESTRUCTURA DEL SISTEMA

```text
📁 SISTEMA_ACONDICIONAMIENTO_TERMICO
│
├── 📁 00 - BASE IA
│   ├── Prompts_y_Roles           (Instrucciones de comportamiento para agentes)
│   ├── Checkpoints_Sistema       (Estados de guardado del avance del desarrollo)
│   └── Archivos_Configuracion    (Modelos JSON, mapeo de zonas térmicas)
│
├── 📁 01 - BASE INFORMACION
│   ├── Normativa_Legal           (Leyes, OGUC, Resoluciones en .md)
│   ├── Plantillas_Excel_Motor    (Excels de cálculo en BLANCO / Vírgenes)
│   └── Plantillas_Informe_Base   (Esqueleto del informe final en .md o .docx)
│
├── 📁 02 - INPUT PROYECTOS
│   └── 📁 [ID_PROYECTO]          (Ej: PRJ_001_Casa_Los_Pinos)
│       ├── 01_Arquitectura       (Planos, elevaciones, cuadros de vanos)
│       └── 02_Especificaciones   (EETT en formato texto o .md)
│
├── 📁 03 - DATA Y CALCULOS
│   └── 📁 [ID_PROYECTO]          (Debe coincidir exactamente con el ID de INPUT)
│       ├── 01_Data_Extraida      (El JSON bruto generado por el Agente Extractor)
│       ├── 02_Excel_Calculados   (Copias del motor Excel rellenadas con data)
│       └── 03_Resultados_Estudios(Outputs de cálculo, tablas y gráficos)
│
└── 📁 04 - OUTPUT INFORMES
    └── 📁 [ID_PROYECTO]          (Debe coincidir exactamente con el ID de INPUT)
        ├── 01_Borradores_Parciales(Fragmentos de texto armados por la IA)
        └── 02_Informe_Final_Unificado (PDF final consolidado y revisado por el arquitecto)
```

---

## REGLAS OPERATIVAS PARA LA IA

### 1. Reglas de LECTURA (Búsqueda de Información)
*   **Para entender cómo operar o qué extraer:** El agente debe leer SIEMPRE la carpeta `00 - BASE IA`. Aquí residen sus prompts y el esquema del JSON esperado.
*   **Para buscar límites normativos:** El agente (Especialmente el Revisor) debe consultar `01 - BASE INFORMACION / Normativa_Legal`.
*   **Para iniciar un nuevo proyecto:** El agente Lector/Extractor debe apuntar EXCLUSIVAMENTE a `02 - INPUT PROYECTOS / [ID_PROYECTO]`. **Nunca** debe buscar planos o especificaciones fuera de esa subcarpeta.

### 2. Reglas de ESCRITURA (Generación y Guardado)
*   **Manejo de IDs Cruzados:** Si el agente recibe la instrucción de procesar el proyecto alojado en `02 - INPUT PROYECTOS / PRJ_005_Edificio_Centro`, DEBE crear (si no existen) las subcarpetas `PRJ_005_Edificio_Centro` dentro de `03 - DATA Y CALCULOS` y `04 - OUTPUT INFORMES`.
*   **Guardado del JSON:** El Agente Lector/Extractor SIEMPRE debe guardar su archivo de salida (`datos_extraidos.json`) en la ruta `03 - DATA Y CALCULOS / [ID_PROYECTO] / 01_Data_Extraida`.
*   **Manejo de Planillas Excel (CRÍTICO):** El agente calculista **NUNCA debe modificar** los archivos en `01 - BASE INFORMACION / Plantillas_Excel_Motor`. Su instrucción estricta es:
    1.  Copiar el archivo Excel virgen desde `01`.
    2.  Pegarlo en `03 - DATA Y CALCULOS / [ID_PROYECTO] / 02_Excel_Calculados`.
    3.  Modificar, inyectar datos y ejecutar cálculos SOLO en esta copia.
*   **Generación de Informes:** Los textos y tablas consolidadas por el Agente Redactor deben guardarse en `04 - OUTPUT INFORMES / [ID_PROYECTO] / 01_Borradores_Parciales`. La subcarpeta `02_Informe_Final_Unificado` está reservada para el veto humano (solo el Arquitecto depositará allí el PDF final aprobado).

### 3. Regla de Alertas e Inconsistencias
Si al buscar en `02 - INPUT PROYECTOS` el agente detecta que faltan documentos o el JSON generado tiene el valor `"listo_para_calculo": false`, la IA debe detener la escritura en `03` y `04`, y generar un log de alerta para el Arquitecto. El flujo no avanza a cálculo sin un JSON validado.