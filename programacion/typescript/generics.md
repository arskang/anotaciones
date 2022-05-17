###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [NameSpaces](./namespaces.md) | [Inicio](./README.md) | [Decoradores](./decoradores.md) |

### Genéricos

- **Funciones genéricas**: es como una plantilla de código, sirve para reutilizar código y evitar el uso del tipo ```any```; como estándar se utiliza ```T``` para su declaración entre ```<>``` y siempre antes del paréntesis de la función; se pueden recibir ```n``` tipos separados por ```,```. El tipo se puede definir en los parámetros o antes de utilizar la función [⬆️](#navegación)
```ts
// Correcto
// En este caso la S no se utiliza,
// pero de esta forma se pueden definir «n» tipos
export const genericFunction = <T, S>(argument: T): T => {
    return argument;
} // ✅

// Correcto
const hello: string = 'Hello world'; // ✅
const three: number = 3; // ✅

// Correcto
// En este caso la salida devuelve el tipo de entrada
console.log(genericFunction(hello).toUpperCase()); // ✅
console.log(genericFunction(three).toFixed(2)); // ✅
console.log(genericFunction<boolean>(true).valueOf()); // ✅
console.log(genericFunction<string[]>(['Hello', 'World']).join(' ')); // ✅
```

- **Agrupar exportaciones**: una forma eficiente de manejar exportaciones de diferentes archivos, es crear un archivo ```index``` que se encargue de exportar todo nuestros métodos [⬆️](#navegación)
```ts
// # index.ts

// Correcto
export { getGreeter } from './archivo1';  // ✅
export { sumTwoNumbers } from './archivo2';  // ✅
```