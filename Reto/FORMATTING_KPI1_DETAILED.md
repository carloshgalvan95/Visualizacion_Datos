# Formateo Detallado - KPI 1: Ventas Totales por Día
## Paso 5: Formatear la Visualización (DUAL-AXIS CHART)

---

## 🎯 CONTEXTO: Dónde Estás

Deberías tener en pantalla:
- Una gráfica con `Fecha Venta (Date)` en Columns
- `Total (MXN)` en Rows
- `Unidades` en Rows (segundo eje, después del paso 4)
- Ya creaste el dual-axis en el paso 4

Ahora vamos a **formatear ambas líneas** para que se vean profesionales.

---

## 📊 PARTE A: FORMATEAR TOTAL (MXN) - PRIMERA LÍNEA

### ✅ **PASO 5.1: Identificar el Marks Card Correcto**

En Tableau, cuando tienes dual-axis, verás **DOS Marks Cards** en el lado izquierdo:

```
┌─────────────────────────┐
│ Marks                   │ ← Card principal (puede decir "All")
├─────────────────────────┤
│ Marks                   │
│ SUM(Total (MXN))        │ ← Este es el que queremos formatear PRIMERO
├─────────────────────────┤
│ Marks                   │
│ SUM(Unidades)           │ ← Este lo formatearemos después
└─────────────────────────┘
```

**Ubicación**: Lado izquierdo de la pantalla, debajo de "Data" y "Analytics"

---

### ✅ **PASO 5.2: Cambiar Tipo de Marca a LINE**

1. **Localiza** el Marks Card que dice **"SUM(Total (MXN))"**

2. **Busca el dropdown** que está justo arriba de "Color, Size, Label, Detail, Tooltip"
   - Por defecto puede decir: "Automatic" o tener un ícono
   - Este dropdown controla el TIPO de marca (bar, line, circle, etc.)

3. **Click en el dropdown**
   
4. **Selecciona: "Line"** (tiene un ícono de línea diagonal)

✅ **Resultado**: Tu gráfica debería cambiar de puntos/barras a una **línea continua**

---

### ✅ **PASO 5.3: Cambiar Color a Azul Oscuro**

Ahora vamos a cambiar el color de la línea:

#### **Click en "Color"**
1. En el mismo Marks Card de **SUM(Total (MXN))**
2. **Click en el botón "Color"** (tiene un ícono de paleta de pintura)
3. Se abre un menú desplegable

#### **Ventana de Color**

Verás algo como esto:
```
┌─────────────────────────────┐
│ Color                       │
│ ┌─────────────────────────┐ │
│ │ [Color actual]          │ │
│ └─────────────────────────┘ │
│                             │
│ Opacity: [slider]    100%   │
│                             │
│ Effects:                    │
│ □ Border                    │
│                             │
│ [More Colors...] [Palettes] │
└─────────────────────────────┘
```

#### **Aplicar Color Específico (Hex Code)**

4. **Click en "More Colors..."** (botón abajo a la izquierda)

5. Se abre una ventana nueva: "Pick a Color"

6. **Busca la pestaña/opción que dice:**
   - En Windows: Puede ser un campo de texto o pestañas
   - Busca donde puedas ingresar valores RGB o Hex

7. **Selecciona/Click en la pestaña que permita ingresar código hexadecimal**
   - Puede decir "Custom" o "RGB" o mostrar campos de entrada

8. **Busca un campo que diga "Hex" o "#"**

9. **Ingresa el código**: `1f77b4`
   - Nota: NO incluyas el símbolo #
   - O si pide con #: `#1f77b4`

10. **Click "OK"**

✅ **Resultado**: Tu línea ahora es **azul oscuro**

#### **MÉTODO ALTERNATIVO si no encuentras el Hex:**

Si no encuentras cómo ingresar hex code:

1. En "More Colors", usa los sliders RGB y configura:
   - **R (Red)**: 31
   - **G (Green)**: 119
   - **B (Blue)**: 180

2. Click "OK"

---

### ✅ **PASO 5.4: Cambiar Tamaño de la Línea (Medium-Large)**

1. En el mismo Marks Card de **SUM(Total (MXN))**

2. **Click en "Size"** (tiene un ícono de círculos de diferentes tamaños)

3. Se abre un slider (barra deslizante)
```
┌─────────────────────┐
│ Size                │
│                     │
│ ●----●-----○        │ ← Slider
│ Small     Large     │
└─────────────────────┘
```

4. **Arrastra el slider hacia la derecha**
   - Posición recomendada: Aproximadamente 2/3 del camino hacia "Large"
   - La línea en tu gráfica cambiará en tiempo real

5. **Ajusta hasta que la línea se vea clara pero no demasiado gruesa**
   - Sugerencia: Grosor entre 2-4 píxeles

6. **Click fuera del menú** para cerrar

✅ **Resultado**: Tu línea ahora es más **visible y profesional**

---

### ✅ **PASO 5.5: Agregar Etiqueta Condicional (valores > $1000)**

Este es un paso OPCIONAL pero útil:

#### **Crear Campo Calculado para Etiqueta Condicional**

1. **Botón derecho en el área de datos** (Data pane, lado izquierdo)

2. **Selecciona: "Create Calculated Field..."**

3. **Nombre del campo**: `Label Total >1000`

4. **Escribe la fórmula**:
```
IF SUM([Total (MXN)]) > 1000 
THEN STR(ROUND(SUM([Total (MXN)]), 0)) + " MXN"
ELSE ""
END
```

5. **Click "OK"**

#### **Agregar la Etiqueta a la Visualización**

6. **Arrastra el campo** `Label Total >1000` que acabas de crear

7. **Suéltalo en "Label"** del Marks Card de **SUM(Total (MXN))**

8. La etiqueta aparecerá en puntos donde el total es mayor a $1000

#### **Formatear las Etiquetas (Opcional)**

9. **Click en "Label"** en el Marks Card

10. Se abre menú de configuración:
```
┌─────────────────────────────┐
│ Marks to Label: [Automatic] │
│                              │
│ Font: [Tableau Book] [10] ▼ │
│                              │
│ Alignment: [■■■]             │
│                              │
│ [Allow labels to overlap]   │
└──────────────────────────────┘
```

11. **Ajusta como prefieras**:
    - Font size: 9 o 10
    - Alignment: "Center" o "Right"
    - **IMPORTANTE**: NO marques "Allow labels to overlap" (se verán amontonadas)

12. **Click fuera** para cerrar

✅ **Resultado**: Ahora tienes etiquetas solo en valores importantes

---

## 📊 PARTE B: FORMATEAR UNIDADES - SEGUNDA LÍNEA (ÁREA)

Ahora vamos con el segundo eje (Unidades).

---

### ✅ **PASO 5.6: Cambiar a ÁREA (Area Chart)**

1. **Localiza el Marks Card que dice**: **"SUM(Unidades)"**
   - Está DEBAJO del Marks Card de Total (MXN)

2. **Click en el dropdown de tipo de marca**
   - El que tiene iconos (puede decir "Automatic" o "Line")

3. **Selecciona: "Area"**
   - Tiene un ícono que parece una montaña rellena

✅ **Resultado**: La línea de Unidades ahora es un **área rellena**

---

### ✅ **PASO 5.7: Cambiar Color a Naranja con Transparencia**

#### **Aplicar Color Naranja**

1. En el Marks Card de **SUM(Unidades)**

2. **Click en "Color"**

3. **Click en "More Colors..."**

4. **Ingresa el código hexadecimal**: `ff7f0e`
   - O en RGB:
     - R: 255
     - G: 127
     - B: 14

5. **Click "OK"** (pero NO cierres el menú de Color todavía)

#### **Aplicar Transparencia (50%)**

6. El menú de Color debe seguir abierto, ahora verás:
```
┌─────────────────────────────┐
│ Color                       │
│ ┌─────────────────────────┐ │
│ │ [Naranja]               │ │
│ └─────────────────────────┘ │
│                             │
│ Opacity: [●------○]    100% │ ← AQUÍ
│                             │
└─────────────────────────────┘
```

7. **Busca el slider que dice "Opacity"**

8. **Arrastra el slider hacia la IZQUIERDA** hasta que diga **50%**
   - O escribe directamente: `50` si hay un campo de texto

9. **Observa la gráfica**: El área naranja ahora es **semi-transparente**

10. **Click fuera** para cerrar el menú

✅ **Resultado**: Ahora tienes un **área naranja transparente** que permite ver la línea azul detrás

---

## 📈 PARTE C: AGREGAR LÍNEA DE REFERENCIA (PROMEDIO)

Esta línea muestra el promedio de ventas como referencia visual.

---

### ✅ **PASO 5.8: Agregar Reference Line**

#### **Abrir el Panel de Analytics**

1. **Mira en la parte izquierda de la pantalla**, arriba del Data pane

2. Verás **dos pestañas**:
```
┌──────────┬───────────┐
│   Data   │ Analytics │ ← Click AQUÍ
└──────────┴───────────┘
```

3. **Click en la pestaña "Analytics"**

#### **Ventana de Analytics**

Se abre un panel con opciones como:
- Summarize
  - Constant Line
  - Average Line
  - Median Line
  - etc.
- Model
- Custom

#### **Agregar Average Line**

4. **Busca la sección "Summarize"** (primera sección)

5. **Arrastra "Average Line"** (o "Reference Line")
   - Click y MANTÉN presionado el botón del mouse
   - Arrastra hacia tu gráfica

6. Cuando arrastres, verás opciones:
```
Table    Pane    Cell
```

7. **Suelta en "Pane"** (opción del medio)
   - Esto crea una línea para todo el panel

#### **Ventana "Edit Reference Line"**

Se abre una ventana de configuración:

```
┌─────────────────────────────────┐
│ Edit Reference Line             │
├─────────────────────────────────┤
│ Line                            │
│ ┌─────────────────────────────┐ │
│ Value: [Average] ▼              │
│                                 │
│ Label: [Computation] ▼          │
│                                 │
│ Formatting                      │
│ Line: [──────] ▼                │
│ Fill: [None] ▼                  │
│                                 │
│ [OK]  [Cancel]                  │
└─────────────────────────────────┘
```

8. **Configurar como sigue**:

   **Value**: 
   - Asegúrate que esté seleccionado: **"Average"**
   - Y que esté calculando: **"Total (MXN)"** (debería ser automático)

   **Label**: 
   - Dropdown: Selecciona **"Value"** o **"Computation"**
   - O escribe custom: `Promedio: <Value>`

   **Formatting - Line**:
   - Click en el dropdown que muestra el estilo de línea
   - Selecciona: **Línea punteada** (dotted o dashed)
   - Color: Click en el cuadro de color → Selecciona **ROJO**

   **Formatting - Fill**:
   - Déjalo en: **"None"**

9. **Click "OK"**

✅ **Resultado**: Ahora tienes una **línea punteada roja** que cruza horizontalmente mostrando el promedio

---

## 🎨 CÓMO SE VE TU GRÁFICA AHORA

Después de todos estos pasos, deberías ver:

```
Ventas a lo largo del tiempo
┌────────────────────────────────────────┐
│                                        │
│      ╱╲  ╱╲                           │ ← Línea azul (Total MXN)
│     ╱  ╲╱  ╲    ╱╲                    │
│ - - - - - - - - - - - - - - - - - -  │ ← Línea roja punteada (promedio)
│    ╱            ╲╱                     │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓               │ ← Área naranja transparente (Unidades)
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓            │
└────────────────────────────────────────┘
  Nov 1    Nov 15    Dic 1    Dic 15
```

**Características**:
- ✅ Línea azul oscura y gruesa (Total MXN)
- ✅ Área naranja semi-transparente (Unidades)
- ✅ Línea roja punteada horizontal (Promedio)
- ✅ Etiquetas en valores > $1000 (opcional)

---

## 🚨 TROUBLESHOOTING - Problemas Comunes

### ❌ "No veo dos Marks Cards"

**Problema**: Solo ves un Marks Card
**Solución**: 
- Asegúrate de haber completado el Paso 4 (crear dual-axis)
- Click derecho en el segundo eje (derecha) → "Dual Axis"

---

### ❌ "No encuentro 'More Colors'"

**Problema**: No veo la opción More Colors en el menú de Color
**Solución**:
- El menú puede verse diferente según la versión de Tableau
- Busca un botón que diga "Edit Colors..." o un cuadro de color clickeable
- O busca "Custom Color" o "Advanced"

---

### ❌ "El área naranja tapa completamente la línea azul"

**Problema**: No puedo ver la línea azul detrás del área
**Solución**:
- Aumenta la transparencia (Opacity) del área naranja a 40% o 30%
- O en el Marks Card de Unidades → Click en "Color" → Ajusta Opacity slider más a la izquierda

---

### ❌ "La línea de referencia no aparece"

**Problema**: Arrastré Average Line pero no pasa nada
**Solución**:
- Asegúrate de soltar en "Pane" (no en Table o Cell)
- Verifica que estés en la pestaña "Analytics" (no "Data")
- Si aparece pero no la ves, puede estar fuera del rango visible: ajusta el eje Y

---

### ❌ "Las etiquetas se ven amontonadas"

**Problema**: Muchas etiquetas se superponen
**Solución**:
- En Label options → NO marques "Allow labels to overlap"
- Aumenta el valor del filtro en la fórmula (cambiar 1000 a 2000 o 5000)
- O reduce el tamaño de fuente de las etiquetas

---

### ❌ "Los colores no son exactamente como los especificaste"

**Problema**: El azul o naranja se ven diferentes
**Solución**:
- Verifica que ingresaste correctamente el código hex (sin espacios)
- `1f77b4` para azul
- `ff7f0e` para naranja
- Tableau a veces ajusta colores si hay un tema aplicado: Format → Workbook → Theme → "Default"

---

## 📝 CHECKLIST RÁPIDO - PASO 5 COMPLETADO

Marca cada uno cuando lo completes:

- [ ] Marks Card de Total (MXN) - Tipo: Line
- [ ] Marks Card de Total (MXN) - Color: Azul oscuro (#1f77b4)
- [ ] Marks Card de Total (MXN) - Tamaño: Medium-Large
- [ ] Marks Card de Total (MXN) - Etiquetas condicionales (opcional)
- [ ] Marks Card de Unidades - Tipo: Area
- [ ] Marks Card de Unidades - Color: Naranja (#ff7f0e)
- [ ] Marks Card de Unidades - Opacity: 50%
- [ ] Reference Line agregada (línea punteada roja de promedio)

---

## ➡️ SIGUIENTE PASO

Una vez que completes el Paso 5, continúa con:

**Paso 6**: Añadir líneas de referencia adicionales (ya hiciste una!)
**Paso 7**: Tooltip enriquecido
**Paso 8**: Formato del eje

**¿Necesitas ayuda con alguno de estos pasos?** 
- Avísame cuando llegues al siguiente paso donde te atores
- O si algo de este Paso 5 no funciona, dime exactamente qué mensaje de error ves o qué no encuentras

---

## 💡 TIPS PROFESIONALES

1. **Orden de Layers**: Si el área naranja tapa la línea azul:
   - Puedes reordenar: en Rows, arrastra para cambiar el orden de SUM(Total) y SUM(Unidades)

2. **Color Consistency**: Guarda este azul y naranja para usar en otras visualizaciones
   - Crea una paleta personalizada para consistencia

3. **Preview mientras trabajas**: La gráfica se actualiza en tiempo real
   - No necesitas "aplicar" cambios, solo ajusta y observa

4. **Deshacer**: Si algo sale mal
   - Ctrl + Z (Windows) o Cmd + Z (Mac) para deshacer
   - Puedes deshacer varios pasos

---

**¡Éxito con tu formateo! 🎨**

*Cuando termines este paso o si te atoras en algo específico, avísame y te guío en el siguiente paso.*

