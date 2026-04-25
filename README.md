# 📊 Portafolio de Modelos Excel y Dashboards Operativos
### Francisco Rolando García — Business & Operations Analytics Specialist

> **Ing. Industrial (USAC) · M.Sc. Estadística Aplicada (USAC) · M.Sc. Gestión de Calidad (U. Galileo)**
> HACCP Alliance Certified · FSMA Qualified Individual (Agexport)
> 📍 Antigua Guatemala · 📧 franciskogarcia@protonmail.ch

---

## 🗂️ Estructura del Portafolio

Este archivo documenta el workbook `Portafolio_Excel_Francisco_Garcia.xlsx`, que contiene cinco módulos analíticos construidos a partir de trabajo real en dos empresas agroindustriales de exportación. Cada módulo replica metodologías, estructuras de datos y sistemas de reporte que se diseñaron e implementaron en producción.

| Hoja | Empresa | Tipo de Análisis |
|------|---------|-----------------|
| `PB_Dashboard` | Palo Blanco S.A. | Dashboard operativo semanal |
| `PB_Costo_Calibre` | Palo Blanco S.A. | Modelo de costos por calibre |
| `SUMAR_KPIs` | Alimentos Sumar S.A. | Reporte mensual de KPIs de producción |
| `SUMAR_SPC` | Alimentos Sumar S.A. | Control estadístico de proceso (SPC) |
| `SUMAR_Ejecutivo` | Alimentos Sumar S.A. | Reporte ejecutivo mensual |

---

## 🏭 MÓDULO 1 — Dashboard Operativo Semanal (Palo Blanco S.A.)

### Contexto
**Empresa:** Palo Blanco S.A. — Empaque y exportación de aguacate fresco (Agosto 2024 – Diciembre 2025)
**Operación:** 120 personas por turno, 2 turnos, despacho semanal a EE.UU. bajo protocolo PIPAA-MAGA.

### Objetivo del análisis
Crear un sistema de visibilidad diaria/semanal que permitiera al equipo de producción y dirección monitorear en tiempo real los tres indicadores críticos del negocio: productividad (cajas/hora), merma total y costo por kg. Antes de este sistema, la gerencia recibía información con 3–5 días de retraso y sin posibilidad de comparación histórica.

### Herramientas usadas
- **Excel** — tablas de datos estructuradas con semáforo automático (verde/amarillo/rojo) por umbral de productividad
- **Gráfico de línea** — tendencia de productividad (Cajas/hora) semana a semana
- **Gráfico de barras** — evolución del porcentaje de merma semanal
- **Fórmulas dinámicas** — KPIs de cabecera actualizados automáticamente desde la tabla de datos
- **Codificación por color** — estándar financiero: azul = inputs, negro = fórmulas

### Variables monitoreadas

| KPI | Descripción | Fórmula base |
|-----|-------------|-------------|
| **Productividad (Caj/h)** | Cajas empacadas / horas efectivas de turno | `=Cajas / Horas_turno` |
| **% Merma** | Kg descartados / Kg totales ingresados | `=KG_merma / KG_entrada` |
| **Costo / kg (Q)** | Costo operativo total / kg neto empacado | Modelo de costos vinculado |
| **Índice de calidad %** | Cajas aprobadas / cajas totales empacadas | `=Cajas_OK / Total_cajas` |

### Insights clave

- **La productividad aumentó 28% en 12 semanas** (de 69.7 a 89.4 cajas/hora), resultado directo de la implementación de un sistema de pago por rendimiento diseñado con base en los datos del dashboard.
- **La merma mostró correlación negativa con el calibre dominante:** los lotes con predominio de calibre 64 mostraron menores tasas de merma que los de calibre 48, evidenciando una oportunidad de optimización en la programación de cosecha.
- **Los turnos nocturnos consistentemente registraron productividad 4–6% menor** que los diurnos con la misma dotación de personal — hallazgo que derivó en ajustes al plan de distribución de trabajo por turno.
- **El semáforo automático permitió identificar en S01 y S04 condiciones de revisión** (productividad < 70 cajas/hora), generando acciones correctivas inmediatas sin necesidad de análisis manual.

---

## 💰 MÓDULO 2 — Modelo de Costos por Calibre (Palo Blanco S.A.)

### Contexto
El aguacate fresco para exportación se clasifica por calibre (número de frutos por caja estándar de 4 kg). Cada calibre tiene diferente rendimiento de empaque, tasa de merma y precio de venta. Sin un modelo que separara estos costos por categoría, la gerencia tomaba decisiones de precio y aceptación de lotes con información agregada que distorsionaba la rentabilidad real.

### Objetivo del análisis
Construir un modelo financiero dinámico que calculara el **costo real por kg** para cada calibre, incorporando merma diferenciada, costo de mano de obra proporcional, insumos, logística y cadena de frío — y que arrojara el margen bruto por calibre para soportar decisiones de negociación con clientes y programación de línea.

### Herramientas usadas
- **Excel con supuestos centralizados** — todos los inputs (costo MO/hora, costo insumos/caja, tarifa logística, etc.) viven en celdas nombradas en la sección de supuestos; el modelo completo se recalcula cambiando un solo valor
- **Codificación de color financiera:**
  - 🔵 **Azul** = inputs modificables por el usuario (costos unitarios, % merma por calibre)
  - ⚫ **Negro** = fórmulas calculadas (nunca se editan directamente)
- **Fila de totales y promedios ponderados** — permite comparar el costo promedio ponderado del lote contra el costo estándar presupuestado
- **Formato condicional implícito** — calibres de descarte/rechazo resaltados visualmente para separación inmediata

### Estructura del modelo

```
KG Entrada
    └─► × % Merma = KG Merma
    └─► × (1 - %Merma) = KG Neto
              └─► Costo MO/kg = (Tarifa MO × Horas × Personal) / KG_Neto
              └─► Costo Insumos/kg = Costo_caja / Kg_por_caja
              └─► Costo Logística/kg = Tarifa_logística (input fijo)
              └─► Costo Frío/kg = Tarifa_frío × días
              └─► COSTO TOTAL/kg = Σ de los anteriores
              └─► MARGEN BRUTO/kg = Precio_venta - Costo_total
              └─► MARGEN % = Margen_bruto / Precio_venta
```

### Insights clave

- **El calibre 84+ (Small) opera con merma 58% mayor que el calibre 40 (L+)**, generando un costo/kg estructuralmente más alto que hace inviable su exportación directa si el diferencial de precio no lo compensa — el modelo permitió cuantificarlo y presentarlo a dirección.
- **El "Rechazo/Descarte" (10% de merma promedio) representaba el 4.1% del peso total ingresado** que se perdía sin valorización — el modelo fue insumo para evaluar la factibilidad de un canal de venta secundario (mercado local).
- **Sensibilidad al costo de MO:** un incremento del 10% en el costo de mano de obra impacta el margen bruto en ~1.2 puntos porcentuales para calibres medianos — dato crítico para negociación de contratos y planificación de temporada.
- **Los calibres 56 y 64 son los más rentables** por combinar menor merma con mayor volumen de procesamiento — hallazgo que influyó en la estrategia de selección de proveedores de campo.

---

## 📊 MÓDULO 3 — KPIs Mensuales de Producción (Alimentos Sumar S.A.)

### Contexto
**Empresa:** Alimentos Sumar S.A. — Procesado y exportación de frutas y verduras congeladas (Junio 2013 – Junio 2024)
**Operación:** 250+ personas, múltiples líneas de proceso (brócoli, ejote, zanahoria, mango, piña, mora), producción continua para clientes en EE.UU. y Europa.

### Objetivo del análisis
Reemplazar el reporte de producción tradicional (tabla plana en Word, sin comparativos ni tendencias) por un **sistema de KPIs mensual** que permitiera a la dirección evaluar el desempeño operativo con una sola vista, identificar meses fuera de meta y tomar decisiones basadas en datos históricos.

### Herramientas usadas
- **Excel estructurado** — una fila por mes, una columna por KPI, totales y promedios anuales automáticos
- **OEE (Overall Equipment Effectiveness)** — cálculo compuesto: Disponibilidad × Rendimiento × Calidad
- **Gráfico de línea dual** — OEE vs. Rendimiento mensual superpuestos para identificar divergencias
- **Semáforo automático** — verde si OEE ≥ 75% Y rendimiento ≥ 92%, amarillo si OEE ≥ 70%, rojo si por debajo

### KPIs incluidos y su interpretación

| KPI | ¿Qué mide? | Meta 2024 |
|-----|-----------|----------|
| **% OEE** | Eficiencia global del equipo productivo | ≥ 75% |
| **% Rendimiento** | Kg producto terminado / Kg materia prima | ≥ 91% |
| **% Merma** | Pérdida total en proceso / entrada total | ≤ 5.5% |
| **% Reprocesos** | Producto que requirió retrabajo | < 4% |
| **Paros (h/mes)** | Horas de línea detenida no programadas | < 12 h |
| **Cajas/hora** | Productividad volumétrica del turno | > 140 |
| **% Calidad** | Producto aprobado / producto total | ≥ 97% |

### Insights clave

- **Mejora sostenida durante los 12 meses de 2024:** OEE pasó de 72.2% (enero) a 78.1% (diciembre), con una mejora de +5.9pp en el año — superando la meta anual de 75% por amplio margen.
- **Los paros no programados se redujeron 31%** (de 14.2h en enero a 9.8h en diciembre), resultado directo del plan de mantenimiento preventivo basado en análisis de causas raíz documentado con los datos del KPI dashboard.
- **La merma siguió tendencia descendente lineal** durante todo el año (de 5.80% a 4.85%), reflejando el impacto acumulado de rutinas Kaizen en puntos críticos de proceso. Meta < 5.5% superada desde el mes 5.
- **Ningún mes cerró en rojo** (OEE < 70%) durante 2024 — resultado que contrasta con 3 meses rojos en 2023, antes de la implementación del sistema de seguimiento semanal.

---

## 📉 MÓDULO 4 — Control Estadístico de Proceso / SPC (Alimentos Sumar S.A.)

### Contexto
El procesado de brócoli congelado es uno de los productos con mayor variabilidad en merma dentro de la planta de Sumar, debido a la heterogeneidad de la materia prima recibida de campo. La dirección necesitaba saber si la variabilidad observada era **ruido aleatorio del proceso** (causa común) o si respondía a **causas asignables** que requerían intervención.

### Objetivo del análisis
Implementar un **Gráfico de Control X-barra / Rango (R)** para monitorear la estabilidad estadística del proceso en la variable % de merma por lote, con base en muestreo sistemático de n=5 unidades por lote, y detectar automáticamente puntos fuera de los límites de control calculados (±3σ).

### Herramientas usadas
- **Excel** — cálculo de X-barra (media de subgrupo) y Rango R para cada lote
- **Constantes de control para n=5:** A₂ = 0.577, D₃ = 0, D₄ = 2.114 (tablas SPC estándar)
- **Fórmulas de límites de control:**
  - UCL(X̄) = X̄̄ + A₂ × R̄
  - LCL(X̄) = X̄̄ − A₂ × R̄ (con mínimo en 0)
  - UCL(R) = D₄ × R̄
- **Gráfico de línea con 3 series** — X-barra del proceso + UCL + LCL, con líneas de límite en rojo discontinuo
- **Detección automática de puntos fuera de control** — fórmula `IF(xbar > UCL OR xbar < LCL, "🔴 Fuera", "🟢 Control")`
- **R/SPSS/MINITAB** — validación cruzada de los límites calculados en Excel

### Conceptos estadísticos aplicados

```
Gran Media (X̄̄)  = Promedio de todas las X-barras de los 25 lotes
Media de Rangos (R̄) = Promedio de todos los rangos R

Límite Superior de Control:  UCL(X̄) = X̄̄ + A₂·R̄
Límite Inferior de Control:  LCL(X̄) = X̄̄ − A₂·R̄
Límite Superior de Rango:    UCL(R)  = D₄·R̄

Un proceso está "en control estadístico" cuando TODOS los puntos
caen dentro de UCL y LCL sin patrones sistemáticos (rachas, tendencias).
```

### Insights clave

- **El proceso mostró tendencia descendente sostenida** en X-barra durante los 25 lotes monitoreados, evidenciando el impacto de las mejoras implementadas (estandarización de parámetros de corte y temperatura de ingreso de materia prima).
- **La variabilidad intra-lote (Rango R) se redujo progresivamente**, lo que indica mayor homogeneidad en la materia prima recibida tras la implementación de protocolos de recepción más estrictos con proveedores de campo.
- **Los lotes fuera de control correspondieron consistentemente a días con cambio de proveedor** — hallazgo que no era visible en el reporte tradicional de promedios, pero que el gráfico SPC reveló inmediatamente como causa asignable.
- **Implicación práctica:** el proceso pasó de "en control con alta variabilidad" a "en control con variabilidad reducida" a lo largo del año, lo que significa que futuras mejoras deben enfocarse en reducir la variación de causas comunes (diseño del proceso) y no solo en reaccionar a eventos aislados.

---

## 📋 MÓDULO 5 — Reporte Ejecutivo Mensual (Alimentos Sumar S.A.)

### Contexto
La dirección general de Sumar requería un reporte mensual de producción que pudiera revisarse en menos de 10 minutos, sin necesidad de navegar múltiples archivos o interpretar tablas de datos crudos. El reporte anterior era un correo con texto y una tabla de Excel sin formato, que tomaba 30+ minutos de preparación manual.

### Objetivo del análisis
Diseñar una **plantilla de reporte ejecutivo estandarizado** que integrara en una sola hoja: los KPIs del mes, la narrativa de logros, el comparativo vs. año anterior, y las acciones pendientes — todo actualizable en menos de 15 minutos al cierre de cada mes.

### Herramientas usadas
- **Excel con estructura de reporte ejecutivo** — diseño visual tipo "management report" con secciones claramente delimitadas
- **Bloque de 6 KPIs con semáforo** — lectura inmediata del estado del mes sin necesidad de contexto adicional
- **Tabla comparativa 2023 vs. 2024** — variación absoluta y porcentual calculada automáticamente para cada indicador
- **Sección de logros narrativos** — 5 puntos de análisis cualitativo con respaldo cuantitativo
- **Plan de acciones** — priorización visual (Alta / Media / Baja) con descripción de la acción pendiente

### Estructura del reporte (replicable mensualmente)

```
┌─────────────────────────────────────────────────────────┐
│  ENCABEZADO: Empresa · Período · Responsable            │
├─────────────────────────────────────────────────────────┤
│  STRIP DE 6 KPIs CON SEMÁFORO (actualizar mensualmente) │
├─────────────────────────────────────────────────────────┤
│  LOGROS PRINCIPALES (5 narrativas con datos)            │
├─────────────────────────────────────────────────────────┤
│  COMPARATIVO AÑO ANTERIOR (fórmulas automáticas)        │
├─────────────────────────────────────────────────────────┤
│  ACCIONES PENDIENTES Q SIGUIENTE (con prioridad)        │
└─────────────────────────────────────────────────────────┘
```

### Insights clave

- **El reporte de diciembre 2024 cerró con mejora en los 6 KPIs vs. noviembre** — el primer mes del año en lograr mejora simultánea en todos los indicadores, resultado del efecto acumulado de las iniciativas de mejora continua del año.
- **El comparativo 2023 vs. 2024 mostró una mejora del 14.3% en merma promedio** (de 6.07% a 5.20%), equivalente a aproximadamente 50 toneladas métricas de producto recuperado anualmente — un argumento directo para presentar a dirección el ROI del sistema de control de producción implementado.
- **El diseño del reporte redujo el tiempo de preparación mensual de 30+ minutos a menos de 15 minutos**, al estandarizar la estructura y eliminar la necesidad de construir el formato desde cero cada mes.
- **La sección de acciones pendientes con prioridades** generó un mecanismo de rendición de cuentas que antes no existía — cada reporte revisaba el cumplimiento de las acciones del mes anterior.

---

## 🛠️ Stack Tecnológico Utilizado

| Herramienta | Uso en este portafolio |
|-------------|----------------------|
| **Microsoft Excel (Advanced)** | Todos los módulos: tablas, fórmulas, gráficos, formato condicional |
| **Power BI** | Dashboards interactivos complementarios (no incluidos en este archivo) |
| **R / SPSS** | Validación estadística de límites de control SPC y análisis de varianza |
| **MINITAB** | Análisis de capacidad de proceso (Cp, Cpk) como insumo para el SPC |
| **SAP Business One** | Fuente de datos de costos, órdenes de producción y entradas de almacén |

---

## 📐 Convenciones de Codificación (Financial Modeling Standards)

Todos los modelos en este portafolio siguen el estándar profesional de color para hojas financieras:

| Color del texto | Significado |
|----------------|-------------|
| 🔵 **Azul** | Input hardcoded — el usuario puede/debe modificar este valor |
| ⚫ **Negro** | Fórmula calculada — nunca se edita directamente |
| 🟢 **Verde** | Referencia a otra hoja del mismo workbook |
| 🔴 **Rojo** | Alerta automática — valor fuera de límite o meta |

---

## 📈 Resultados Documentados

Los modelos en este portafolio no son ejercicios teóricos. Reflejan implementaciones reales cuyos resultados fueron medidos y verificados:

> **▼ 33%** reducción de merma total en Palo Blanco — logrado mediante clasificación por calibre mejorada, control de peso en línea y reducción de reproceso
>
> **▲ 28%** incremento de productividad en Palo Blanco — logrado mediante sistema de pago por rendimiento diseñado con base en datos del dashboard
>
> **▲ 5.9pp** mejora en OEE en Sumar durante 2024 — de 72.2% a 78.1%, superando meta anual de 75%
>
> **▼ 14.3%** reducción de merma promedio en Sumar 2023→2024 — equivalente a ~50 toneladas métricas anuales recuperadas

---

## 📬 Contacto

**Francisco Rolando García**
Industrial Engineer · Operations & Data Analytics
📍 Antigua Guatemala, Guatemala (Remote-ready | UTC-6)
📧 franciskogarcia@protonmail.ch
📱 +502 3048 3287
🔗 [linkedin.com/in/francisco-garcia-6751003a4](https://linkedin.com/in/francisco-garcia-6751003a4)

---

*Este portafolio fue diseñado para demostrar capacidades reales en modelado de negocios, análisis de datos operativos y construcción de sistemas de reporte ejecutivo — habilidades directamente aplicables a roles de Business & Operations Analytics, AI document evaluation, y consultoría agroindustrial remota.*
