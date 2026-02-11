# Utils (Backend)

Esta carpeta contiene funciones auxiliares y utilidades reutilizables para el backend.

## Convenciones

- **Nombre de archivos:** camelCase (ejemplo: `emailValidator.js`, `tokenGenerator.js`)
- **Exportar funciones** individuales o como objeto
- **Mantener funciones puras** cuando sea posible

## Ejemplo: emailValidator.js

Validación de emails.

```javascript
/**
 * Valida si un string es un email válido
 * @param {string} email
 * @returns {boolean}
 */
const isValidEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

module.exports = { isValidEmail };
```

## Ejemplo: responseHandler.js

Funciones para estandarizar respuestas HTTP.

```javascript
/**
 * Envía una respuesta de éxito
 * @param {object} res - Objeto response de Express
 * @param {number} statusCode - Código de estado HTTP
 * @param {any} data - Datos a enviar
 * @param {string} message - Mensaje opcional
 */
const sendSuccess = (res, statusCode = 200, data, message = 'Success') => {
  res.status(statusCode).json({
    success: true,
    message,
    data,
  });
};

/**
 * Envía una respuesta de error
 * @param {object} res - Objeto response de Express
 * @param {number} statusCode - Código de estado HTTP
 * @param {string} message - Mensaje de error
 * @param {any} errors - Detalles adicionales del error
 */
const sendError = (res, statusCode = 500, message = 'Error', errors = null) => {
  res.status(statusCode).json({
    success: false,
    message,
    ...(errors && { errors }),
  });
};

module.exports = {
  sendSuccess,
  sendError,
};
```

## Ejemplo: dateHelpers.js

Funciones para trabajar con fechas.

```javascript
/**
 * Verifica si una fecha es futura
 * @param {Date|string} date
 * @returns {boolean}
 */
const isFutureDate = (date) => {
  return new Date(date) > new Date();
};

/**
 * Agrega días a una fecha
 * @param {Date} date
 * @param {number} days
 * @returns {Date}
 */
const addDays = (date, days) => {
  const result = new Date(date);
  result.setDate(result.getDate() + days);
  return result;
};

/**
 * Calcula la diferencia en días entre dos fechas
 * @param {Date} date1
 * @param {Date} date2
 * @returns {number}
 */
const daysDifference = (date1, date2) => {
  const diffTime = Math.abs(date2 - date1);
  return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
};

module.exports = {
  isFutureDate,
  addDays,
  daysDifference,
};
```

## Ejemplo: asyncHandler.js

Wrapper para manejar errores en funciones asíncronas.

```javascript
/**
 * Wrapper para manejar errores en controladores async
 * Evita tener que usar try-catch en cada controlador
 * @param {Function} fn - Función async del controlador
 * @returns {Function}
 */
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

module.exports = asyncHandler;
```

**Uso en un controlador:**
```javascript
const asyncHandler = require('../utils/asyncHandler');

exports.getAllEvents = asyncHandler(async (req, res) => {
  const events = await Event.find();
  res.json(events);
  // No necesitas try-catch, asyncHandler se encarga de los errores
});
```

## Ejemplo: logger.js

Funciones de logging personalizadas.

```javascript
const colors = {
  reset: '\x1b[0m',
  red: '\x1b[31m',
  green: '\x1b[32m',
  yellow: '\x1b[33m',
  blue: '\x1b[34m',
};

const log = {
  info: (message) => {
    console.log(`${colors.blue}[INFO]${colors.reset} ${message}`);
  },
  success: (message) => {
    console.log(`${colors.green}[SUCCESS]${colors.reset} ${message}`);
  },
  warning: (message) => {
    console.log(`${colors.yellow}[WARNING]${colors.reset} ${message}`);
  },
  error: (message) => {
    console.error(`${colors.red}[ERROR]${colors.reset} ${message}`);
  },
};

module.exports = log;
```

## Ejemplo: pagination.js

Utilidad para paginar resultados.

```javascript
/**
 * Aplica paginación a una query de Mongoose
 * @param {object} query - Query de Mongoose
 * @param {number} page - Número de página (default 1)
 * @param {number} limit - Elementos por página (default 10)
 * @returns {Promise<object>} Resultados paginados
 */
const paginate = async (query, page = 1, limit = 10) => {
  const skip = (page - 1) * limit;
  
  const [data, total] = await Promise.all([
    query.skip(skip).limit(limit),
    query.model.countDocuments(query.getQuery()),
  ]);

  return {
    data,
    pagination: {
      page,
      limit,
      total,
      pages: Math.ceil(total / limit),
      hasNext: page < Math.ceil(total / limit),
      hasPrev: page > 1,
    },
  };
};

module.exports = { paginate };
```

## Utilidades a implementar

- `emailValidator.js` - Validar emails
- `responseHandler.js` - Estandarizar respuestas HTTP
- `dateHelpers.js` - Funciones para fechas
- `asyncHandler.js` - Manejo de errores async
- `logger.js` - Logging personalizado
- `pagination.js` - Paginar resultados (opcional)

## Buenas prácticas

✅ Crear funciones pequeñas y reutilizables  
✅ Documentar funciones con JSDoc  
✅ Mantener funciones sin efectos secundarios cuando sea posible  
✅ Escribir tests para funciones críticas  
✅ Usar nombres descriptivos
