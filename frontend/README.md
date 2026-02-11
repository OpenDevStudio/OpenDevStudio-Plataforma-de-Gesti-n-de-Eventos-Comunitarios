# Frontend - React

Esta carpeta contiene todo el código del cliente (interfaz de usuario).

## Estructura

```
frontend/
├── public/              # Archivos estáticos (index.html, favicon, etc.)
├── src/
│   ├── components/      # Componentes reutilizables de React
│   ├── pages/          # Páginas principales de la aplicación
│   ├── services/       # Servicios para llamadas a la API
│   ├── styles/         # Estilos CSS/SCSS globales
│   ├── utils/          # Funciones auxiliares y helpers
│   ├── App.js          # Componente principal
│   └── index.js        # Punto de entrada
├── package.json        # Dependencias del frontend
└── .env.example        # Variables de entorno de ejemplo
```

## Instalación

```bash
cd frontend
npm install
```

## Ejecución

```bash
npm start
```

El proyecto se abrirá en [http://localhost:3000](http://localhost:3000)

## Scripts disponibles

- `npm start` - Inicia el servidor de desarrollo
- `npm test` - Ejecuta los tests
- `npm run build` - Genera la versión de producción
- `npm run lint` - Ejecuta el linter

## Buenas prácticas

- Crear componentes pequeños y reutilizables
- Usar nombres descriptivos para componentes y archivos
- Mantener la lógica de negocio en servicios separados
- Documentar componentes complejos
