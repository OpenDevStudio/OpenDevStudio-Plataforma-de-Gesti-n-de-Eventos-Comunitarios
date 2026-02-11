# Public

Esta carpeta contiene archivos estáticos que se sirven directamente sin procesamiento.

## Archivos comunes

### index.html
El archivo HTML principal de la aplicación React.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Plataforma de Gestión de Eventos Comunitarios"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>Eventos Comunitarios</title>
  </head>
  <body>
    <noscript>Necesitas habilitar JavaScript para usar esta aplicación.</noscript>
    <div id="root"></div>
  </body>
</html>
```

### manifest.json
Configuración de PWA (Progressive Web App).

```json
{
  "short_name": "Eventos",
  "name": "Plataforma de Eventos Comunitarios",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

### robots.txt
Configuración para motores de búsqueda.

```txt
# https://www.robotstxt.org/robotstxt.html
User-agent: *
Disallow:
```

## Estructura típica

```
public/
├── index.html       # HTML principal
├── favicon.ico      # Icono del sitio
├── logo192.png      # Logo 192x192
├── logo512.png      # Logo 512x512
├── manifest.json    # Configuración PWA
├── robots.txt       # Configuración SEO
└── assets/          # Imágenes y recursos estáticos
    ├── images/
    └── icons/
```

## Agregar imágenes estáticas

Puedes agregar imágenes en `public/assets/images/`:

```
public/assets/images/
├── hero-bg.jpg
├── placeholder-event.png
└── logo.svg
```

**Uso en componentes:**
```jsx
<img src="/assets/images/logo.svg" alt="Logo" />
```

O usando `process.env.PUBLIC_URL`:
```jsx
<img src={`${process.env.PUBLIC_URL}/assets/images/logo.svg`} alt="Logo" />
```

## Diferencia entre public/ y src/

### Archivos en `public/`:
- Se sirven tal cual (sin procesamiento)
- Se referencian con rutas absolutas
- Ideal para: imágenes estáticas, favicon, manifest.json

### Archivos en `src/`:
- Pasan por Webpack (se optimizan y procesan)
- Se importan en los componentes
- Ideal para: imágenes usadas en componentes, estilos

**Ejemplo en src/:**
```jsx
import logo from '../assets/logo.png';
<img src={logo} alt="Logo" />
```

## Cambiar el favicon

1. Reemplaza `favicon.ico` con tu propio icono
2. Actualiza las rutas en `index.html` si cambias el nombre

## Configurar el título y meta tags

Edita `index.html`:
```html
<title>Tu Título Aquí</title>
<meta name="description" content="Tu descripción" />
```

O usa React Helmet para títulos dinámicos:
```jsx
import { Helmet } from 'react-helmet';

<Helmet>
  <title>Eventos - {eventTitle}</title>
</Helmet>
```

## Buenas prácticas

✅ Mantener archivos mínimos en public/  
✅ Optimizar imágenes antes de subirlas  
✅ Usar nombres descriptivos para archivos  
✅ No subir archivos grandes innecesarios  
✅ Configurar correctamente manifest.json para PWA
