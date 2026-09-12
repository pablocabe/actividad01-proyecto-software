# DOMAIN.md — Cocina Cabe

Documentación del dominio del proyecto: el prompt original que originó el sitio
y las decisiones tomadas para implementarlo.

---

## 1. Prompt original

>
> Desarrolla un sitio web estático (sin scripts) para publicar recetas de cocina. Las recetas están divididas en dos categorías: salado y dulce. Para esto, hay que contemplar las siguientes condiciones:
>
> 1. Se debe mostrar una barra de navegación con enlaces a:
>     - Información de contacto con un formulario simple.
>     - Información de quien mantiene el sitio (Pablo Cabe).
>     - Catálogo de recetas agrupadas en las dos categorías mencionadas.
> 2. Colores y estilo:
>
>     **Ancla:** Premier League Purple `#37003C` como acento, no como atmósfera completa.
>
>     **Modo claro:**
>
>     - **Fondo:** `#F9F8F6`
>     - **Superficie:** `#FFFFFF`
>     - **Texto:** `#140F16`
>     - **Texto secundario:** `#5A5260`
>     - **Borde general:** `#DDD8E0`
>     - **Borde de tarjetas de receta:** `#37003C`
>     - **Acento:** `#37003C`
>     - **Hover:** `#4D1A55`
>     - **Texto sobre acento:** `#FFFFFF`
>     - **Error:** `#B42318`
>     - **Colores secundarios para categorías:**
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
version-grok/
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

| Archivo   | Receta                           | Categoría |
| --------- | -------------------------------- | --------- |
| `receta1` | Locro de maíz                    | Salada    |
| `receta2` | Matambre a la pizza              | Salada    |
| `receta3` | Guiso de lentejas                | Salada    |
| `receta4` | Chocotorta                       | Dulce     |
| `receta5` | Pastelitos criollos              | Dulce     |
| `receta6` | Panqueques con dulce de leche    | Dulce     |

Cada receta tiene dos archivos asociados con el mismo nombre base: `imagenes/recetaN.png`
(fotografía representativa) y `recetarios/recetaN.pdf` (recetario con ingredientes,
preparación y notas de cocina).

## 4. Decisiones de implementación

### Navegación

La barra es la misma en las tres páginas y enlaza al catálogo (`index.html`), a la
información de quien mantiene el sitio (`sobre-mi.html`) y al contacto (`contacto.html`).
La página activa se marca con `aria-current="page"`.

### Radio Button Hack

El filtrado no usa JavaScript. En `index.html`, dentro de `<section class="catalogo">`,
conviven como hermanos tres `<input type="radio" name="categoria">` ocultos visualmente,
la lista de etiquetas y las dos secciones de recetas:

```
section.catalogo
├── input#filtro-todas   (checked por defecto)
├── input#filtro-salado
├── input#filtro-dulce
├── ul.filtros           (labels con for="filtro-…")
├── section#grupo-salado
└── section#grupo-dulce
```

Las reglas de `estilosIA.css` combinan `:checked` con el combinador de hermanos `~`:

```css
.grupo { display: none; }

#filtro-todas:checked  ~ .grupo,
#filtro-salado:checked ~ #grupo-salado,
#filtro-dulce:checked  ~ #grupo-dulce { display: block; }
```

Los radios se ocultan con la técnica *visually hidden* (`clip-path: inset(50%)`) y no con
`display: none`, para que sigan siendo enfocables con el teclado y anunciados por lectores
de pantalla. El foco se traslada del input a su etiqueta visible con
`#filtro-x:focus-visible ~ .filtros .chip[for="filtro-x"]`.

### Paleta y tipografía

Los colores del prompt están declarados como variables CSS en `:root`. El violeta
`#37003C` se usa como acento (marca, navegación activa, bordes de ficha, botones y pie)
sobre fondos crema, sin teñir toda la interfaz. No hay modo oscuro ni `data-theme`.

Las tipografías se cargan desde Google Fonts: **Outfit** para marca y títulos,
**Source Sans 3** para interfaz y cuerpo, con familias de reserva del sistema.

### Tarjetas de receta

Cada receta es un `<article class="ficha">` con borde de 2 px en `#37003C`, radio de
0.7 rem y relleno de 1.1 rem. Incluye categoría, nombre, fotografía con `alt`
descriptivo, texto breve, porciones/tiempo y el enlace de descarga del PDF con
`download`. Las fichas se acomodan en una grilla `repeat(auto-fill, minmax(17rem, 1fr))`.

### Responsividad

Diseño *mobile first*:

- hasta 30 rem: una columna, filtros apilados y menú en lista vertical;
- hasta 48 rem: cabecera en columna y navegación en tres columnas;
- desde 60 rem: páginas internas en dos columnas (`1.7fr` + `1fr`).

Los títulos usan `clamp()` para escalar de forma fluida.

### Accesibilidad (WAI/WCAG 2.1 AA)

- Enlace *saltar al contenido principal* como primer elemento enfocable.
- Encabezados jerárquicos, sin saltos de nivel.
- `lang="es"` y textos alternativos descriptivos en todas las imágenes.
- Foco visible con `:focus-visible` (contorno de 3 px).
- Formulario con `<label>` asociado, `<fieldset>` / `<legend>`, `autocomplete`,
  `required` y campos obligatorios señalados con texto.
- Contraste: texto `#140F16` sobre `#F9F8F6` supera 15:1; blanco sobre `#37003C`
  supera 14:1.
- Se respeta `prefers-reduced-motion` y `forced-colors`.
- Los grupos ocultos por el filtro usan `display: none`, de modo que quedan fuera
  del árbol de accesibilidad.

### Restricciones cumplidas

Sin JavaScript, sin frameworks CSS y sin lenguajes de servidor en el sitio. Todas
las reglas de estilo están en `estilosIA.css`; no hay atributos `style` ni bloques
`<style>` en el HTML.
