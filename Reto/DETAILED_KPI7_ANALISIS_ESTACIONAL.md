# Guía Detallada: KPI 7 - Análisis Estacional
## Paso a Paso Completo: Noviembre vs Diciembre

---

## 🎯 OBJETIVO DEL KPI

Entender cómo varían los patrones de compra entre noviembre y diciembre para:
- Identificar diferencias en comportamiento de compra entre meses
- Detectar días específicos con picos de venta (eventos comerciales)
- Encontrar productos estacionales vs generales
- Optimizar el momento de lanzamiento de campañas

---

## 📋 QUÉ VAMOS A CREAR

Crearemos **5 visualizaciones**:

1. **Sheet 7A**: Comparativo Nov vs Dic - Métricas lado a lado
2. **Sheet 7B**: Patrones por Día de la Semana - ¿Qué días venden más?
3. **Sheet 7C**: Evolución Semanal con Hitos - Timeline con eventos importantes
4. **Sheet 7D**: Heat Map por Semana del Mes (Opcional)
5. **Sheet 7E**: Análisis de Productos Estacionales - Scatter plot

---

## 🚀 PREPARACIÓN: Campos Calculados Necesarios

Antes de empezar, crea estos campos calculados:

### **1. Semana del Mes**
```
DATEDIFF('week', 
    DATETRUNC('month', [Fecha Venta (Date)]), 
    [Fecha Venta (Date)]
) + 1
```

### **2. Periodo del Mes**
```
IF DAY([Fecha Venta (Date)]) <= 10 THEN "Primera Decena"
ELSEIF DAY([Fecha Venta (Date)]) <= 20 THEN "Segunda Decena"
ELSE "Tercera Decena"
END
```

### **3. Es Fin de Semana**
```
IF DATENAME('weekday', [Fecha Venta (Date)]) IN ('sábado', 'domingo') 
THEN 'Fin de Semana' 
ELSE 'Entre Semana' 
END
```

### **4. Ventas Noviembre**
```
SUM(IF MONTH([Fecha Venta (Date)]) = 11 THEN [Total (MXN)] ELSE 0 END)
```

### **5. Ventas Diciembre**
```
SUM(IF MONTH([Fecha Venta (Date)]) = 12 THEN [Total (MXN)] ELSE 0 END)
```

### **6. % Cambio Nov-Dic**
```
([Ventas Diciembre] - [Ventas Noviembre]) / [Ventas Noviembre]
```

### **7. Ratio Diciembre/Noviembre**
```
[Ventas Diciembre] / [Ventas Noviembre]
```

✅ **Crea todos estos campos antes de empezar las visualizaciones!**

---

# 📊 SHEET 7A: COMPARATIVO NOVIEMBRE VS DICIEMBRE

## 🎯 Qué Muestra

Una comparación lado a lado de métricas clave entre noviembre y diciembre para identificar crecimiento o cambios.

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **Click en "New Worksheet"** (o icono +)

2. **Double-click en la pestaña** para renombrar

3. **Escribe**: `7A_Nov_vs_Dic_Comparativo`

4. **Press Enter**

✅ **Nueva hoja lista para trabajar**

---

### **PASO 2: Agregar Mes Nombre a Columns**

1. **Busca "Mes Nombre"** en Data pane
   - Es el campo calculado que creaste: `DATENAME('month', [Fecha Venta (Date)])`

2. **Drag "Mes Nombre"** hacia **Columns**

3. **Aparece como píldora AZUL**: `Mes Nombre`

✅ **Deberías ver dos columnas: una para cada mes**

---

### **PASO 3: Preparar Múltiples Métricas**

Vamos a mostrar varias métricas al mismo tiempo. Para esto usaremos **Measure Names y Measure Values**:

1. **En Data pane**, busca abajo una sección especial que dice:
   - **"Measure Names"** (nombres de medidas)
   - **"Measure Values"** (valores de medidas)

2. **Drag "Measure Names"** hacia **Rows**

3. **Drag "Measure Values"** hacia la vista central (el área del gráfico)
   - O puedes arrastrarlo también a Rows

✅ **Aparece una lista de TODAS tus métricas (ventas, unidades, etc.)**

---

### **PASO 4: Seleccionar Solo las Métricas Relevantes**

Ahora tienes TODAS las medidas, pero solo queremos algunas específicas:

1. **Busca la tarjeta "Measure Values"** en el área de Marks Card
   - Es una pequeña ventana que lista todas las medidas incluidas

2. **Click en esa tarjeta** para expandirla (si está colapsada)

3. **Verás una lista de medidas** con checkboxes o X para remover

4. **REMUEVE todas excepto estas 6**:
   - ✅ `Total (MXN)` - mantener
   - ✅ `Unidades` - mantener
   - ✅ `IDVenta` (para contar transacciones) - mantener
   - ❌ Todo lo demás - remover (click en X)

5. **Ahora AGREGA manualmente las que faltan**:
   - Drag `Precio unitario de venta` a la tarjeta Measure Values
   - Drag cualquier otra métrica relevante

**Métricas finales que queremos**:
- Total Ventas (MXN)
- Total Unidades
- Número de Transacciones (COUNTD de IDVenta)
- Ticket Promedio (Total/Transacciones)
- Productos Únicos (COUNTD de IDproducto)

---

### **PASO 5: Crear Métrica de Ticket Promedio**

Como necesitamos ticket promedio, creemos un campo calculado:

1. **Right-click en Data pane** → **"Create Calculated Field..."**

2. **Name**: `Ticket Promedio`

3. **Formula**:
```
SUM([Total (MXN)]) / COUNTD([IDVenta])
```

4. **Click OK**

5. **Drag "Ticket Promedio"** a la tarjeta Measure Values

---

### **PASO 6: Formatear como Barras Horizontales**

1. **Verifica tu configuración actual**:
   - **Rows**: `Measure Names`
   - **Columns**: `Mes Nombre`
   - **Marks Card**: Debería tener Measure Values

2. **Cambiar orientación si es necesario**:
   - Si las barras están verticales, intercambia Rows y Columns:
   - **Columns**: `Measure Names`
   - **Rows**: `Mes Nombre`

3. **Asegurar que son barras**:
   - Marks Card dropdown → Select **"Bar"**

✅ **Deberías ver barras comparando Nov vs Dic para cada métrica**

---

### **PASO 7: Colorear por Mes**

1. **Drag "Mes Nombre"** hacia **Color** en Marks Card

2. **Click en "Color"** → **"Edit Colors..."**

3. **Asignar colores temáticos**:
   - **Noviembre**: Naranja otoñal
     - Click en "noviembre" → More Colors → Hex: `#d95f02`
   - **Diciembre**: Rojo navideño
     - Click en "diciembre" → More Colors → Hex: `#cc0000`

4. **Click OK**

✅ **Barras ahora tienen colores estacionales**

---

### **PASO 8: Agregar Etiquetas con Valores**

1. **Drag "Measure Values"** hacia **Label** en Marks Card

2. **Click en "Label"** para configurar

3. **En el diálogo**:
   - **Marca**: "Show mark labels"
   - **Alignment**: End (al final de la barra)
   - **Font**: Tableau Book, Size 10

4. **Formatear números**:
   - Para currency: Click derecho en el campo en Label → Format
   - Para percentages: Similar proceso

---

### **PASO 9: Agregar Porcentaje de Cambio**

Vamos a crear una columna adicional que muestre el % de cambio:

1. **Create Calculated Field**: `% Cambio Text`

2. **Formula**:
```
// Para mostrar como texto con color
IF [Ventas Diciembre] > [Ventas Noviembre] 
THEN "▲ +" + STR(ROUND([% Cambio Nov-Dic] * 100, 1)) + "%"
ELSE "▼ " + STR(ROUND([% Cambio Nov-Dic] * 100, 1)) + "%"
END
```

3. **Drag este campo a Label** (además del valor)

---

### **PASO 10: Crear Vista Alternativa - Butterfly Chart** (Opcional)

Si quieres una vista más impactante:

1. **Duplicate esta sheet** (right-click en tab → Duplicate)

2. **Rename**: `7A_Butterfly_Nov_Dic`

3. **Modificar valores**:
   - Create calculated field: `Ventas Noviembre Negativo`
     - Formula: `-1 * [Ventas Noviembre]`
   
4. **En la vista**:
   - Replace Measure Values con dos métricas separadas
   - Una para Nov (valores negativos), otra para Dic (valores positivos)

✅ **Esto crea efecto mariposa con Nov a la izquierda, Dic a la derecha**

---

### **PASO 11: Formato Final**

1. **Título de la sheet**:
   - "Comparativo: Noviembre vs Diciembre 2021"
   - Bold, Size 14

2. **Ejes**:
   - Right-click en eje → "Format"
   - Numbers: Currency para $ amounts

3. **Tooltip**:
```
📅 Mes: <Mes Nombre>
📊 Métrica: <Measure Names>

💰 Valor: <Measure Values>
```

4. **Gridlines**: Worksheet → Format → Borders → Quitar si molestan

---

## ✅ CHECKLIST - SHEET 7A COMPLETA

- [ ] Sheet nombrada "7A_Nov_vs_Dic_Comparativo"
- [ ] Mes Nombre en Columns (Nov y Dic)
- [ ] Measure Names en Rows
- [ ] 5-6 métricas clave seleccionadas en Measure Values
- [ ] Barras coloreadas (Naranja Nov, Rojo Dic)
- [ ] Labels con valores
- [ ] % cambio visible (opcional)
- [ ] Título descriptivo
- [ ] Formato de currency/números correcto

---

# 📅 SHEET 7B: PATRONES POR DÍA DE LA SEMANA

## 🎯 Qué Muestra

¿Qué días de la semana venden más? ¿Hay diferencias entre Nov y Dic?

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **New Worksheet** → Rename: `7B_Patrones_DiaSemana`

---

### **PASO 2: Agregar Día de la Semana a Columns**

1. **Busca "Día de la Semana"** en Data pane
   - Es el campo calculado: `DATENAME('weekday', [Fecha Venta (Date)])`

2. **Drag hacia Columns**

3. **Aparece como**: `Día de la Semana` (píldora azul)

✅ **Ves 7 columnas: Lunes a Domingo**

**IMPORTANTE**: Ordenar correctamente

4. **Right-click en la píldora "Día de la Semana"** en Columns

5. **Select "Sort..."**

6. **En el diálogo de Sort**:
   - Select: **"Manual"**
   - **Arrastra para ordenar**: Lunes, Martes, Miércoles, Jueves, Viernes, Sábado, Domingo

7. **Click OK**

✅ **Días ahora en orden correcto (Lun-Dom)**

---

### **PASO 3: Agregar Total (MXN) a Rows**

1. **Drag "Total (MXN)"** hacia **Rows**

2. **Aparece como**: `SUM(Total (MXN))` (verde)

✅ **Aparece una línea o barras**

---

### **PASO 4: Agregar Mes Nombre para Comparar**

Para ver Nov vs Dic en la misma gráfica:

1. **Drag "Mes Nombre"** hacia **Color** en Marks Card

✅ **Ahora tienes DOS líneas: una naranja (Nov), una roja (Dic)**

---

### **PASO 5: Formatear como Line Chart**

1. **Marks Card dropdown** → Select: **"Line"**

2. **Ajustar colores** (si no están bien):
   - Click "Color" → Edit Colors
   - Nov: Naranja `#d95f02`
   - Dic: Rojo `#cc0000`

3. **Ajustar grosor de líneas**:
   - Click "Size" → Slider a la derecha (líneas más gruesas)

---

### **PASO 6: Agregar Markers (Puntos) en las Líneas**

Para ver mejor cada punto de dato:

1. **Click en "Color"** en Marks Card

2. **En el menú que se abre**:
   - Busca "Markers" o "Mark borders"
   - Select: "On" o ajusta opacity

**Alternativa más fácil**:

3. **Drag "Mes Nombre"** TAMBIÉN hacia **Shape** en Marks Card

4. **Los puntos aparecen automáticamente**

✅ **Líneas con puntos en cada día**

---

### **PASO 7: Agregar Línea de Promedio General**

1. **Tab Analytics** → Drag **"Average Line"**

2. **Drop en "Pane"**

3. **Configure**:
   - Value: Average of `Total (MXN)`
   - Label: "Promedio"
   - Line: Dashed (punteada)
   - Color: Gris oscuro

4. **Click OK**

✅ **Línea horizontal de referencia**

---

### **PASO 8: Agregar Segunda Capa - Unidades como Barras** (Opcional)

Para ver también volumen de unidades vendidas:

1. **Drag "Unidades"** hacia **Rows** (al lado de Total MXN)

2. **Right-click en `SUM(Unidades)`** en Rows

3. **Select "Dual Axis"**

4. **Los dos ejes se combinan**

5. **En el Marks Card de Unidades**:
   - Change type to: **"Bar"**
   - Color: Gris claro con 40% opacity
   - Send to back (si tapa las líneas)

6. **Right-click en eje derecho**:
   - Uncheck "Synchronize Axis" (queremos escalas diferentes)

✅ **Líneas de ventas con barras de unidades de fondo**

---

### **PASO 9: Resaltar Días Específicos** (Opcional)

Para destacar el día con más ventas:

1. **Create Calculated Field**: `Día Top Ventas`

2. **Formula**:
```
IF SUM([Total (MXN)]) = WINDOW_MAX(SUM([Total (MXN)])) 
THEN "Día Pico" 
ELSE "Normal" 
END
```

3. **Drag a Color** en Marks Card de la línea

4. **Edit Colors**:
   - "Día Pico": Amarillo brillante o dorado
   - "Normal": Colores normales Nov/Dic

---

### **PASO 10: Tooltip Informativo**

```
📅 Día: <Día de la Semana>
🗓️ Mes: <Mes Nombre>

💰 VENTAS
Total: $<SUM(Total (MXN))> MXN
Unidades: <SUM(Unidades)>
Transacciones: <COUNTD(IDVenta)>

📊 COMPARACIÓN
Promedio del mes: $<WINDOW_AVG(SUM([Total (MXN)]))> MXN
```

---

## ✅ CHECKLIST - SHEET 7B COMPLETA

- [ ] Sheet nombrada "7B_Patrones_DiaSemana"
- [ ] Día de la Semana en Columns (ordenado Lun-Dom)
- [ ] Total (MXN) como líneas
- [ ] Dos líneas de color (Nov vs Dic)
- [ ] Puntos/markers visibles
- [ ] Línea de promedio (referencia)
- [ ] Barras de unidades (opcional, dual-axis)
- [ ] Tooltip formateado
- [ ] Título: "Patrones de Venta por Día de la Semana"

---

# 📈 SHEET 7C: EVOLUCIÓN SEMANAL CON HITOS

## 🎯 Qué Muestra

Timeline continuo de ventas con anotaciones de eventos importantes (Buen Fin, Black Friday, Navidad, etc.)

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **New Worksheet** → Rename: `7C_Evolucion_Semanal`

---

### **PASO 2: Configurar Eje de Tiempo**

1. **Drag "Fecha Venta (Date)"** hacia **Columns**

2. **Asegurarse que sea continuo y a nivel día**:
   - Click en el dropdown de la píldora
   - Select: **"Day"** (con ícono de calendario)
   - Asegúrate que sea **VERDE** (continuo), no azul

✅ **Columns muestra**: `DAY(Fecha Venta (Date))` en verde

---

### **PASO 3: Agregar Ventas como Área**

1. **Drag "Total (MXN)"** hacia **Rows**

2. **Marks Card dropdown** → Select: **"Area"**

✅ **Gráfica de área mostrando ventas diarias**

---

### **PASO 4: Aplicar Gradient de Color Dinámico**

Para que el color cambie según el nivel de ventas:

1. **Drag "Total (MXN)"** TAMBIÉN hacia **Color** en Marks Card

2. **Click en "Color"** → **"Edit Colors..."**

3. **Select Palette**: "Orange-Gold" o "Blue" (Sequential)

4. **Stepped Color**: Desmarcar (queremos gradient continuo)

5. **Click OK**

6. **Ajustar Opacity**:
   - En menú Color → Opacity slider a 70%

✅ **Área con gradient de color según valor de ventas**

---

### **PASO 5: Crear Campo de Eventos Importantes**

Ahora viene la parte especial - marcar eventos:

1. **Create Calculated Field**: `Eventos Comerciales`

2. **Formula** (ajusta las fechas según tu dataset):
```
// Eventos Importantes Nov-Dic 2021
IF [Fecha Venta (Date)] = DATE('2021-11-19') THEN 'Black Friday'
ELSEIF [Fecha Venta (Date)] = DATE('2021-11-26') THEN 'Buen Fin'
ELSEIF [Fecha Venta (Date)] = DATE('2021-11-29') THEN 'Cyber Monday'
ELSEIF [Fecha Venta (Date)] = DATE('2021-12-01') THEN 'Inicio Diciembre'
ELSEIF [Fecha Venta (Date)] = DATE('2021-12-12') THEN 'Día de Guadalupe'
ELSEIF [Fecha Venta (Date)] = DATE('2021-12-24') THEN 'Nochebuena'
ELSEIF [Fecha Venta (Date)] = DATE('2021-12-25') THEN 'Navidad'
ELSEIF [Fecha Venta (Date)] = DATE('2021-12-31') THEN 'Fin de Año'
ELSE NULL
END
```

3. **Click OK**

---

### **PASO 6: Agregar Líneas Verticales para Eventos**

#### **Método A: Usando Reference Lines (Más fácil)**

1. **Tab Analytics** → Drag **"Reference Line"**

2. **Drop en "Pane"**

3. **En el diálogo**:
   - **Value**: Select "Cell" calculation
   - Busca opción para "Constant"
   - Enter la fecha: `11/19/2021` (formato de tu dataset)

4. **Formatting**:
   - **Line**: Dashed vertical
   - **Color**: Negro o gris oscuro
   - **Label**: "Black Friday"

5. **Click OK**

6. **Repetir para cada evento** (necesitas crear una reference line por cada uno)

**Problema**: Esto es tedioso para muchos eventos.

#### **Método B: Usando Annotations (Más visual)**

1. **Hover sobre el punto** de la fecha del evento (ej: 19 Nov)

2. **Right-click en ese punto**

3. **Select "Annotate" → "Point"**

4. **En el cuadro de texto**:
   - Escribe: "🛍️ Black Friday"

5. **Ajustar posición**: Drag la anotación arriba o al lado

6. **Format annotation**:
   - Right-click en annotation → Format
   - Background: Amarillo claro
   - Border: Negro
   - Font: Bold

7. **Repetir para cada evento**

#### **Método C: Marks con Shapes (Más dinámico)**

1. **Drag "Eventos Comerciales"** hacia **Shape** en Marks Card

2. **Los eventos aparecen como shapes** en sus fechas

3. **Click en "Shape"** → **"Edit Shapes"**

4. **Assign specific shapes**:
   - Black Friday: ⬇ (flecha abajo)
   - Navidad: 🎄 (si hay símbolos disponibles)
   - Etc.

5. **Drag "Eventos Comerciales"** TAMBIÉN a **Label**

6. **Las etiquetas aparecen** sobre los puntos de evento

✅ **Eventos marcados visualmente en la línea de tiempo**

---

### **PASO 7: Agregar Líneas Verticales con Bands**

Para hacer los eventos MÁS evidentes:

1. **Create another calculated field**: `Es Día de Evento`

2. **Formula**:
```
IF NOT ISNULL([Eventos Comerciales]) THEN TRUE ELSE FALSE END
```

3. **Drag a Color** en Marks Card

4. **Edit Colors**:
   - True: Rojo brillante (días de evento)
   - False: Color normal del gradient

**O mejor aún - usar Reference Bands**:

5. **Analytics** → **Reference Band**

6. **Configure**:
   - From: 19 de noviembre
   - To: 19 de noviembre (mismo día = línea)
   - Fill: Amarillo semi-transparente
   - Label: "Black Friday"

---

### **PASO 8: Título Dinámico con Stats**

1. **Create Calculated Field**: `Título con Stats`

2. **Formula**:
```
"Evolución de Ventas - Total: $" + 
STR(ROUND(SUM([Total (MXN)]), 0)) + 
" MXN en " + 
STR(DATEDIFF('day', MIN([Fecha Venta (Date)]), MAX([Fecha Venta (Date)]))) + 
" días"
```

3. **Use este campo en el título de la sheet**

---

## ✅ CHECKLIST - SHEET 7C COMPLETA

- [ ] Sheet nombrada "7C_Evolucion_Semanal"
- [ ] Fecha en Columns (día continuo)
- [ ] Ventas como área chart con gradient
- [ ] Campo "Eventos Comerciales" creado
- [ ] Eventos marcados visualmente (anotaciones o reference lines)
- [ ] Colors distintivos para eventos importantes
- [ ] Tooltip con información de evento (si aplica)
- [ ] Título: "Evolución Temporal con Eventos Clave"

---

# 🗓️ SHEET 7D: HEAT MAP - SEMANA DEL MES

## 🎯 Qué Muestra

Comportamiento de ventas por semana dentro de cada mes (Nov y Dic).

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **New Worksheet** → Rename: `7D_HeatMap_Semana`

---

### **PASO 2: Configurar Estructura del Heat Map**

1. **Columns**: Drag **"Semana del Mes"** (campo calculado que creaste)

2. **Rows**: Drag **"Mes Nombre"**

✅ **Grid de 2 filas (Nov, Dic) × 4-5 columnas (semanas)**

---

### **PASO 3: Agregar Ventas como Color**

1. **Drag "Total (MXN)"** hacia **Color** en Marks Card

2. **Marks type** → Select: **"Square"**

✅ **Heat map con cuadrados de colores**

---

### **PASO 4: Configurar Paleta de Colores**

1. **Click "Color"** → **"Edit Colors..."**

2. **Select Palette**: "Green-Gold" (Sequential)

3. **Stepped Color**: Marcar checkbox
   - Steps: 4 o 5

4. **Click OK**

✅ **Colores más intensos = más ventas esa semana**

---

### **PASO 5: Agregar Etiquetas con Valores y %**

1. **Drag "Total (MXN)"** hacia **Label**

2. **Format**: Currency, sin decimales

3. **También agregar % del total**:
   - Create field: `% del Total por Semana`
   - Formula: `SUM([Total (MXN)]) / TOTAL(SUM([Total (MXN)]))`
   - Drag a Label también

4. **Tooltip**:
```
📅 <Mes Nombre> - Semana <Semana del Mes>

💰 Ventas: $<SUM(Total (MXN))> MXN
📊 % del Total: <% del Total por Semana>
📦 Unidades: <SUM(Unidades)>
```

---

### **PASO 6: Dual Encoding - Agregar Size**

Para hacer más evidente la magnitud:

1. **Drag "% con publicidad"** o **"Total (MXN)"** hacia **Size**

2. **Los cuadrados cambian de tamaño** según la métrica

✅ **Dual encoding: color Y tamaño**

---

## ✅ CHECKLIST - SHEET 7D COMPLETA

- [ ] Sheet nombrada "7D_HeatMap_Semana"
- [ ] Semana del Mes en Columns
- [ ] Mes Nombre en Rows
- [ ] Color por Total (MXN) - sequential palette
- [ ] Marks type: Square
- [ ] Labels con valores y %
- [ ] Size encoding (opcional)
- [ ] Borders blancos entre cuadrados
- [ ] Tooltip informativo

---

# 🎯 SHEET 7E: ANÁLISIS DE PRODUCTOS ESTACIONALES

## 🎯 Qué Muestra

Scatter plot que compara ventas de cada producto en Nov vs Dic para identificar:
- Productos con más ventas en Diciembre (navideños)
- Productos estables en ambos meses
- Productos más fuertes en Noviembre

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **New Worksheet** → Rename: `7E_Productos_Estacionales`

---

### **PASO 2: Verificar Campos Calculados**

Asegúrate que tienes estos campos (deberías haberlos creado en Preparación):

- `Ventas Noviembre` 
- `Ventas Diciembre`
- `Ratio Diciembre/Noviembre`

Si no los tienes, créalos ahora (ve a sección Preparación arriba).

---

### **PASO 3: Configurar Scatter Plot**

1. **Columns**: Drag **"Ventas Noviembre"**

2. **Rows**: Drag **"Ventas Diciembre"**

3. **Detail**: Drag **"IDproducto"** (o Título de la publicación)

✅ **Aparece un punto por cada producto**

---

### **PASO 4: Agregar Tamaño basado en Ventas Totales**

Para ver qué productos son más importantes:

1. **Drag "Total (MXN)"** hacia **Size** en Marks Card

✅ **Productos grandes = más ventas totales, pequeños = menos ventas**

---

### **PASO 5: Colorear por Ratio Dic/Nov**

1. **Drag "Ratio Diciembre/Noviembre"** hacia **Color**

2. **Click "Color"** → **"Edit Colors..."**

3. **Select Palette**: "Red-Yellow-Green" (Diverging)

4. **Configure**:
   - **Center**: `1.0` (ventas iguales ambos meses)
   - **Start** (Rojo): Ratio < 1 (más fuerte en Nov)
   - **End** (Verde): Ratio > 1 (más fuerte en Dic)

5. **Click OK**

✅ **Verde = productos navideños, Rojo = productos de noviembre, Amarillo = estables**

---

### **PASO 6: Agregar Línea Diagonal de Referencia (45°)**

Esta línea muestra ventas iguales en Nov y Dic:

1. **Tab Analytics** → Drag **"Reference Line"**

2. **Drop en "Pane"**

3. **En el diálogo**:
   - **Value**: Custom
   - **En el custom formula field, escribe**: 
```
AVG([Ventas Diciembre]) = AVG([Ventas Noviembre])
```

**Esto es complicado, mejor usa este método**:

4. **Create Calculated Field**: `Línea 45 Grados`

5. **Formula**:
```
[Ventas Noviembre]
```

6. **Drag este campo a Rows** (junto a Ventas Diciembre)

7. **Dual axis** → synchronize axis

8. **Mark type** de la línea → Line

9. **Color**: Gris oscuro, dashed

**O más fácil - Add Trend Line**:

10. **Analytics** → **Trend Line**

11. **Drop en la vista**

12. **Select**: Linear

13. **Edit**: Force intercept at 0, slope = 1 (si hay opción)

---

### **PASO 7: Agregar Cuadrantes de Referencia**

Para dividir en 4 áreas estratégicas:

#### **Línea Vertical (media de Nov)**

1. **Analytics** → **Reference Line**

2. **Drop en "Pane"** 

3. **Configure**:
   - Scope: Columns
   - Value: Average of Ventas Noviembre
   - Line: Dashed, gris claro

#### **Línea Horizontal (media de Dic)**

4. **Drag otra Reference Line**

5. **Configure**:
   - Scope: Rows  
   - Value: Average of Ventas Diciembre
   - Line: Dashed, gris claro

✅ **4 cuadrantes visibles**:
- **Alto Dic / Bajo Nov** (arriba izquierda): Productos navideños
- **Alto Nov / Bajo Dic** (abajo derecha): Productos de Noviembre  
- **Alto Alto** (arriba derecha): Best sellers ambos meses
- **Bajo Bajo** (abajo izquierda): Productos secundarios

---

### **PASO 8: Agregar Labels Solo para Productos Importantes**

No queremos etiquetar TODOS los productos (se ve amontonado):

1. **Create Calculated Field**: `Label Top Productos`

2. **Formula**:
```
IF SUM([Total (MXN)]) > 10000 
THEN [Título de la publicación]
END
```

3. **Drag este campo a Label**

✅ **Solo productos grandes tienen etiqueta**

---

### **PASO 9: Tooltip Rico en Información**

```
📦 Producto: <Título de la publicación>

💰 VENTAS POR MES
Noviembre: $<Ventas Noviembre> MXN
Diciembre: $<Ventas Diciembre> MXN
Total: $<SUM(Total (MXN))> MXN

📊 ESTACIONALIDAD
Ratio Dic/Nov: <Ratio Diciembre/Noviembre>
Clasificación: <
  IF [Ratio Diciembre/Noviembre] > 1.5 THEN "⭐ Producto Navideño"
  ELSEIF [Ratio Diciembre/Noviembre] < 0.7 THEN "🍂 Producto de Noviembre"
  ELSE "➡️ Producto Estable"
  END
>

📈 UNIDADES
Nov: <SUM(IF MONTH([Fecha Venta (Date)]) = 11 THEN [Unidades] END)>
Dic: <SUM(IF MONTH([Fecha Venta (Date)]) = 12 THEN [Unidades] END)>
```

---

### **PASO 10: Filtros Adicionales**

1. **Filtrar productos pequeños** para limpieza:
   - Drag "Total (MXN)" a Filters
   - At least: $1,000 (o el mínimo que quieras analizar)

2. **Mostrar solo Top 20 productos**:
   - Drag campo de Rank a Filters
   - Top 20

---

## ✅ CHECKLIST - SHEET 7E COMPLETA

- [ ] Sheet nombrada "7E_Productos_Estacionales"
- [ ] Ventas Noviembre en Columns
- [ ] Ventas Diciembre en Rows
- [ ] Un punto por producto (Detail: IDproducto)
- [ ] Size: Total ventas (más grande = más importante)
- [ ] Color: Ratio Dic/Nov (diverging: verde-amarillo-rojo)
- [ ] Línea diagonal de referencia (ventas iguales)
- [ ] Líneas de cuadrantes (medias)
- [ ] Labels solo para productos importantes
- [ ] Tooltip detallado con clasificación
- [ ] Filtro de productos relevantes
- [ ] Título: "Análisis de Estacionalidad por Producto"

---

# 🎨 DISEÑO Y CONSISTENCIA

## Paleta de Colores para KPI 7

Mantén consistencia en todas las sheets:

**Meses**:
- Noviembre: Naranja otoñal `#d95f02`
- Diciembre: Rojo navideño `#cc0000`

**Estacionalidad**:
- Verde `#2ca02c`: Productos navideños (ratio > 1.5)
- Amarillo `#ffd700`: Productos estables (ratio 0.7-1.5)
- Rojo `#d62728`: Productos de noviembre (ratio < 0.7)

**Eventos**:
- Black Friday / Buen Fin: Amarillo `#ffec00`
- Navidad: Rojo navideño `#cc0000`
- Cyber Monday: Azul `#1f77b4`

---

## Formato de Números Consistente

- **Currency**: $X,XXX MXN (sin decimales para grandes cantidades)
- **Percentages**: XX.X% (1 decimal)
- **Ratios**: X.XX (2 decimales)
- **Counts**: X,XXX (sin decimales)

---

# 🚨 TROUBLESHOOTING COMÚN

## ❌ "Las fechas de eventos no coinciden"

**Problema**: Tu dataset tiene fechas de 2022, no 2021

**Solución**:
- Actualiza el campo calculado "Eventos Comerciales" con las fechas correctas de tu dataset
- Verifica el año con: `MIN([Fecha Venta (Date)])` en una vista

---

## ❌ "Los cuadrantes en 7E no se ven"

**Problema**: Líneas de referencia están fuera del rango visible

**Solución**:
1. Right-click en ejes → Edit Axis
2. Range: Uniform axis range
3. Include zero: Marcar
4. Vuelve a agregar las reference lines

---

## ❌ "El scatter plot 7E tiene demasiados puntos"

**Problema**: Todos los productos aparecen, es difícil de leer

**Solución**:
- Aplica filtro: Total (MXN) > $5,000
- O usa filtro Top 30 productos
- O aumenta transparency de puntos pequeños

---

## ❌ "Semana del Mes muestra números raros (5, 6)"

**Problema**: El cálculo de semanas cuenta desde inicio del año

**Solución**: Usa el campo calculado correcto:
```
DATEDIFF('week', 
    DATETRUNC('month', [Fecha Venta (Date)]), 
    [Fecha Venta (Date)]
) + 1
```
Esto cuenta semanas DENTRO del mes (1-5)

---

## ❌ "Annotations se mueven cuando filtro datos"

**Problema**: Annotations son estáticas, no dinámicas

**Solución**:
- En lugar de annotations, usa Marks con campo de Eventos
- O usa Reference Lines con valores fijos de fecha

---

# ✨ TIPS PARA ANÁLISIS

## Preguntas que Estas Visualizaciones Responden

**Sheet 7A - Comparativo**:
- ¿Cuánto crecen las ventas de Nov a Dic?
- ¿Qué métricas cambian más?
- ¿El ticket promedio sube o baja?

**Sheet 7B - Día de Semana**:
- ¿Qué día de la semana vende más?
- ¿Hay diferencia Nov vs Dic en patrones semanales?
- ¿Fin de semana vende más que entre semana?

**Sheet 7C - Timeline**:
- ¿Qué eventos generan picos de venta?
- ¿Cuándo empiezan las compras navideñas?
- ¿Hay días "muertos" después de eventos?

**Sheet 7D - Heat Map**:
- ¿Qué semanas del mes son más fuertes?
- ¿Primera vs última semana de diciembre?

**Sheet 7E - Productos Estacionales**:
- ¿Qué productos solo venden bien en Navidad?
- ¿Hay productos todo-el-año?
- ¿Dónde invertir en publicidad según temporada?

---

# 🎯 INSIGHTS ESPERADOS

Después de crear estas visualizaciones, deberías poder decir:

✅ "Las ventas crecieron X% de Nov a Dic"
✅ "Los viernes son el mejor día para campañas"
✅ "Black Friday genera un pico de X% vs promedio"
✅ "Producto XYZ es claramente navideño (3x ventas en Dic)"
✅ "La primera semana de Dic es la más fuerte"

---

# 🚀 ¡FELICITACIONES!

Has completado las 5 visualizaciones del **KPI 7 - Análisis Estacional**:

✅ **7A** - Comparativo Nov vs Dic (métricas clave)
✅ **7B** - Patrones por día de semana
✅ **7C** - Timeline con eventos importantes
✅ **7D** - Heat map por semana del mes
✅ **7E** - Scatter plot de productos estacionales

**Estas visualizaciones te dan una comprensión completa de los patrones estacionales de tu negocio!** 📊

---

## 📚 SIGUIENTES PASOS

1. **Analizar los insights** que encontraste
2. **Documentar hallazgos** clave
3. **Preparar para integrar** en dashboard final
4. **Continuar con KPI 8** (Optimización Logística)

---

**¿Tienes dudas sobre alguna visualización? ¡Pregunta! Estoy aquí para ayudarte.** 💪

*Guía creada para Numismática México - MNA Visualización de Datos*  
*Octubre 2025*

