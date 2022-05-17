###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Interfaces](./interfaces.md) | [Inicio](./README.md) | [Genéricos](./generics.md) |

### NameSpaces

- **Namespace básico**: no es muy normal utilizar los ```namespaces``` en proyectos; su declaración es muy parecida a una clase y dentro de ella se pueden declarar ```n``` métodos, variables o clases [⬆️](#navegación)
```ts
// Correcto
namespace Validations {
    const validateText = (text: string): boolean => {
        return text.length > 0;
    }
} // ✅
```

- **Imports y exports**: al igual que las nuevas versiones de ```javascript``` en ```typescript``` es posible importar y exportar con las palabras reservadas: ```import``` y ```export```; esta implementación estuvo antes en ```typescript``` [⬆️](#navegación)
```ts
//# archivo1.ts

// Correcto
export const getHello = (name: string): string => {
    return `Hello ${name}`;
} // ✅
```

```ts
//# archivo2.ts

// Correcto
import { getHello } from './archivo1' // ✅
```

- **Importación con alias**: para asignar un alias se debe utilizar la palabra reservada ```as``` [⬆️](#navegación)
```ts
//# archivo2.ts

// Correcto
import { getHello as getGreeter } from './archivo1' // ✅
```

- **Export default**: a diferencia de las exportaciones comunes, en una exportación por default; es posible asignarle el nombre que se desee al importar. Los dos tipos de exportaciones pueden coexistir [⬆️](#navegación)
```ts
//# archivo1.ts

// Correcto
export const getHello = (name: string): string => {
    return `Hello ${name}`;
} // ✅

// Correcto
export default (a: number, b: number): number => {
    return a + b;
} // ✅
```

```ts
//# archivo2.ts

// Correcto
import sumTwoNumbers, { getHello } from './archivo1' // ✅
```