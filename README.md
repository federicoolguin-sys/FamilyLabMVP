# Accenture Family Lab — Landing page

Landing page del programa **Accenture Family Lab**, en colaboración con **MindHub**: formación digital gratuita, online y en vivo para familiares de colaboradores de Accenture.

Es un sitio estático hecho con **HTML + Tailwind CSS v4**. El CSS se compila con **Tailwind CLI**; no hay framework ni JavaScript propio.

Se publica con **GitHub Pages** en modo *Deploy from a branch*, desde la raíz de la rama `main`.

## Requisitos

- [Node.js](https://nodejs.org/) 18 o superior (probado con Node 22)
- npm

## Estructura

```
.
├── index.html               # Página única de la landing (la que publica GitHub Pages)
├── robots.txt
├── .nojekyll                # Evita que GitHub Pages procese el sitio con Jekyll
├── assets/
│   ├── css/styles.css       # CSS generado por el build (SÍ se versiona)
│   └── img/                 # Logo, imagen del hero y favicon
├── src/
│   └── styles.css           # Entrada de Tailwind: tema, colores de marca y componentes
├── screen.png               # Captura de referencia del diseño original
└── package.json
```

- Los colores de marca (`brand-magenta`, `brand-navy`, etc.) y la fuente están definidos en el bloque `@theme` de `src/styles.css`.
- Los estilos reutilizables (`btn-primary`, `faq-item`, `faq-summary`, etc.) están en `@layer components` del mismo archivo.
- Tailwind solo escanea `index.html` para generar las clases que se usan.

## Desarrollo local

```bash
npm install
npm run dev
```

`npm run dev` deja Tailwind en modo *watch*: cada vez que guardes `index.html` o `src/styles.css`, se regenera `assets/css/styles.css`.

Para ver la página, abre `index.html` en el navegador y recarga después de cada cambio.

## Build para producción

GitHub Pages publica los archivos tal como están en el repositorio y **no corre el build**. Por eso `assets/css/styles.css` se sube al repositorio y **hay que regenerarlo antes de cada commit** que cambie el HTML o `src/styles.css`:

```bash
npm install
npm run build
```

Esto genera `assets/css/styles.css` minificado (~28 KB), con solo las clases que usa la página.

Si cambias clases de Tailwind en el HTML y no corres el build, esos estilos no van a aparecer en producción.

### Checklist antes de publicar

1. `npm run build` terminó sin errores.
2. Abrir `index.html` y revisar la página en escritorio y en celular.
3. Verificar que los enlaces a los formularios de Google Forms sigan activos.
4. Revisar que la fecha de inicio ("Comenzamos el 2 de noviembre") y el año del footer estén vigentes.
5. Incluir `assets/css/styles.css` en el commit.

## Deploy en GitHub Pages

Configuración (una sola vez): en **Settings → Pages → Build and deployment**, elegir **Source: Deploy from a branch**, rama **`main`** y carpeta **`/ (root)`**.

Para publicar cambios:

```bash
npm run build
git add .
git commit -m "Descripción del cambio"
git push
```

GitHub publica automáticamente en cada push a `main` (se puede seguir en la pestaña **Actions**, como *pages build and deployment*). El sitio queda en `https://USUARIO.github.io/REPOSITORIO/`.

Todas las rutas del HTML son relativas (`assets/...`), por eso el sitio funciona bajo la subruta `/REPOSITORIO/`. Si agregas recursos nuevos, no uses rutas que empiecen con `/`.

## Pendientes recomendados

- Cuando esté definido el dominio final, agregar en el `<head>` de `index.html`:
  - `<link rel="canonical" href="https://DOMINIO/">`
  - `<meta property="og:image" content="https://DOMINIO/assets/img/...">`, con una imagen de 1200×630 para que el enlace muestre vista previa al compartirlo.
- Reemplazar `assets/img/hero-familia.jpg` (512×279) por una versión de mayor resolución si está disponible.
