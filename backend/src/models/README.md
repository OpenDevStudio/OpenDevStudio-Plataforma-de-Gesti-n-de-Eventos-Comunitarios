# Models

Esta carpeta contiene los modelos de datos usando Mongoose para MongoDB.

## Convenciones

- **Nombre de archivos:** PascalCase (ejemplo: `User.js`, `Event.js`)
- **Un modelo por archivo**
- **Definir validaciones** en el esquema
- **Usar middlewares** para lógica pre/post guardado

## Estructura básica de un modelo

```javascript
const mongoose = require('mongoose');

const modelSchema = new mongoose.Schema(
  {
    campo1: {
      type: String,
      required: [true, 'Campo1 es requerido'],
      trim: true,
    },
    campo2: {
      type: Number,
      default: 0,
    },
  },
  {
    timestamps: true, // Agrega createdAt y updatedAt automáticamente
  }
);

// Middleware o métodos del modelo
modelSchema.methods.customMethod = function() {
  // Lógica personalizada
};

module.exports = mongoose.model('ModelName', modelSchema);
```

## Ejemplo: User.js

```javascript
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

const userSchema = new mongoose.Schema(
  {
    nombre: {
      type: String,
      required: [true, 'El nombre es requerido'],
      trim: true,
    },
    email: {
      type: String,
      required: [true, 'El email es requerido'],
      unique: true,
      lowercase: true,
      trim: true,
      match: [/^\S+@\S+\.\S+$/, 'Email no válido'],
    },
    password: {
      type: String,
      required: [true, 'La contraseña es requerida'],
      minlength: [6, 'La contraseña debe tener al menos 6 caracteres'],
      select: false, // No incluir en queries por defecto
    },
    rol: {
      type: String,
      enum: ['usuario', 'admin'],
      default: 'usuario',
    },
  },
  {
    timestamps: true,
  }
);

// Middleware para hashear la contraseña antes de guardar
userSchema.pre('save', async function (next) {
  // Solo hashear si la contraseña fue modificada
  if (!this.isModified('password')) return next();

  try {
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
    next();
  } catch (error) {
    next(error);
  }
});

// Método para comparar contraseñas
userSchema.methods.comparePassword = async function (candidatePassword) {
  return await bcrypt.compare(candidatePassword, this.password);
};

// No incluir password al convertir a JSON
userSchema.methods.toJSON = function () {
  const user = this.toObject();
  delete user.password;
  return user;
};

module.exports = mongoose.model('User', userSchema);
```

## Ejemplo: Event.js

```javascript
const mongoose = require('mongoose');

const eventSchema = new mongoose.Schema(
  {
    titulo: {
      type: String,
      required: [true, 'El título es requerido'],
      trim: true,
      maxlength: [100, 'El título no puede exceder 100 caracteres'],
    },
    descripcion: {
      type: String,
      required: [true, 'La descripción es requerida'],
      trim: true,
    },
    fecha: {
      type: Date,
      required: [true, 'La fecha es requerida'],
    },
    hora: {
      type: String,
      required: [true, 'La hora es requerida'],
    },
    ubicacion: {
      type: String,
      required: [true, 'La ubicación es requerida'],
      trim: true,
    },
    categoria: {
      type: String,
      enum: ['Deportes', 'Cultura', 'Tecnología', 'Educación', 'Social', 'Otro'],
      default: 'Otro',
    },
    cupoMaximo: {
      type: Number,
      default: 50,
      min: [1, 'El cupo debe ser al menos 1'],
    },
    organizador: {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'User',
      required: true,
    },
    inscritos: [
      {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User',
      },
    ],
  },
  {
    timestamps: true,
  }
);

// Virtual para obtener cupos disponibles
eventSchema.virtual('cuposDisponibles').get(function () {
  return this.cupoMaximo - this.inscritos.length;
});

// Incluir virtuals al convertir a JSON
eventSchema.set('toJSON', { virtuals: true });
eventSchema.set('toObject', { virtuals: true });

module.exports = mongoose.model('Event', eventSchema);
```

## Ejemplo: Comment.js

```javascript
const mongoose = require('mongoose');

const commentSchema = new mongoose.Schema(
  {
    contenido: {
      type: String,
      required: [true, 'El contenido es requerido'],
      trim: true,
      maxlength: [500, 'El comentario no puede exceder 500 caracteres'],
    },
    autor: {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'User',
      required: true,
    },
    evento: {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'Event',
      required: true,
    },
  },
  {
    timestamps: true,
  }
);

module.exports = mongoose.model('Comment', commentSchema);
```

## Tipos de datos comunes

- `String` - Texto
- `Number` - Números
- `Date` - Fechas
- `Boolean` - Verdadero/Falso
- `ObjectId` - Referencia a otro documento
- `Array` - Listas
- `Mixed` - Cualquier tipo

## Validaciones comunes

- `required` - Campo obligatorio
- `unique` - Valor único en la colección
- `minlength` / `maxlength` - Longitud de string
- `min` / `max` - Valor numérico
- `enum` - Valores permitidos
- `match` - Expresión regular

## Modelos a implementar

- `User.js` - Usuarios
- `Event.js` - Eventos
- `Comment.js` - Comentarios

## Buenas prácticas

✅ Definir validaciones en el esquema  
✅ Usar referencias (ObjectId) para relaciones  
✅ Implementar middlewares para lógica automática  
✅ Usar virtuals para campos calculados  
✅ Documentar campos complejos  
✅ No guardar contraseñas en texto plano
