# Estructura del Proyecto

```
proyecto-eventos-comunitarios/
├── frontend/                    # Aplicación React
│   ├── public/                 # Archivos estáticos
│   │   ├── index.html
│   │   └── favicon.ico
│   ├── src/
│   │   ├── components/         # Componentes reutilizables
│   │   ├── pages/              # Páginas principales
│   │   ├── services/           # Servicios de API
│   │   ├── styles/             # Estilos globales
│   │   ├── utils/              # Utilidades
│   │   ├── App.js              # Componente principal
│   │   └── index.js            # Punto de entrada
│   ├── .env.example            # Variables de entorno
│   ├── package.json
│   └── README.md
│
├── backend/                     # API Node.js + Express
│   ├── src/
│   │   ├── controllers/        # Lógica de negocio
│   │   ├── models/             # Modelos de MongoDB
│   │   ├── routes/             # Definición de rutas
│   │   ├── middlewares/        # Middlewares personalizados
│   │   ├── config/             # Configuración
│   │   ├── utils/              # Utilidades
│   │   └── server.js           # Punto de entrada
│   ├── .env.example            # Variables de entorno
│   ├── package.json
│   └── README.md
│
├── .gitignore                  # Archivos ignorados por Git
├── README.md                   # Documentación principal
├── CONTRIBUTING.md             # Guía de contribución
├── issues.md                   # Lista de issues
├── kanban.md                   # Guía del tablero Kanban
└── roles.md                    # Roles del equipo
```

## Descripción de carpetas principales

### Frontend (`/frontend`)
Contiene toda la interfaz de usuario construida con React.

### Backend (`/backend`)
Contiene la API REST construida con Node.js y Express.

## Flujo de datos

```
Frontend (React)
    ↓
Services (axios)
    ↓
Backend API (Express)
    ↓
Controllers
    ↓
Models (Mongoose)
    ↓
MongoDB
```
