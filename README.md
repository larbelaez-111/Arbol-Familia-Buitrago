# Árbol Genealógico — Familia Buitrago

App de una sola página (`index.html`) que lee en vivo tu Google Sheet publicado
como CSV y dibuja el árbol genealógico interactivo (zoom, pan, buscador,
expandir/colapsar ramas).

## Cómo funciona

- El sheet se lee cada vez que se abre la página (link "Publicar en la web"
  con `output=csv`), no hay backend ni base de datos aparte.
- El árbol se arma automáticamente a partir de las columnas `Papa` y `Mama`,
  comparando contra la columna `Nombre` (tolera espacios extra o apellidos
  incompletos).
- La generación `G1` (la pareja fundadora) se funde en un solo nodo raíz.
- Si el link del sheet cambia, solo hay que editar la constante
  `SHEET_CSV_URL` al inicio del `<script>` en `index.html`.

## Publicar en GitHub Pages

1. Crea un repositorio nuevo en GitHub (puede ser privado o público).
2. Sube `index.html` a la raíz del repositorio.
3. Ve a **Settings → Pages**.
4. En "Source" elige la rama `main` y la carpeta `/ (root)`. Guarda.
5. Espera 1-2 minutos; GitHub te dará una URL tipo
   `https://tu-usuario.github.io/tu-repo/`.

No necesitas ningún build step ni dependencias — es HTML puro que carga
D3 y PapaParse desde CDN.

## Mantener el sheet como "base de datos"

- El Google Sheet debe seguir **publicado en la web** (Archivo → Compartir →
  Publicar en la web). Si alguna vez lo "dejas de publicar", la app dejará de
  leer datos.
- Cualquier edición que guardes en el sheet aparece en la app la próxima vez
  que alguien recargue la página (o le dé clic a "Actualizar datos" en la
  cabecera) — no es instantáneo en tiempo real, es "al recargar".
- El link publicado es de solo lectura pública: cualquiera con el link puede
  ver los datos crudos del CSV, pero no puede editarlos desde ahí.

## Verificación pendiente de tu parte

No pude probar el `fetch` en vivo contra Google Sheets desde este entorno
(el sandbox no tiene salida a internet general para probarlo aquí), pero sí
verifiqué:

- Que el link que compartiste tiene el formato correcto de publicación CSV.
- Que las 66 filas del sheet real se leen y enlazan sin huérfanos usando la
  misma lógica que quedó en `index.html` (probado con Node localmente).

Para confirmar que todo funciona de punta a punta, solo abre `index.html`
en tu navegador (doble clic) o súbelo a GitHub Pages y ábrelo — ahí sí hay
internet real y el fetch al sheet debería funcionar sin problema, porque los
links "Publicar en la web" de Google están pensados exactamente para este
uso (CORS abierto).
