# Presentación — Experimento Blender · Claude Code · Vídeo generativo

## Qué es

Presentación visual en HTML (una sola página, `index.html`) que explica **por qué** se hizo el
experimento de dirigir vídeo generativo desde una escena 3D editable: partir de una referencia
fotográfica, reconstruir el salón en Blender con Claude Code vía MCP, hacer blocking con un dummy,
grabar cámara a mano con VirtuCamera, y usar el render como base para sustitución visual con IA
(Magnific), moviendo ahora el flujo a ComfyUI para iterar en local.

No es una app ni un dashboard. Es una "bitácora visual": vídeos grandes, texto breve, fondo
oscuro neutro, acento arena.

**El texto visible del deck está en inglés** (traducido el 2026-09-09 a petición del cliente).
Los nombres de pantalla viven en `data-name` de cada `<section class="screen">`; el índice y el
total del header se calculan solos en JS. Los comentarios del CSS/HTML siguen en ES/mezcla —
inofensivos.

## Estado

**Terminado y verificado** (2026-09-09). Las 8 pantallas revisadas en navegador desktop 16:9
(1600×900). Navegación por teclado y clic verificada. Pósteres/fotogramas de respaldo en todos
los vídeos: si el autoplay falla, la presentación sigue siendo legible.

## Cómo se abre

Abrir `index.html` directamente en un navegador de escritorio (Chrome/Safari), a pantalla
completa. Diseñada **solo** para pantalla panorámica 16:9 — no es responsive ni tiene versión
móvil (decisión de producto pedida en el encargo).

- Flechas / barra espaciadora / botones ‹ › → navegar.
- `Home` / `End` → primera / última pantalla.
- `F` → pantalla completa.
- Los tics inferiores son clicables.

La tipografía usa Google Fonts (Archivo, Archivo Expanded, Spline Sans Mono) con fallback de
sistema; funciona sin conexión, solo cambia la fuente.

## Estructura (8 pantallas)

1. **El objetivo** — referencia (foto) + resultado Magnific + hoja de personaje. Frase grande:
   "No sustituir al director de fotografía. Darle una herramienta editable."
2. **El pipeline** — diagrama lineal de 8 etapas con miniatura + frase por etapa; las dos
   últimas (ComfyUI "en curso", Unreal Engine "siguiente") van sin imagen, marcadas en arena.
3. **Referencia y reconstrucción 3D** — comparativa grande foto de referencia ↔ render 3D
   automático de la misma escena, + miniatura del viewport de Blender con el panel
   "MCP for Blender" conectado (puerto 9876). *(Pantalla añadida a petición del cliente sobre
   el encargo original de 6.)*
4. **Blender como espacio de control** — 3 vídeos: blocking de media tarde, entorno aislado,
   dummy simplificado.
5. **Cámara como un gesto humano** — VirtuCamera: toma manual + cámara diseñada.
6. **Qué funcionó y qué limitó** — dos columnas + clip del resultado final de Magnific.
7. **Estado actual y siguiente iteración** — primer test + render final de Magnific + blocking.
   Roadmap: *Now* ComfyUI (iteración local), *Next* llevar la escena 3D a Unreal Engine.
   Cierre: "3D works as a shared language between directing, camera, animation and video
   generation."
8. **Credits** — Miguel Ángel Ballesteros, Alba Díaz, Teyo Cuadrado. Pantalla de texto,
   sin media.

Nota persistente en el pie: "Material de experimentación · posibilidades y límites actuales del
flujo". El encargo prohíbe afirmar que el resultado es perfecto o apto para producción.

## Arquitectura

- `index.html` — todo: HTML + CSS + JS inline. Sin build, sin dependencias locales.
- `blender-claude-code-experiment-presentation.pdf` — export apaisado de las 8 pantallas
  (1 por página, 2400×1350). Los vídeos salen como fotograma fijo. Se regenera con Playwright:
  servir la carpeta, recorrer `window.__go(0..7)`, capturar JPEG de cada pantalla y `page.pdf()`.
- `assets/video/` — 7 mp4 (verticales 720×1080 / 720×1280), copiados de `~/Desktop/test`.
- `assets/poster/` — fotogramas de respaldo (generados con `qlmanage`, macOS QuickLook).
- `assets/img/` — `environment.jpg` (referencia), `character_reference.png`,
  `blender_mcp_viewport.jpg` (viewport de Blender aportado por el cliente: `wireframe_blender.png`).

### Cómo escala a 16:9

`.stage` es una caja fija de `min(100vw, 177.78vh)` × `min(56.25vw, 100vh)` centrada y con
letterbox. Lleva `container-type: size`; **todo el interior se dimensiona en `cqh`/`cqw`**
(1cqh = 1% de la altura del escenario), así el texto y los vídeos escalan con la pantalla.

### JS (una IIFE)

`go(n)` activa una pantalla, arranca sus vídeos (los inactivos se pausan), actualiza índice y
tics. Los vídeos usan `preload="none"` + `data-src` y solo cargan al activarse la pantalla.
`window.__go` queda expuesto para pruebas.

## Fuentes de datos / recuperación

Los materiales originales están en `~/Desktop/test/` (no versionado). Si se pierden los assets,
se pueden regenerar desde ahí: copiar los mp4, recrear pósters con
`qlmanage -t -s 1400 -o <destino> <video.mp4>`, y `sips` para convertir/reescalar imágenes.

## Publicación

- Repo **público** en GitHub: `mabz-miguel/blender-claude-code-experiment` (remote `origin`).
- **GitHub Pages** activo desde `main` / raíz →
  **https://mabz-miguel.github.io/blender-claude-code-experiment/**
  (vídeos servidos desde Pages, funcionan). Cada `git push` a `main` redespliega solo.
- El PDF (`blender-claude-code-experiment-presentation.pdf`) va versionado en el repo.

## Pendiente / posibles mejoras

- `assets/img/blender_render.jpg` = render 3D limpio de la escena (origen:
  `~/Desktop/test/living-room-blender.png`). Para actualizarlo: `sips -Z 2000 -s format jpeg
  -s formatOptions 86 <origen> --out assets/img/blender_render.jpg`.
- Falta decidir si este repo va a GitHub (privado). De momento solo local.
- Sin `git init` todavía.
