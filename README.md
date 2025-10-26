# Trabajo de Grado — Prototipo de Videojuego Serio Ferroviario (UIS)

Este repositorio contiene el informe en LaTeX (formato ICONTEC adaptado) y los recursos del proyecto de grado.

## 1. Estructura del repo

- `main.tex` → archivo principal
- `icontec_style.sty` → estilo ICONTEC (márgenes, portada, Arial, etc.)
- `chapters/` → capítulos (`01-introduccion.tex`, `02-...`)
- `figures/` → imágenes
- `fonts/` → fuentes Arial (`ARIAL.ttf`, `ARIALBD.ttf`, `ARIALI.ttf`, `ARIALBI.ttf`)
- `referencias.bib` → bibliografía
- `latexmkrc` → config de compilación
- `.vscode/settings.json` → configuración de compilación para VS Code + LaTeX Workshop

El PDF final se genera en `build/main.pdf`.

## 2. Requisitos

### 2.1 Windows + MiKTeX
Instalar:
1. **MiKTeX** (https://miktex.org/download)
   - Durante la instalación: marcar  
     - "Install missing packages on-the-fly" = Yes
     - Agregar MiKTeX al PATH (opción por defecto en instalador completo)
2. Abrir **MiKTeX Console** → pestaña *Packages* y asegurarse de tener (si no, instalarlos):
   - `babel-spanish`
   - `biblatex`
   - `biber`
   - `csquotes`
   - `fontspec`
   - `geometry`
   - `titlesec`
   - `fancyhdr`
   - `microtype`
   - `graphicx` (viene en `graphics`)
   - `booktabs`
   - `enumitem`
   - `float`
   - `caption`
   - `listings`
   - `hyperref`
   - `cleveref`
   - `xurl`
   - `newtx` (para las matemáticas)
   - `latexmk`

> Nota: muchos ya vienen o se instalan solos la primera vez que compiles.

3. Instalar **Strawberry Perl** (https://strawberryperl.com/)  
   Necesario para que `latexmk` funcione bien en Windows.

4. Reiniciar VS Code después de instalar Perl/MiKTeX para que el PATH se refresque.

### 2.2 VS Code
Instalar:
- VS Code
- Extensión **LaTeX Workshop** (`James Yu`)
- Extensión **LaTeX Languages Support** (opcional, solo para sintaxis)

El repo incluye `.vscode/settings.json`, que ya:
- fuerza el motor `lualatex`
- usa `latexmk`
- manda TODO lo generado a `build/`
- habilita SyncTeX para salto PDF↔código

## 3. Compilación

### Opción A: desde VS Code (recomendada)
1. Abrir esta carpeta en VS Code.
2. Asegurarse de que `fonts/` existe con las fuentes Arial `.ttf`.
3. Dar clic en **Build LaTeX project** (icono de TeX Workshop, normalmente arriba a la derecha o en la barra lateral).
4. El PDF aparece en `build/main.pdf`.  
   También se abre en el visor integrado.

### Opción B: terminal
Puedes compilar igual que VS Code usando `latexmk` con LuaLaTeX:

```powershell
latexmk
````

ó explícito:

```powershell
latexmk -lualatex main.tex
```

`latexmkrc` ya se encarga de:

* usar `lualatex` (requerido porque usamos `fontspec` y Arial TTF)
* usar `biber` para la bibliografía
* mandar temporales y el PDF final a `build/`

### Limpiar temporales

```powershell
latexmk -C
```

Eso borra auxiliares (`.aux`, `.log`, etc.) y el contenido de `build/` menos el `.pdf` según configuración de latexmk.

## 4. Notas importantes

* **Compilar con LuaLaTeX siempre.**
  No funciona con pdflatex porque cargamos fuentes del sistema (`fontspec` + `fonts/ARIAL.ttf`).
* No borres la carpeta `fonts/`. Sin eso no hay Arial (requisito ICONTEC).
* Las citas en el texto van como notas de pie. El estilo lo maneja `biblatex` + `biber`.
  Para actualizar referencias: compila normalmente, `latexmk` llama a `biber` solo.

## 5. Flujo de trabajo entre autores

1. Clonar el repo:

   ```powershell
   git clone <URL_DEL_REPO.git>
   cd <repo>
   ```

2. Abrir en VS Code, instalar extensiones cuando VS Code lo sugiera.

3. Editar solo dentro de `chapters/*.tex`, no directamente en `main.tex` excepto:

   * metadatos de portada vía `\icontecsetup{...}`
   * orden de inclusión de capítulos
   * paquetes globales

4. Hacer commit SIN archivos temporales:

   * ya hay `.gitignore` que excluye `build/`, `*.aux`, etc.

