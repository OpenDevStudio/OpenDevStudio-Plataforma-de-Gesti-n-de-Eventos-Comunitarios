# Config

Esta carpeta contiene archivos de configuración para diferentes aspectos de la aplicación.

## Convenciones

- **Nombre de archivos:** camelCase (ejemplo: `database.js`, `jwt.js`)
- **Exportar** configuraciones o funciones de inicialización
- **Usar variables de entorno** para valores sensibles

## Ejemplo: database.js

Configuración de conexión a MongoDB.

```javascript
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });

    console.log(`✅ MongoDB conectado: ${conn.connection.host}`);
  } catch (error) {
    console.error('❌ Error al conectar a MongoDB:', error.message);
    process.exit(1); // Salir del proceso si no se puede conectar
  }
};

// Manejar desconexiones
mongoose.connection.on('disconnected', () => {
  console.warn('⚠️  MongoDB desconectado');
});

// Cerrar conexión si el proceso termina
process.on('SIGINT', async () => {
  await mongoose.connection.close();
  console.log('MongoDB conexión cerrada debido a terminación de la aplicación');
  process.exit(0);
});

module.exports = connectDB;
```

**Uso en server.js:**
```javascript
const connectDB = require('./config/database');

// Conectar a la base de datos
connectDB();
```

## Ejemplo: jwt.js

Configuración relacionada con JWT.

```javascript
const jwt = require('jsonwebtoken');

const generateToken = (payload) => {
  return jwt.sign(payload, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRE || '7d',
  });
};

const verifyToken = (token) => {
  try {
    return jwt.verify(token, process.env.JWT_SECRET);
  } catch (error) {
    throw new Error('Token inválido');
  }
};

module.exports = {
  generateToken,
  verifyToken,
};
```

## Ejemplo: corsConfig.js

Configuración de CORS.

```javascript
const corsOptions = {
  origin: function (origin, callback) {
    // Lista de orígenes permitidos
    const allowedOrigins = [
      'http://localhost:3000',
      'http://localhost:5173',
      process.env.FRONTEND_URL,
    ];

    // Permitir requests sin origin (apps móviles, Postman, etc.)
    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('No permitido por CORS'));
    }
  },
  credentials: true,
  optionsSuccessStatus: 200,
};

module.exports = corsOptions;
```

**Uso en server.js:**
```javascript
const cors = require('cors');
const corsOptions = require('./config/corsConfig');

app.use(cors(corsOptions));
```

## Ejemplo: env.js

Validación de variables de entorno requeridas.

```javascript
const requiredEnvVars = [
  'MONGODB_URI',
  'JWT_SECRET',
  'PORT',
];

const validateEnv = () => {
  const missing = requiredEnvVars.filter(varName => !process.env[varName]);

  if (missing.length > 0) {
    console.error('❌ Faltan las siguientes variables de entorno:');
    missing.forEach(varName => console.error(`   - ${varName}`));
    process.exit(1);
  }

  console.log('✅ Variables de entorno validadas correctamente');
};

module.exports = validateEnv;
```

**Uso en server.js:**
```javascript
require('dotenv').config();
const validateEnv = require('./config/env');

validateEnv();
```

## Ejemplo: constants.js

Constantes usadas en la aplicación.

```javascript
const USER_ROLES = {
  USER: 'usuario',
  ADMIN: 'admin',
};

const EVENT_CATEGORIES = [
  'Deportes',
  'Cultura',
  'Tecnología',
  'Educación',
  'Social',
  'Otro',
];

const HTTP_STATUS = {
  OK: 200,
  CREATED: 201,
  BAD_REQUEST: 400,
  UNAUTHORIZED: 401,
  FORBIDDEN: 403,
  NOT_FOUND: 404,
  SERVER_ERROR: 500,
};

module.exports = {
  USER_ROLES,
  EVENT_CATEGORIES,
  HTTP_STATUS,
};
```

## Ejemplo: logger.js

Configuración de logging (opcional, con winston).

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' }),
  ],
});

// Si no estamos en producción, también loguear a consola
if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple(),
  }));
}

module.exports = logger;
```

## Archivos de configuración a implementar

- `database.js` - Conexión a MongoDB
- `jwt.js` - Utilidades de JWT (opcional)
- `corsConfig.js` - Configuración de CORS
- `env.js` - Validación de variables de entorno
- `constants.js` - Constantes de la aplicación

## Variables de entorno (.env)

Ejemplo de archivo `.env`:

```env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/eventos-comunitarios

# JWT
JWT_SECRET=tu_secreto_super_seguro_aqui
JWT_EXPIRE=7d

# Frontend URL (para CORS)
FRONTEND_URL=http://localhost:3000
```

## Buenas prácticas

✅ Separar configuraciones por responsabilidad  
✅ Usar variables de entorno para datos sensibles  
✅ Validar variables de entorno al iniciar  
✅ Documentar las configuraciones necesarias  
✅ No subir archivos `.env` al repositorio  
✅ Proporcionar un `.env.example` como plantilla
