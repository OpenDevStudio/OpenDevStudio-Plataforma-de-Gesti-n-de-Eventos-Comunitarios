# Backend - Node.js + Express

Esta carpeta contiene todo el código del servidor (API y lógica de negocio).

## Estructura

```
backend/
├── src/
│   ├── controllers/    # Controladores (lógica de negocio)
│   ├── models/         # Modelos de MongoDB (Mongoose)
│   ├── routes/         # Definición de rutas de la API
│   ├── middlewares/    # Middlewares (auth, validación, etc.)
│   ├── config/         # Configuración de la aplicación
│   ├── utils/          # Funciones auxiliares
│   └── server.js       # Punto de entrada del servidor
├── package.json        # Dependencias del backend
└── .env.example        # Variables de entorno de ejemplo
```

## Instalación

```bash
cd backend
npm install
```

## Ejecución

```bash
npm start
```

El servidor se iniciará en [http://localhost:5000](http://localhost:5000)

## Scripts disponibles

- `npm start` - Inicia el servidor
- `npm run dev` - Inicia el servidor con nodemon (auto-reload)
- `npm test` - Ejecuta los tests
- `npm run lint` - Ejecuta el linter

## Variables de entorno

Crea un archivo `.env` basándote en `.env.example`:

```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/eventos-comunitarios
JWT_SECRET=tu_secreto_aqui
NODE_ENV=development
```

## Buenas prácticas

- Separar la lógica de negocio en controladores
- Validar datos de entrada en middlewares
- Usar variables de entorno para configuración sensible
- Documentar endpoints con comentarios claros
- Manejar errores de forma consistente
