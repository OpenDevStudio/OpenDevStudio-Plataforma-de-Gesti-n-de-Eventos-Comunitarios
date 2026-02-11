# Components

Esta carpeta contiene componentes reutilizables de React que se usan en múltiples páginas de la aplicación.

## Convenciones

- **Nombre de archivos:** PascalCase (ejemplo: `EventCard.jsx`, `Navbar.jsx`)
- **Estructura:** Un componente por archivo
- **Exportación:** Usar `export default` para el componente principal
- **Props:** Documentar props con comentarios o PropTypes

## Estructura recomendada de un componente

```jsx
import React from 'react';
import './ComponentName.css'; // Si tiene estilos propios

const ComponentName = ({ prop1, prop2 }) => {
  return (
    <div className="component-name">
      {/* JSX del componente */}
    </div>
  );
};

export default ComponentName;
```

## Ejemplos de componentes a crear

### EventCard.jsx
Tarjeta para mostrar un evento en la lista.

```jsx
import React from 'react';

const EventCard = ({ event }) => {
  return (
    <div className="event-card">
      <h3>{event.titulo}</h3>
      <p>{event.descripcion}</p>
      <small>{new Date(event.fecha).toLocaleDateString()}</small>
    </div>
  );
};

export default EventCard;
```

### Navbar.jsx
Barra de navegación principal.

```jsx
import React from 'react';
import { Link } from 'react-router-dom';

const Navbar = ({ user, onLogout }) => {
  return (
    <nav className="navbar">
      <Link to="/">Inicio</Link>
      <Link to="/eventos">Eventos</Link>
      {user ? (
        <>
          <Link to="/perfil">Perfil</Link>
          <button onClick={onLogout}>Cerrar Sesión</button>
        </>
      ) : (
        <>
          <Link to="/login">Login</Link>
          <Link to="/register">Registro</Link>
        </>
      )}
    </nav>
  );
};

export default Navbar;
```

## Buenas prácticas

✅ Mantener componentes pequeños y enfocados  
✅ Extraer lógica compleja en custom hooks  
✅ Usar nombres descriptivos  
✅ Documentar props complejas  
✅ Reutilizar componentes en lugar de duplicar código
