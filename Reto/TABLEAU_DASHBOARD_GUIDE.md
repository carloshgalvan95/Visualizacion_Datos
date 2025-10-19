# Guía Completa: Dashboard Tableau - Numismática México
## Estrategia Comercial de Fin de Año

---

## 📋 TABLA DE CONTENIDOS

1. [KPI 1: Ventas Totales por Día](#kpi-1-ventas-totales-por-día)
2. [KPI 2: Top 10 Productos por Ventas](#kpi-2-top-10-productos-por-ventas)
3. [KPI 3: Ventas por Estado](#kpi-3-ventas-por-estado)
4. [KPI 4: Tiempo de Entrega Promedio](#kpi-4-tiempo-de-entrega-promedio)
5. [KPI 5: ROI de Publicidad](#kpi-5-roi-de-publicidad)
6. [KPI 6: Efectividad por Canal](#kpi-6-efectividad-por-canal)
7. [KPI 7: Análisis Estacional](#kpi-7-análisis-estacional)
8. [KPI 8: Optimización Logística](#kpi-8-optimización-logística)
9. [Ensamblaje del Dashboard Final](#ensamblaje-del-dashboard-final)
10. [Filtros Globales y Acciones](#filtros-globales-y-acciones)

---

## 🚀 PREPARACIÓN INICIAL

### Importar Datos en Tableau

1. **Abrir Tableau Desktop**
2. **Conectar a datos**:
   - Clic en "Microsoft Excel"
   - Navegar a `VentasNum2025.xlsx`
   - Seleccionar la hoja "Ventas"
3. **Verificar los campos**:
   - Asegurar que `Fecha Venta` esté como tipo String (la convertiremos después)
   - `FechaCamino` y `FechaEntrega` deben ser tipo Date
   - `Total (MXN)` debe ser tipo Number (decimal)
   - `Unidades` debe ser tipo Number (whole)

### Campos Calculados Globales (Crear ANTES de empezar)

Estos campos calculados se usarán en múltiples visualizaciones:

**1. Fecha Venta (Date)**
```
// Convertir string de fecha a date format
DATE(
    DATEPARSE('dd/MM/yyyy', [Fecha Venta])
)
```

**2. Mes Nombre**
```
DATENAME('month', [Fecha Venta (Date)])
```

**3. Año**
```
YEAR([Fecha Venta (Date)])
```

**4. Mes-Año**
```
STR(MONTH([Fecha Venta (Date)])) + "-" + STR(YEAR([Fecha Venta (Date)]))
```

**5. Días para Entrega**
```
DATEDIFF('day', [FechaCamino], [FechaEntrega])
```

**6. Noviembre-Diciembre Filter**
```
MONTH([Fecha Venta (Date)]) = 11 OR MONTH([Fecha Venta (Date)]) = 12
```

**7. Es Venta con Publicidad**
```
IF [Venta por publicidad] = "Sí" THEN TRUE ELSE FALSE END
```

**8. Ingresos Netos (sin comisiones)**
```
[Ingresos por productos (MXN)]
```

**9. Semana del Año**
```
WEEK([Fecha Venta (Date)])
```

**10. Día de la Semana**
```
DATENAME('weekday', [Fecha Venta (Date)])
```

---

## KPI 1: VENTAS TOTALES POR DÍA

### 📊 Objetivo
Identificar los días más relevantes en noviembre y diciembre para maximizar ingresos y planificar estrategias promocionales.

### 🎨 Tipo de Visualización
**Dual-Axis Line Chart con Heat Map Calendar**

### 📝 Paso a Paso

#### SHEET 1A: "Ventas Diarias - Línea de Tiempo"

1. **Crear nueva hoja** → Nombrar: `1A_Ventas_Diarias_Timeline`

2. **Arrastrar campos**:
   - **Columns**: `Fecha Venta (Date)` (como día continuo)
   - **Rows**: `Total (MXN)` (SUM)

3. **Aplicar filtro de meses críticos**:
   - Arrastrar `Mes Nombre` a Filters
   - Seleccionar: "noviembre" y "diciembre"
   - O usar el campo calculado `Noviembre-Diciembre Filter` = True

4. **Crear dual-axis con unidades vendidas**:
   - Arrastrar `Unidades` a Rows (al lado derecho de la gráfica)
   - Click derecho en el eje de Unidades → "Dual Axis"
   - Click derecho en el eje derecho → "Synchronize Axis" (NO sincronizar, queremos escalas diferentes)

5. **Formatear la visualización**:
   - Marks de `Total (MXN)`: 
     - Tipo: Line
     - Color: Azul oscuro (#1f77b4)
     - Tamaño: Medium-Large
     - Agregar etiqueta condicional para valores > $1000
   - Marks de `Unidades`:
     - Tipo: Area
     - Color: Naranja claro con 50% transparencia (#ff7f0e)
     - Crear línea de referencia para promedio

6. **Añadir líneas de referencia**:
   - Analytics → Line → Reference Line
   - Agregar en `Total (MXN)`: Promedio (línea punteada roja)
   - Etiqueta: "Promedio: <VALUE>"

7. **Tooltip enriquecido**:
```
Fecha: <Fecha Venta (Date)>
Día: <Día de la Semana>

💰 VENTAS
Total: $<SUM(Total (MXN))> MXN
Unidades: <SUM(Unidades)>

📊 VS PROMEDIO
<AGG((SUM([Total (MXN)]) - WINDOW_AVG(SUM([Total (MXN)]))) / WINDOW_AVG(SUM([Total (MXN)])))*100>% del promedio
```

8. **Formato del eje**:
   - Eje Y (Total MXN): Formato currency con separador de miles
   - Eje X: Formato "dd-MMM" para mostrar día-mes

#### SHEET 1B: "Calendar Heat Map"

1. **Crear nueva hoja** → Nombrar: `1B_Ventas_Calendar_Heatmap`

2. **Estructura del calendar**:
   - **Columns**: `DATEPART('week', [Fecha Venta (Date)])`
   - **Rows**: `DATENAME('weekday', [Fecha Venta (Date)])`

3. **Ordenar días de la semana**:
   - Click derecho en `DATENAME('weekday')` en Rows
   - Sort → Manual
   - Orden: Lunes, Martes, Miércoles, Jueves, Viernes, Sábado, Domingo

4. **Aplicar color por ventas**:
   - Arrastrar `Total (MXN)` a Color en Marks
   - Marks: Square (cuadrado)
   - Tamaño: Grande
   - Color Palette: Green-Gold (Sequential)
     - Editar colores: Verde claro → Amarillo → Naranja → Rojo oscuro
     - Stepped Color: 4 pasos

5. **Añadir etiquetas**:
   - Arrastrar `DAY([Fecha Venta (Date)])` a Label
   - Arrastrar `Total (MXN)` a Label (formato: $K)
   - Formato: Negrita, centro

6. **Filtrar por Nov-Dic**:
   - Aplicar mismo filtro de meses críticos

7. **Tooltip**:
```
📅 <DATENAME('weekday', [Fecha Venta (Date)])>
Fecha: <Fecha Venta (Date)>

💰 Ventas del Día: $<SUM(Total (MXN))> MXN
📦 Unidades: <SUM(Unidades)>
🎯 Transacciones: <COUNTD(IDVenta)>
```

### 💡 Insights a Destacar
- Identificar picos de venta (días específicos)
- Patrones de día de la semana (¿viernes/domingos venden más?)
- Comparar primeras semanas vs últimas de diciembre
- Días previos a festividades importantes

---

## KPI 2: TOP 10 PRODUCTOS POR VENTAS

### 📊 Objetivo
Identificar los productos estrella para focalizar inventario y estrategias publicitarias.

### 🎨 Tipo de Visualización
**Horizontal Bar Chart con Sparklines de Tendencia**

### 📝 Paso a Paso

#### SHEET 2: "Top 10 Productos"

1. **Crear nueva hoja** → Nombrar: `2_Top10_Productos`

2. **Crear campo calculado - Rank de Productos**:
```
RANK_UNIQUE(SUM([Total (MXN)]), 'desc')
```

3. **Arrastrar campos**:
   - **Rows**: `IDproducto`
   - **Columns**: `Total (MXN)` (SUM)

4. **Aplicar filtro Top 10**:
   - Arrastrar el campo calculado `Rank de Productos` a Filters
   - Rango: 1 a 10

5. **Ordenar de mayor a menor**:
   - Click en el ícono de orden descendente en la barra de herramientas
   - O click derecho en eje → Sort → Descending by `Total (MXN)`

6. **Formatear barras**:
   - Marks: Bar (horizontal)
   - Color: Gradient basado en valor
     - Arrastrar `Total (MXN)` a Color
     - Palette: Blue-Teal (Sequential)
   - Tamaño: Automático

7. **Agregar etiquetas de valor**:
   - Arrastrar `Total (MXN)` a Label
   - Formato: Currency, 0 decimales, con "K" para miles
   - Posición: End (al final de la barra)
   - Agregar también `Unidades` vendidas en segunda línea

8. **Añadir información adicional**:
   - Crear campo calculado `% del Total`:
```
SUM([Total (MXN)]) / TOTAL(SUM([Total (MXN)])) 
```
   - Arrastrar a Label
   - Formato: Percentage con 1 decimal

9. **Enriquecer con datos del sheet Producto** (si aplica):
   - Si hay sheet de Producto con nombres descriptivos, hacer join
   - Usar nombre descriptivo en lugar de IDproducto en el eje

10. **Tooltip mejorado**:
```
🏆 Ranking: <Rank de Productos>
📦 Producto: <IDproducto>

💰 PERFORMANCE
Ventas Totales: $<SUM(Total (MXN))> MXN
Unidades Vendidas: <SUM(Unidades)>
Precio Promedio: $<AVG(Precio unitario de venta de la publicación (MXN))> MXN

📊 MÉTRICAS
% del Total: <% del Total>
Transacciones: <COUNTD(IDVenta)>

📈 Con Publicidad: <SUM(IF [Es Venta con Publicidad] THEN [Total (MXN)] END)> MXN
```

11. **Agregar línea de referencia (Pareto 80/20)**:
   - Analytics → Reference Line
   - Agregar banda sombreada para productos que sumen 80% de ventas

#### SHEET 2B: "Sparklines de Tendencia por Producto" (Opcional avanzado)

1. **Crear nueva hoja** → Nombrar: `2B_Tendencia_Productos`

2. **Configuración**:
   - **Columns**: `Fecha Venta (Date)` (continuo, día)
   - **Rows**: `IDproducto` (filtrado Top 10)
   - **Marks**: `Total (MXN)` como Line

3. **Formatear como sparkline**:
   - Ocultar ejes y headers
   - Líneas muy delgadas
   - Color: Gris claro
   - Sin etiquetas
   - Height: Muy pequeño (para incorporar en dashboard)

### 💡 Insights a Destacar
- Los top 3 productos y su contribución
- Productos con alto volumen pero bajo precio vs bajo volumen alto precio
- Productos que podrían beneficiarse de publicidad adicional

---

## KPI 3: VENTAS POR ESTADO

### 📊 Objetivo
Identificar regiones geográficas con mayor potencial para estrategias diferenciadas.

### 🎨 Tipo de Visualización
**Filled Map + Tree Map combinado**

### 📝 Paso a Paso

#### SHEET 3A: "Mapa de Ventas México"

1. **Crear nueva hoja** → Nombrar: `3A_Mapa_Ventas_Estados`

2. **Configurar geografía**:
   - Arrastrar `Estado` a la vista
   - Tableau debería reconocer automáticamente como geografía
   - Si no: Click derecho → Geographic Role → State/Province
   - Asegurar que esté configurado para México

3. **Añadir métrica de ventas**:
   - Arrastrar `Total (MXN)` a Color
   - Marks: Map (Filled Map)

4. **Configurar paleta de colores**:
   - Editar colores: Orange-Red (Sequential)
   - Stepped Color: 5 categorías
   - Start: Naranja claro (#fee6ce)
   - End: Rojo oscuro (#a50f15)
   - Incluir border negro para claridad

5. **Añadir etiquetas en estados principales**:
   - Arrastrar `Estado` a Label
   - Filtrar etiquetas: Solo mostrar si `Total (MXN)` > 10,000
   - Crear campo calculado:
```
IF SUM([Total (MXN)]) > 10000 THEN [Estado] END
```

6. **Tooltip enriquecido**:
```
📍 <Estado>

💰 VENTAS
Total: $<SUM(Total (MXN))> MXN
Unidades: <SUM(Unidades)>
Pedidos: <COUNTD(IDVenta)>

📊 DISTRIBUCIÓN
% del Total Nacional: <SUM([Total (MXN)]) / TOTAL(SUM([Total (MXN)]))>
Ticket Promedio: $<SUM([Total (MXN)]) / COUNTD([IDVenta])> MXN

🏙️ Municipios Activos: <COUNTD(Municipio/Alcaldía)>
```

7. **Agregar capa de tamaño (dual encoding)**:
   - Arrastrar `Unidades` a Size
   - Esto crea "bubble map" sobre el mapa de color

8. **Filtros aplicables**:
   - Agregar filtro por `Mes Nombre` (mostrar selector)
   - Agregar filtro por `Canal de venta`

#### SHEET 3B: "Tree Map de Estados"

1. **Crear nueva hoja** → Nombrar: `3B_Treemap_Estados`

2. **Configuración Tree Map**:
   - Arrastrar `Estado` a Marks (Detail)
   - Arrastrar `Total (MXN)` a Size
   - Arrastrar `Total (MXN)` a Color
   - Arrastrar `Estado` a Label

3. **Marks configuration**:
   - Tipo: Square (automático)
   - Color: Same palette as map (Orange-Red)
   - Borders: Blanco, grosor medio

4. **Labels**:
   - Formato: 
```
<Estado>
$<SUM(Total (MXN))>K
```
   - Tamaño fuente: Proporcional al cuadro
   - Alineación: Centro

5. **Crear grupos geográficos** (campo calculado):
```
// Región de México
IF [Estado] IN ('Baja California', 'Baja California Sur', 'Sonora', 'Chihuahua', 'Sinaloa') THEN 'Norte'
ELSEIF [Estado] IN ('Distrito Federal', 'Estado De México', 'Morelos', 'Puebla', 'Tlaxcala', 'Hidalgo') THEN 'Centro'
ELSEIF [Estado] IN ('Jalisco', 'Guanajuato', 'Michoacán', 'Querétaro', 'Aguascalientes') THEN 'Bajío'
ELSEIF [Estado] IN ('Veracruz', 'Oaxaca', 'Chiapas', 'Tabasco', 'Campeche', 'Yucatán', 'Quintana Roo') THEN 'Sur-Sureste'
ELSEIF [Estado] IN ('Nuevo León', 'Coahuila', 'Tamaulipas', 'San Luis Potosí', 'Zacatecas', 'Durango') THEN 'Noreste'
ELSE 'Otros'
END
```
   - Arrastrar `Región de México` a Color para agrupar visualmente

### 💡 Insights a Destacar
- Top 5 estados por volumen de ventas
- Estados subutilizados con potencial
- Concentración de ventas en zona centro (CDMX, EdoMex)
- Oportunidades en estados con baja penetración

---

## KPI 4: TIEMPO DE ENTREGA PROMEDIO

### 📊 Objetivo
Analizar tiempos de entrega para optimizar logística y mejorar satisfacción del cliente.

### 🎨 Tipo de Visualización
**Distribution Chart (Box Plot) + Line Chart de Tendencia**

### 📝 Paso a Paso

#### SHEET 4A: "Box Plot - Tiempos de Entrega"

1. **Crear nueva hoja** → Nombrar: `4A_Tiempos_Entrega_BoxPlot`

2. **Crear campos calculados adicionales**:

**Días Hábiles para Entrega**:
```
// Excluir fines de semana del cálculo (aproximado)
[Días para Entrega] * (5/7)
```

**Categoría de Entrega**:
```
IF [Días para Entrega] = 0 THEN 'Mismo Día'
ELSEIF [Días para Entrega] = 1 THEN '1 Día'
ELSEIF [Días para Entrega] <= 3 THEN '2-3 Días'
ELSEIF [Días para Entrega] <= 5 THEN '4-5 Días'
ELSE '6+ Días'
END
```

3. **Configurar Box Plot**:
   - **Columns**: `Transportista`
   - **Rows**: `Días para Entrega`
   - En Show Me, seleccionar "Box-and-Whisker Plot"

4. **Formatear el Box Plot**:
   - Marks (Circle): Color por `Transportista`
   - Usar palette "Tableau 10" para distinguir
   - Mostrar outliers como puntos individuales

5. **Añadir línea de promedio general**:
   - Analytics → Reference Line
   - Scope: Entire Table
   - Value: Average of `Días para Entrega`
   - Label: "Promedio General: <VALUE> días"
   - Color: Rojo, línea punteada

6. **Agregar etiquetas de mediana**:
   - Para cada transportista, mostrar la mediana
   - Formato: "<VALUE> días"

7. **Tooltip**:
```
🚚 <Transportista>

⏱️ TIEMPOS DE ENTREGA
Promedio: <AVG(Días para Entrega)> días
Mediana: <MEDIAN(Días para Entrega)> días
Mínimo: <MIN(Días para Entrega)> días
Máximo: <MAX(Días para Entrega)> días

📊 VOLUMEN
Entregas: <COUNTD(IDVenta)>
```

#### SHEET 4B: "Tiempos de Entrega por Día (Tendencia)"

1. **Crear nueva hoja** → Nombrar: `4B_Tendencia_TiemposEntrega`

2. **Configuración**:
   - **Columns**: `FechaEntrega` (continuo, día)
   - **Rows**: `AVG(Días para Entrega)`

3. **Crear gráfica combinada**:
   - Marks: Line para promedio
   - Agregar segunda métrica: `COUNTD(IDVenta)` como barra

4. **Formateo**:
   - Línea de promedio: Azul oscuro, grosor medio
   - Área sombreada (confidence band) si hay suficientes datos
   - Barras de volumen: Gris claro, 40% transparencia

5. **Añadir bandas de objetivo**:
   - Analytics → Reference Band
   - Entre 1-3 días: Zona verde (excelente)
   - Entre 4-5 días: Zona amarilla (aceptable)
   - Más de 5 días: Zona roja (mejorar)

#### SHEET 4C: "Heat Map - Entrega por Estado y Transportista"

1. **Crear nueva hoja** → Nombrar: `4C_HeatMap_Estado_Transportista`

2. **Configuración**:
   - **Columns**: `Transportista`
   - **Rows**: `Estado`
   - **Marks**: Square
   - **Color**: `AVG(Días para Entrega)`

3. **Filtrar estados principales**:
   - Solo mostrar estados con más de 20 entregas
   - Campo calculado:
```
IF SUM({FIXED [Estado]: COUNTD([IDVenta])}) > 20 THEN [Estado] END
```

4. **Paleta de colores**:
   - Red-Yellow-Green (Diverging)
   - Center: 3 días (objetivo)
   - Verde = Rápido (0-2 días)
   - Amarillo = Normal (2-4 días)
   - Rojo = Lento (4+ días)

5. **Etiquetas**:
   - Mostrar `AVG(Días para Entrega)` redondeado a 1 decimal
   - Formato: "X.X d"

6. **Tooltip**:
```
📍 <Estado>
🚚 <Transportista>

⏱️ Tiempo Promedio: <AVG(Días para Entrega)> días
📦 Entregas Realizadas: <COUNTD(IDVenta)>

💰 Costo Promedio Envío: $<AVG(Costos de envío)> MXN
```

### 💡 Insights a Destacar
- Transportistas más rápidos y confiables
- Regiones con problemas de entrega
- Correlación entre costo de envío y tiempo de entrega
- Días con mayor retraso (cerca de festividades?)

---

## KPI 5: ROI DE PUBLICIDAD

### 📊 Objetivo
Identificar qué productos y regiones generan mayor retorno en campañas publicitarias.

### 🎨 Tipo de Visualización
**Waterfall Chart + Scatter Plot de ROI**

### 📝 Paso a Paso

#### Campos Calculados Necesarios

1. **Ventas CON Publicidad**:
```
IF [Es Venta con Publicidad] THEN [Total (MXN)] ELSE 0 END
```

2. **Ventas SIN Publicidad**:
```
IF NOT [Es Venta con Publicidad] THEN [Total (MXN)] ELSE 0 END
```

3. **% Ventas con Publicidad**:
```
SUM([Ventas CON Publicidad]) / SUM([Total (MXN)])
```

4. **ROI Simplificado** (asumiendo costo de publicidad como % de ventas):
```
// Asumiendo que comisión marketplace es mayor con publicidad
// Si no hay datos específicos de costo, usar ventas como proxy
SUM([Ventas CON Publicidad]) / TOTAL(SUM([Ventas CON Publicidad]))
```

5. **Efectividad de Publicidad por Producto**:
```
// Ratio de ventas con pub vs sin pub por producto
SUM([Ventas CON Publicidad]) / (SUM([Ventas SIN Publicidad]) + 1)
// +1 para evitar división por cero
```

#### SHEET 5A: "Waterfall - Contribución de Publicidad"

1. **Crear nueva hoja** → Nombrar: `5A_Waterfall_Publicidad`

2. **Crear estructura waterfall**:
   - **Columns**: 
     - Primero: `Canal de venta`
     - Segundo: `Venta por publicidad`
   - **Rows**: `Total (MXN)` (SUM)

3. **Configurar como Waterfall**:
   - En Show Me, seleccionar "Gantt Bar"
   - Agregar table calculation para crear efecto waterfall
   - O usar Analysis → "Totals" → "Show Column Grand Totals"

4. **Alternativa más simple - Stacked Bar**:
   - **Columns**: `Canal de venta`
   - **Rows**: `Total (MXN)`
   - **Color**: `Venta por publicidad`
   - Marks: Bar (Stacked)

5. **Formateo**:
   - Color Sí (Con publicidad): Verde (#2ca02c)
   - Color No (Sin publicidad): Gris (#7f7f7f)
   - Añadir etiquetas de valor y porcentaje

6. **Agregar referencia de performance**:
   - Calcular diferencial: ¿Cuánto más se vende con publicidad?
   - Crear campo calculado:
```
// Uplift de publicidad
(SUM([Ventas CON Publicidad]) / COUNTD(IF [Es Venta con Publicidad] THEN [IDVenta] END))
/
(SUM([Ventas SIN Publicidad]) / COUNTD(IF NOT [Es Venta con Publicidad] THEN [IDVenta] END))
- 1
```
   - Mostrar como % en tooltip

#### SHEET 5B: "Scatter Plot - ROI por Producto"

1. **Crear nueva hoja** → Nombrar: `5B_Scatter_ROI_Productos`

2. **Configuración del scatter**:
   - **Columns**: `SUM(Ventas SIN Publicidad)`
   - **Rows**: `SUM(Ventas CON Publicidad)`
   - **Detail**: `IDproducto`
   - **Size**: `SUM(Total (MXN))` (tamaño total de ventas)
   - **Color**: `% Ventas con Publicidad` (gradient)

3. **Agregar línea de tendencia**:
   - Analytics → Trend Line
   - Linear model
   - Mostrar R-squared y P-value

4. **Añadir línea diagonal de referencia** (45°):
   - Analytics → Reference Line
   - Custom: Y = X (ventas iguales con y sin pub)
   - Productos arriba de la línea: Benefician MÁS de publicidad
   - Productos debajo: Venden bien sin publicidad

5. **Cuadrantes de estrategia**:
   - Agregar Reference Lines en media de cada eje
   - Crear 4 cuadrantes:
     - Alto/Alto: Estrellas (invertir más)
     - Alto/Bajo: Dependientes de publicidad
     - Bajo/Alto: Oportunidad orgánica
     - Bajo/Bajo: Revisar producto

6. **Tooltip**:
```
📦 Producto: <IDproducto>

💰 VENTAS
Con Publicidad: $<SUM(Ventas CON Publicidad)> MXN
Sin Publicidad: $<SUM(Ventas SIN Publicidad)> MXN
Total: $<SUM(Total (MXN))> MXN

📊 EFECTIVIDAD
% Con Publicidad: <% Ventas con Publicidad>
Uplift: <(Uplift de publicidad)>%

📈 RECOMENDACIÓN
[Incluir text dinámico basado en cuadrante]
```

#### SHEET 5C: "ROI por Estado (Geo-comparativo)"

1. **Crear nueva hoja** → Nombrar: `5C_ROI_Geografia`

2. **Configuración**:
   - **Rows**: `Estado`
   - **Columns**: 
     - `SUM(Ventas CON Publicidad)`
     - `SUM(Ventas SIN Publicidad)`
   - Crear Dual Bar Chart (barras horizontales lado a lado)

3. **Ordenar por efectividad**:
   - Sort descendente por `% Ventas con Publicidad`

4. **Filtrar Top 15 estados**:
   - Por volumen total de ventas

5. **Color coding**:
   - Verde para ventas con publicidad
   - Gris para ventas sin publicidad

6. **Agregar indicador de potencial**:
   - Estados con alto volumen SIN publicidad = oportunidad
   - Marcar con icono o símbolo

### 💡 Insights a Destacar
- Productos estrella que requieren publicidad vs los que venden orgánicamente
- Canales más efectivos para publicidad
- Regiones donde la publicidad tiene mayor impacto
- ROI real considerando costos de comisión

---

## KPI 6: EFECTIVIDAD POR CANAL

### 📊 Objetivo
Determinar cuál canal de venta (Mercado Libre vs Mercado Shops) es más efectivo durante alta demanda.

### 🎨 Tipo de Visualización
**Comparative Dashboard con múltiples métricas**

### 📝 Paso a Paso

#### Campos Calculados Necesarios

1. **Ticket Promedio por Canal**:
```
SUM([Total (MXN)]) / COUNTD([IDVenta])
```

2. **Unidades por Transacción**:
```
SUM([Unidades]) / COUNTD([IDVenta])
```

3. **Tasa de Conversión con Publicidad**:
```
COUNTD(IF [Es Venta con Publicidad] THEN [IDVenta] END) / COUNTD([IDVenta])
```

#### SHEET 6A: "Comparativo de Canales - KPIs"

1. **Crear nueva hoja** → Nombrar: `6A_Comparativo_Canales`

2. **Crear visualización de barras comparativas**:
   - **Columns**: `Canal de venta`
   - **Rows**: Múltiples métricas (usar Measure Values)

3. **Configurar Measure Values**:
   - Agregar al shelf de Rows: Measure Values
   - En Measure Values card, incluir solo:
     - `Total (MXN)` (SUM)
     - `Unidades` (SUM)
     - `IDVenta` (COUNTD) - renombrar a "Transacciones"
     - `Ticket Promedio por Canal` (AVG)
     - `Tasa de Conversión con Publicidad`

4. **Formatear como barras**:
   - Marks: Bar
   - Color por Canal: 
     - Mercado Libre: Amarillo (#ffec00)
     - Mercado Shops: Azul (#3483fa)

5. **Normalizar métricas** (para comparar en misma escala):
   - Crear campo calculado para cada métrica normalizada (0-100):
```
// Normalización (ejemplo para Total MXN)
(SUM([Total (MXN)]) - MIN({FIXED : SUM([Total (MXN)])})) 
/ 
(MAX({FIXED : SUM([Total (MXN)])}) - MIN({FIXED : SUM([Total (MXN)])}))
* 100
```

6. **Agregar etiquetas**:
   - Valor absoluto al final de cada barra
   - Formato apropiado (currency, number, percentage)

#### SHEET 6B: "Evolución Temporal por Canal"

1. **Crear nueva hoja** → Nombrar: `6B_Tendencia_Canales`

2. **Configuración**:
   - **Columns**: `Fecha Venta (Date)` (continuo, semana)
   - **Rows**: `Total (MXN)` (SUM)
   - **Color**: `Canal de venta`

3. **Tipo de gráfica**: Area Chart (stacked)
   - Marks: Area
   - Stacked areas para ver proporción y total simultáneamente

4. **Agregar línea de separación**:
   - Crear segunda mark layer
   - Type: Line
   - Color: Blanco, grosor fino
   - Para distinguir áreas

5. **Filtro interactivo de fecha**:
   - Agregar `Fecha Venta (Date)` a Filters
   - Show Filter → Slider o Date Range

#### SHEET 6C: "Product Mix por Canal"

1. **Crear nueva hoja** → Nombrar: `6C_ProductMix_Canal`

2. **Configuración Tree Map comparativo**:
   - **Columns**: `Canal de venta`
   - **Rows**: (vacío, solo marks)
   - **Marks Card**:
     - `IDproducto` en Detail
     - `Total (MXN)` en Size
     - `Unidades` en Color
     - `IDproducto` en Label

3. **Marks**: Automatic (debería ser Square)

4. **Color**: Sequential, Blue-Green

5. **Insights visuales**:
   - ¿Hay productos exclusivos de un canal?
   - ¿Distribución similar o diferente entre canales?

#### SHEET 6D: "Efectividad por Región y Canal"

1. **Crear nueva hoja** → Nombrar: `6D_Efectividad_Regional_Canal`

2. **Configuración**: Side-by-side maps
   - Crear dos worksheets de mapas (uno por canal)
   - O usar filtro de canal en un solo mapa

3. **Alternative - Heat Table**:
   - **Rows**: `Estado` (filtrado a top 20)
   - **Columns**: `Canal de venta`
   - **Color**: `Total (MXN)`
   - **Label**: `% del Total` por estado

4. **Formato**:
   - Cells: Square
   - Borders: Blanco
   - Color: Green-Gold

5. **Crear campo calculado - Canal Dominante**:
```
IF SUM(IF [Canal de venta] = "Mercado Libre" THEN [Total (MXN)] END) 
   > 
   SUM(IF [Canal de venta] = "Mercado Shops" THEN [Total (MXN)] END)
THEN "Mercado Libre"
ELSE "Mercado Shops"
END
```
   - Usar como indicador visual en tooltip

### 💡 Insights a Destacar
- Canal con mayor volumen total
- Canal con mejor ticket promedio
- Diferencias en product mix entre canales
- Regiones donde un canal domina sobre otro
- Relación entre canal y efectividad de publicidad

---

## KPI 7: ANÁLISIS ESTACIONAL

### 📊 Objetivo
Entender cómo varían los patrones de compra entre noviembre y diciembre, y encontrar patrones estacionales.

### 🎨 Tipo de Visualización
**Multi-faceted Temporal Analysis con comparaciones mes a mes**

### 📝 Paso a Paso

#### Campos Calculados Necesarios

1. **Semana del Mes**:
```
DATEDIFF('week', 
    DATETRUNC('month', [Fecha Venta (Date)]), 
    [Fecha Venta (Date)]
) + 1
```

2. **Periodo del Mes**:
```
IF DAY([Fecha Venta (Date)]) <= 10 THEN "Primera Decena"
ELSEIF DAY([Fecha Venta (Date)]) <= 20 THEN "Segunda Decena"
ELSE "Tercera Decena"
END
```

3. **Es Fin de Semana**:
```
IF DATENAME('weekday', [Fecha Venta (Date)]) IN ('sábado', 'domingo') 
THEN 'Fin de Semana' 
ELSE 'Entre Semana' 
END
```

4. **Índice Estacional** (vs promedio del periodo):
```
SUM([Total (MXN)]) / WINDOW_AVG(SUM([Total (MXN)])) * 100
```

#### SHEET 7A: "Comparativo Noviembre vs Diciembre"

1. **Crear nueva hoja** → Nombrar: `7A_Nov_vs_Dic_Comparativo`

2. **Configuración - Bullet Chart comparativo**:
   - **Rows**: Diferentes métricas (Measure Names)
   - **Columns**: `Mes Nombre`

3. **Métricas a comparar**:
   - Total Ventas (MXN)
   - Total Unidades
   - Promedio Ticket
   - # Transacciones
   - % Ventas con Publicidad
   - Productos Únicos Vendidos

4. **Formateo**:
   - Marks: Bar
   - Color por mes:
     - Noviembre: Naranja otoñal (#d95f02)
     - Diciembre: Rojo navideño (#cc0000)
   - Añadir etiquetas con valores
   - Agregar % de cambio entre meses

5. **Crear campo calculado - % Cambio Nov-Dic**:
```
(SUM(IF MONTH([Fecha Venta (Date)]) = 12 THEN [Total (MXN)] END)
 - 
 SUM(IF MONTH([Fecha Venta (Date)]) = 11 THEN [Total (MXN)] END))
/
SUM(IF MONTH([Fecha Venta (Date)]) = 11 THEN [Total (MXN)] END)
```

6. **Visualización alternativa - Butterfly Chart**:
   - Noviembre a la izquierda (valores negativos)
   - Diciembre a la derecha (valores positivos)
   - Eje en el centro

#### SHEET 7B: "Patrones por Día de la Semana"

1. **Crear nueva hoja** → Nombrar: `7B_Patrones_DiaSemana`

2. **Configuración**:
   - **Columns**: `Día de la Semana`
   - **Rows**: `Total (MXN)` (SUM)
   - **Color**: `Mes Nombre`

3. **Ordenar días correctamente**:
   - Lunes a Domingo
   - Click derecho en Día de la Semana → Sort → Manual

4. **Marks**: Line con puntos
   - Una línea para cada mes
   - Puntos en cada día para mostrar valores exactos

5. **Añadir estadística**:
   - Reference Line en promedio general
   - Highlight días específicos que sobresalen

6. **Agregar segunda capa - Unidades vendidas**:
   - Dual axis con barras de unidades en gris claro

#### SHEET 7C: "Evolución Semanal con Hitos"

1. **Crear nueva hoja** → Nombrar: `7C_Evolucion_Semanal`

2. **Configuración**:
   - **Columns**: `Fecha Venta (Date)` (continuo, día)
   - **Rows**: `Total (MXN)` (SUM)

3. **Marks**: Area con gradient
   - Color gradient por valor
   - Transparencia: 70%

4. **Overlay con eventos importantes**:
   - Crear tabla de fechas clave:
     - Buen Fin (México)
     - Black Friday
     - Cyber Monday
     - Inicio de Diciembre
     - Guadalupe (12 dic)
     - Navidad
     - Fin de año

5. **Anotar eventos en la gráfica**:
   - Right click en punto específico → Annotate → Point
   - O usar Marks con Shape especial
   - Crear campo calculado:
```
// Eventos Importantes
IF [Fecha Venta (Date)] = DATE('2021-11-26') THEN 'Buen Fin'
ELSEIF [Fecha Venta (Date)] = DATE('2021-11-19') THEN 'Black Friday'
ELSEIF [Fecha Venta (Date)] = DATE('2021-11-29') THEN 'Cyber Monday'
ELSEIF [Fecha Venta (Date)] = DATE('2021-12-12') THEN 'Día de Guadalupe'
ELSEIF [Fecha Venta (Date)] = DATE('2021-12-24') THEN 'Nochebuena'
ELSE NULL
END
```

6. **Añadir líneas verticales** en fechas de eventos:
   - Analytics → Reference Line
   - Per Pane, en cada fecha de evento

#### SHEET 7D: "Heat Map - Hora del Día vs Día de la Semana"

*Nota: Solo si tienes datos de hora de venta. Si no, skip esta visualización.*

1. Si no hay hora, crear alternativa:
   **"Comportamiento por Semana del Mes"**

2. **Configuración**:
   - **Columns**: `Semana del Mes`
   - **Rows**: `Mes Nombre`
   - **Color**: `Total (MXN)`
   - **Label**: Valores y % cambio

3. **Marks**: Square (heat map)

4. **Agregar dimensión de publicidad**:
   - Color: Ventas totales
   - Size: % con publicidad
   - Dual encoding para más insights

#### SHEET 7E: "Análisis de Productos Estacionales"

1. **Crear nueva hoja** → Nombrar: `7E_Productos_Estacionales`

2. **Objetivo**: Identificar productos que venden más en nov vs dic

3. **Configuración - Scatter Plot**:
   - **Columns**: Ventas Noviembre (por producto)
   - **Rows**: Ventas Diciembre (por producto)
   - **Detail**: `IDproducto`
   - **Size**: Ventas totales
   - **Color**: Ratio Dic/Nov

4. **Crear campos calculados**:

**Ventas Noviembre**:
```
SUM(IF MONTH([Fecha Venta (Date)]) = 11 THEN [Total (MXN)] ELSE 0 END)
```

**Ventas Diciembre**:
```
SUM(IF MONTH([Fecha Venta (Date)]) = 12 THEN [Total (MXN)] ELSE 0 END)
```

**Ratio Diciembre/Noviembre**:
```
[Ventas Diciembre] / [Ventas Noviembre]
```

5. **Interpretar cuadrantes**:
   - Productos en diagonal: Ventas estables ambos meses
   - Arriba de diagonal: Más fuertes en Diciembre
   - Debajo de diagonal: Más fuertes en Noviembre

6. **Color coding**:
   - Verde: Ratio > 1.5 (muy fuerte en Dic)
   - Amarillo: Ratio 0.7-1.5 (equilibrado)
   - Rojo: Ratio < 0.7 (más fuerte en Nov)

### 💡 Insights a Destacar
- Crecimiento de ventas entre noviembre y diciembre
- Días específicos con picos (eventos comerciales)
- Productos que son específicamente navideños vs generales
- Diferencias en comportamiento de compra entre semana vs fin de semana
- Momento óptimo para lanzar campañas

---

## KPI 8: OPTIMIZACIÓN LOGÍSTICA

### 📊 Objetivo
Identificar qué transportistas ofrecen mejor rendimiento por región para optimizar la cadena de suministro.

### 🎨 Tipo de Visualización
**Performance Matrix + Geographic Analysis**

### 📝 Paso a Paso

#### Campos Calculados Necesarios

1. **Costo por Día de Envío**:
```
[Costos de envío] / [Días para Entrega]
```

2. **Eficiencia del Transportista** (score compuesto):
```
// Score basado en velocidad y costo (normalizado 0-100)
// Menor tiempo = mejor, menor costo = mejor
(
  (1 - ([Días para Entrega] - {FIXED : MIN([Días para Entrega])}) / 
       ({FIXED : MAX([Días para Entrega])} - {FIXED : MIN([Días para Entrega])}))
  * 0.6  // 60% peso a velocidad
) 
+ 
(
  (1 - ([Costos de envío] - {FIXED : MIN([Costos de envío])}) / 
       ({FIXED : MAX([Costos de envío])} - {FIXED : MIN([Costos de envío])}))
  * 0.4  // 40% peso a costo
)
* 100
```

3. **Categoría de Desempeño**:
```
IF [Eficiencia del Transportista] >= 75 THEN 'Excelente'
ELSEIF [Eficiencia del Transportista] >= 60 THEN 'Bueno'
ELSEIF [Eficiencia del Transportista] >= 40 THEN 'Aceptable'
ELSE 'Necesita Mejora'
END
```

4. **Volumen por Transportista**:
```
COUNTD([IDVenta])
```

#### SHEET 8A: "Scatter - Costo vs Velocidad por Transportista"

1. **Crear nueva hoja** → Nombrar: `8A_Costo_vs_Velocidad`

2. **Configuración**:
   - **Columns**: `AVG(Días para Entrega)`
   - **Rows**: `AVG(Costos de envío)`
   - **Detail**: `Transportista`
   - **Color**: `Transportista`
   - **Size**: `COUNTD(IDVenta)` (volumen)
   - **Label**: `Transportista`

3. **Objetivo de la visualización**:
   - Cuadrante inferior izquierdo = Óptimo (rápido y barato)
   - Cuadrante superior derecho = Peor (lento y caro)

4. **Añadir cuadrantes con reference lines**:
   - Línea vertical en mediana de días
   - Línea horizontal en mediana de costos
   - Etiquetar cuadrantes:
     - Bajo costo/Rápido: ⭐ "Óptimo"
     - Alto costo/Rápido: 💰 "Premium"
     - Bajo costo/Lento: ⏰ "Económico"
     - Alto costo/Lento: ⚠️ "Revisar"

5. **Formateo**:
   - Color palette: "Color Blind" o "Tableau 10"
   - Size range: Proporcional pero no demasiado grande
   - Borders blancos en burbujas

6. **Tooltip enriquecido**:
```
🚚 <Transportista>

⏱️ TIEMPO
Promedio: <AVG(Días para Entrega)> días
Mediana: <MEDIAN(Días para Entrega)> días

💰 COSTO
Promedio: $<AVG(Costos de envío)> MXN
Total: $<SUM(Costos de envío)> MXN

📊 VOLUMEN
Entregas: <COUNTD(IDVenta)>
% del Total: <COUNTD(IDVenta) / TOTAL(COUNTD(IDVenta))>

⚡ EFICIENCIA
Score: <Eficiencia del Transportista>
Categoría: <Categoría de Desempeño>
```

#### SHEET 8B: "Heat Map - Transportista por Estado"

1. **Crear nueva hoja** → Nombrar: `8B_HeatMap_Transportista_Estado`

2. **Configuración**:
   - **Columns**: `Transportista`
   - **Rows**: `Estado` (filtrado a top 15 por volumen)
   - **Color**: `Eficiencia del Transportista` (score)
   - **Size**: `COUNTD(IDVenta)` (volumen de entregas)

3. **Marks**: Circle

4. **Paleta de colores**:
   - Red-Yellow-Green (Diverging)
   - Center en 50 (desempeño medio)
   - Verde intenso: 75-100 (excelente)
   - Rojo intenso: 0-25 (malo)

5. **Filtrar celdas con datos mínimos**:
   - Solo mostrar combinaciones con al menos 5 entregas
   - Campo calculado:
```
IF {FIXED [Estado], [Transportista]: COUNTD([IDVenta])} >= 5 
THEN [Transportista] 
END
```

6. **Añadir indicadores**:
   - Label: Score de eficiencia (formato: "XX")
   - Shape opcional: ⭐ para scores > 80

#### SHEET 8C: "Ranking de Transportistas por Región"

1. **Crear nueva hoja** → Nombrar: `8C_Ranking_Transportistas`

2. **Crear campo calculado - Mejor Transportista por Estado**:
```
// Usa RANK para identificar el mejor por estado
IF RANK(AVG([Eficiencia del Transportista]), 'desc') = 1 
THEN [Transportista] 
END
```

3. **Configuración - Mapa de México**:
   - Arrastrar `Estado` (automático genera mapa)
   - **Color**: `Mejor Transportista por Estado`
   - **Tooltip**: Incluir top 3 transportistas del estado

4. **Paleta de colores**:
   - Categorical
   - Un color distintivo por transportista

5. **Alternativa - Tabla de Rankings**:
   - **Rows**: `Estado`
   - **Columns**: `Transportista`
   - **Text**: Rank (1, 2, 3, 4, 5)
   - **Color**: Gradient (1=verde oscuro, 5=rojo claro)

#### SHEET 8D: "Timeline - Evolución del Desempeño"

1. **Crear nueva hoja** → Nombrar: `8D_Tendencia_Desempeño`

2. **Configuración**:
   - **Columns**: `FechaEntrega` (continuo, semana)
   - **Rows**: `AVG(Días para Entrega)`
   - **Color**: `Transportista`
   - **Filters**: Mes = Nov, Dic

3. **Marks**: Line

4. **Añadir banda de SLA (Service Level Agreement)**:
   - Reference band: 1-3 días (zona verde - objetivo)
   - Reference line: 3 días (línea de objetivo)

5. **Identificar tendencias**:
   - ¿Algún transportista empeora en diciembre (alta demanda)?
   - ¿Consistencia en el tiempo?

#### SHEET 8E: "Costos Totales por Transportista"

1. **Crear nueva hoja** → Nombrar: `8E_Costos_Transportista`

2. **Configuración - Treemap de costos**:
   - **Marks**: `Transportista` (Detail)
   - **Size**: `SUM(Costos de envío)`
   - **Color**: `AVG(Costo por Día de Envío)`
   - **Label**: Transportista + Total de costos

3. **Interpretar**:
   - Tamaño = Costo total (volumen × tarifa)
   - Color = Eficiencia del costo
   - Identificar oportunidades de negociación

4. **Añadir contexto de volumen**:
   - Secondary label: "X entregas"
   - Tooltip: Costo promedio por entrega

#### SHEET 8F: "Recomendación de Transportista"

1. **Crear nueva hoja** → Nombrar: `8F_Recomendacion_Transportista`

2. **Objetivo**: Dashboard de decisión rápida

3. **Crear tabla con recomendaciones**:
   - **Rows**: `Estado` (top 20)
   - **Columns**:
     - Mejor para Velocidad
     - Mejor para Costo
     - Mejor Balanceado (eficiencia)
     - Más Utilizado

4. **Crear campos calculados para cada columna**:

**Mejor para Velocidad**:
```
{FIXED [Estado]: MIN(IF RANK(AVG([Días para Entrega])) = 1 THEN [Transportista] END)}
```

**Mejor para Costo**:
```
{FIXED [Estado]: MIN(IF RANK(AVG([Costos de envío])) = 1 THEN [Transportista] END)}
```

**Mejor Balanceado**:
```
{FIXED [Estado]: MIN(IF RANK(AVG([Eficiencia del Transportista]), 'desc') = 1 THEN [Transportista] END)}
```

5. **Formateo**:
   - Text table
   - Color por transportista (mismo que en otras vistas)
   - Borders claros

6. **Tooltip con detalles**:
   - Stats específicos del transportista en ese estado
   - Número de entregas realizadas

### 💡 Insights a Destacar
- Transportista más eficiente overall
- Variación regional en desempeño
- Relación costo-velocidad (¿premium vale la pena?)
- Transportistas con inconsistencia en servicio
- Oportunidades de consolidación o cambio de proveedor
- Capacidad durante temporadas altas (¿se mantiene el desempeño en dic?)

---

## ENSAMBLAJE DEL DASHBOARD FINAL

### 🎨 Estrategia de Layout

#### Dashboard Principal: "Overview Ejecutivo"

**Dimensiones recomendadas**: 1920 x 1080 (HD) o 1366 x 768 (compatibilidad)

### Layout Structure

```
┌─────────────────────────────────────────────────────────┐
│  🏢 NUMISMÁTICA MÉXICO - ESTRATEGIA FIN DE AÑO 2021    │
│  [Filtros Globales: Mes | Canal | Publicidad]           │
├──────────────────────┬──────────────────────────────────┤
│  📊 KPIs PRINCIPALES │  📈 TENDENCIA VENTAS DIARIAS     │
│  - Total Ventas      │  (KPI 1: Timeline + Calendar)    │
│  - Unidades          │                                   │
│  - Ticket Promedio   │                                   │
│  - # Transacciones   │                                   │
├──────────────────────┴──────────────────────────────────┤
│  🏆 TOP 10 PRODUCTOS  │  🗺️ VENTAS POR ESTADO          │
│  (KPI 2: Bars)        │  (KPI 3: Map + Treemap)         │
│                       │                                   │
├───────────────────────┴──────────────────────────────────┤
│  💰 ROI PUBLICIDAD    │  📦 EFECTIVIDAD CANALES         │
│  (KPI 5: Waterfall)   │  (KPI 6: Comparative)           │
├───────────────────────┴──────────────────────────────────┤
│  Navigation: [Logística] [Estacionalidad]                │
└──────────────────────────────────────────────────────────┘
```

### Paso a Paso - Crear Dashboard

1. **Crear Nuevo Dashboard**:
   - Dashboard → New Dashboard
   - Nombre: "Dashboard_Principal_Numismatica"

2. **Configurar dimensiones**:
   - Size: Automatic (responsive)
   - O Fixed: 1920 x 1080

3. **Agregar contenedor de título**:
   - Objects → Horizontal Container
   - Altura: 100px
   - Background color: Azul oscuro (#003d5c)

4. **Crear título con formato**:
   - Objects → Text
   - Título: "NUMISMÁTICA MÉXICO - Dashboard Estratégico Fin de Año"
   - Subtítulo: "Noviembre - Diciembre 2021"
   - Font: Tableau Bold, 24pt, Blanco
   - Alignment: Centro

5. **Contenedor de filtros globales**:
   - Objects → Horizontal Container
   - Altura: 60px
   - Background: Gris claro (#f5f5f5)

6. **Agregar filtros**:
   - Desde cualquier sheet, agregar a dashboard:
     - `Mes Nombre` (Multiple Values Dropdown)
     - `Canal de venta` (Single Value List)
     - `Venta por publicidad` (Single Value List)
   - Aplicar a todas las worksheets relevantes
   - Formato: Horizontal, compacto

7. **Sección de KPIs (BANs - Big Ass Numbers)**:
   - Crear 4 nuevas sheets individuales para cada KPI:

**Sheet: KPI_Total_Ventas**
```
- Marca: Text
- Contenido: SUM([Total (MXN)])
- Formato: $XXX,XXX MXN
- Tamaño fuente: 36pt
- Color: Verde (#2ca02c)
- Título: "Ventas Totales"
```

**Sheet: KPI_Unidades**
```
- SUM([Unidades])
- Formato: X,XXX unidades
- Color: Azul (#1f77b4)
- Título: "Unidades Vendidas"
```

**Sheet: KPI_Ticket_Promedio**
```
- AVG([Total (MXN)])
- Formato: $XXX MXN
- Color: Naranja (#ff7f0e)
- Título: "Ticket Promedio"
```

**Sheet: KPI_Transacciones**
```
- COUNTD([IDVenta])
- Formato: X,XXX
- Color: Morado (#9467bd)
- Título: "# Transacciones"
```

8. **Añadir indicadores de cambio** (opcional):
   - Crear campo calculado con % cambio vs periodo anterior
   - Añadir ícono ▲ o ▼ según tendencia

9. **Agregar sheets principales al dashboard**:
   - Arrastrar cada sheet creada previamente
   - Ajustar tamaño proporcionalmente
   - Quitar títulos redundantes (ya está en header)

10. **Configurar interacciones (Actions)**:

**Action 1: Filter por Fecha**
```
- Dashboard → Actions → Add Action → Filter
- Name: "Filtrar por Día"
- Source: 1A_Ventas_Diarias_Timeline
- Target: Todos los sheets
- Clearing selection: Show all values
```

**Action 2: Highlight por Producto**
```
- Dashboard → Actions → Add Action → Highlight
- Name: "Highlight Producto"
- Source: 2_Top10_Productos
- Target: Todos los sheets con IDproducto
```

**Action 3: Filter por Estado**
```
- Dashboard → Actions → Add Action → Filter
- Name: "Filtrar por Estado"
- Source: 3A_Mapa_Ventas_Estados
- Target: Sheets relevantes (logística, productos en estado)
```

11. **Formateo estético**:

**Colors**:
- Background del dashboard: Blanco o gris muy claro (#fafafa)
- Borders: Gris medio (#cccccc), 1px
- Containers: Padding de 8-10px

**Fonts**:
- Títulos: Tableau Bold, 14-16pt
- Contenido: Tableau Regular, 10-12pt
- KPIs: Tableau Bold, 32-36pt

**Spacing**:
- Usar containers con padding uniforme
- Separadores horizontales entre secciones (línea gris)

12. **Añadir objetos decorativos**:
   - Image: Logo de Numismática México (si existe)
   - Text: Fecha de actualización
   - Text: "Dashboard creado para MNA - Visualización de Datos"

13. **Botones de navegación** (hacia otros dashboards):
   - Objects → Image (usar imágenes como botones)
   - O Text con formato de link
   - Dashboard → Actions → Go to URL o Go to Sheet

---

#### Dashboard Secundario 1: "Análisis Estacional"

1. **Crear nuevo dashboard**: "Dashboard_Estacional"

2. **Layout**:
```
┌────────────────────────────────────────────┐
│  📅 ANÁLISIS ESTACIONAL NOV-DIC           │
├────────────────────────────────────────────┤
│  Comparativo Nov vs Dic (Sheet 7A)        │
├────────────────────────────────────────────┤
│  Patrones por Día Semana (7B)  │  Heat    │
│                                  │  Map     │
│                                  │  (7D)    │
├──────────────────────────────────┴──────────┤
│  Evolución con Eventos Importantes (7C)    │
├─────────────────────────────────────────────┤
│  Productos Estacionales (7E)                │
└─────────────────────────────────────────────┘
```

3. **Agregar navegación de regreso**: Botón → Dashboard Principal

---

#### Dashboard Secundario 2: "Optimización Logística"

1. **Crear nuevo dashboard**: "Dashboard_Logistica"

2. **Layout**:
```
┌────────────────────────────────────────────┐
│  🚚 OPTIMIZACIÓN LOGÍSTICA                │
├────────────────────────────────────────────┤
│  Costo vs Velocidad (8A)  │  Ranking      │
│                            │  Regional     │
│                            │  (8C)         │
├────────────────────────────┴───────────────┤
│  Heat Map Transportista-Estado (8B)       │
├────────────────────────────────────────────┤
│  Evolución Desempeño (8D) │ Costos (8E)   │
├────────────────────────────┴───────────────┤
│  Tabla de Recomendaciones (8F)            │
└────────────────────────────────────────────┘
```

---

## FILTROS GLOBALES Y ACCIONES

### Filtros Recomendados

1. **Filtro de Fecha (Global)**:
   - Tipo: Date Range
   - Default: Nov-Dec 2021
   - Aplicar a: Todos los sheets

2. **Filtro de Canal (Global)**:
   - Tipo: Multiple Values List
   - Default: Ambos seleccionados
   - Show "All" option

3. **Filtro de Publicidad (Global)**:
   - Tipo: Single Value List
   - Options: Todos | Con Publicidad | Sin Publicidad
   - Default: Todos

4. **Filtro de Estado (Contextual)**:
   - Solo en dashboard principal
   - Multi-select dropdown
   - Aplicar a sheets geográficos y logísticos

5. **Filtro de Producto (Contextual)**:
   - Top N filter (N=10)
   - User can adjust N
   - Show/Hide en dashboard según necesidad

### Dashboard Actions - Completas

#### Action 1: Cross-Filter Temporal
```yaml
Name: Filtrar por Fecha Seleccionada
Type: Filter
Source Sheet: 1A_Ventas_Diarias_Timeline
Run on: Select
Target Sheets: All using related data source
Clearing selection: Show all values
```

#### Action 2: Drill-down Geográfico
```yaml
Name: Explorar Estado
Type: Filter
Source Sheet: 3A_Mapa_Ventas_Estados
Run on: Select
Target Sheets: 
  - 8B_HeatMap_Transportista_Estado
  - 8F_Recomendacion_Transportista
Target Filters:
  - Estado
Clearing selection: Show all values
```

#### Action 3: Highlight de Producto
```yaml
Name: Destacar Producto
Type: Highlight
Source Sheet: 2_Top10_Productos
Run on: Hover
Target Sheets: All using IDproducto
Target Highlighting: Selected Fields
  - IDproducto
```

#### Action 4: Highlight de Transportista
```yaml
Name: Destacar Transportista
Type: Highlight
Source Sheet: 8A_Costo_vs_Velocidad
Run on: Hover
Target Sheets: All using Transportista
Target Highlighting: Selected Fields
  - Transportista
```

#### Action 5: Navegación a Dashboard Logística
```yaml
Name: Ver Detalle Logístico
Type: Go to Sheet
Source Sheet: 4A_Tiempos_Entrega_BoxPlot
Run on: Menu (right-click)
Target Sheet: Dashboard_Logistica
```

#### Action 6: Navegación a Dashboard Estacional
```yaml
Name: Ver Análisis Estacional
Type: Go to Sheet
Source Sheet: 7A_Nov_vs_Dic_Comparativo
Run on: Menu
Target Sheet: Dashboard_Estacional
```

#### Action 7: Filtro de Canal
```yaml
Name: Analizar por Canal
Type: Filter
Source Sheet: 6A_Comparativo_Canales
Run on: Select
Target Sheets: All except KPI summary sheets
Target Filters:
  - Canal de venta
```

#### Action 8: URL Action (Opcional)
```yaml
Name: Ver Producto en Mercado Libre
Type: Go to URL
Source Sheet: 2_Top10_Productos
Run on: Menu
URL: https://articulo.mercadolibre.com.mx/<IDproducto>
```

---

## 🎨 PALETA DE COLORES RECOMENDADA

### Color Scheme Principal - "Numismática Professional"

**Colores Primarios**:
- Azul Corporativo: `#003d5c` (títulos, headers)
- Verde Financiero: `#2ca02c` (ventas, positivo)
- Oro Numismático: `#d4af37` (highlights, premium)

**Colores Secundarios**:
- Naranja: `#ff7f0e` (alertas, publicidad)
- Azul Claro: `#1f77b4` (datos, información)
- Gris Oscuro: `#333333` (texto)

**Colores de Estado**:
- Rojo: `#d62728` (negativo, alertas)
- Verde: `#2ca02c` (positivo, objetivos cumplidos)
- Amarillo: `#ffd700` (advertencia, en progreso)
- Gris: `#7f7f7f` (neutral, sin publicidad)

**Gradientes**:
1. Heat Maps: Amarillo → Naranja → Rojo
   - `#fff5eb → #fd8d3c → #a50f15`

2. Performance: Verde → Amarillo → Rojo (divergente)
   - `#2ca02c → #ffec00 → #d62728`

3. Geographic: Azul secuencial
   - `#deebf7 → #3182bd → #08519c`

### Aplicar Colores Consistentemente

1. **Crear Custom Color Palette**:
   - Preferences → Preferences File
   - Añadir paleta personalizada en XML
   - Nombre: "Numismatica_Colors"

2. **Uso por Dimensión**:
   - Canal de venta:
     - Mercado Libre: `#ffec00` (Amarillo oficial)
     - Mercado Shops: `#3483fa` (Azul oficial)
   
   - Publicidad:
     - Sí: Verde `#2ca02c`
     - No: Gris `#7f7f7f`
   
   - Transportistas:
     - Usar Tableau 10 color palette (categorical)
     - Asignar consistentemente en todos los sheets

3. **Guardar como Theme**:
   - Format → Workbook Theme
   - Save Custom Theme
   - Aplicar a todos los sheets

---

## 📱 CONSIDERACIONES DE DISEÑO

### Responsiveness

1. **Dashboard Sizing**:
   - Recomendado: Automatic
   - Alternativa: Range (1280x720 a 1920x1080)
   - Testar en diferentes resoluciones

2. **Layout Containers**:
   - Usar containers flexibles
   - Definir min-width y min-height
   - Permitir scroll vertical si necesario

### Accesibilidad

1. **Contraste de Colores**:
   - Ratio mínimo: 4.5:1 (texto)
   - Usar herramienta: WebAIM Contrast Checker

2. **Color Blindness**:
   - No usar solo color para diferenciar
   - Añadir shapes, patterns, labels
   - Testar con "Color Blind Palette"

3. **Alt Text**:
   - Worksheet → Accessibility
   - Describir cada visualización para screen readers

### Performance

1. **Data Optimization**:
   - Filtrar a Nov-Dic desde la fuente
   - Usar extractos (.hyper) en lugar de conexión directa
   - Limitar datos a necesario (no cargar todo el año)

2. **Calculation Optimization**:
   - Preferir Table Calculations sobre Calculated Fields cuando posible
   - Evitar LOD calculations anidados
   - Usar agregaciones eficientes

3. **Dashboard Load**:
   - Limitar número de sheets a 8-10 por dashboard
   - Usar "Show Sheets as Tabs" para secundarios
   - Considerar lazy loading (show on demand)

---

## ✅ CHECKLIST FINAL PRE-ENTREGA

### Datos y Cálculos
- [ ] Todos los campos calculados funcionan sin errores
- [ ] Fechas están correctamente parseadas
- [ ] Filtros de Nov-Dic aplicados donde corresponde
- [ ] No hay valores NULL inesperados en visualizaciones
- [ ] Agregaciones son correctas (SUM, AVG, etc.)

### Visualizaciones
- [ ] Cada KPI tiene al menos una visualización
- [ ] Títulos descriptivos y claros
- [ ] Ejes etiquetados correctamente
- [ ] Leyendas visibles y comprensibles
- [ ] Tooltips informativos y formateados
- [ ] Colores consistentes entre sheets

### Dashboard
- [ ] Layout es profesional y organizado
- [ ] Navegación es intuitiva
- [ ] Filtros funcionan correctamente
- [ ] Actions (filter, highlight) están configuradas
- [ ] Responsive en diferentes tamaños
- [ ] Headers/títulos son claros
- [ ] Branding (si aplica) está presente

### Formato y Estética
- [ ] Paleta de colores es consistente
- [ ] Fonts y tamaños son apropiados
- [ ] Spacing y padding son uniformes
- [ ] Borders y líneas son sutiles
- [ ] No hay elementos superpuestos
- [ ] Background no distrae

### Performance
- [ ] Dashboard carga en menos de 10 segundos
- [ ] Interacciones son fluidas (no lag)
- [ ] Filtros responden rápidamente

### Documentación
- [ ] Nombres de sheets son descriptivos
- [ ] Campos calculados tienen comentarios
- [ ] Dashboard tiene metadata (autor, fecha)
- [ ] Existe una hoja de "Notas" o "About" con metodología

### Insights y Análisis
- [ ] Cada visualización responde a su pregunta de negocio
- [ ] Insights principales son evidentes
- [ ] Se identifican patrones y tendencias
- [ ] Hay comparativas y benchmarks
- [ ] Recomendaciones son accionables

---

## 💡 TIPS PRO PARA DESTACAR

### 1. Storytelling Visual
- Usa anotaciones para destacar puntos clave
- Crea una narrativa: Problema → Análisis → Insight → Acción
- Ordena visualizaciones en secuencia lógica

### 2. Interactividad Avanzada
- Parámetros para que usuario ajuste pesos de eficiencia
- Botón de "Reset Filters"
- Toggle entre vistas (Map vs Treemap)

### 3. Insights Automáticos
- Usa Analytics pane:
  - Trend lines
  - Forecasts (proyectar para 2022)
  - Clustering
  - Reference lines en percentiles

### 4. Context Awareness
- Mostrar "Last Updated" dinámicamente
- Destacar filtros activos
- Indicar cuando hay selección activa

### 5. Mobile Considerations
- Crear layout específico para tablet/mobile
- Dashboard → Device Layouts → Add Phone Layout
- Simplificar y apilar verticalmente

---

## 📚 RECURSOS ADICIONALES

### Tableau Learning
- Tableau Public Gallery: Ideas de visualización
- Makeover Monday: Inspiración semanal
- Tableau Community Forums: Soporte técnico

### Data Viz Best Practices
- "Storytelling with Data" - Cole Nussbaumer Knaflic
- "The Big Book of Dashboards" - Steve Wexler
- Tableau Blueprint: Metodología oficial

### Testing Tools
- WAVE (accesibilidad)
- Color Oracle (simulador de daltonismo)
- GTMetrix (performance)

---

## 🎯 RUBRICA DE AUTOEVALUACIÓN

Usa esta rúbrica basada en los criterios de evaluación del documento:

### Elaboración de Visualizaciones (25 pts)
- **Destacado (25)**: Cada KPI tiene visualización que responde COMPLETAMENTE a la pregunta
- **Aceptable (13)**: Visualizaciones responden parcialmente
- **Básico (7)**: Visualizaciones no responden a preguntas
- **Autoevaluación**: ___/25

### Diseño de Visualizaciones (25 pts)
- **Destacado (25)**: Selección ADECUADA de colores, formas, marcas en TODAS
- **Aceptable (13)**: Algunos elementos no son adecuados
- **Básico (7)**: Selección inadecuada
- **Autoevaluación**: ___/25

### Identificación de Insights (25 pts)
- **Destacado (25)**: Interpreta visualizaciones con insights claros; filtros facilitan hallazgos
- **Aceptable (13)**: Interpretación con imprecisiones
- **Básico (7)**: Muchas imprecisiones, no identifica insights
- **Autoevaluación**: ___/25

### Propuesta y Respuesta (25 pts)
- **Destacado (25)**: Visualizaciones resumen datos, responden objetivo, generan propuesta con bibliografía APA
- **Aceptable (13)**: Resumen con menor precisión
- **Básico (7)**: Poca precisión
- **Autoevaluación**: ___/25

**TOTAL**: ___/100

---

## 🚀 PRÓXIMOS PASOS (DESPUÉS DE CREAR EL DASHBOARD)

1. **Generar Insights Document**:
   - Crear PDF con screenshots de visualizaciones
   - Documentar insights principales por KPI
   - Añadir recomendaciones estratégicas
   - Incluir bibliografía en formato APA

2. **Preparar Presentación**:
   - Crear Story en Tableau para presentación
   - Secuencia lógica: Contexto → Análisis → Conclusiones
   - 10-15 minutos de duración

3. **Documentar Metodología**:
   - Campos calculados y su propósito
   - Decisiones de diseño y justificación
   - Limitaciones de los datos
   - Supuestos realizados

4. **Exportar Entregables**:
   - .twbx (Tableau Workbook Packaged)
   - PDF del dashboard principal
   - PDF con insights y recomendaciones

---

## ✨ ¡ÉXITO EN TU PROYECTO!

Esta guía te proporciona una ruta completa para crear un dashboard profesional, innovador y altamente funcional. Recuerda:

- **Creatividad**: No te limites a barras y líneas básicas
- **Usuario final**: Piensa en el Director de Marketing usando tu dashboard
- **Historia**: Los datos deben contar una historia convincente
- **Acción**: Cada insight debe llevar a una decisión de negocio

**¡Mucha suerte con tu proyecto en la Maestría en Inteligencia Artificial Aplicada!** 🎓

---

*Guía creada para el proyecto "Numismática México" - Visualización de Datos*  
*Tecnológico de Monterrey - MNA*  
*Octubre 2025*

