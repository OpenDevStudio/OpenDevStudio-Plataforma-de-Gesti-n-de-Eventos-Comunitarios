# Routes

Esta carpeta contiene la definición de todas las rutas de la API.

## Convenciones

- **Nombre de archivos:** camelCase (ejemplo: `auth.js`, `events.js`)
- **Estructura:** Agrupar rutas por recurso
- **Usar Router** de Express
- **Aplicar middlewares** específicos por ruta

## Estructura básica de un archivo de rutas

```javascript
const express = require('express');
const router = express.Router();
const controller = require('../controllers/controller');
const authMiddleware = require('../middlewares/authMiddleware');

// Ruta pública
router.get('/public', controller.publicMethod);

// Ruta protegida
router.post('/protected', authMiddleware, controller.protectedMethod);

module.exports = router;
```

## Ejemplo: auth.js

```javascript
const express = require('express');
const router = express.Router();
const authController = require('../controllers/authController');

/**
 * @route   POST /api/auth/register
 * @desc    Registrar un nuevo usuario
 * @access  Public
 */
router.post('/register', authController.register);

/**
 * @route   POST /api/auth/login
 * @desc    Iniciar sesión
 * @access  Public
 */
router.post('/login', authController.login);

module.exports = router;
```

## Ejemplo: events.js

```javascript
const express = require('express');
const router = express.Router();
const eventController = require('../controllers/eventController');
const authMiddleware = require('../middlewares/authMiddleware');

/**
 * @route   GET /api/events
 * @desc    Obtener todos los eventos
 * @access  Public
 */
router.get('/', eventController.getAllEvents);

/**
 * @route   GET /api/events/:id
 * @desc    Obtener un evento por ID
 * @access  Public
 */
router.get('/:id', eventController.getEventById);

/**
 * @route   POST /api/events
 * @desc    Crear un nuevo evento
 * @access  Private (requiere autenticación)
 */
router.post('/', authMiddleware, eventController.createEvent);

/**
 * @route   PUT /api/events/:id
 * @desc    Actualizar un evento
 * @access  Private (solo organizador)
 */
router.put('/:id', authMiddleware, eventController.updateEvent);

/**
 * @route   DELETE /api/events/:id
 * @desc    Eliminar un evento
 * @access  Private (solo organizador)
 */
router.delete('/:id', authMiddleware, eventController.deleteEvent);

/**
 * @route   POST /api/events/:id/register
 * @desc    Inscribirse a un evento
 * @access  Private
 */
router.post('/:id/register', authMiddleware, eventController.registerToEvent);

module.exports = router;
```

## Ejemplo: users.js

```javascript
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');
const authMiddleware = require('../middlewares/authMiddleware');

/**
 * @route   GET /api/users/profile
 * @desc    Obtener perfil del usuario autenticado
 * @access  Private
 */
router.get('/profile', authMiddleware, userController.getProfile);

/**
 * @route   PUT /api/users/profile
 * @desc    Actualizar perfil del usuario
 * @access  Private
 */
router.put('/profile', authMiddleware, userController.updateProfile);

module.exports = router;
```

## Integración en server.js

```javascript
const express = require('express');
const app = express();

// Importar rutas
const authRoutes = require('./routes/auth');
const eventRoutes = require('./routes/events');
const userRoutes = require('./routes/users');

// Usar rutas
app.use('/api/auth', authRoutes);
app.use('/api/events', eventRoutes);
app.use('/api/users', userRoutes);

// ...resto de la configuración
```

## Métodos HTTP

- **GET** - Obtener recursos
- **POST** - Crear recursos
- **PUT** - Actualizar recursos (completo)
- **PATCH** - Actualizar recursos (parcial)
- **DELETE** - Eliminar recursos

## Rutas a implementar

- `auth.js` - Autenticación (registro, login)
- `events.js` - CRUD de eventos
- `users.js` - Perfil de usuario
- `comments.js` - Comentarios en eventos
- `admin.js` - Rutas de administración

## Códigos de estado HTTP

- **200** - OK (éxito general)
- **201** - Created (recurso creado)
- **400** - Bad Request (datos inválidos)
- **401** - Unauthorized (no autenticado)
- **403** - Forbidden (sin permisos)
- **404** - Not Found (recurso no encontrado)
- **500** - Internal Server Error (error del servidor)

## Buenas prácticas

✅ Documentar cada ruta con comentarios  
✅ Agrupar rutas por recurso  
✅ Aplicar middlewares apropiados  
✅ Usar nombres descriptivos en las rutas  
✅ Seguir convenciones RESTful  
✅ Versionar la API si es necesario (/api/v1/)
