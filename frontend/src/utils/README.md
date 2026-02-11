# Utils (Frontend)

Esta carpeta contiene funciones auxiliares y utilidades reutilizables para el frontend.

## Convenciones

- **Nombre de archivos:** camelCase (ejemplo: `formatDate.js`, `validators.js`)
- **Exportar funciones** individuales o como objeto
- **Mantener funciones puras** cuando sea posible (sin efectos secundarios)

## Ejemplo: formatDate.js

Utilidades para formatear fechas.

```javascript
/**
 * Formatea una fecha en formato local
 * @param {Date|string} date - Fecha a formatear
 * @returns {string} Fecha formateada
 */
export const formatDate = (date) => {
  const d = new Date(date);
  return d.toLocaleDateString('es-ES', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  });
};

/**
 * Formatea la hora
 * @param {string} timeString - Hora en formato HH:MM
 * @returns {string} Hora formateada
 */
export const formatTime = (timeString) => {
  const [hours, minutes] = timeString.split(':');
  return `${hours}:${minutes}`;
};

/**
 * Verifica si una fecha es futura
 * @param {Date|string} date - Fecha a verificar
 * @returns {boolean}
 */
export const isFutureDate = (date) => {
  return new Date(date) > new Date();
};
```

## Ejemplo: validators.js

Funciones de validación.

```javascript
/**
 * Valida un email
 * @param {string} email
 * @returns {boolean}
 */
export const isValidEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

/**
 * Valida que una contraseña sea segura
 * @param {string} password
 * @returns {boolean}
 */
export const isStrongPassword = (password) => {
  return password.length >= 6;
};

/**
 * Valida que dos contraseñas coincidan
 * @param {string} password1
 * @param {string} password2
 * @returns {boolean}
 */
export const passwordsMatch = (password1, password2) => {
  return password1 === password2;
};
```

## Ejemplo: constants.js

Constantes usadas en el frontend.

```javascript
export const EVENT_CATEGORIES = [
  'Deportes',
  'Cultura',
  'Tecnología',
  'Educación',
  'Social',
  'Otro',
];

export const USER_ROLES = {
  USER: 'usuario',
  ADMIN: 'admin',
};

export const API_ROUTES = {
  AUTH: {
    LOGIN: '/auth/login',
    REGISTER: '/auth/register',
  },
  EVENTS: {
    BASE: '/events',
    BY_ID: (id) => `/events/${id}`,
    REGISTER: (id) => `/events/${id}/register`,
  },
};
```

## Ejemplo: localStorage.js

Utilidades para manejar localStorage.

```javascript
/**
 * Guarda un valor en localStorage
 * @param {string} key
 * @param {any} value
 */
export const setItem = (key, value) => {
  try {
    localStorage.setItem(key, JSON.stringify(value));
  } catch (error) {
    console.error('Error guardando en localStorage:', error);
  }
};

/**
 * Obtiene un valor de localStorage
 * @param {string} key
 * @returns {any}
 */
export const getItem = (key) => {
  try {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) : null;
  } catch (error) {
    console.error('Error leyendo de localStorage:', error);
    return null;
  }
};

/**
 * Elimina un valor de localStorage
 * @param {string} key
 */
export const removeItem = (key) => {
  try {
    localStorage.removeItem(key);
  } catch (error) {
    console.error('Error eliminando de localStorage:', error);
  }
};

/**
 * Limpia todo localStorage
 */
export const clear = () => {
  try {
    localStorage.clear();
  } catch (error) {
    console.error('Error limpiando localStorage:', error);
  }
};
```

## Ejemplo: errorMessages.js

Mensajes de error centralizados.

```javascript
export const ERROR_MESSAGES = {
  NETWORK: 'Error de conexión. Verifica tu internet.',
  UNAUTHORIZED: 'Debes iniciar sesión para continuar.',
  FORBIDDEN: 'No tienes permisos para esta acción.',
  NOT_FOUND: 'El recurso solicitado no existe.',
  SERVER_ERROR: 'Error del servidor. Intenta más tarde.',
  VALIDATION: {
    EMAIL: 'El email no es válido.',
    PASSWORD_LENGTH: 'La contraseña debe tener al menos 6 caracteres.',
    PASSWORDS_MATCH: 'Las contraseñas no coinciden.',
    REQUIRED: 'Este campo es requerido.',
  },
};

/**
 * Obtiene el mensaje de error apropiado según el código de estado HTTP
 * @param {number} statusCode
 * @returns {string}
 */
export const getErrorMessage = (statusCode) => {
  switch (statusCode) {
    case 401:
      return ERROR_MESSAGES.UNAUTHORIZED;
    case 403:
      return ERROR_MESSAGES.FORBIDDEN;
    case 404:
      return ERROR_MESSAGES.NOT_FOUND;
    case 500:
      return ERROR_MESSAGES.SERVER_ERROR;
    default:
      return 'Ha ocurrido un error inesperado.';
  }
};
```

## Utilidades a implementar

- `formatDate.js` - Formato de fechas
- `validators.js` - Validaciones
- `constants.js` - Constantes del frontend
- `localStorage.js` - Manejo de localStorage
- `errorMessages.js` - Mensajes de error centralizados

## Buenas prácticas

✅ Crear funciones pequeñas y reutilizables  
✅ Documentar funciones con JSDoc  
✅ Preferir funciones puras cuando sea posible  
✅ Exportar funciones individualmente  
✅ Escribir tests para funciones críticas
