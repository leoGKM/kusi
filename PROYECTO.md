# Kusi — resumen del proyecto

## Estado actual

Kusi es un proyecto web educativo en español para aprender quechua jugando. El repositorio contiene una página de presentación interactiva y, por separado, una escena de realidad aumentada (RA) ejecutada con un runtime incluido en `external/`.

No hay `package.json`, gestor de dependencias, scripts de build ni pruebas configuradas en la raíz. La página principal está escrita directamente en HTML, CSS y JavaScript; la escena de RA está serializada en `bundle.js`. No se detectó un flujo de desarrollo o despliegue documentado.

Al revisar el repositorio, Git mostraba como no seguidos `assets/`, `bundle.js`, `external/`, `image-targets/` e `realidad-aumentada.html`. `index.html` ya estaba seguido. El propio archivo `PROYECTO.md` se está añadiendo con este documento.

## Árbol del proyecto

Se omiten archivos binarios, recursos pesados, dependencias/runtime compilados y carpetas de build. Se muestran las ubicaciones de los recursos omitidos para que se entienda qué necesita la escena.

```text
.
├── assets/
│   ├── 1Mira_es_un_conejo.m4a       [audio, omitido]
│   └── cuy.glb                      [modelo 3D de ~70,8 MB, omitido]
├── external/
│   ├── runtime/
│   │   ├── LICENSE
│   │   └── resources/               [fuentes, decodificadores y otros recursos omitidos]
│   └── xr/
│       ├── LICENSE
│       └── resources/                [modelos y workers omitidos]
├── image-targets/
│   ├── conejo.json
│   └── [imágenes PNG del objetivo, omitidas]
├── bundle.js
├── index.html
├── realidad-aumentada.html
└── PROYECTO.md
```

`external/` también contiene los runtimes JavaScript de terceros para la aplicación y RA; se omiten del árbol por ser archivos compilados/pesados. En particular, `index.html` carga `external/runtime/runtime.js` y `external/xr/xr.js`.

## Archivos clave

- `index.html`: página principal en español; contiene el diseño, estilos y lógica interactiva de la experiencia educativa.
- `realidad-aumentada.html`: página mínima independiente que carga el runtime de RA y `bundle.js`; su título y metadatos aún dicen “Empty Project”.
- `bundle.js`: registra un componente de ejemplo, define la escena de RA e inicializa la aplicación del runtime.
- `image-targets/conejo.json`: metadatos del objetivo planar de imagen llamado `conejo`, usado para el seguimiento de RA.
- `assets/cuy.glb`: modelo 3D de cuy referenciado por la escena.
- `assets/1Mira_es_un_conejo.m4a`: audio en bucle referenciado por la escena.
- `external/runtime/`: runtime y recursos para representar la escena; incluye su licencia.
- `external/xr/`: runtime y recursos para capacidades de realidad extendida; incluye su licencia.

## Dependencias y configuración

No existe `package.json` ni otro manifiesto de dependencias de la aplicación en la raíz, por lo que no hay contenido de `package.json` que reproducir ni comandos npm definidos.

La configuración disponible está integrada directamente en los HTML y el bundle:

- `index.html` define idioma español (`lang="es"`), estilos CSS y JavaScript inline; solicita las fuentes Fredoka, Nunito e Inter a Google Fonts.
- `realidad-aumentada.html` carga el runtime desde `external/runtime/runtime.js`, el runtime de RA desde `external/xr/xr.js` y luego `bundle.js`.
- `bundle.js` configura una cámara orientada a RA en móviles y una escena asociada al objetivo `conejo`.

## Resumen del código existente

### Página educativa (`index.html`)

Es una landing page responsive para público infantil, con una paleta de colores y tipografías definidas mediante variables CSS. Presenta el juego Kusi, secciones sobre personajes, realidad aumentada, instrucciones y material imprimible.

La sección de personajes incluye cuatro pestañas y una tarjeta interactiva. El arreglo `charactersData` mantiene los datos pedagógicos de cada personaje; `selectCharacter(idx)` actualiza la pestaña y el contenido mostrado, `toggleCardFlip()` voltea la tarjeta y `playFriendlyChime(event)` genera un tono con Web Audio API para dar respuesta al botón de audio.

### Escena de RA (`bundle.js` y `realidad-aumentada.html`)

`realidad-aumentada.html` carga la escena en el runtime incluido. El bundle registra `example-component` y crea una escena con cámara, luces y un objetivo de imagen llamado `conejo`. Al reconocer ese objetivo, la escena asocia el modelo `assets/cuy.glb` y reproduce en bucle `assets/1Mira_es_un_conejo.m4a`.

El objetivo y sus imágenes derivadas viven en `image-targets/`; los datos del modelo y el audio están en `assets/`. Esos recursos binarios, incluido el modelo de aproximadamente 70,8 MB, deben conservarse para que la experiencia de RA pueda cargarlos.
