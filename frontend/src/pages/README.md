# Pages

Esta carpeta contiene las páginas principales de la aplicación. Cada archivo representa una ruta específica.

## Convenciones

- **Nombre de archivos:** PascalCase (ejemplo: `Login.jsx`, `Events.jsx`)
- **Una página por archivo**
- **Usar componentes** de la carpeta `components` para construir las páginas
- **Gestionar estado local** con `useState` y `useEffect`

## Estructura recomendada de una página

```jsx
import React, { useState, useEffect } from 'react';
import { useNavigate } from 'react-router-dom';
import ComponentName from '../components/ComponentName';
import someService from '../services/someService';

const PageName = () => {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const navigate = useNavigate();

  useEffect(() => {
    // Cargar datos al montar el componente
    const fetchData = async () => {
      try {
        const result = await someService.getData();
        setData(result);
      } catch (error) {
        console.error('Error:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) return <div>Cargando...</div>;

  return (
    <div className="page-name">
      <h1>Título de la Página</h1>
      {/* Contenido */}
    </div>
  );
};

export default PageName;
```

## Ejemplos de páginas

### Login.jsx
Página de inicio de sesión.

```jsx
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import authService from '../services/authService';

const Login = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await authService.login(email, password);
      navigate('/eventos');
    } catch (err) {
      setError('Credenciales incorrectas');
    }
  };

  return (
    <div className="login-page">
      <h2>Iniciar Sesión</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <input
          type="password"
          placeholder="Contraseña"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        {error && <p className="error">{error}</p>}
        <button type="submit">Ingresar</button>
      </form>
    </div>
  );
};

export default Login;
```

### Events.jsx
Página que lista todos los eventos.

```jsx
import React, { useState, useEffect } from 'react';
import EventCard from '../components/EventCard';
import eventService from '../services/eventService';

const Events = () => {
  const [events, setEvents] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchEvents = async () => {
      try {
        const data = await eventService.getAllEvents();
        setEvents(data);
      } catch (error) {
        console.error('Error al cargar eventos:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchEvents();
  }, []);

  if (loading) return <div>Cargando eventos...</div>;

  return (
    <div className="events-page">
      <h1>Eventos Disponibles</h1>
      <div className="events-grid">
        {events.map(event => (
          <EventCard key={event._id} event={event} />
        ))}
      </div>
    </div>
  );
};

export default Events;
```

## Páginas a implementar

- `Login.jsx` - Inicio de sesión
- `Register.jsx` - Registro de usuario
- `Events.jsx` - Lista de eventos
- `EventDetail.jsx` - Detalle de un evento
- `CreateEvent.jsx` - Formulario para crear evento
- `Profile.jsx` - Perfil del usuario
- `Admin.jsx` - Panel de administración

## Buenas prácticas

✅ Separar lógica compleja en custom hooks  
✅ Manejar estados de carga y error  
✅ Validar datos antes de enviar formularios  
✅ Usar servicios para llamadas a la API  
✅ Implementar navegación con React Router
