El `_quarto.yml` es un fichero YAML: claves y valores organizados por sangría. En un proyecto tipo libro tiene cuatro bloques. Este ejemplo los incluye todos, ajustado a un temario de DWES:

```yaml
# 1. PROYECTO: qué tipo de proyecto es y dónde se genera
project:
  type: book
  output-dir: _book

# 2. LIBRO: metadatos y estructura del temario
book:
  title: "Desarrollo Web en Entorno Servidor"
  subtitle: "2º DAW · Curso 2026/27"
  author: "Manuel Navas Damas"
  date: last-modified
  repo-url: https://github.com/TUUSUARIO/dwes-apuntes
  downloads: [pdf]
  chapters:
    - index.qmd
    - part: "Fundamentos de PHP"
      chapters:
        - temas/t01-sintaxis.qmd
        - temas/t02-formularios.qmd
        - temas/t03-pdo-mysql.qmd
    - part: "Laravel"
      chapters:
        - temas/t05-rutas-vistas.qmd
        - temas/t06-eloquent.qmd
  appendices:
    - anexos/a1-entorno.qmd
    - anexos/a2-git.qmd

# 3. FILTROS Y OPCIONES GENERALES
filters:
  - include-code-files
lang: es

# 4. FORMATOS DE SALIDA: opciones de la web y del PDF
format:
  html:
    theme: cosmo
    toc: true
    code-copy: true
  pdf:
    documentclass: scrreprt
    papersize: a4
```

## 1. `project`

Lo configuras una vez y no lo vuelves a tocar. `type: book` activa el modo libro (capítulos numerados, navegación anterior/siguiente, PDF único) y `output-dir` indica dónde se genera el resultado.

## 2. `book`: el temario

Es la parte que irás modificando a lo largo del curso.

- **`title`, `subtitle`, `author`, `date`** son los datos de la portada. `last-modified` pone automáticamente la fecha del último cambio.
- **`repo-url`** añade en la web un enlace al repositorio.
- **`downloads: [pdf]`** añade en la web un botón para descargar el PDF. Viene bien para el alumnado.
- **`chapters`** define la estructura del libro. Hay tres reglas:
  - El primero tiene que ser `index.qmd`, que hace de portada o presentación.
  - **El orden de la lista es el orden del libro**. No influye el nombre del fichero.
  - Un `.qmd` que no esté en la lista no aparece en el libro.
- **`part`** agrupa temas en bloques ("Fundamentos de PHP", "Laravel"). En la web se ven como secciones del menú y en el PDF como páginas de parte.
- **`appendices`** son capítulos que se numeran con letras (A, B…). Úsalos para la instalación del entorno, la chuleta de Git o la referencia de Artisan.

**El título de cada tema no se escribe aquí**: Quarto lo toma del primer `# Título` de cada `.qmd`. Para un capítulo sin número (presentación, prólogo), añade `{.unnumbered}` al título:

```markdown
# Presentación {.unnumbered}
```

## 3. Filtros y opciones generales

- **`filters`** lista las extensiones activas. Añadirás una línea aquí cada vez que instales otra extensión.
- **`lang: es`** traduce las etiquetas automáticas ("Capítulo", "Figura", "Tabla de contenidos").

## 4. `format`

Tiene un subbloque por cada salida, y cada uno tiene sus propias opciones.

- **`html`**: `theme` es el aspecto (puedes probar `cosmo`, `flatly`, `litera`…), `toc` muestra el índice lateral del tema y `code-copy` añade el botón de copiar en los bloques de código.
- **`pdf`**: `documentclass` es la clase LaTeX (`scrreprt` es la de KOMA-Script para informes) y `papersize` el tamaño de página. Aquí puedes añadir más opciones LaTeX cuando las necesites: `geometry`, `include-in-header`, `fontsize`…

## Errores frecuentes con YAML

- **La sangría se hace con espacios, nunca con tabuladores.** Usa siempre 2 espacios por nivel.
- **Los guiones (`-`) marcan elementos de una lista** y tienen que ir alineados entre sí.
- **Pon entre comillas los títulos con caracteres especiales** (`:`, `#`, `·`).
- **Las rutas son relativas a la raíz del proyecto**, la carpeta donde está `_quarto.yml`.
- Cuando Quarto da un error de YAML suele indicar la línea. Casi siempre es un problema de sangría.

## Flujo para añadir un tema

1. Crea `temas/t04-sesiones.qmd` empezando por `# Sesiones y cookies`.
2. Añade `- temas/t04-sesiones.qmd` en la posición que le toque dentro de `chapters`.
3. Comprueba el resultado con `quarto preview`.

Si me pasas el índice de tu temario, te devuelvo el bloque `book` ya escrito con las partes y los temas.