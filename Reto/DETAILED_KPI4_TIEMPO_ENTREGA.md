# Guía Detallada: KPI 4 - Tiempo de Entrega Promedio
## Paso a Paso Completo con Instrucciones Específicas

---

## 🎯 OBJETIVO DEL KPI

Analizar tiempos de entrega para:
- Identificar transportistas más rápidos y confiables
- Detectar regiones con problemas de entrega
- Optimizar logística y mejorar satisfacción del cliente

---

## 📋 QUÉ VAMOS A CREAR

Crearemos **3 visualizaciones**:

1. **Sheet 4A**: Box Plot - Distribución de tiempos por transportista
2. **Sheet 4B**: Line Chart - Tendencia de tiempos en el tiempo
3. **Sheet 4C**: Heat Map - Tiempos por estado y transportista

---

## 🚀 PREPARACIÓN: Campos Calculados Necesarios

Antes de empezar, asegúrate de tener estos campos calculados creados:

### **1. Días para Entrega** (Ya deberías tenerlo)
```
DATEDIFF('day', [FechaCamino], [FechaEntrega])
```

### **2. Días Hábiles para Entrega** (Opcional, mejora)
```
// Semanas completas * 5 días hábiles
INT([Días para Entrega] / 7) * 5

// Más días restantes (máximo 5)
+ MIN([Días para Entrega] % 7, 5)

// Ajuste si empieza en fin de semana
- CASE DATEPART('weekday', [FechaCamino])
    WHEN 1 THEN 1  // Domingo
    WHEN 7 THEN 1  // Sábado
    ELSE 0
  END
```

### **3. Categoría de Entrega** (Opcional, para análisis)
```
IF [Días para Entrega] = 0 THEN 'Mismo Día'
ELSEIF [Días para Entrega] = 1 THEN '1 Día'
ELSEIF [Días para Entrega] <= 3 THEN '2-3 Días'
ELSEIF [Días para Entrega] <= 5 THEN '4-5 Días'
ELSE '6+ Días'
END
```

✅ **Ahora sí, comencemos con las visualizaciones!**

---

# 📊 SHEET 4A: BOX PLOT - Tiempos de Entrega

## 🎯 Qué Muestra

Un **Box Plot** (diagrama de caja) muestra la distribución de tiempos de entrega para cada transportista:
- La caja = 50% del medio de los datos
- La línea en la caja = Mediana
- Los bigotes = Rango completo
- Puntos = Valores atípicos (outliers)

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **Mira en la parte inferior de tu pantalla** donde están las pestañas de sheets

2. **Busca un ícono o espacio vacío** al lado de tu última sheet

3. **Opciones para crear nueva sheet**:
   - **Opción A**: Click en el icono **"+"** o **"New Worksheet"**
   - **Opción B**: Click derecho en espacio vacío → "New Worksheet"
   - **Opción C**: Menú: Worksheet → New Worksheet

4. **Se crea una nueva sheet en blanco**

5. **Renombrar la sheet**:
   - **Double-click** en la pestaña (donde dice "Sheet X")
   - **Escribe**: `4A_Tiempos_Entrega_BoxPlot`
   - **Press Enter**

✅ **Resultado**: Tienes una hoja nueva llamada "4A_Tiempos_Entrega_BoxPlot"

---

### **PASO 2: Agregar Transportista a Columns**

1. **Busca "Transportista"** en el Data pane (panel izquierdo)
   - Está en la sección de Dimensions o dentro de tu tabla "Ventas"

2. **Click y MANTÉN presionado** el botón del mouse sobre "Transportista"

3. **Arrastra** hacia el área superior que dice **"Columns"**
   - Es una barra horizontal cerca del top de tu workspace
   - Dice "Drop field here" cuando está vacía

4. **Suelta el botón del mouse**

✅ **Resultado**: 
- En Columns shelf aparece una píldora AZUL que dice: `Transportista`
- En tu vista ves columnas con nombres de transportistas (99minutos, DHL, FedEx, Mercado Envios, Paquetexpress)

---

### **PASO 3: Agregar Días para Entrega a Rows**

1. **Busca "Días para Entrega"** en Data pane
   - Debería estar en Measures (tiene símbolo #)
   - Si no lo ves, búscalo en calculated fields

2. **Arrastra "Días para Entrega"** hacia **"Rows"**
   - Es la barra vertical en el lado izquierdo del workspace
   - Dice "Drop field here" cuando está vacía

3. **Suéltalo**

✅ **Resultado**:
- En Rows aparece una píldora VERDE: `SUM(Días para Entrega)`
- En tu vista ves barras verticales (por ahora, es normal)

---

### **PASO 4: Convertir a Box Plot usando Show Me**

Ahora viene la parte importante - cambiar a Box Plot:

#### **A. Abrir Show Me**

1. **Busca "Show Me"** en la esquina superior derecha de tu pantalla
   - Es un panel que puede estar colapsado o expandido
   - Tiene un ícono de diferentes tipos de gráficas

2. **Si está colapsado** (solo ves el título):
   - **Click en "Show Me"** para expandir el panel

3. **Si está expandido**: 
   - Verás una cuadrícula con ~20 tipos diferentes de visualizaciones

#### **B. Seleccionar Box-and-Whisker Plot**

4. **Busca el ícono del Box Plot** en la cuadrícula de Show Me
   - Se parece a esto: `─┤□□□├─` (una caja con líneas a los lados)
   - Nombre: "box-and-whisker plots" o "box plots"
   - Generalmente está en la fila 3-4 de la cuadrícula

5. **Click UNA VEZ** en ese ícono

6. **Tableau automáticamente transforma tu visualización**

✅ **Resultado**: ¡Ahora tienes Box Plots! Uno por cada transportista mostrando la distribución de tiempos de entrega.

---

### **PASO 5: Entender Tu Box Plot**

Antes de formatear, veamos qué muestra:

```
Transportista 1:
                 ●  (outlier - entrega muy tardía)
          ╷
    ──────┤      (Q3 - 75% de entregas por debajo)
    ██████│      (Caja - rango intercuartil)
    ██████┼──    (Mediana - línea en medio de la caja)
    ██████│
    ──────┤      (Q1 - 25% de entregas por debajo)
          ╵
     ●            (outlier - entrega muy rápida)
```

**Elementos**:
- **Caja (rectángulo)**: Contiene el 50% central de los datos
- **Línea en la caja**: Mediana (50% de entregas están por debajo/encima)
- **Bigotes (whiskers)**: Líneas que se extienden hacia min/max
- **Puntos individuales**: Outliers (valores extremos)

---

### **PASO 6: Colorear por Transportista**

Vamos a dar color a cada transportista para distinguirlos mejor:

1. **Busca "Transportista"** en Data pane

2. **Arrástralo hacia el Marks Card** (panel izquierdo, debajo de Filters)

3. **Suéltalo específicamente en "Color"**
   - Verás un botón/área que dice "Color" con un ícono de paleta

✅ **Resultado**: Cada transportista ahora tiene su propio color

#### **Opcional: Ajustar Colores**

4. **Click en "Color"** en el Marks Card

5. **Se abre un menú desplegable**

6. **Click en "Edit Colors..."**

7. **Selecciona una paleta**:
   - Recomendado: "Tableau 10" o "Color Blind"
   - Puedes asignar colores específicos a cada transportista

8. **Click OK**

---

### **PASO 7: Agregar Línea de Referencia (Promedio General)**

Ahora vamos a agregar una línea horizontal que muestre el promedio de todos los transportistas:

#### **A. Abrir Panel de Analytics**

1. **Mira en el panel izquierdo**, arriba del Data pane

2. **Verás dos pestañas**:
   ```
   ┌──────────┬───────────┐
   │   Data   │ Analytics │
   └──────────┴───────────┘
   ```

3. **Click en la pestaña "Analytics"**

✅ **El panel cambia y muestra opciones como**:
- Summarize (Constant Line, Average Line, Median Line, etc.)
- Model
- Custom

#### **B. Agregar Average Line**

4. **Busca la sección "Summarize"** (primera sección)

5. **Busca "Average Line"** en esa sección

6. **Click en "Average Line" y MANTÉN presionado**

7. **Arrastra hacia tu gráfica** (la visualización con los box plots)

8. **Cuando estés sobre la gráfica, aparecen opciones**:
   ```
   Table    Cell    Pane
   ```

9. **Suelta sobre "Pane"** (opción del medio)

✅ **Aparece una ventana: "Edit Reference Line, Band, or Box"**

#### **C. Configurar la Reference Line**

10. **En la ventana que se abrió, configura**:

**Sección "Line"**:
- **Value**: Asegúrate que diga **"Average"** (promedio)
  - Dropdown debería mostrar opciones como Average, Median, Constant, etc.
  - Selecciona: **Average**

- **Label**: Controla qué texto muestra la línea
  - Dropdown con opciones: None, Value, Computation, Custom
  - Selecciona: **"Value"** o **"Computation"**
  - O escribe custom: `Promedio General: <Value> días`

**Sección "Formatting"**:

- **Line**: Define el estilo de la línea
  - Click en el dropdown que muestra el estilo de línea
  - Opciones: Sólida (────), Punteada (····), Rayada (----), etc.
  - **Selecciona**: Línea punteada o rayada

- **Color de línea**:
  - Click en el cuadrito de color al lado
  - **Selecciona**: Rojo (#d62728 o cualquier rojo)

- **Fill**: Define relleno (área sombreada)
  - **Déjalo en**: "None" (no queremos área, solo línea)

11. **Click "OK"**

✅ **Resultado**: Una línea horizontal roja punteada cruza todo el gráfico mostrando el promedio general de días de entrega

---

### **PASO 8: Agregar Etiquetas de Mediana**

Opcional pero útil - mostrar el valor de la mediana en cada box:

1. **Click en "Label"** en el Marks Card

2. **Se abre un menú de configuración**

3. **Marca la casilla**: "Show mark labels"

4. **En la sección "Marks to Label"**:
   - Puedes ajustar a solo mostrar min/max/median
   - O dejar en "Automatic"

5. **Ajusta el font**:
   - Font: Tableau Book o similar
   - Size: 9 o 10
   - Alignment: Center

6. **Click fuera del menú** para cerrar

✅ **Resultado**: Ves valores numéricos en o cerca de los box plots

---

### **PASO 9: Formatear el Tooltip (Hover Information)**

Cuando pases el mouse sobre los box plots, aparece información. Vamos a mejorarla:

1. **Click en "Tooltip"** en el Marks Card

2. **Se abre un editor de texto**

3. **Borra el contenido existente** (Ctrl+A, Delete)

4. **Copia y pega este contenido**:

```
🚚 <ATTR(Transportista)>

⏱️ TIEMPOS DE ENTREGA
Promedio: <AVG(Días para Entrega)> días
Mediana: <MEDIAN(Días para Entrega)> días
Mínimo: <MIN(Días para Entrega)> días
Máximo: <MAX(Días para Entrega)> días

📊 VOLUMEN
Entregas: <COUNTD(IDVenta)>
```

**IMPORTANTE**: Para agregar los campos correctamente:

- **NO escribas** `<AVG(Días para Entrega)>` manualmente
- En lugar de eso:
  - Posiciona el cursor donde quieres el valor
  - Click en **"Insert"** (esquina superior derecha del editor)
  - Selecciona el campo apropiado de la lista
  - Tableau lo inserta como `<AGG(campo)>`

5. **Click "OK"**

✅ **Resultado**: Tooltips informativos con emojis y estructura clara

---

### **PASO 10: Formatear Ejes**

#### **A. Formato del Eje Y (Días para Entrega)**

1. **Right-click en el eje Y** (lado izquierdo donde están los números)

2. **Select "Format..."**

3. **Se abre un panel de Format a la izquierda**

4. **En la sección "Axis"**:
   - **Title**: Puedes cambiar el título del eje
     - Doble-click → Escribe: "Días de Entrega"
   
5. **En la sección "Scale"** (pestañas abajo):
   - **Numbers**: Formato de números
     - Selecciona: "Number (Standard)"
     - Decimales: 0

6. **Cerrar el panel de Format**

#### **B. Formato del Eje X (Transportistas)**

1. **Right-click en el eje X** (abajo donde están los nombres)

2. **Select "Format..."**

3. **Ajustar**:
   - Font size: 10-11
   - Alignment: Horizontal (si los nombres son largos, puedes rotarlos)
   - Para rotar: Click en eje → Edit Axis → Tick Marks → "Rotated" 45°

✅ **Resultado**: Ejes limpios y legibles

---

### **PASO 11: Agregar Filtros (Opcional)**

Para análisis más específico:

#### **Filtro de Fecha (Nov-Dic)**

1. **Drag "Filtro NovDic"** (o tu campo de filtro de fechas) a Filters shelf

2. **Select "True"**

3. **Click OK**

#### **Filtro de Estado (Para explorar regiones)**

1. **Drag "Estado"** a Filters

2. **Select "All"** por ahora (puedes filtrar después)

3. **Click OK**

4. **Right-click en el filtro** → **"Show Filter"**
   - Aparece un selector a la derecha de tu visualización

✅ **Resultado**: Puedes filtrar por fechas y regiones dinámicamente

---

### **PASO 12: Ajustes Finales de Diseño**

#### **A. Título de la Sheet**

1. **Double-click donde dice el nombre de la sheet** (parte superior del workspace)
   - Generalmente dice "Sheet Name"

2. **Escribe un título descriptivo**:
```
Distribución de Tiempos de Entrega por Transportista
```

3. **Formatear el título**:
   - Selecciona el texto
   - Worksheet → Show Title (si no está visible)
   - Right-click en el título → Format Title
   - Font: Bold, Size 14-16

#### **B. Ajustar Tamaño de Vista**

1. **Mira en la toolbar superior**

2. **Busca iconos de ajuste de vista**:
   - "Fit Width" (ajustar al ancho)
   - "Fit Entire View" (ajustar toda la vista)
   - "Standard" (tamaño estándar)

3. **Prueba "Fit Entire View"** para ver todo el gráfico sin scroll

---

## ✅ CHECKLIST - SHEET 4A COMPLETA

Marca cuando completes cada elemento:

- [ ] Sheet creada y nombrada "4A_Tiempos_Entrega_BoxPlot"
- [ ] Transportista en Columns
- [ ] Días para Entrega en Rows
- [ ] Convertido a Box Plot (usando Show Me)
- [ ] Coloreado por Transportista
- [ ] Línea de referencia de promedio (roja, punteada)
- [ ] Etiquetas de mediana (opcional)
- [ ] Tooltip formateado
- [ ] Ejes formateados
- [ ] Filtros aplicados (Nov-Dic)
- [ ] Título claro y descriptivo

---

# 📈 SHEET 4B: LINE CHART - Tendencia de Tiempos

## 🎯 Qué Muestra

Una gráfica de línea que muestra cómo varían los tiempos promedio de entrega a lo largo del tiempo, con barras de volumen.

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **Click en el ícono "New Worksheet"** (o botón +)

2. **Renombrar**: `4B_Tendencia_TiemposEntrega`

---

### **PASO 2: Agregar FechaEntrega a Columns**

1. **Busca "FechaEntrega"** en Data pane
   - Es un campo de tipo Date (tiene ícono de calendario)

2. **Drag "FechaEntrega"** hacia **Columns**

3. **Tableau automáticamente lo pone como YEAR(FechaEntrega)**

4. **Necesitamos cambiarlo a día continuo**:
   - **Click en el dropdown** de la píldora en Columns
   - **Select**: "Day" 
   - **Asegúrate que sea CONTINUA (verde)**, no discreta (azul)
   
   **Visual**: Deberías ver `DAY(FechaEntrega)` en píldora VERDE

---

### **PASO 3: Agregar Promedio de Días a Rows**

1. **Drag "Días para Entrega"** hacia **Rows**

2. **Aparece como**: `SUM(Días para Entrega)`

3. **Cambiar a Promedio**:
   - **Click en el dropdown** de la píldora en Rows
   - **Select**: "Measure (Sum)" → Cambiar a **"Average"**
   - Ahora dice: `AVG(Días para Entrega)`

✅ **Resultado**: Una línea mostrando el promedio diario de días de entrega

---

### **PASO 4: Crear Dual-Axis con Volumen (Barras)**

Ahora vamos a agregar barras que muestren cuántas entregas hubo cada día:

1. **Drag "IDVenta"** (o cualquier campo único) hacia **Rows**
   - Lo sueltas AL LADO de `AVG(Días para Entrega)` en Rows
   - NO reemplaces, sino que agrega una segunda métrica

2. **Aparece**: `COUNTD(IDVenta)` en Rows (cuenta entregas)

3. **Crear dual-axis**:
   - **Right-click en `COUNTD(IDVenta)`** en Rows (el segundo)
   - **Select "Dual Axis"**

4. **Los dos ejes se fusionan** - ahora ves línea + barras superpuestas

---

### **PASO 5: Formatear las Dos Capas**

Ahora tienes DOS Marks Cards (uno para cada métrica). Vamos a formatearlas:

#### **A. Marks Card de AVG(Días para Entrega) - LÍNEA**

1. **Encuentra el Marks Card que dice**: `AVG(Días para Entrega)`

2. **Cambiar tipo a Line**:
   - Click en el dropdown (dice "Automatic" o tiene un ícono)
   - Select: **"Line"**

3. **Cambiar color**:
   - Click en "Color"
   - Select: Azul oscuro (#1f77b4)

4. **Cambiar grosor**:
   - Click en "Size"
   - Arrastrar slider hacia la derecha (línea más gruesa)

#### **B. Marks Card de COUNTD(IDVenta) - BARRAS**

5. **Encuentra el Marks Card que dice**: `COUNTD(IDVenta)`

6. **Cambiar tipo a Bar**:
   - Dropdown → Select: **"Bar"**

7. **Cambiar color a gris claro con transparencia**:
   - Click en "Color"
   - Select: Gris claro (#cccccc)
   - **Ajustar Opacity**: Slider a 40% (para ver la línea detrás)

---

### **PASO 6: Desincronizar Ejes** (Importante)

Los dos ejes tienen escalas diferentes (días vs cantidad). NO queremos sincronizarlos:

1. **Right-click en el eje derecho** (donde está COUNTD(IDVenta))

2. **Busca "Synchronize Axis"** en el menú

3. **Asegúrate que NO esté marcado** (sin checkmark)
   - Si está marcado, click para desmarcarlo

✅ **Resultado**: Cada eje mantiene su propia escala

---

### **PASO 7: Agregar Bandas de Objetivo** (Zonas Verde/Amarillo/Rojo)

Vamos a sombrear el fondo según objetivos de tiempo:

#### **Banda Verde: 1-3 días (Excelente)**

1. **Tab Analytics** → Drag "Reference Band"

2. **Drop en "Pane"**

3. **Configure**:
   - **Band From**: Constant → Escribe **1**
   - **Band To**: Constant → Escribe **3**
   - **Label**: "Excelente (1-3 días)"
   - **Formatting**:
     - Fill Above: None
     - Fill: Green (#d4edda) con 30% opacity
     - Fill Below: None
     - Border: None

4. **Click OK**

#### **Banda Amarilla: 4-5 días (Aceptable)**

5. **Drag otra "Reference Band"**

6. **Configure**:
   - **Band From**: Constant → **3**
   - **Band To**: Constant → **5**
   - **Fill**: Yellow (#fff3cd) con 30% opacity

#### **Banda Roja: 5+ días (Mejorar)**

7. **Drag otra "Reference Band"**

8. **Configure**:
   - **Band From**: Constant → **5**
   - **Band To**: Constant → **10** (o el máximo que veas)
   - **Fill**: Red (#f8d7da) con 30% opacity

✅ **Resultado**: El fondo muestra zonas de rendimiento

---

### **PASO 8: Filtros y Formato Final**

1. **Agregar filtro Nov-Dic**:
   - Drag "Filtro NovDic" a Filters → Select True

2. **Formatear tooltips** para cada capa (AVG y COUNTD)

3. **Título**: "Evolución de Tiempos de Entrega Promedio"

---

## ✅ CHECKLIST - SHEET 4B COMPLETA

- [ ] Sheet nombrada "4B_Tendencia_TiemposEntrega"
- [ ] FechaEntrega en Columns (día continuo, verde)
- [ ] AVG(Días para Entrega) como línea azul
- [ ] COUNTD(IDVenta) como barras grises transparentes
- [ ] Dual-axis configurado (ejes desincronizados)
- [ ] Bandas de objetivo (verde/amarillo/rojo)
- [ ] Filtro Nov-Dic aplicado
- [ ] Tooltips formateados

---

# 🔥 SHEET 4C: HEAT MAP - Estado × Transportista

## 🎯 Qué Muestra

Un mapa de calor que muestra qué transportista es más rápido en cada estado.

---

## 📝 PASO A PASO

### **PASO 1: Crear Nueva Hoja**

1. **New Worksheet** → Nombrar: `4C_HeatMap_Estado_Transportista`

---

### **PASO 2: Estructura Básica**

1. **Columns**: Drag **"Transportista"**

2. **Rows**: Drag **"Estado"**

3. **Color**: Drag **"Días para Entrega"**
   - Click en el dropdown de la píldora → Select **"Average"**
   - Ahora dice: `AVG(Días para Entrega)`

✅ **Resultado**: Una tabla con colores

---

### **PASO 3: Cambiar a Squares (Heat Map)**

1. **Marks Card**: Cambiar dropdown a **"Square"**

✅ **Resultado**: Cuadrados de colores en lugar de texto

---

### **PASO 4: Configurar Paleta de Colores Divergente**

1. **Click en "Color"** en Marks Card

2. **Click "Edit Colors..."**

3. **Select Palette**: "Red-Yellow-Green (Diverging)"

4. **Configure**:
   - **Stepped Color**: Marcar checkbox
   - **Steps**: 5
   - **Center**: Escribe **3** (el objetivo de días)
   - **Start**: Rojo (lento)
   - **Center**: Amarillo (normal)
   - **End**: Verde (rápido)

5. **Reversed**: Marca si es necesario para que verde = rápido

6. **Click OK**

✅ **Resultado**: 
- Verde = Entregas rápidas (0-2 días)
- Amarillo = Entregas normales (2-4 días)
- Rojo = Entregas lentas (4+ días)

---

### **PASO 5: Filtrar Estados Principales**

Solo queremos ver estados con suficientes datos:

#### **Método A: Filtro Manual Top 15**

1. **Drag "Estado"** a Filters

2. **Select Top tab**

3. **By field: Top 15**

4. **By: Total (MXN)** o **COUNTD(IDVenta)**

5. **Click OK**

#### **Método B: Filtro con Cálculo**

1. **Create Calculated Field**: `Estado Con Datos Suficientes`

```
{FIXED [Estado]: COUNTD([IDVenta])} >= 20
```

2. **Drag a Filters** → Select "True"

---

### **PASO 6: Agregar Etiquetas con Valores**

1. **Drag "Días para Entrega"** a **Label** en Marks Card

2. **Asegúrate que sea AVG** (promedio)

3. **Format labels**:
   - Click en "Label"
   - Font: Bold, Size 10
   - Alignment: Center, Middle
   - Format numbers: Decimal, 1 place → mostrará "2.5" por ejemplo

4. **Opcional**: Agregar texto
   - En el editor de Label, escribe: `<AVG(Días para Entrega)> d`
   - Esto muestra: "2.5 d"

---

### **PASO 7: Ajustar Tamaño y Bordes**

1. **Size**: Ajusta para que los cuadrados sean del mismo tamaño

2. **Click "Color"** → **Border**: 
   - Color: Blanco
   - Width: Medium

✅ **Resultado**: Heat map limpio con bordes blancos

---

### **PASO 8: Tooltip Enriquecido**

```
📍 <Estado>
🚚 <Transportista>

⏱️ Tiempo Promedio: <AVG(Días para Entrega)> días
📦 Entregas Realizadas: <COUNTD(IDVenta)>

💰 Costo Promedio Envío: $<AVG(Costos de envío)> MXN
```

---

## ✅ CHECKLIST - SHEET 4C COMPLETA

- [ ] Sheet nombrada "4C_HeatMap_Estado_Transportista"
- [ ] Transportista en Columns
- [ ] Estado en Rows (Top 15 o filtrado)
- [ ] AVG(Días para Entrega) en Color
- [ ] Mark type: Square
- [ ] Color palette: Red-Yellow-Green divergente (center en 3)
- [ ] Labels mostrando valores (X.X d)
- [ ] Borders blancos
- [ ] Tooltip formateado
- [ ] Filtro Nov-Dic aplicado

---

# 🎨 CONSEJOS DE DISEÑO FINAL

## Consistencia de Colores

Para las 3 sheets, mantén consistencia:

**Transportistas** (úsalos en todas las sheets):
- 99minutos: Naranja
- DHL: Rojo
- FedEx: Morado
- Mercado Envios: Azul
- Paquetexpress: Verde

**Zonas de Tiempo**:
- 0-3 días: Verde (excelente)
- 3-5 días: Amarillo (aceptable)
- 5+ días: Rojo (mejorar)

---

## Formato de Números

En todas las sheets, asegúrate que:
- Días de entrega: 1 decimal (2.5 días)
- Costos: Currency sin decimales ($150 MXN)
- Cantidades: Number sin decimales (45 entregas)

---

# 🚨 TROUBLESHOOTING COMÚN

## ❌ "El Box Plot no se crea / Show Me está gris"

**Causa**: Tableau necesita datos desagregados para Box Plot

**Solución**:
1. Ve a Analysis → Aggregate Measures → Desmarcar
2. O arrastra "Días para Entrega" a Detail en Marks Card
3. Asegúrate de tener una dimensión (Transportista) y una medida

---

## ❌ "El heat map está vacío / todas las celdas son iguales"

**Causa**: No hay suficientes datos o filtro muy restrictivo

**Solución**:
1. Verifica filtros - temporalmente quítalos todos
2. Asegúrate que hay datos en esa combinación Estado-Transportista
3. Reduce el mínimo de entregas requeridas (de 20 a 5)

---

## ❌ "Las bandas de referencia no aparecen en 4B"

**Causa**: Eje automático ajustando rango

**Solución**:
1. Right-click en eje Y → Edit Axis
2. Range: Fixed
3. Fixed start: 0
4. Fixed end: 10 (o el máximo que veas en tus datos)
5. Click OK
6. Ahora agrega las bandas de nuevo

---

## ❌ "Los colores del heat map están al revés"

**Causa**: Paleta invertida

**Solución**:
1. Click en Color → Edit Colors
2. Marca o desmarca "Reversed"
3. Asegúrate: Verde = valores bajos (rápido), Rojo = valores altos (lento)

---

# ✨ ¡FELICITACIONES!

Has completado las 3 visualizaciones del KPI 4. Ahora tienes:

✅ **Box Plot** - Distribución y comparación de transportistas
✅ **Line Chart** - Tendencia temporal con zonas de objetivo
✅ **Heat Map** - Performance por estado y transportista

**Estos 3 sheets te dan una vista 360° de tu logística de entrega!** 🚀

---

## 📚 SIGUIENTES PASOS

1. **Probar las visualizaciones**:
   - Hover sobre diferentes elementos
   - Usa los filtros
   - Analiza los insights

2. **Documentar insights encontrados**:
   - ¿Qué transportista es más rápido?
   - ¿Qué estados tienen problemas?
   - ¿Mejora o empeora en diciembre?

3. **Prepararse para integrar en dashboard**:
   - Estas 3 sheets se combinarán en el dashboard final
   - Ya están listas para usar!

---

**¿Listo para el siguiente KPI (KPI 5: ROI de Publicidad)?** 

¡O si tienes dudas sobre KPI 4, pregunta! 💪

