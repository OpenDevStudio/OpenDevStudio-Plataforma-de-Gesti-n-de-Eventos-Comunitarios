# Middlewares

Esta carpeta contiene middlewares personalizados para procesar peticiones antes de llegar a los controladores.

## ¿Qué es un middleware?

Un middleware es una función que tiene acceso a los objetos de petición (req), respuesta (res) y la siguiente función (next) en el ciclo de petición-respuesta.

## Estructura básica

```javascript
const middlewareName = (req, res, next) => {
  // Realizar alguna operación
  // Modificar req o res si es necesario
  
  // Llamar a next() para continuar al siguiente middleware o controlador
  next();
  
  // O enviar una respuesta y terminar la petición
  // res.status(400).json({ message: 'Error' });
};

module.exports = middlewareName;
```

## Ejemplo: authMiddleware.js

Este middleware verifica que el usuario esté autenticado mediante un token JWT.

```javascript
const jwt = require('jsonwebtoken');
const User = require('../models/User');

const authMiddleware = async (req, res, next) => {
  try {
    // 1. Obtener el token del header Authorization
    const authHeader = req.headers.authorization;

    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      return res.status(401).json({ message: 'Token no proporcionado' });
    }

    const token = authHeader.split(' ')[1];

    // 2. Verificar el token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);

    // 3. Buscar el usuario
    const user = await User.findById(decoded.id);

    if (!user) {
      return res.status(401).json({ message: 'Usuario no encontrado' });
    }

    // 4. Adjuntar el usuario al objeto request
    req.user = user;

    // 5. Continuar al siguiente middleware o controlador
    next();
  } catch (error) {
    if (error.name === 'JsonWebTokenError') {
      return res.status(401).json({ message: 'Token inválido' });
    }
    if (error.name === 'TokenExpiredError') {
      return res.status(401).json({ message: 'Token expirado' });
    }
    res.status(500).json({ message: 'Error del servidor' });
  }
};

module.exports = authMiddleware;
```

## Ejemplo: adminMiddleware.js

Este middleware verifica que el usuario tenga rol de administrador.

```javascript
const adminMiddleware = (req, res, next) => {
  // Este middleware debe usarse DESPUÉS de authMiddleware
  // para que req.user ya exista
  
  if (!req.user) {
    return res.status(401).json({ message: 'Usuario no autenticado' });
  }

  if (req.user.rol !== 'admin') {
    return res.status(403).json({ message: 'Acceso denegado. Se requiere rol de administrador.' });
  }

  next();
};

module.exports = adminMiddleware;
```

## Ejemplo: validationMiddleware.js

Middleware para validar datos de entrada.

```javascript
const validateEventData = (req, res, next) => {
  const { titulo, descripcion, fecha, ubicacion } = req.body;

  // Validar campos requeridos
  if (!titulo || !fecha || !ubicacion) {
    return res.status(400).json({
      message: 'Faltan campos requeridos',
      required: ['titulo', 'fecha', 'ubicacion'],
    });
  }

  // Validar que la fecha sea futura
  const eventDate = new Date(fecha);
  if (eventDate < new Date()) {
    return res.status(400).json({
      message: 'La fecha del evento debe ser futura',
    });
  }

  // Si todo está bien, continuar
  next();
};

module.exports = { validateEventData };
```

## Ejemplo: errorHandler.js

Middleware global para manejo de errores (debe ser el último).

```javascript
const errorHandler = (err, req, res, next) => {
  console.error('Error:', err);

  // Error de validación de Mongoose
  if (err.name === 'ValidationError') {
    const errors = Object.values(err.errors).map(e => e.message);
    return res.status(400).json({
      message: 'Error de validación',
      errors,
    });
  }

  // Error de duplicado (código 11000 de MongoDB)
  if (err.code === 11000) {
    return res.status(400).json({
      message: 'Ya existe un registro con esos datos',
    });
  }

  // Error genérico
  res.status(err.status || 500).json({
    message: err.message || 'Error del servidor',
  });
};

module.exports = errorHandler;
```

## Uso de middlewares en rutas

### Middleware en una ruta específica
```javascript
router.post('/events', authMiddleware, eventController.createEvent);
```

### Múltiples middlewares en secuencia
```javascript
router.post('/admin/users', authMiddleware, adminMiddleware, adminController.createUser);
```

### Middleware aplicado a todas las rutas de un router
```javascript
const router = express.Router();

// Este middleware se aplicará a todas las rutas de este router
router.use(authMiddleware);

router.get('/profile', userController.getProfile);
router.put('/profile', userController.updateProfile);
```

## Middlewares a implementar

- `authMiddleware.js` - Verificar autenticación JWT
- `adminMiddleware.js` - Verificar rol de administrador
- `validationMiddleware.js` - Validar datos de entrada
- `errorHandler.js` - Manejo global de errores
- `corsConfig.js` - Configuración de CORS (opcional)

## Orden de ejecución

El orden de los middlewares es importante:

```javascript
// En server.js
app.use(express.json()); // 1. Parsear JSON
app.use(cors()); // 2. Configurar CORS

// Rutas
app.use('/api/auth', authRoutes);
app.use('/api/events', eventRoutes);

// Middleware de error (debe ser el último)
app.use(errorHandler); // Último
```

## Buenas prácticas

✅ Mantener middlewares pequeños y enfocados  
✅ Documentar el propósito de cada middleware  
✅ Siempre llamar a `next()` o enviar una respuesta  
✅ Manejar errores apropiadamente  
✅ Reutilizar middlewares en múltiples rutas  
✅ El middleware de errores debe ser el último
