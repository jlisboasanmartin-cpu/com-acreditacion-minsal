# Handoff: Portal de Capacitación COM Macul

## Overview

Portal de capacitación interna para el **Centro Odontológico de Macul (COM)**, dependiente de la Corporación Municipal de Macul. Prepara al personal para una visita de acreditación MINSAL (Protocolo REG 1.1 — Versión 3).

El portal tiene 4 vistas:
1. **Inicio** (`index.html`) — Selección de perfil
2. **Perfil** (`perfil.html`) — Actividades del perfil seleccionado
3. **Cápsula** (`capsula.html`) — Presentación educativa de 8 láminas
4. **Cuestionario** (`cuestionario.html`) — 8 preguntas aleatorias con feedback

---

## Sobre los archivos de diseño

Los archivos HTML en este paquete son **prototipos de diseño de alta fidelidad** (hifi), no código de producción. La tarea es **recrear estas interfaces en el entorno tecnológico elegido** (React, Vue, Next.js, etc.) usando sus patrones y librerías establecidas.

**Fidelidad: Alta (hifi)** — Los prototipos son pixel-perfect con colores finales, tipografía, espaciado e interacciones. El desarrollador debe recrear la UI fielmente.

---

## Tecnología actual de los prototipos

- HTML/CSS/JS puro (sin framework)
- Google Fonts: Inter + Quicksand
- Sin dependencias externas — todo vanilla JS
- Estado persistido en `localStorage`
- Datos centralizados en `com-data.js`

---

## Arquitectura de datos

**`com-data.js`** es el único punto de verdad. Contiene:

```js
COM.perfiles[]        // 8 perfiles con id, nombre, color, icon, etc.
COM.capsulas{}        // Cápsulas por perfil (8 láminas cada una)
COM.preguntas{}       // Pool de 12 preguntas por perfil
COM.getPerfilById()   // Helper
COM.getPreguntasAleatorias() // Helper — devuelve N preguntas aleatorias del pool
```

La fecha de acreditación está **hardcodeada** en `index.html` y `perfil.html` como `new Date('2026-06-02')`. **Debe extraerse a `com-data.js`** como variable global:
```js
COM.fechaAcreditacion = '2026-06-02';
```

---

## Tokens de diseño

```css
--primary:       #1a6b8a   /* Azul principal */
--primary-dark:  #0e4a61   /* Azul oscuro — headers, footers */
--primary-light: #d0e8f2   /* Azul muy claro — fondos de íconos */
--accent:        #2ecc71   /* Verde — completado, correcto */
--accent-dark:   #27ae60
--bg:            #f0f4f7   /* Fondo general */
--bg-white:      #ffffff
--border:        #dce8ef
--surface:       #e8f0f5   /* Fondos de pills, separadores */
--text:          #1a2b35
--text-mid:      #3d5a6b
--text-muted:    #6b8a9a
--radius:        12px
--radius-lg:     18px
--radius-xl:     24px
--shadow:        0 4px 16px rgba(14,74,97,0.10)
--font-body:     'Inter', system-ui, sans-serif
--font-display:  'Quicksand', 'Inter', sans-serif
```

Cada perfil tiene su propio color y color claro:
| Perfil | Color | ColorLight |
|--------|-------|-----------|
| Cirujano Dentista | #1a6b8a | #e8f4f8 |
| TONS | #2980b9 | #eaf3fb |
| Higienista Dental | #16a085 | #e8f8f5 |
| Administrativo SOME/OIRS | #8e44ad | #f5eefb |
| Operador de Radiografía | #d35400 | #fdf0e8 |
| Encargado de Calidad | #27ae60 | #eafaf1 |
| Referente SIGGES | #c0392b | #fdf0ef |
| Dirección | #0e4a61 | #e6eef2 |

---

## Assets

Todos los assets están en la carpeta `assets/`:

| Archivo | Uso |
|---------|-----|
| `corpo-macul-blanco.svg` | Logo institucional (usar con `filter: brightness(0) invert(1)` sobre fondos oscuros) |
| `muela_saludo.png` | Dr. Com saludando — portadas, inicio |
| `muela_atencion.png` | Dr. Com señalando — láminas informativas, cuestionario intro |
| `muela_formulario.png` | Dr. Com con formulario — láminas de procesos |
| `muela_computador.png` | Dr. Com con laptop — láminas digitales, cápsula |
| `muela_lupa.png` | Dr. Com con lupa — láminas de búsqueda/revisión |
| `muela_union.png` | Dr. Com sonriente — láminas de unificación |
| `muela_radiografia.png` | Dr. Com con Rx — perfil radiografía |
| `muela_animo.png` | Dr. Com thumbs up — resultados medios |
| `muela_victoria.png` | Dr. Com celebrando — resultados excelentes, cierre |

---

## Pantallas

### 1. Inicio (`index.html`)

**Layout:** `flex-column`, `height: 100vh`, `overflow: hidden`
- Header: `56px`, `background: #0e4a61`, logo blanco + título
- Intro band: `~130px`, gradiente `#0e4a61 → #1a6b8a`, título + stats + Dr. Com
- Grid de perfiles: `4 columnas × 2 filas`, `gap: 16px`, `flex: 1`
- Footer: `40px`, `background: #0e4a61`, logo + texto + badge versión

**Cards de perfil:**
- `border-radius: 14px`, `border: 1.5px solid #dce8ef`
- Barra de color superior: `4px height`, color del perfil
- Ícono: `40×40px`, `border-radius: 11px`, fondo `colorLight`, color `perfil.color`
- Nombre: Quicksand 15px bold, `min-height: 40px`
- Footer card: tiempo + preguntas + flecha animada (aparece en hover)
- Estado completado: `border-color: #abebc6`, badge verde "Completado"
- Hover: `translateY(-4px)`, sombra pronunciada

**Estado desde localStorage:**
- `progress_{perfilId}` → porcentaje (0-100)
- `quiz_done_{perfilId}` → '1' si completó el cuestionario

---

### 2. Perfil (`perfil.html?id={perfilId}`)

**Layout:** `flex-column`, `height: 100vh`, `overflow: hidden`
- Header: igual al index
- Body: `grid-template-columns: 260px 1fr`, `flex: 1`, `min-height: 0`
- Footer: `44px`, institucional, ancho completo

**Panel izquierdo (260px):**
- Ícono del perfil: `60×60px`, `border-radius: 16px`
- Nombre: Quicksand 22px bold
- Descripción: Inter 13px, `color: #6b8a9a`
- Badges: "PROTOCOLO REG 1.1" + "ACREDITACIÓN MINSAL"
- Barra de progreso: `8px height`, color del perfil
- Badge "Completado": aparece cuando `displayPct >= 100`
- Dr. Com: `flex: 1`, mascota al fondo, `width: 210px`
- La mascota cambia a `muela_victoria.png` cuando está completado

**Panel derecho:**
- Grid: `2 columnas × 2 filas`, `1fr 1fr` (tarjetas se estiran)
- Cápsula: `grid-column: 1 / -1` (fila completa), con Dr. Com al costado derecho
- Cuestionario + Guía: segunda fila, mitad cada uno

**Activity cards:**
- Título: Quicksand 20px bold
- Descripción: Inter 14px
- Ícono: `48×48px`, `border-radius: 13px`
- Estado: pill "Pendiente" / "Lámina X/8" / "✓ Completada"
- Done state: `border-color: #abebc6`
- Flecha: aparece en hover, color del perfil

---

### 3. Cápsula (`capsula.html?id={perfilId}`)

**Layout:** scroll permitido, nav fijo en `bottom: 44px`, footer institucional fijo en `bottom: 0`

**Header:** `60px`, `background: #0e4a61`, logo blanco + contexto + botón volver

**Slide card:** `border-radius: 20px`, `grid-template-columns: 1fr 300px`
- **Panel izquierdo (contenido):** padding `48px 52px 40px`
- **Panel derecho (mascota):** gradiente `var(--bg) → #eef6fb`, burbuja de diálogo + imagen Dr. Com `220px`

**Tipos de lámina:**
1. `portada` — badge versión + badge perfil + título + índice de temas en grid 2 col
2. `contenido` — título + intro + lista de pasos numerados + callout de alerta
3. `cierre` — igual a contenido + CTA "Ir al cuestionario"

**Pasos numerados:**
- Número: círculo `30×30px`, `background: color del perfil`, texto blanco 13px bold
- Texto: 15px, `border-bottom: 1px solid var(--surface)`
- Keyword destacada: `<strong>`, color `var(--text)`

**Callouts:**
- Warning: `background: #fffbea`, `border-left: 3px solid #f0c040`
- Info: `background: var(--primary-light)`, `border-left: 3px solid var(--primary)`

**Nav de láminas:** fijo `bottom: 44px`, blanco, dots + Anterior/Siguiente
- Dot activo: color del perfil, `width: 22px` (pill)
- Dot completado: verde `#2ecc71`, opacidad 0.4
- Navegación por teclado: ← →

**Mascota por lámina (perfil dentista):**
| Lámina | Mascota |
|--------|---------|
| 0 portada | muela_saludo |
| 1 REG 1.1 | muela_atencion |
| 2-3 formularios | muela_formulario |
| 4 unificación | muela_union |
| 5 digital | muela_computador |
| 6 RISPAC | muela_lupa |
| 7 cierre | muela_victoria |

**Estado guardado:** `capsule_pos_{perfilId}` en localStorage

---

### 4. Cuestionario (`cuestionario.html?id={perfilId}`)

**Layout:** `flex-column`, `height: 100vh`

**Pantalla Intro:** 2 columnas
- Izquierda: eyebrow + título + stats (8/pool/shuffle) + callout info + botón
- Derecha: gradiente azul, Dr. Com con burbuja de diálogo

**Pantalla Pregunta:** card con:
- Header: contador + tipo + scores correcto/incorrecto
- Progress bar: `5px`, color del perfil
- Body: pregunta izquierda + mascota derecha (`200px`)
- 4 opciones: `border-radius: 12px`, `border: 2px solid`
  - Hover: border azul, fondo azul claro, `translateX(3px)`
  - Correcto: fondo verde, animación bounce
  - Incorrecto: fondo rojo, animación shake
  - Dimmed: `opacity: 0.32`
- Feedback box: aparece tras responder, verde/rojo con explicación
- Botón "Siguiente" aparece tras responder

**Mascota durante preguntas:** rota entre `muela_atencion`, `muela_formulario`, `muela_lupa`, `muela_computador`
- Tras respuesta: cambia a `muela_animo` (correcto) o `muela_atencion` (incorrecto)

**Pantalla Resultados:** `flex-column`, ocupa toda la altura
- Hero oscuro: mascota 72px + título + score ring `80px`
  - Score ≥80%: color verde, mascota `muela_victoria`
  - Score ≥60%: color del perfil, mascota `muela_animo`
  - Score <60%: color rojo, mascota `muela_animo`
- Barra de stats: correctas + incorrectas + total + botones de acción
- Review scrollable: lista de preguntas con estado ok/error + explicaciones

**Estado guardado:** `quiz_done_{perfilId}` = '1' en localStorage al completar

---

## Flujo de navegación

```
index.html
  └─► perfil.html?id={perfilId}
        ├─► capsula.html?id={perfilId}  →  cuestionario.html?id={perfilId}
        ├─► cuestionario.html?id={perfilId}
        └─► [modal guía de estudio — solo UI, sin backend real]
```

---

## Pendientes para producción

1. **Extraer fecha de acreditación** a `com-data.js`:
   ```js
   COM.fechaAcreditacion = '2026-06-02';
   ```

2. **Actualizar subtítulo del perfil TONS** en `com-data.js` (actualmente dice "Técnico en Odontología...")

3. **Responsive de la cápsula** — agregar media query `@768px` que apile las columnas verticalmente

4. **Backend para Guía de Estudio** — el modal existe pero no envía email real

5. **Manejo de error de perfilId inválido** — mejorar el mensaje antes del redirect

6. **Persistencia del mejor puntaje** — guardar en localStorage y mostrar en perfil

7. **Modo impresión** — `@media print` para la revisión del cuestionario

---

## Capturas de pantalla

| Pantalla | Archivo |
|----------|---------|
| Inicio — grilla de perfiles | `screenshots/01-inicio.png` |
| Perfil — Cirujano Dentista | `screenshots/02-perfil.png` |
| Cápsula — Lámina 1 (portada) | `screenshots/03-capsula-lamina1.png` |
| Cápsula — Lámina 2 (contenido) | `screenshots/04-capsula-lamina2.png` |
| Cuestionario — Pantalla intro | `screenshots/05-cuestionario-intro.png` |
| Cuestionario — Pregunta activa | `screenshots/06-cuestionario-pregunta.png` |

---

## Archivos incluidos

| Archivo | Descripción |
|---------|-------------|
| `index.html` | Pantalla de inicio con 8 tarjetas de perfil |
| `perfil.html` | Página de perfil con actividades |
| `capsula.html` | Visor de cápsula educativa (8 láminas) |
| `cuestionario.html` | Cuestionario de práctica (8 preguntas aleatorias) |
| `com-styles.css` | Variables CSS y estilos base compartidos |
| `com-data.js` | Todos los datos: perfiles, cápsulas, preguntas |
| `assets/` | Logo SVG + 10 variantes de Dr. Com (PNG transparente) |
