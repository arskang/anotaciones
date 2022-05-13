###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Tipos básicos](./tipos-basicos.md) | [Inicio](./README.md) | [Objetos y tipos personalizados](./objetos-tipos-personalizados.md) |

### Funciones y objetos

- **Funciones básicas**: aunque ```typescript``` puede interpretar el tipo de dato a retornar es recomendable definir la salida [⬆️](#navegación)
```ts
// Correcto
function sayHello(): string {
    return 'Hello';
} // ✅

// Correcto
const sayHelloTwo = (): void => {
    console.log('Hello');
}; // ✅
```

- **Parámetros obligatorios**: de manera predeterminado los parámetros definidos en una función son obligatorios [⬆️](#navegación)
```ts
// Correcto
function sayHello(msg: string): string {
    return `Hello ${msg}`;
} // ✅

// Correcto
sayHello('Bruce'); // ✅

// Incorrecto
sayHello(); // ❌
sayHello(null); // ❌
sayHello(true); // ❌
```

- **Parámetros opcionales**: para especificar que un parámetro es opcional se debe utilizar el siguiente símbolo ```?``` antes de la definición del tipo [⬆️](#navegación)
```ts
// Correcto
function sayHello(msg?: string): string {
    return `Hello ${msg || ''}`;
} // ✅

// Correcto
sayHello('Bruce'); // ✅
sayHello(); // ✅

// Incorrecto
sayHello(null); // ❌
sayHello(true); // ❌
```

- **Parámetros por defecto**: cuando se asigna un valor predeterminado a un parámetro se puede considerar que es un dato opcional [⬆️](#navegación)
```ts
// Correcto
function sayHello(msg?: string, scream: boolean = false): string {
    const message = `Hello ${msg || ''}`
    return scream ? message.toUpperCase() : message;
} // ✅

// Correcto
sayHello('Bruce', true); // ✅
sayHello('Bruce'); // ✅
sayHello(); // ✅
```

- **Parámetros REST**: utilizando el ```spread operator```se puede obtener el ```resto```de los parámetros no definidos que se envían a la función. Al devolver el valor como un arreglo también es posible utilizar ```tuples``` [⬆️](#navegación)
```ts
// Correcto
function getFullName(name: string, ...restNames: string[]): string {
    return `${name} ${restNames.join(' ')}`;
} // ✅

// Correcto
getFullName('Peter', 'Benjamin', 'Parker'); // ✅
getFullName('Peter'); // ✅

// Incorrecto
getFullName(); // ❌
getFullName(null); // ❌
getFullName(true); // ❌

// == TUPLES ==

// Correcto
function getFullNameTwo(...restNames: [string, string]): string {
    return restNames.join(' ');
} // ✅

// Correcto
getFullNameTwo('Peter', 'Parker'); // ✅

// Incorrecto
getFullNameTwo(); // ❌
getFullNameTwo(null); // ❌
getFullNameTwo(true); // ❌
getFullNameTwo('Peter'); // ❌
getFullNameTwo('Peter', 'Benjamin', 'Parker'); // ❌
```

- **Tipo Function**: es posible definir el tipo de dato ```Function```a una variable (acepta todo tipo de funciones) o especificar cada uno de los parámetros y salida de nuestra función, sintáxis: ```:(<...parámetros>) => <salida>``` [⬆️](#navegación)

```ts
// Correcto
let myFunction: Function; // ✅

// Correcto
myFunction = (a: number, b: number): number => {
    return a + b;
} // ✅
myFunction = (): void => {
    console.log('Hello');
} // ✅


// Correcto
let myFunctionSum: (a: number, b: number) => number; // ✅

// Correcto
myFunctionSum = (a: number, b: number): number => {
    return a + b;
} // ✅

// Incorrecto
myFunctionSum = (): void => {
    console.log('Hello');
} // ❌
```