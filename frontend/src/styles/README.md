# Styles

Esta carpeta contiene estilos globales y variables CSS/SCSS para toda la aplicación.

## Estructura recomendada

```
styles/
├── index.css          # Punto de entrada, importa todos los estilos
├── variables.css      # Variables CSS (colores, fuentes, etc.)
├── global.css         # Estilos globales (reset, body, etc.)
├── components.css     # Estilos compartidos de componentes
└── ...otros archivos
```

## Ejemplo: variables.css

Variables CSS para colores, fuentes y tamaños.

```css
:root {
  /* Colores principales */
  --color-primary: #4f46e5;
  --color-primary-dark: #4338ca;
  --color-primary-light: #6366f1;
  
  --color-secondary: #10b981;
  --color-secondary-dark: #059669;
  
  --color-danger: #ef4444;
  --color-warning: #f59e0b;
  --color-success: #10b981;
  --color-info: #3b82f6;
  
  /* Colores de texto */
  --color-text-primary: #1f2937;
  --color-text-secondary: #6b7280;
  --color-text-light: #9ca3af;
  
  /* Colores de fondo */
  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f3f4f6;
  --color-bg-dark: #1f2937;
  
  /* Fuentes */
  --font-family-base: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell', sans-serif;
  --font-family-heading: 'Inter', sans-serif;
  
  /* Tamaños de fuente */
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;
  --font-size-3xl: 1.875rem;
  
  /* Espaciado */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  --spacing-2xl: 3rem;
  
  /* Bordes */
  --border-radius-sm: 0.25rem;
  --border-radius-md: 0.5rem;
  --border-radius-lg: 0.75rem;
  --border-radius-xl: 1rem;
  
  /* Sombras */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
  
  /* Transiciones */
  --transition-base: 0.2s ease-in-out;
  --transition-slow: 0.3s ease-in-out;
}
```

## Ejemplo: global.css

Estilos globales y reset básico.

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: var(--font-family-base);
  font-size: var(--font-size-base);
  color: var(--color-text-primary);
  background-color: var(--color-bg-primary);
  line-height: 1.6;
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-family-heading);
  font-weight: 600;
  line-height: 1.2;
  margin-bottom: var(--spacing-md);
}

h1 { font-size: var(--font-size-3xl); }
h2 { font-size: var(--font-size-2xl); }
h3 { font-size: var(--font-size-xl); }

a {
  color: var(--color-primary);
  text-decoration: none;
  transition: color var(--transition-base);
}

a:hover {
  color: var(--color-primary-dark);
}

button {
  font-family: inherit;
  cursor: pointer;
}

input, textarea, select {
  font-family: inherit;
  font-size: inherit;
}
```

## Ejemplo: components.css

Estilos comunes de componentes.

```css
/* Botones */
.btn {
  padding: var(--spacing-sm) var(--spacing-lg);
  border: none;
  border-radius: var(--border-radius-md);
  font-size: var(--font-size-base);
  font-weight: 500;
  cursor: pointer;
  transition: all var(--transition-base);
}

.btn-primary {
  background-color: var(--color-primary);
  color: white;
}

.btn-primary:hover {
  background-color: var(--color-primary-dark);
}

.btn-secondary {
  background-color: var(--color-secondary);
  color: white;
}

.btn-danger {
  background-color: var(--color-danger);
  color: white;
}

.btn-disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

/* Tarjetas */
.card {
  background-color: var(--color-bg-primary);
  border-radius: var(--border-radius-lg);
  box-shadow: var(--shadow-md);
  padding: var(--spacing-lg);
  transition: box-shadow var(--transition-base);
}

.card:hover {
  box-shadow: var(--shadow-lg);
}

/* Formularios */
.form-group {
  margin-bottom: var(--spacing-lg);
}

.form-label {
  display: block;
  margin-bottom: var(--spacing-sm);
  font-weight: 500;
  color: var(--color-text-primary);
}

.form-input {
  width: 100%;
  padding: var(--spacing-sm) var(--spacing-md);
  border: 1px solid #d1d5db;
  border-radius: var(--border-radius-md);
  font-size: var(--font-size-base);
  transition: border-color var(--transition-base);
}

.form-input:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1);
}

/* Alertas */
.alert {
  padding: var(--spacing-md);
  border-radius: var(--border-radius-md);
  margin-bottom: var(--spacing-md);
}

.alert-success {
  background-color: #d1fae5;
  color: #065f46;
  border: 1px solid #10b981;
}

.alert-error {
  background-color: #fee2e2;
  color: #991b1b;
  border: 1px solid #ef4444;
}

/* Utilidades */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--spacing-lg);
}

.text-center {
  text-align: center;
}

.mt-1 { margin-top: var(--spacing-sm); }
.mt-2 { margin-top: var(--spacing-md); }
.mt-3 { margin-top: var(--spacing-lg); }

.mb-1 { margin-bottom: var(--spacing-sm); }
.mb-2 { margin-bottom: var(--spacing-md); }
.mb-3 { margin-bottom: var(--spacing-lg); }
```

## Ejemplo: index.css

Punto de entrada que importa todos los estilos.

```css
/* Variables */
@import './variables.css';

/* Estilos globales */
@import './global.css';

/* Componentes */
@import './components.css';

/* Puedes importar más archivos según necesites */
```

## Uso en la aplicación

En `src/index.js` o `src/App.js`:

```javascript
import './styles/index.css';
```

## Alternativa con CSS Modules

Si prefieres usar CSS Modules para estilos específicos de componentes:

```javascript
// En un componente
import styles from './ComponentName.module.css';

<div className={styles.container}>
  <button className={styles.button}>Click</button>
</div>
```

## Buenas prácticas

✅ Usar variables CSS para mantener consistencia  
✅ Crear clases reutilizables  
✅ Seguir una convención de nombres (BEM, etc.)  
✅ Mantener los archivos organizados por responsabilidad  
✅ Evitar estilos inline en componentes  
✅ Usar media queries para diseño responsive
