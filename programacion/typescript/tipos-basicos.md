###### Navegación
| 🏠 | ▶︎ |
| - | - |
| [Inicio](./README.md) | [Funciones y objetos](./funciones-objetos.md) |

### Tipos de datos

La siguiente información esta basada en el ```modo: strict``` de ```typescript```; se puede configurar para evitar ciertas restricciones, pero si ese es el caso mejor usa ```javascript```

- **String**: solo permite asignar cadenas de texto [⬆️](#navegación)
```ts
// Correcto
let a: string = 'Hola mundo'; // ✅
a = "Hola mundo"; // ✅
a = `Hola ${"mundo"}`; // ✅

// Incorrecto
a = 10; // ❌
```

- **Number**: como su nombre lo indica solo permite la captura de números y ```NaN``` (javascript lo considera un número) [⬆️](#navegación)
```ts
// Correcto
let a: number = 10; // ✅
a = 11; // ✅
a = NaN; // ✅

// Incorrecto
a = 'Hola mundo'; // ❌
```

- **Boolean**: solo se puede asignar ```true``` o ```false``` [⬆️](#navegación)
```ts
// Correcto
let a: boolean = true; // ✅
a = false; // ✅

// Incorrecto
a = 10; // ❌
a = 'Hola mundo'; // ❌
```

- **Arrays**: permite asignar arreglos del tipo (o tipos) de dato definido [⬆️](#navegación)
```ts
// Correcto
const a: string[] = ['1', '2', '3', '4', '...']; // ✅
const b: (string | number)[] = ['1', 2, '3', 4, '...']; // ✅
const c: any[] = [1, '2', true, [], {}, null, undefined]; // ✅
```

- **Tuples**: se podría decir que son arreglos a los cuales se les define un tamaño y el tipo de dato para cada posición [⬆️](#navegación)
```ts
// Correcto
const a: [string, number] = ['1', 2]; // ✅
```

- **Enums**: son constantes que nos devuelven un valor númerico secuencial, por estándar se recomienda utilizar ```UpperCamelCase``` para declararlas. El valor inicial es ```0```, pero puede modficiarse el valor de cada enumeración y los siguientes que no tengan un valor definido tomaran el *valor anterior* ```+ 1``` [⬆️](#navegación)
```ts
// Correcto
enum Volumen {
    min, // 0
    medium, // 1 
    max // 2
} // ✅

// Correcto
enum NewVolumen {
    min = 3, // 3
    medium, // 4
    max // 5
} // ✅

// Uso
Volumen.min // 0
NewVolumen.min // 3
```

- **Void**: normalmente se define en funciones en las que no se retornan valores [⬆️](#navegación)
```ts
// Correcto
function getGreeter(): void {
    console.log('Helo world!')
} // ✅

// Incorrecto
function getGreeterTwo(): void {
    return 'Helo world!';
} // ❌
```

- **Never**: se define en funciones en las que siempre se debe devolver un errror [⬆️](#navegación)
```ts
// Correcto
function getError(message: string): never {
    throw new Error(message);
} // ✅

// Incorrecto
function getMessage(message: string): never {
    if (message.trim() === '') {
        throw new Error('No hay mensaje');
    }
    return message;
} // ❌
```

- **Undefined**: solo permite asignar ese tipo de valor [⬆️](#navegación)
```ts
// Correcto
let nada: undefined = undefined; // ✅
```

- **Null**: solo permite asignar ese tipo de valor [⬆️](#navegación)
```ts
// Correcto
let nulo: null = null; // ✅
```

- **Any**: se puede asignar cualquier tipo de dato, no se recomienda su uso «mejor usa ```vanilla javascript```» [⬆️](#navegación)
```ts
// Correcto
let a: any = 10; // ✅
a = '11'; // ✅
a = true; // ✅
a = {}; // ✅
a = []; // ✅
// etc.
```

