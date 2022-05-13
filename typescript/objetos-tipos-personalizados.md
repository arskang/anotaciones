###### Navegación
| ◀ | 🏠 | ▶︎ |
| - | - | - |
| [Funciones y objetos](./funciones-objetos.md) | [Inicio](./README.md) | [Depuración de errores y el archivo tsconfig.json](./depuracion-tsconfig.md) |

### Objetos y tipos personalizados

- **Type**: es posible crear tipos de datos personalizados, pueden generarse con tipos de datos primitivos, uniones o tuplas . En los objetos es posible separar las propiedades con ```,``` o ```;``` [⬆️](#navegación)
```ts
// Correcto
type Name = string; // ✅

// Correcto
type ID = string | number; // ✅

// Correcto
type Person = {
    name: string,
    age: number,
    skills: string[],
    getName?: () => string,
} // ✅

// Correcto
type PersonTwo = {
    name: string;
    age: number;
    skills: string[];
    getName?: () => string;
} // ✅
```