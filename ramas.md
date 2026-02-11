# Estrategia de Ramas - Plataforma de Eventos Comunitarios

## 🌳 Ramas principales

### `main`
- Rama de producción
- Siempre debe estar estable y desplegable
- Solo se hace merge desde ramas de desarrollo después de revisión
- **Protegida:** Requiere Pull Request y revisión

### `develop` (opcional - para equipos grandes)
- Rama de integración
- Contiene las últimas características desarrolladas
- Se usa si quieres tener una rama intermedia antes de producción

## 🔀 Estrategia de branching

Para este proyecto, usaremos **Feature Branch Workflow**:

```
main
 ├── feature/nombre-descriptivo
 ├── bugfix/nombre-descriptivo
 └── hotfix/nombre-descriptivo
```

## 📋 Ramas para Sprint 1

Basándose en las 11 issues del Sprint 1, crear estas ramas:

### Setup e Infraestructura
```bash
git checkout -b feature/setup-frontend
git checkout -b feature/setup-backend
git checkout -b feature/mongodb-connection
```

### Modelos y Autenticación Backend
```bash
git checkout -b feature/user-model
git checkout -b feature/auth-register
git checkout -b feature/auth-login
git checkout -b feature/jwt-middleware
```

### Componentes Frontend
```bash
git checkout -b feature/register-component
git checkout -b feature/login-component
git checkout -b feature/react-router
git checkout -b feature/auth-service
```

## 📋 Ramas para Sprint 2

### Backend
```bash
git checkout -b feature/event-model
git checkout -b feature/events-crud
git checkout -b feature/event-registration
git checkout -b feature/user-profile
```

### Frontend
```bash
git checkout -b feature/events-list-page
git checkout -b feature/create-event-form
git checkout -b feature/event-detail-page
git checkout -b feature/profile-page
git checkout -b feature/event-service
```

## 📋 Ramas para Sprint 3

### Comentarios
```bash
git checkout -b feature/comment-model
git checkout -b feature/comments-endpoints
git checkout -b feature/comments-ui
```

### Administración
```bash
git checkout -b feature/admin-middleware
git checkout -b feature/admin-endpoints
git checkout -b feature/admin-page
```

### Deployment
```bash
git checkout -b feature/frontend-deployment
git checkout -b feature/backend-deployment
git checkout -b feature/documentation
```

## 🚀 Flujo de trabajo

### 1. Crear una nueva rama
```bash
# Asegurarte de estar en main y actualizado
git checkout main
git pull origin main

# Crear rama para tu feature
git checkout -b feature/nombre-descriptivo
```

### 2. Trabajar en la rama
```bash
# Hacer commits
git add .
git commit -m "feat: descripción del cambio"

# Subir la rama al repositorio remoto
git push -u origin feature/nombre-descriptivo
```

### 3. Crear Pull Request
- Ir a GitHub
- Hacer clic en "Compare & pull request"
- Completar el template del PR
- Asignar revisores
- Esperar feedback y hacer cambios si es necesario

### 4. Merge después de aprobación
```bash
# El revisor o el autor hace merge desde GitHub
# Después, actualizar local
git checkout main
git pull origin main

# Eliminar la rama local (opcional)
git branch -d feature/nombre-descriptivo
```

## 🎯 Convenciones de nombres

### Feature branches
- **Formato:** `feature/nombre-corto-descriptivo`
- **Ejemplos:**
  - `feature/user-authentication`
  - `feature/event-list`
  - `feature/admin-panel`

### Bugfix branches
- **Formato:** `bugfix/descripcion-del-bug`
- **Ejemplos:**
  - `bugfix/login-validation`
  - `bugfix/event-date-format`

### Hotfix branches (para bugs críticos en producción)
- **Formato:** `hotfix/descripcion-urgente`
- **Ejemplos:**
  - `hotfix/security-jwt`
  - `hotfix/database-connection`

## 📝 Convenciones de commits

Usar **Conventional Commits**:

```bash
feat: nueva funcionalidad
fix: corrección de bug
docs: cambios en documentación
style: cambios de formato (sin afectar código)
refactor: refactorización de código
test: agregar o modificar tests
chore: tareas de mantenimiento
```

**Ejemplos:**
```bash
git commit -m "feat: agregar componente de registro de usuarios"
git commit -m "fix: corregir validación de email en login"
git commit -m "docs: actualizar README con instrucciones de instalación"
git commit -m "style: formatear código con prettier"
```

## 🔒 Protección de ramas

### Configurar en GitHub:

1. Ir a **Settings** → **Branches**
2. Agregar regla para `main`:
   - ✅ Require pull request reviews before merging (1-2 reviewers)
   - ✅ Require status checks to pass
   - ✅ Require branches to be up to date before merging
   - ✅ Include administrators

## 👥 Asignación de ramas por persona

### Ejemplo para 8 personas:

**Frontend Team (4 personas):**
- **Persona 1:** `feature/setup-frontend`, `feature/register-component`
- **Persona 2:** `feature/login-component`, `feature/auth-service`
- **Persona 3:** `feature/react-router`, `feature/events-list-page`
- **Persona 4:** `feature/create-event-form`, `feature/event-detail-page`

**Backend Team (4 personas):**
- **Persona 5:** `feature/setup-backend`, `feature/mongodb-connection`
- **Persona 6:** `feature/user-model`, `feature/auth-register`
- **Persona 7:** `feature/auth-login`, `feature/jwt-middleware`
- **Persona 8:** `feature/event-model`, `feature/events-crud`

## 📊 Visualización del flujo

```
main (producción)
  │
  ├─── feature/setup-frontend ──┐
  │                              ├─→ PR → merge a main
  ├─── feature/setup-backend ───┘
  │
  ├─── feature/user-model ──────┐
  │                              ├─→ PR → merge a main
  ├─── feature/auth-register ───┘
  │
  └─── ... (continuar con más features)
```

## ✅ Checklist antes de crear PR

- [ ] El código funciona correctamente en local
- [ ] Se hicieron commits con mensajes descriptivos
- [ ] El código sigue las convenciones del proyecto
- [ ] Se actualizó la documentación si es necesario
- [ ] Se probó en diferentes escenarios
- [ ] La rama está actualizada con `main`

## 🔄 Mantener rama actualizada

Si tu rama tiene varios días de trabajo y `main` ha cambiado:

```bash
# Estando en tu rama feature
git checkout feature/tu-rama
git fetch origin
git rebase origin/main

# O si prefieres merge
git merge origin/main
```

## 📚 Recursos adicionales

- [Git Flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)
