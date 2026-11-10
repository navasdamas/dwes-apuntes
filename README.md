# Apuntes DWES

Estos pasos te dejan los dos repos creados, conectados y con un primer tema que se previsualiza y se publica. Los comandos son para Linux/macOS; en Windows usa Git Bash.

## 0. Requisitos

1. **Git** y una cuenta de **GitHub**.
2. **Quarto**: descárgalo de quarto.org e instálalo. Compruébalo con:
   ```bash
   quarto check
   ```
3. **VS Code** con la extensión **Quarto**.
4. **Para el PDF**: si ya tienes LaTeX, no hace falta nada más. Si no, instala TinyTeX:
   ```bash
   quarto install tinytex
   ```

## 1. Repo de ejemplos

Crea en GitHub un repo **público** vacío llamado `dwes-ejemplos`, sin README. Después, en local:

```bash
mkdir dwes-ejemplos && cd dwes-ejemplos
git init -b main
mkdir -p php/t01-sintaxis laravel proyecto
```

Añade un primer ejemplo, `php/t01-sintaxis/01-variables.php`:

```php
<?php
$nombre = "Alumno";
$edad = 20;
echo "Hola, $nombre. Tienes $edad años.\n";
```

Crea un `README.md` mínimo:

```markdown
# Ejemplos DWES

Versiones: PHP 8.3 · Laravel 12

- `php/`: ejemplos sin framework. Ejecutar con `php -S localhost:8000`
- `laravel/`: un proyecto Laravel por tema
- `proyecto/`: aplicación que evoluciona (ver etiquetas)
```

Ajusta las versiones a las que vayas a usar. Para terminar este paso, crea el `.gitignore`, haz el primer commit y súbelo:

```bash
printf "vendor/\n.env\nnode_modules/\n.DS_Store\n" > .gitignore
git add .
git commit -m "Estructura inicial"
git remote add origin https://github.com/TUUSUARIO/dwes-ejemplos.git
git push -u origin main
```

No crees ahora los proyectos Laravel. Cuando llegues al tema correspondiente, los generas con:

```bash
composer create-project laravel/laravel laravel/t05-rutas-vistas
```

## 2. Repo de apuntes

Crea en GitHub otro repo **público** vacío llamado `dwes-apuntes`. Si lo haces privado, GitHub Pages exige un plan de pago (puede cubrirlo GitHub Education para docentes).

```bash
cd ..
quarto create project book dwes-apuntes
cd dwes-apuntes
```

Quarto genera ficheros de muestra que no necesitas. Bórralos:

```bash
rm intro.qmd summary.qmd references.qmd references.bib
mkdir temas
```

## 3. Conectar los ejemplos como submódulo

```bash
git init -b main
git submodule add https://github.com/TUUSUARIO/dwes-ejemplos.git ejemplos
```

## 4. Extensión para incluir código

```bash
quarto add quarto-ext/include-code-files
```

Responde que sí a las preguntas. Se crea una carpeta `_extensions/`, que sí se sube al repo.

## 5. Configuración del libro

Sustituye el contenido de `_quarto.yml` por esto:

```yaml
project:
  type: book
  output-dir: _book

book:
  title: "Desarrollo Web en Entorno Servidor"
  author: "Manuel Navas Damas"
  chapters:
    - index.qmd
    - part: "PHP"
      chapters:
        - temas/t01-sintaxis.qmd

filters:
  - include-code-files

lang: es

format:
  html:
    theme: cosmo
    code-copy: true
    toc: true
  pdf:
    documentclass: scrreprt
```

`lang: es` pone en castellano las etiquetas automáticas ("Capítulo", "Tabla de contenidos"…).

Deja `index.qmd` como portada. Por ejemplo:

```markdown
# Presentación {.unnumbered}

Apuntes del módulo DWES (2º DAW).
```

## 6. Primer tema de prueba

Crea `temas/t01-sintaxis.qmd`:

````markdown
# Sintaxis básica de PHP

## Variables

Las variables empiezan por `$`:

```{.php include="../ejemplos/php/t01-sintaxis/01-variables.php"}
```

::: {.callout-tip}
Ejecuta el ejemplo con `php 01-variables.php`.
:::
````

La ruta de `include` es relativa al fichero `.qmd`, de ahí el `../` inicial. Si el código no aparece al renderizar, prueba sin `../`.

## 7. Comprobar que todo funciona

```bash
quarto preview      # abre el navegador y recarga al guardar
quarto render       # genera HTML y PDF en _book/
```

Abre `_book/` y comprueba que el código del ejemplo aparece en la web y en el PDF.

## 8. Primer commit de los apuntes

```bash
echo "_book/" >> .gitignore
git add .
git commit -m "Estructura inicial de los apuntes"
git remote add origin https://github.com/TUUSUARIO/dwes-apuntes.git
git push -u origin main
```

## 9. Publicar en GitHub Pages

```bash
quarto publish gh-pages
```

Este comando renderiza, crea la rama `gh-pages` y la sube. En GitHub, ve a **Settings → Pages** del repo y comprueba que la fuente es la rama `gh-pages`. La web quedará en `https://TUUSUARIO.github.io/dwes-apuntes/`, y ese es el enlace que pones en Moodle Centros.

## Chuleta para el día a día

| Tarea | Comando |
|---|---|
| Clonar los apuntes en otro ordenador | `git clone --recurse-submodules URL` |
| Traer los últimos cambios de los ejemplos | `git submodule update --remote ejemplos`, luego commit |
| Escribir con vista previa | `quarto preview` |
| Publicar una nueva versión | `quarto publish gh-pages` |
| Añadir un tema | crear `temas/tXX-….qmd` y añadirlo a `chapters` en `_quarto.yml` |

Un detalle sobre el submódulo: cuando cambies algo en `dwes-ejemplos`, primero haz commit y push allí. Después, en `dwes-apuntes`, ejecuta `git submodule update --remote ejemplos` y haz commit para que los apuntes apunten a la nueva versión.

Si quieres, adapto el `_quarto.yml` a tu temario real del módulo, con las partes y los temas ya definidos.