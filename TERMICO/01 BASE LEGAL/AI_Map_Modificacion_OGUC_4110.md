# AI SYSTEM PROMPT & MEMORY MAP: Modificación Art. 4.1.10 OGUC (D.S. 15/2021)

**Contexto del Archivo:** Este documento contiene la norma técnica principal para el sistema. Fija los límites máximos y exigencias de diseño para la envolvente térmica en Chile (obligatorio desde 28-nov-2025). 
**Objetivo de este mapa:** Instruir a los Agentes IA sobre **qué buscar, dónde encontrarlo y cómo parsearlo** dentro del archivo `Modificacion-art-4110-OGUC_Reglamentacion-Termica.md`.

---

## 1. MAPA DE EXIGENCIAS POR TABLAS (Búsqueda Rápida)
Para verificar cumplimiento, la IA debe buscar directamente las siguientes tablas mediante anclajes de texto (Text Anchors):

| Búsqueda / Palabra Clave | Contenido a extraer | Condición de uso para la IA |
| :--- | :--- | :--- |
| `"Tabla 1"` o `"Tabla N° 1"` | Límite de Transmitancia (Valor U) y Resistencia (Rt) para Muros, Techos y Pisos Ventilados. | Usar siempre para envolvente opaca. Cruzar fila con la Zona Térmica (A a I) del proyecto. |
| `"Tabla 2"` o `"Tabla N° 2"` | Límite máximo de superficie de ventanas (% de fachada). | Usar si el proyecto NO detalla orientación de ventanas. |
| `"Tabla 3"` o `"Tabla N° 3"` | Límite máximo de ventanas por ORIENTACIÓN (% según Norte, Sur, Este, Oeste). | Usar si el cuadro de vanos del proyecto especifica la orientación. (Prioridad sobre Tabla 2). |
| `"Tabla 5"` o `"Tabla N° 5"` | Transmitancia térmica máxima para ventanas (U). | Usar para verificar si el vidrio/marco propuesto cumple el aislamiento exigido. |
| `"Tabla 6"` o `"Tabla N° 6"` | Transmitancia térmica lineal para Sobrecimientos. | Usar solo para elementos de contacto con el terreno. |
| `"Tabla 7", "Tabla 8", "Tabla 9"`| Clase de infiltración de aire requerida según Zona Térmica. | Extraer clase requerida para contrastar con las EETT de sellos o informe de ensayo. |

---

## 2. MAPA ESTRUCTURAL DE CONCEPTOS (Secciones)

Si la IA necesita extraer reglas conceptuales en lugar de números, debe escanear las siguientes secciones:

### A. Alcance y Excepciones
*   **Target de Búsqueda:** `"uso residencial"`, `"excepciones"`, `"ampliaciones"`.
*   **Regla AI:** Verificar primero el destino del proyecto. Si es "equipamiento", "salud" u "hoteles", la IA debe alertar que "queda fuera del alcance residencial actual" (según Numeral 1).
*   **Regla Ampliaciones:** Buscar si el proyecto es ampliación. La norma tiene exenciones de cumplimiento dependiendo de los m2 ampliados respecto a lo existente.

### B. Riesgo de Condensación
*   **Target de Búsqueda:** `"condensación superficial"`, `"condensación intersticial"`, `"NCh1973"`.
*   **Regla AI:** En el documento del Art 4.1.10 la IA solo encontrará **la exigencia** (el "qué": no debe haber condensación). Para extraer los **parámetros de cálculo** (el "cómo": 19°C, 75% HR), la IA debe abandonar este documento y saltar a la **"Resolución Exenta 1802"**.

### C. Zonas Térmicas
*   **Target de Búsqueda:** `"Zona Térmica"`, `"Clasificación"`.
*   **Regla AI:** Las exigencias están categorizadas por letras (A, B, C, D, E, F, G, H, I). Si el input del proyecto solo entrega "Comuna", la IA debe usar su conocimiento interno o un archivo anexo (Manual de Aplicación / NCh1079) para cruzar "Comuna -> Zona Térmica" ANTES de leer las Tablas 1 a 9.

---

## 3. HEURÍSTICAS DE LECTURA PARA LA IA (Reglas de Extracción)

1.  **Prioridad de PDA:** Antes de aplicar la `Tabla 1` de este documento, la IA debe buscar si el contexto menciona un "Plan de Descontaminación Atmosférica (PDA)". Si existe PDA, los valores de transmitancia del PDA sobrescriben este documento.
2.  **Identificación de Puentes Térmicos:** Cuando la IA evalúe muros (madera o metal metalcon), debe buscar obligatoriamente la `"Sección B"` (zona de entramado). La exigencia del Art. 4.1.10 requiere calcular el U Ponderado considerando el porcentaje de madera/metal.
3.  **Puertas Opacas:** Buscar `"Transmitancia térmica de puertas opacas"`. La IA debe tratarlas como muros perimetrales, aplicando el mismo valor U exigido en la `Tabla 1` para muros.
4.  **Cálculos ciegos:** La IA no debe calcular U_ventanas o U_muros leyendo fórmulas de este texto. Este texto solo contiene **Límites de Validación**. Los algoritmos de cálculo matemático residen en el código Python o Excel (`Analisishigrotermico.xlsm` / `CalculoUVentanasDITEC`).

---
**FIN DEL SYSTEM PROMPT MAP**
*Este archivo sirve como índice semántico para que los agentes LLM sepan cómo recuperar constraints normativos sin tener que leer el documento legal completo en cada iteración.*