# Accenture Family Lab — Landing page

Landing page del programa **Accenture Family Lab**, en colaboración con **MindHub**: formación digital gratuita, online y en vivo para familiares de colaboradores de Accenture.

Es un sitio estático hecho con **HTML + Tailwind CSS v4**. El CSS se compila con **Tailwind CLI**; no hay framework ni JavaScript propio.

## Requisitos

- [Node.js](https://nodejs.org/) 18 o superior (probado con Node 22)
- npm

## Estructura

```
.
├── public/                  # Carpeta que se publica en producción
│   ├── index.html           # Página única de la landing
│   ├── robots.txt
│   └── assets/
│       ├── css/styles.css   # CSS generado por el build (no se versiona)
│       └── img/             # Logo, imagen del hero y favicon
├── src/
│   └── styles.css           # Entrada de Tailwind: tema, colores de marca y componentes
├── screen.png               # Captura de referencia del diseño original
└── package.json
```

- Los colores de marca (`brand-magenta`, `brand-navy`, etc.) y la fuente están definidos en el bloque `@theme` de `src/styles.css`.
- Los estilos reutilizables (`btn-primary`, `faq-item`, `faq-summary`, etc.) están en `@layer components` del mismo archivo.
- Tailwind solo escanea `public/**/*.html` para generar las clases que se usan.

## Desarrollo local

```bash
npm install
npm run dev
```

`npm run dev` deja Tailwind en modo *watch*: cada vez que guardes `public/index.html` o `src/styles.css`, se regenera `public/assets/css/styles.css`.

Para ver la página, abre `public/index.html` en el navegador y recarga después de cada cambio. También puedes servir la carpeta con cualquier servidor estático, por ejemplo:

```bash
npx serve public
```

## Build para producción

**Antes de cada deploy hay que generar el CSS**, porque `public/assets/css/styles.css` no está en el repositorio (ver `.gitignore`):

```bash
npm install
npm run build
```

Esto genera `public/assets/css/styles.css` minificado (~28 KB), con solo las clases que usa la página.

Si agregas clases de Tailwind nuevas en el HTML, vuelve a correr el build. Si no lo haces, esos estilos no van a aparecer en producción.

### Checklist antes de publicar

1. `npm run build` terminó sin errores.
2. Abrir `public/index.html` y revisar la página en escritorio y en celular.
3. Verificar que los enlaces a los formularios de Google Forms sigan activos.
4. Revisar que la fecha de inicio ("Comenzamos el 2 de noviembre") y el año del footer estén vigentes.

## Deploy

El sitio es 100% estático. En cualquier hosting, la configuración es:

| Opción              | Valor                         |
| ------------------- | ----------------------------- |
| Comando de build    | `npm install && npm run build` |
| Carpeta a publicar  | `public`                      |
| Versión de Node     | 18 o superior                 |

### GitHub Pages (configurado)

El repositorio incluye el workflow `.github/workflows/deploy.yml`. En cada push a `main`, el workflow instala dependencias, corre `npm run build` y publica la carpeta `public/`. No hace falta subir el CSS generado.

Configuración inicial (una sola vez):

1. Crear el repositorio en GitHub y subir el código a la rama `main`:

   ```bash
   git init
   git add .
   git commit -m "Landing Accenture Family Lab"
   git branch -M main
   git remote add origin https://github.com/USUARIO/REPOSITORIO.git
   git push -u origin main
   ```

2. En GitHub, ir a **Settings → Pages → Build and deployment → Source** y elegir **GitHub Actions**.
3. El deploy se puede seguir en la pestaña **Actions**. Al terminar, el sitio queda en `https://USUARIO.github.io/REPOSITORIO/`.

Para volver a desplegar sin cambios de código: **Actions → Deploy a GitHub Pages → Run workflow**.

Todas las rutas del HTML son relativas (`assets/...`), por eso el sitio funciona bajo la subruta `/REPOSITORIO/`. Si agregas recursos nuevos, no uses rutas que empiecen con `/`.

### Netlify / Vercel / Cloudflare Pages

1. Conectar el repositorio.
2. Configurar el comando de build y la carpeta de publicación de la tabla anterior. En Vercel, elegir el preset **Other**.
3. Desplegar. Cada push a la rama principal vuelve a correr el build automáticamente.

### Hosting sin build (FTP, S3, servidor propio)

1. Correr `npm run build` en tu máquina.
2. Subir **el contenido** de la carpeta `public/` (incluyendo `assets/css/styles.css`) a la raíz del sitio.

## Pendientes recomendados

- Cuando esté definido el dominio final, agregar en el `<head>` de `public/index.html`:
  - `<link rel="canonical" href="https://DOMINIO/">`
  - `<meta property="og:image" content="https://DOMINIO/assets/img/...">`, con una imagen de 1200×630 para que el enlace muestre vista previa al compartirlo.
- Reemplazar `public/assets/img/hero-familia.jpg` (512×279) por una versión de mayor resolución si está disponible.
