# Checkpoint — Skill "Informe de Acondicionamiento Térmico"

**Fecha:** 26 julio 2026
**Alcance actual:** Solo viviendas (uso residencial, Numeral 1 del Art. 4.1.10 OGUC). Uso equipamiento (educación/salud) y hoteles quedan fuera por ahora.

---

## 1. Marco normativo relevante

- **Art. 4.1.10 OGUC** (modificado por DS 15/2021 – DO 27-05-2024, vigencia desde 28-nov-2025): exige a la envolvente térmica cumplir con transmitancia/resistencia térmica, % máximo de ventanas por orientación, ausencia de riesgo de condensación superficial e intersticial, infiltración de aire máxima, y ventilación mínima.
- **Res. Ex. 1802 MINVU (26-nov-2025):** fija las condiciones oficiales de cálculo:
  - Condensación (NCh1973): análisis en Sección A (mayor Rt) y Sección B (menor Rt / puente térmico), condiciones interiores fijas **19°C / 75% HR**, exteriores según **Anexo 1** (por comuna/Zona Térmica).
  - Transmitancia (NCh853) y transmisión por terreno (NCh3117): reglas de cálculo de Rt total, error relativo máximo 20%.
- Exigencias de **uso residencial** están en las Tablas 1, 2, 3, 4, 5, 6, 7, 8, 9 del documento de modificación del Art. 4.1.10 (envolvente opaca, ventanas, sobrecimientos, infiltración).
- Infiltración de aire y ventilación se acreditan **documentalmente** (ensayo de terceros o solución de sellos), no mediante cálculo propio en esta etapa.

---

## 2. Herramientas Excel disponibles y su rol

| Archivo | Rol | Notas |
|---|---|---|
| `Analisis_higrotermico_v2026_06_23.xlsm` | **Motor principal**: transmitancia (NCh853) + condensación superficial/intersticial (NCh1973) por elemento de envolvente (muro, techo, piso ventilado, sobrecimiento) | Implementa el criterio oficial de la Res. 1802 (19°C/75%, Anexo 1 por comuna). Tiene biblioteca de materiales DITEC/ISO10456, secciones A/B, cálculo de fracción de área para puentes térmicos (madera/albañilería), hoja Resumen (Caso Proyectado vs Caso base). 7 módulos VBA (Mod_analisis, Mod_condensacion, Mod_configuracion, Mod_importar_exportar, Mod_materiales, Mod_resumen, Mod_transmitancia) — probablemente utilitarios de importar/exportar, no afectan el cálculo base (hecho con fórmulas/tablas). |
| `CalculoUVentanasDITECV_2024_1_0.xlsx` | Cálculo de U por ventana/puerta vidriada según **NCh3137-1** | Herramienta oficial DITEC-MINVU. Un archivo = una ventana. Entrada: dimensiones, tipo de marco, tipo de vidriado. Salida: Uw, aporte de marco/vidrio/espaciador. |
| `Calculos_tablas_finales_ejemplo.xlsx` | Ejemplo de **tabla resumen de cumplimiento de ventanas por orientación** (Tabla 3 OGUC) | Formato objetivo de salida, no motor de cálculo (tabla mayormente hardcodeada a mano). |
| `Condensaciones_DITEC_V2026_02.xlsm` | Versión **antigua** del cálculo de condensación | Usa criterio previo a la Res. 1802 (HR interior 65/75/80%, temperatura "media mínima mensual"). **Descartada como motor**; se deja solo como posible respaldo/verificación cruzada. |

### Mapeo exigencia → herramienta

| Exigencia (Art. 4.1.10, N°1 uso residencial) | Tabla OGUC | Herramienta |
|---|---|---|
| Transmitancia/Rt: techumbre, muros, piso ventilado, puertas opacas | Tabla 1 | `Analisis_higrotermico` → hoja Transmitancia |
| Complejo de ventanas (% máx. por orientación o Upvm) | Tabla 3 / Tabla 5 | `CalculoUVentanasDITEC` (por ventana) + tabla resumen tipo `Calculos_tablas_finales` |
| Sobrecimientos | Tabla 6 | `Analisis_higrotermico` (tratado como sección adicional) |
| Condensación superficial e intersticial | NCh1973 / Res. 1802 | `Analisis_higrotermico` → hoja Condensación |
| Infiltraciones de aire | Tabla 7 / Tabla 9 | **Documental** — solución de sellos por defecto (ver decisión pendiente abajo) |
| Ventilación | NCh3308/3309 | **Documental** — informe de terceros |

---

## 3. Estructura del informe de ejemplo (target de la Fase 3)

1. Generalidades + implicancias normativas
2. Zona térmica del emplazamiento (mapa + comuna)
3. Condiciones climáticas (Text, HR según Anexo 1)
4. Transmitancia máxima de envolvente: tabla comparativa exigencia-zona vs. solución de proyecto, con estado CUMPLE/NO CUMPLE
5. Cálculo y justificación de % de superficie vidriada por orientación (metodología + tabla resumen)
6. Fichas individuales por ventana (plano/corte + parámetros)
7. Planillas de cálculo de transmitancia por ventana (salida de `CalculoUVentanasDITEC`)
8. Planillas de análisis higrotérmico por solución constructiva (muro, techo): materiales, capas, Rt, condensación superficial/intersticial, gráficos de presión de vapor y temperatura
9. Puertas opacas: memoria descriptiva + fichas de soluciones tipo (LOSCAT)

### Reverse-engineering: origen de cada dato del informe

- **(E) = se extrae de EETT/planos** | **(D) = se deriva/calcula automáticamente** | **(N) = texto normativo fijo**

| Sección | Dato | Origen |
|---|---|---|
| Generalidades | Proyecto, dirección, propietario, arquitecto, constructor | E |
| Zona térmica | Comuna → Zona (A-I) | D (NCh1079/Anexo 1) |
| Condiciones climáticas | Text, HR exterior | D (Anexo 1 Res. 1802) |
| Transmitancia envolvente | Composición de capas de cada elemento | **E** |
| Transmitancia envolvente | U/Rt exigido por Tabla 1, U/Rt del proyecto, ¿cumple? | D |
| Ventanas | Código, dimensiones, orientación, área muro asociada, tipo marco/vidrio (o U certificado) | **E** |
| Ventanas | % máx. por orientación, U por ventana, ¿cumple? | D |
| Análisis higrotérmico | Mismas capas que envolvente (dato reutilizado) | E (ya capturado) |
| Análisis higrotérmico | Rt sección A/B, ¿cumple superficial/intersticial? | D |
| Puertas opacas | Marca/modelo o solución tipo, U certificado o ficha LOSCAT | **E** |
| Todo el informe | Marco legal, definiciones, metodología | N |

**Conclusión clave:** lo único que realmente hay que extraer de la EETT es la **composición de capas de cada elemento de la envolvente** (con secciones A/B si hay puente térmico) y el **cuadro de vanos** (ventanas + puertas). Todo lo demás se deriva o se calcula. El dato más probable de faltar en una EETT genérica es el detalle del **puente térmico** (Sección B).

---

## 4. Flujo de 3 fases acordado

### Fase 1 — Extracción y captura de datos del proyecto
Levantar desde la EETT (formato `.md`, provista por el usuario) un modelo de datos JSON por proyecto:
1.1 Ficha general del proyecto
1.2 Inventario de envolvente térmica (capas por elemento, Sección A y B)
1.3 Puentes térmicos (fracción de área si estructura madera/albañilería)
1.4 Cuadro de vanos (ventanas)
1.5 Puertas opacas
1.6 Referencias de infiltración/ventilación (documental)

Claude extrae lo que pueda del `.md` y pregunta lo que falte. Se almacena en JSON.

### Fase 2 — Estudio de cumplimiento, elemento por elemento
2.1 Copiar planilla plantilla correspondiente (`Analisis_higrotermico` o `CalculoUVentanasDITEC`) por elemento, con ID único.
2.2 Completar con datos del JSON, recalcular (LibreOffice/`recalc.py`), leer resultado.
2.3 Verificar cumplimiento. Si no cumple, Claude **propone alternativa**, la valida recalculando, y **avisa antes de dar por buena** la solución final.
2.4 Para ventanas: armar tabla resumen por orientación (Tabla 3) y verificar % máximo.
2.5 Exportar cada resultado a un resumen consolidado (JSON/Markdown) reutilizable para el informe, incluyendo gráficos como imagen.

### Fase 3 — Confección del informe final
3.1 Ensamblar según estructura del informe de ejemplo, leyendo el resumen consolidado de la Fase 2 (sin recálculos en esta fase).
3.2 Nota de alcance explícita sobre infiltración/ventilación (acreditación documental, no calculada).
3.3 Entregable en `.docx`. Plantilla propia a definir en conjunto (ver pendientes).

---

## 5. Decisiones acordadas

1. **EETT:** se entregan en `.md`. Claude extrae lo que pueda y pregunta lo que falte.
2. **Almacenamiento del proceso:** JSON. Claude avisa de errores/inconsistencias. El proceso (JSON) se revisa juntos solo si hay fallas; el informe final sí se revisa siempre en persona.
3. **Infiltración de aire:** por defecto, solución de sellos (partida EETT), salvo que el proyecto especifique medición por empresa/ensayo NCh3295.
   - ⚠️ **Pendiente de confirmar:** la OGUC permite esta alternativa solo mientras en la región no existan profesionales/laboratorios acreditados para el ensayo, y solo para lotes de 10 o menos unidades. Falta definir si esto aplica siempre a los proyectos del usuario o se verifica caso a caso.
4. **Elementos que no cumplen:** Claude propone alternativa, valida recalculando, y avisa antes de generar el informe final (no se autogenera el informe sin validación humana en ese punto).
5. **Plantilla del informe final:** se construirá una propia. El usuario aportará su plantilla y otras de referencia para revisarlas en conjunto antes de fijar el formato definitivo.

---

## 6. Borrador v0 del JSON (modelo de datos de Fase 1)

```json
{
  "proyecto": {
    "nombre": "",
    "direccion": "",
    "comuna": "",
    "propietario": {"nombre": "", "rut": ""},
    "arquitecto_patrocinante": {"nombre": "", "rut": ""},
    "constructor": {"nombre": "", "rut": ""},
    "destino": "residencial",
    "superficie_construida_m2": null
  },
  "envolvente": [
    {
      "id": "MURO-01",
      "tipo": "muro_perimetral",
      "descripcion": "Muro existente + ampliación",
      "aplica_a": "fachadas exteriores excepto medianero norte",
      "secciones": {
        "A_mayor_resistencia": {
          "nombre": "EMB / SIP / YC",
          "capas": [
            {"material": "", "espesor_m": null, "lambda_o_R": null, "mu": null, "biblioteca_o_fuente": ""}
          ]
        },
        "B_menor_resistencia_puente_termico": {
          "nombre": "",
          "capas": [],
          "descripcion_puente": "ej: pie derecho madera 2x4 @ 60cm"
        }
      }
    }
  ],
  "vanos": {
    "ventanas": [
      {"codigo": "V-01", "ancho_mm": null, "alto_mm": null, "area_m2": null,
       "orientacion": "", "area_muro_asociada_m2": null,
       "vidrio": {"tipo": "DVH/simple", "espesores_mm": [], "gas": "", "baja_emisividad": null},
       "marco": {"tipo": "PVC/Metal RPT/Madera", "espesor_mm": null},
       "U_certificado_W_m2K": null}
    ],
    "puertas_opacas": [
      {"codigo": "P-01", "marca_modelo": "", "U_certificado_W_m2K": null, "ficha_loscat": null}
    ]
  },
  "infiltracion_aire": {
    "metodo": "solucion_sellos",
    "detalle_sellos": "según partida EETT",
    "aplicabilidad_verificada": null
  },
  "ventilacion": {
    "metodo": "informe_terceros",
    "referencia_informe": null
  },
  "estado_extraccion": {
    "campos_faltantes": [],
    "inconsistencias_detectadas": []
  }
}
```

Este esquema es v0: se afinará cuando se revisen EETT reales y las plantillas de informe del usuario, especialmente para que los nombres de campo calcen 1 a 1 con lo que esperan `Analisis_higrotermico` (hojas Materiales/Configuración) y `CalculoUVentanasDITEC` (hoja Datos).

---

## 7. Próximos pasos / pendientes

- [ ] Usuario confirma alcance de la excepción de infiltración de aire (punto 3 de decisiones).
- [ ] Usuario envía una EETT de ejemplo en `.md` para probar la extracción contra el JSON v0.
- [ ] Usuario envía su plantilla de informe y otras de referencia para definir el formato final del `.docx` (Fase 3).
- [ ] Afinar nombres de campo del JSON para que calcen con las hojas de entrada de `Analisis_higrotermico` y `CalculoUVentanasDITEC`.
- [ ] Definir naming/organización de carpetas por proyecto (`01_datos/`, `02_calculos/`, `03_resultados/`, `04_informe/` o similar).
