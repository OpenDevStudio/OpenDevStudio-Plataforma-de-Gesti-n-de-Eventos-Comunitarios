# Services

Esta carpeta contiene servicios para manejar las llamadas HTTP a la API del backend.

## Convenciones

- **Nombre de archivos:** camelCase (ejemplo: `authService.js`, `eventService.js`)
- **Usar axios** para las peticiones HTTP
- **Centralizar** toda la lógica de comunicación con el backend
- **Exportar** funciones específicas para cada operación

## Configuración base de axios

Crear un archivo `api.js` con la configuración base:

```javascript
import axios from 'axios';

const API_URL = process.env.REACT_APP_API_URL || 'http://localhost:5000/api';

const api = axios.create({
  baseURL: API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Interceptor para agregar el token a todas las peticiones
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

export default api;
```

## Ejemplo: authService.js

```javascript
import api from './api';

const authService = {
  // Registrar un nuevo usuario
  register: async (nombre, email, password) => {
    const response = await api.post('/auth/register', {
      nombre,
      email,
      password,
    });
    return response.data;
  },

  // Iniciar sesión
  login: async (email, password) => {
    const response = await api.post('/auth/login', { email, password });
    if (response.data.token) {
      localStorage.setItem('token', response.data.token);
      localStorage.setItem('user', JSON.stringify(response.data.user));
    }
    return response.data;
  },

  // Cerrar sesión
  logout: () => {
    localStorage.removeItem('token');
    localStorage.removeItem('user');
  },

  // Obtener el usuario actual
  getCurrentUser: () => {
    const userStr = localStorage.getItem('user');
    return userStr ? JSON.parse(userStr) : null;
  },

  // Verificar si el usuario está autenticado
  isAuthenticated: () => {
    return !!localStorage.getItem('token');
  },
};

export default authService;
```

## Ejemplo: eventService.js

```javascript
import api from './api';

const eventService = {
  // Obtener todos los eventos
  getAllEvents: async () => {
    const response = await api.get('/events');
    return response.data;
  },

  // Obtener un evento por ID
  getEventById: async (id) => {
    const response = await api.get(`/events/${id}`);
    return response.data;
  },

  // Crear un nuevo evento
  createEvent: async (eventData) => {
    const response = await api.post('/events', eventData);
    return response.data;
  },

  // Actualizar un evento
  updateEvent: async (id, eventData) => {
    const response = await api.put(`/events/${id}`, eventData);
    return response.data;
  },

  // Eliminar un evento
  deleteEvent: async (id) => {
    const response = await api.delete(`/events/${id}`);
    return response.data;
  },

  // Inscribirse a un evento
  registerToEvent: async (id) => {
    const response = await api.post(`/events/${id}/register`);
    return response.data;
  },
};

export default eventService;
```

## Manejo de errores

```javascript
// En los componentes, manejar errores así:
try {
  const data = await eventService.getAllEvents();
  setEvents(data);
} catch (error) {
  if (error.response) {
    // Error de respuesta del servidor
    console.error('Error:', error.response.data.message);
  } else if (error.request) {
    // No se recibió respuesta
    console.error('No se pudo conectar con el servidor');
  } else {
    // Otro tipo de error
    console.error('Error:', error.message);
  }
}
```

## Servicios a implementar

- `api.js` - Configuración base de axios
- `authService.js` - Autenticación
- `eventService.js` - Gestión de eventos
- `userService.js` - Gestión de perfil de usuario
- `commentService.js` - Gestión de comentarios

## Buenas prácticas

✅ Centralizar todas las llamadas HTTP en servicios  
✅ Usar interceptores para funcionalidad común (tokens)  
✅ Manejar errores apropiadamente  
✅ Usar variables de entorno para URLs  
✅ Documentar cada función del servicio
