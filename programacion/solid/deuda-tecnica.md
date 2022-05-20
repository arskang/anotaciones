###### Navegación
| 🏠 | ▶︎ |
| - | - |
| [Inicio](./README.md) | [Clean Code - clases y comentarios](./clases-comentarios.md) |

##### Glosario

- **Deuda técnica**: es la falta de calidad en el código, que genera una deuda que repercutirá en costos futuros [⬆️](#navegación)

- **Refactorización**: es simplemente un proceso que tiene como objetivo mejorar el código sin alterar su comportamiento para que sea más entendible y tolerante a cambios. Usualmente para que una refactorización fuerte tenga el objetivo esperado, es imprescindible contar con pruebas automáticas [⬆️](#navegación)

- **Clean code**:
    *"Código limpio es aquel que se ha escrito con la intención de que otra persona (o tú mismo en el futuro) lo entienda"* - «Carlos Blé»
    *"Nuestro código tiene que ser simple y directo, debería leerse con la misma facilidad que un texto bien escrito"* - «Grady Booch»
    *"Programar es el arte de decirle a otro humano lo que quieres que la computadora haga"* - «Donald Knuth»

### Clean Code y deuda técnica

- **Nombre pronunciables y expresivos**: se recomienda que los nombres de las variables sean semánticos «pensando en los desarrolladores y no en las máquinas» [⬆️](#navegación)

```ts
// Incorrecto
const n = 53; // ❌
const tx = 0.15; // ❌
const cat = 'T-Shirts'; // ❌
const ddmmyyyy = new Date('August 15, 1965 00:00:00'); // ❌

// Correcto
const numberOfUnits = 53; // ✅
const tax = 0.15; // ✅
const category = 'T-Shirts'; // ✅
const birthDate = new Date('August 15, 1965 00:00:00'); // ✅
```

- **Nombres según el tipo de dato**: hay que tratar de que los nombres se puedan leer y comprender facilmente. Es recomendable construirlos con un verbo que representa la acción seguida del sustantivo [⬆️](#navegación)
```ts
// Incorrecto
// # Arrays
const fruit = ['manzana', 'platano', 'fresa']; // ❌
// # Booleans
const open = true; // ❌
const write = true; // ❌
const noValues = true; // ❌
// # Numbers
const fruits = 10; // ❌
const cars = 3; // ❌
// # Functions
createUserIfNotExists(); // ❌
updateUserIfNotEmpty(); // ❌
sendEmailIfFieldsValid(); // ❌

// Correcto
// # Arrays
const fruitNames = ['manzana', 'platano', 'fresa']; // ✅
// # Booleans
const isOpen = true; // ✅
const canWrite = true; // ✅
const hasValues = false; // ✅
// # Numbers
const maxFruits = 10; // ✅
const totalOfCars = 3; // ✅
// # Functions
createUser(); // ✅
updateUser(); // ✅
sendEmail(); // ✅
```

- **Consideraciones para las clases**: el nombre es lo más importante de la clase. Se recomienda que se formen por un sustantivo o frases de sustantivo y utilizar UpperCamelCase [⬆️](#navegación)
Tres preguntas para saber si es un buen nombre:
    + ¿Qué exactamente hace la clase?
    + ¿Cómo exactamente esta clase realiza cierta tarea?
    + ¿Hay algo específico sobre su ubicación?
    «si algo no tiene sentido, remuevelo o refactoriza»
```ts
// Incorrecto
class Manager {}; // ❌
class Data {}; // ❌
class SpecialMonsterView {}; // ❌
```

- **Nombres de funciones, argumentos y parámetros**: es recomendable que todas nuestras funciones solo hagan lo que indica su nombre, también se debe limitar a 3 parámetros posicionales [⬆️](#navegación)
Otras recomendaciones:
    + Simplicidad es fundamental
    + Deben tener un tamaño reducido
    + Funciones de una sola línea sin causar complejidad
    + Evitar el uso del «else»
    + Prioriza el uso de la condicional ternaria
```ts
// Incorrecto
function sendEmail(): boolean {
    // Verificar si el usuario existe
    // Revisar contraseña
    // Crear usuario en base de datos
    // Si todo sale bien
    return true;
} // ❌

// Correcto
interface SendEmailOptions {
    toWhom: string;
    from: string;
    body: string;
    subject: string;
    apiKey: string;
} // ✅

// Correcto
function sendEmail({
    // Parámetros
    toWhom, from, body, subject, apiKey
}: SendEmailOptions): boolean {
    // Validar correo
    // Enviar correo
    return true;
} // ✅

// Correcto
sendEmail(
    // Argumentos

);  // ✅
```

- **Principio DRY «Don't Repeat Yourself»**: simplemente es evitar tener duplicidad de código. Esto simplifica las pruebas y ayuda a centralizar procesos; esto usualmente lleva a refactorizar [⬆️](#navegación)
```ts
// Incorrecto
class Product {
    constructor(
        public name: string = '',
        public price: number = 0,
        public size: string = '',
    ){}

    toString() {
        // No DRY
        if (this.name.length <= 0) throw new Error('name is empty');
        if (this.price === 0) throw new Error('price is zero');
        if (this.size.length <= 0) throw new Error('size is empty');

        return `${this.name} (${this.price}), ${this.size}`;
    }
} // ❌

// Correcto
type Size = ''| 'S'|'M'|'XL';// ✅

// Correcto
class Product {
    constructor(
        public name: string = '',
        public price: number = 0,
        public size: Size = '',
    ){}

    isProductReady(): boolean {
        for( const key in this ) {
            switch( typeof this[key] ) {
                case 'string':
                    if ( (<string><unknown>this[key]).length <= 0 ) throw Error(`${ key } is empty`);
                break;
                case 'number':
                    if ( (<number><unknown>this[key]) <= 0 ) throw Error(`${ key } is zero`);
                break;
                default:
                    throw Error(`${ typeof this[key] } is not valid`);
            }
        }
        return true;
    }

    
    toString() {
        if ( !this.isProductReady ) return;
        return `${ this.name } (${ this.price }), ${ this.size }`
    }
} // ✅
```