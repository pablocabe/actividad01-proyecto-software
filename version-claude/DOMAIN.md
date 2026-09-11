# DOMAIN.md — Recetas de Casa

Documentación del dominio del proyecto: el prompt original que originó el sitio y las
decisiones tomadas para implementarlo.

---

## 1. Prompt original

>
> Desarrolla un sitio web estático (sin scripts) para publicar recetas de cocina. Las recetas están divididas en dos categorías: salado y dulce. Para esto, hay que contemplar las siguientes condiciones:
>
> 1. Se debe mostrar una barra de navegación con enlaces a:
>     - Información de contacto con un formulario simple.
>     - Información de quien mantiene el sitio (Pablo Cabe).
>     - Catálogo de recetas agrupadas en las dos categorías mencionadas.
> 2. Colores y estislo:
>     **Ancla:** Premier League Purple `#37003C` como acento, no como atmósfera completa.
>
>     **Modo claro:**
>
>     - **Fondo:** `#F9F8F6` (crema cálido, reemplaza el gris frío `#F4F3F5`)
>     - **Superficie:** `#FFFFFF`
>     - **Texto:** `#140F16`
>     - **Texto secundario (opcional):** `#5A5260`
>     - **Borde general:** `#DDD8E0`
>     - **Borde de tarjetas de receta:** `#37003C` (2px, para cumplir el requisito de borde armónico del enunciado)
>     - **Acento:** `#37003C`
>     - **Hover:** `#4D1A55`
>     - **Texto sobre acento:** `#FFFFFF`
>     - **Error:** `#B42318`
>     - **Colores secundarios para categorías (opcional):**
>         - Salado: `#4A5D23` (verde oliva)
>         - Dulce: `#A8576B` (rosa empolvado)
>
>     **Sin modo oscuro. Sin toggle `data-theme`.**
>
>     Un solo acento de marca. Fondos neutros cálidos (sin lavanda de fondo).
> 3. Con respecto a la tipografía, utiliza Outfit en marca / titulos, y Source Sans 3 en UI / cuerpo.
> 4. Respecto a la interacción con el sitio, se debe poder elegir una categoría (lista desplegable o radio buttons) y, al seleccionarla, mostrar las recetas correspondientes en la zona central de la página. Siempre de forma estática, sin Javascript. Utiliza el Radio Button Hack (usando inputs ocultos, la pseudoclase `:checked` y el combinador `~`)
> 5. Se debe mostrar por cada receta un recuadro con un borde y relleno específicos y visibles de manera armónica. Debe incluir el nombre de la receta, una foto y un enlace para descargar la receta completa en PDF.
> 6. El sitio debe ser responsivo.
> 7. Solo se debe usar HTML y CSS. No utilizar JavaScript ni ningún otro lenguaje. Tampoco utilizar frameworks como Bootstrap o Tailwind.
> 8. Estructura semántica: utiliza etiquetas HTML5 (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`). No utilizar <div> para todo.
> 9. Aplica los estándares del W3C (World Wide Web Consortium), especialmente con respecto a la accesibilidad (WAI/WCAG).
> 10. Las reglas de estilo deben estar en un archivo separado denominado estilosIA.css.
> 11. Se debe trabajar con al menos 3 recetas por cada categoría (3 saladas y 3 dulces) cuyos archivos estén organizados en: un directorio denominado imagenes (fotos en formato .png) y un directorio denominado recetarios (con PDFs explicativos). Por cada receta a mostrar en la página, hay dos archivos: uno denominado "imagenes/receta1.png" que corresponde a una imagen representativa y otro denominado "recetarios/receta1.pdf" con la receta.
> 12. Crea DOMAIN.md con este prompt completo para documentarlo.

---

## 2. Estructura del proyecto

```
version-claude/
├── index.html            Catálogo de recetas con filtro por categoría
├── contacto.html         Formulario de contacto y datos del responsable
├── sobre-mi.html         Información sobre Pablo Cabe y el sitio
├── estilosIA.css         Única hoja de estilos del sitio
├── DOMAIN.md             Este documento
├── imagenes/             Fotografías de las recetas (PNG, 800 × 600)
│   ├── receta1.png … receta6.png
└── recetarios/           Recetarios completos en PDF (A4)
    ├── receta1.pdf … receta6.pdf
```

## 3. Recetas publicadas

| Archivo    | Receta                                  | Categoría |
| ---------- | --------------------------------------- | --------- |
| `receta1`  | Empanadas de carne cortada a cuchillo   | Salada    |
| `receta2`  | Milanesa napolitana al horno            | Salada    |
| `receta3`  | Tarta de espinaca y ricota              | Salada    |
| `receta4`  | Alfajores de maicena                    | Dulce     |
| `receta5`  | Flan casero con dulce de leche          | Dulce     |
| `receta6`  | Budín de limón con glaseado             | Dulce     |

Cada receta tiene dos archivos asociados con el mismo nombre base: `imagenes/recetaN.png`
(fotografía representativa) y `recetarios/recetaN.pdf` (recetario completo con ingredientes,
preparación y notas de cocina).

## 4. Decisiones de implementación

### Navegación

La barra de navegación es idéntica en las tres páginas y enlaza al catálogo (`index.html`),
a la información del responsable (`sobre-mi.html`) y al contacto (`contacto.html`). La página
activa se marca con `aria-current="page"`, que además define su estilo destacado en CSS.

### Radio Button Hack

El filtrado por categoría no usa JavaScript. En `index.html`, dentro de `<section class="catalogo">`,
conviven como hermanos tres `<input type="radio" name="categoria">` ocultos visualmente, la lista de
etiquetas `<label>` que los controlan y las dos secciones de recetas:

```
section.catalogo
├── input#filtro-todas   (checked por defecto)
├── input#filtro-salado
├── input#filtro-dulce
├── ul.filtros           (labels con for="filtro-…")
├── section#grupo-salado
└── section#grupo-dulce
```

Las reglas relevantes de `estilosIA.css` combinan `:checked` con el combinador de hermanos `~`:

```css
.grupo { display: none; }

#filtro-todas:checked  ~ .grupo,
#filtro-salado:checked ~ #grupo-salado,
#filtro-dulce:checked  ~ #grupo-dulce { display: block; }
```

Los radios se ocultan con la técnica *visually hidden* (`clip-path: inset(50%)`) y no con
`display: none`, de modo que siguen siendo enfocables con el teclado y anunciados por los lectores
de pantalla. El indicador de foco se traslada del input a su etiqueta visible con
`#filtro-x:focus-visible ~ .filtros .filtro-etiqueta[for="filtro-x"]`.

### Paleta y tipografía

Todos los colores del prompt están declarados como variables CSS en `:root` y se usan
exclusivamente desde ahí. El violeta `#37003C` funciona como acento (navegación activa, botones,
bordes de tarjeta, pie de página) sobre fondos neutros cálidos, sin teñir la atmósfera general.
No hay modo oscuro ni conmutador de tema.

Las tipografías se cargan desde Google Fonts: **Outfit** para marca y títulos, **Source Sans 3**
para interfaz y cuerpo de texto, ambas con familias de reserva del sistema.

### Tarjetas de receta

Cada receta es un `<article>` con borde de 2 px en `#37003C`, radio de 10 px y relleno de 16 px.
Contiene el nombre, la fotografía con `alt` descriptivo y proporción fija 4:3, una descripción
breve, los datos de porciones y tiempo, y el enlace de descarga del PDF con atributo `download`.
Las tarjetas se distribuyen en una grilla `repeat(auto-fill, minmax(17rem, 1fr))`.

### Responsividad

Diseño *mobile first* con cuatro puntos de corte: hasta 30 rem (una columna, filtros apilados),
hasta 48 rem (cabecera en columna y navegación en tres columnas iguales a ancho completo), desde
60 rem (páginas internas en dos columnas) y desde 64 rem (holguras mayores). Los tamaños de título
usan `clamp()` para escalar de forma fluida.

### Distribución de las páginas internas

`sobre-mi.html` y `contacto.html` usan la clase `contenido--dos-columnas`, una grilla en la que la
portada ocupa el ancho completo y los paneles se reparten en una columna principal y otra lateral
más angosta (`minmax(0, 1.7fr) minmax(0, 1fr)`) a partir de 60 rem. Por debajo de ese ancho, la
grilla pasa a una sola columna y los paneles se apilan en el orden del documento. Dentro del
formulario, el
grupo de campos marcado con `campos--grilla` acomoda dos controles por fila cuando hay espacio,
mediante `repeat(auto-fit, minmax(15rem, 1fr))`.

### Accesibilidad (WAI/WCAG 2.1 AA)

- Enlace *saltar al contenido principal* como primer elemento enfocable de cada página.
- Estructura de encabezados jerárquica y sin saltos de nivel.
- `lang="es"` en el elemento raíz y textos alternativos descriptivos en todas las imágenes.
- Foco visible en todo elemento interactivo mediante `:focus-visible` con contorno de 3 px.
- Formulario con `<label>` asociado a cada control, agrupación en `<fieldset>` con `<legend>`,
  `autocomplete`, campos obligatorios señalados con texto y con el atributo `required`.
- Contraste verificado: texto `#140F16` sobre fondo `#F9F8F6` supera 15:1; blanco sobre acento
  `#37003C` supera 14:1.
- Se respeta `prefers-reduced-motion` y se contempla `forced-colors` (modo de alto contraste).
- Los grupos ocultos por el filtro usan `display: none`, con lo que quedan fuera del árbol de
  accesibilidad y no son leídos por las tecnologías de apoyo.

### Restricciones cumplidas

Sin JavaScript, sin frameworks CSS y sin lenguajes de servidor. La totalidad de las reglas de
estilo está en `estilosIA.css`; no hay atributos `style` ni bloques `<style>` en el HTML.
