# Controllers

Esta carpeta contiene los controladores que manejan la lógica de negocio de la aplicación.

## Convenciones

- **Nombre de archivos:** camelCase (ejemplo: `authController.js`, `eventController.js`)
- **Estructura:** Exportar funciones individuales para cada operación
- **Responsabilidad:** Procesar peticiones, validar datos, interactuar con modelos, enviar respuestas

## Estructura básica de un controlador

```javascript
const Model = require('../models/Model');

// Función para operación específica
exports.operationName = async (req, res) => {
  try {
    // 1. Extraer datos del request
    const { param1, param2 } = req.body;

    // 2. Validaciones
    if (!param1) {
      return res.status(400).json({ message: 'Param1 es requerido' });
    }

    // 3. Lógica de negocio
    const result = await Model.find({ param1 });

    // 4. Enviar respuesta
    res.status(200).json(result);
  } catch (error) {
    console.error('Error en operationName:', error);
    res.status(500).json({ message: 'Error del servidor' });
  }
};
```

## Ejemplo: authController.js

```javascript
const User = require('../models/User');
const jwt = require('jsonwebtoken');

// Registrar un nuevo usuario
exports.register = async (req, res) => {
  try {
    const { nombre, email, password } = req.body;

    // Validaciones
    if (!nombre || !email || !password) {
      return res.status(400).json({ message: 'Todos los campos son requeridos' });
    }

    // Verificar si el email ya existe
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(400).json({ message: 'El email ya está registrado' });
    }

    // Crear usuario
    const user = new User({ nombre, email, password });
    await user.save();

    res.status(201).json({ message: 'Usuario registrado exitosamente' });
  } catch (error) {
    console.error('Error en register:', error);
    res.status(500).json({ message: 'Error del servidor' });
  }
};

// Iniciar sesión
exports.login = async (req, res) => {
  try {
    const { email, password } = req.body;

    // Validaciones
    if (!email || !password) {
      return res.status(400).json({ message: 'Email y contraseña requeridos' });
    }

    // Buscar usuario
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(401).json({ message: 'Credenciales incorrectas' });
    }

    // Verificar contraseña
    const isMatch = await user.comparePassword(password);
    if (!isMatch) {
      return res.status(401).json({ message: 'Credenciales incorrectas' });
    }

    // Generar token JWT
    const token = jwt.sign(
      { id: user._id, email: user.email },
      process.env.JWT_SECRET,
      { expiresIn: '7d' }
    );

    res.json({
      token,
      user: {
        id: user._id,
        nombre: user.nombre,
        email: user.email,
        rol: user.rol,
      },
    });
  } catch (error) {
    console.error('Error en login:', error);
    res.status(500).json({ message: 'Error del servidor' });
  }
};
```

## Ejemplo: eventController.js

```javascript
const Event = require('../models/Event');

// Obtener todos los eventos
exports.getAllEvents = async (req, res) => {
  try {
    const events = await Event.find()
      .populate('organizador', 'nombre email')
      .sort({ fecha: 1 });
    res.json(events);
  } catch (error) {
    console.error('Error en getAllEvents:', error);
    res.status(500).json({ message: 'Error del servidor' });
  }
};

// Crear un nuevo evento
exports.createEvent = async (req, res) => {
  try {
    const { titulo, descripcion, fecha, hora, ubicacion, categoria, cupoMaximo } = req.body;

    // Validaciones
    if (!titulo || !fecha || !ubicacion) {
      return res.status(400).json({ message: 'Campos requeridos faltantes' });
    }

    const event = new Event({
      titulo,
      descripcion,
      fecha,
      hora,
      ubicacion,
      categoria,
      cupoMaximo,
      organizador: req.user.id, // Del middleware de autenticación
    });

    await event.save();
    res.status(201).json(event);
  } catch (error) {
    console.error('Error en createEvent:', error);
    res.status(500).json({ message: 'Error del servidor' });
  }
};
```

## Controladores a implementar

- `authController.js` - Registro, login
- `eventController.js` - CRUD de eventos, inscripción
- `userController.js` - Perfil de usuario
- `commentController.js` - CRUD de comentarios
- `adminController.js` - Operaciones de administración

## Buenas prácticas

✅ Validar todos los datos de entrada  
✅ Usar try-catch para manejo de errores  
✅ Retornar códigos de estado HTTP apropiados  
✅ No exponer información sensible en errores  
✅ Documentar funciones complejas con comentarios  
✅ Mantener controladores enfocados y no muy largos
