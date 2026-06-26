# Cinemas del Country — Sitio web

Prototipo navegable del sitio de **Cinemas del Country**: cartelera, próximos estrenos con preventa, selección de butacas, checkout y e-ticket.

Es un sitio **100% estático** (HTML + JS, sin backend ni build). Se abre directo en el navegador y se publica subiendo los archivos a cualquier hosting estático.

---

## Estructura del proyecto

```
cinemas-del-country-web/
├── index.html        ← La página (todo el diseño y la lógica viven aquí)
├── support.js        ← Runtime que renderiza la página (no editar)
├── image-slot.js     ← Componente de imágenes arrastrables (no editar)
├── cinemas-logo.png  ← Logo
└── posters/          ← Pósters de las películas
    ├── toystory.png
    ├── leviticus.png
    ├── disclosure.png
    ├── letras.png
    ├── amos.png
    ├── scary.png
    └── backrooms.png
```

**Casi todo lo que van a editar está en `index.html`.** Los archivos `support.js` e `image-slot.js` son librerías de apoyo: no se tocan.

---

## Cómo verlo en local

No necesita instalación. Dos opciones:

1. **Doble clic** en `index.html` (se abre en el navegador).
2. Servirlo con un servidor local (recomendado, evita problemas con rutas):
   ```bash
   # Con Python (ya viene en Mac/Linux)
   python3 -m http.server 8000
   # Luego abrir http://localhost:8000
   ```

---

## Cómo actualizar el contenido

Todo el contenido está dentro de `index.html`, en el bloque de JavaScript al final del archivo.

### Cambiar las películas en cartelera
Busca la función `movieData()`. Cada película es una línea como esta:

```js
{ title:'Toy Story 5', genre:'Animación', duration:'1h 42m', rating:'AA', score:'9.0',
  sala:'1', formats:['ESP 2D','ESP 3D'], poster:'posters/toystory.png',
  schedule:['12:15','1:15','2:15'] },
```

- `title`, `genre`, `duration`, `rating`, `score`, `sala` → datos de la película.
- `formats` → etiquetas que aparecen sobre el póster (ESP, SUB, 3D…).
- `poster` → ruta a la imagen dentro de `posters/`.
- `schedule` → horarios disponibles.

Para **agregar** una película, copia una línea y cambia sus datos. Para **quitarla**, borra su línea.

### Cambiar los próximos estrenos (preventa)
Busca la función `upcomingData()`. Mismo formato, con campos de estreno:

```js
{ title:'Avatar: Fuego y Ceniza', genre:'Ciencia Ficción', duration:'3h 10m',
  rating:'B', sala:'IMAX', format:'IMAX 3D', poster:'',
  premiere:'19 Dic 2026', dateBig:'19 DIC', dateSub:'Estreno mundial',
  synopsis:'...' },
```

- `premiere` → fecha completa que se muestra en el checkout.
- `dateBig` / `dateSub` → fecha grande y subtítulo sobre el póster.
- `poster:''` → déjalo vacío para que muestre el espacio para arrastrar imagen, o pon una ruta a `posters/`.

### Cambiar pósters
1. Guarda la nueva imagen en la carpeta `posters/` (formato vertical 2:3 recomendado).
2. Actualiza la ruta `poster:` de esa película en `index.html`.

### Cambiar precios
Busca la función `priceFor()`. Ahí están los precios normales y de preventa (en pesos).

---

## Cómo publicarlo (servidor)

Al ser estático, sirve cualquiera de estos:

- **GitHub Pages** (gratis): en el repo → Settings → Pages → Source: rama `main`, carpeta `/root`. Queda publicado en `https://USUARIO.github.io/REPO/`.
- **Netlify / Vercel / Cloudflare Pages**: conecta el repo y publica. No requiere comando de build (es estático).
- **Hosting propio**: sube todos los archivos por FTP a la carpeta pública del servidor.

---

## Trabajo en equipo (Git)

Flujo recomendado para no pisarse entre los 3:

```bash
# Una sola vez: clonar
git clone https://github.com/USUARIO/cinemas-del-country-web.git
cd cinemas-del-country-web

# Cada vez que vayan a trabajar:
git pull                      # traer lo último
git checkout -b mi-cambio     # crear una rama para tu cambio
# ...editar index.html...
git add .
git commit -m "Actualizo cartelera de la semana"
git push -u origin mi-cambio
# Luego abren un Pull Request en GitHub para revisar entre todos
```

Editar siempre en una rama y revisar con Pull Request evita conflictos cuando varios tocan `index.html`.
