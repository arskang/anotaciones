###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Clases](./clases.md) | [Inicio](./README.md) | [NameSpaces](./namespaces.md) |

### Interfaces

- [Interface vs Type](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces) «última consulta: **16/Mayo/2022**»

- **Interface**: las interfaces son muy parecidas a los ```types``` la única diferencia es que una ```interface``` puede expandirse; por eso se recomienda utilizar ```types```cuando no se tiene pensado su expansión [⬆️](#navegación)
```ts
// Correcto
interface Person {
    name: string,
    age: number,
    skills: string[],
}; // ✅
```

- **Estructuras complejas**: no es recomendado anidar tipado dentro de una ```interface```; se debe evitar tener subniveles ya que es posible que se vuelva difícil de mantener. También es posible utilizar la palabra rservada ```extends```para extender interfaces [⬆️](#navegación)
```ts
// Correcto «pero no recomendado»
interface EvilHero {
    name: string,
    age: number,
    powers?: {
        strength: number,
        speed: number,
        durability?: number,
    },
}; // 👎

// Correcto
interface Hero {
    name: string,
    age: number,
    powers?: Powers,
}; // ✅

// Correcto
interface SuperHero extends Hero {
    power?: Number,
}; // ✅

// Correcto
interface Powers {
    strength: number,
    speed: number,
    durability?: number,
}; // ✅

// Correcto
let flash: Hero = {
    name: 'Barry Allen',
    age: 24,
    powers: {
        strength: 10,
        speed: 10,
    },
}; // ✅
```

- **Métodos en la interfaz**: al igual que con los ```types``` en la ```interface``` es posible definir métodos; existén dos formas de hacerlo [⬆️](#navegación)
```ts
// Correcto
interface Hero {
    name: string,
    getName: () => string,
    getFullName(id: string): string,
}; // ✅

// Correcto
let flash: Hero = {
    name: 'Barry Allen',
    getName(): string {
        return this.name;
    },
    getFullName(id: string): string {
        return `${id} - ${this.name}`;
    }
}; // ✅

// Correcto 
flash.getName(); // ✅
flash.getFullName('10'); // ✅

// Incorrecto
flash.getFullName(); // ❌
```

- **Interface en las clases**: para implementar una ```interface``` en una clase se debe utilizar la palabra reservada ```implements```. Es posible utilizar ```extends``` e ```implements``` al mismo tiempo; la declaración siempre debe iniciar con ```extends```. A diferencia de ```extends``` con ```implements``` se pueden implementar más de una interface separado por ```,```  [⬆️](#navegación)
```ts
// Correcto 
interface Human {
    name: string;
} // ✅

// Correcto 
interface Skills {
    speed: string;
    strength: string;
    intelligence: string;
} // ✅

// Correcto 
class Person implements Human, Skills {
    public speed: string = '0';
    public strength: string = '0';
    public intelligence: string = '0';
    constructor(public name: string, skills?: Skills) {
        if (skills) {
            this.speed = skills.speed;
            this.strength = skills.strength;
            this.intelligence = skills.intelligence;
        }
    }
} // ✅

// Correcto 
const jane = new Person('Jane Doe'); // ✅
```

- **Interfaces para las funciones**: es algo muy raro pero es posible definir ```interfaces``` para ```functions```  [⬆️](#navegación)
```ts
// Correcto 
interface addTwoNumbers {
    (a: number, b: number): number;
} // ✅

// Correcto 
let addNumbersFunction: addTwoNumbers; // ✅

// Correcto 
addNumbersFunction = (a: number, b: number): number => {
    return a + b;
} // ✅

// Correcto 
addNumbersFunction(1, 2); // ✅
```