###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Depuración de errores y el archivo tsconfig.json](./depuracion-tsconfig.md) | [Inicio](./README.md) | [Interfaces](./interfaces.md) |

##### Glosario

- **Public**: son propiedades o métodos que se pueden acceder a ellas una vez instanciada la clase. En ```typescript``` es la predeterminada y no es necesario pero se recomienda definirlo [⬆️](#navegación)
- **Static**: son propiedades o métodos que se pueden acceder sin instanciar la clase; llamando directamente desde el nombre de la clase [⬆️](#navegación)
- **Private**: son propiedades o métodos que son imposibles de acceder «al instanciar la clase» y sólo se pueden utilizar dentro de la clase; tampoco pueden heredarse [⬆️](#navegación)
- **Protected**: son propiedades o métodos que son imposibles de acceder «al instanciar la clase», pero a diferencia de ```private``` estás pueden ser heredadas [⬆️](#navegación)
- **Patrón singleton**: Su intención consiste en garantizar que una clase solo tenga una instancia y proporcionar un punto de acceso global a ella [⬆️](#navegación)

### Clases

- **Clase básica**: existen cuatro formas de declarar propiedades en una clase: ```public```, ```static```, ```private``` o ```protected```. *Todo esto es del lado de ```typescript```una vez el código se transpila a ```javascript``` todo es visible* [⬆️](#navegación)

```ts
// Correcto
class Basic {
    private name: string;
    public realName: string;
    static age: number = 87;

    constructor(name: string, realName: string) {
        this.name = name;
        this.realName = realName;
    }
} // ✅

// Correcto
const newBasic = new Basic('Jane', 'Jane Doe'); // ✅
newBasic.realName; // ✅
Basic.age; // ✅

// Incorrecto
newBasic.name; // ❌
newBasic.age; // ❌
Basic.name; // ❌
Basic.realName; // ❌
```

- **Forma corta de asignar propiedades**: se pueden definir las propiedades directamente en el constructor, estás solo pueden ser ```public```o ```private``` y de está forma no es necesario asignar el valor de una en una [⬆️](#navegación)
```ts
// Correcto
class Basic {
    // private name: string;
    // public realName: string;
    static age: number = 87;

    constructor(
        private name: string,
        public realName: string,
    ) {}

} // ✅

// Correcto
const newBasic = new Basic('Jane', 'Jane Doe'); // ✅
newBasic.realName; // ✅
```

- **Métodos**: al igual que con las propiedades es posible declarar métodos ```public```, ```static```, ```private``` o ```protected```. Los métodos ```static``` solo tienen acceso a todo lo ```static```; mientras que los dos restantes pueden llamarse entre si. Si por alguna razón se necesita utilizar un método o variable ```static``` deben ser llamados utilizando el nombre de la clase [⬆️](#navegación)
```ts
// Correcto
class Basic {
    static age: number = 87;

    constructor(
        private name: string,
        public realName: string,
    ) {}

    public getName(): string {
        return this.realName;
    }

    private getPrivateName(): string {
        return this.name;
    }

    static getStringAge(): string {
        return this.age.toString();
    }

} // ✅
```

- **Herencia, super y extends**: las clases pueden heredar métodos y propiedades entre si, para lograr eso se utiliza la palabra reservada ```extends``` y se debe inicializar el constructor de la clase heredada con ```super```; con esta palabra se pueden acceder a todas las propiedades y métodos ```public```o ```protected```. Solo es posible extender a una clase [⬆️](#navegación)
```ts
// Correcto
class Basic {
    static age: number = 87;

    constructor(
        private name: string,
        public realName: string,
    ) {}

    public getName(): string {
        return this.realName;
    }

    private getPrivateName(): string {
        return this.name;
    }

    protected getProtectedName(): string {
        return this.getPrivateName();
    }
} // ✅

// Correcto
class SuperBasic extends Basic {
    constructor(
        name: string,
        realName: string,
    ) {
        super(name, realName);
    }

    public getFullName(): string {
        return super.getProtectedName();
    }
} // ✅

// Correcto
const superBasic = new SuperBasic('Jane', 'Jane Doe'); // ✅
superBasic.getFullName(); // ✅
```

- **Gets y sets**: son métodos que pueden compartir el mismo nombre «pero no la misma palabra reservada» y se utilizan como si fueran propiedades; con la ventaja de que se puede agregar lógica. Los métodos ```get``` no reciben parámetros y siempre deben retornar algo; los ```set``` deben recibir solo un parámetro y no deben retornar algo [⬆️](#navegación)
```ts
// Correcto
class Basic {
    constructor(
        private name: string,
        public realName: string,
    ) {}

    get fullName(): string {
        return `${this.name} - ${this.realName}`;
    }

    set fullName(value: string) {
        this.name = value;
    }
} // ✅

// Correcto
const basic = new Basic('Jane', 'Jane Doe'); // ✅
basic.fullName = 'John Doe'; // ✅
basic.fullName; // ✅
```

- **Clases abstractas**: sirven para crear otras clases; es más como una clase privada que no puede ser instanciada «y es imposible acceder directamente a sus métodos o propiedades» pero si heredar sus propiedades a otra clase. Para definir una clase abstracta se utiliza la palabra reservada ```abstract```. También se pueden utilizar para especificar que se esta esperando una clase o argumento que extienda de la clase abstracta [⬆️](#navegación)
```ts
// Correcto
abstract class Basic {
    constructor(private name: string) {}

    public getName(): string {
        return this.name;
    }
} // ✅

// Correcto
class SuperBasic extends Basic {
    public getFullName(): string {
        return super.getName();
    }
} // ✅

// Correcto
class SimpleBasic {} // ✅

// Correcto
const superBasic = new SuperBasic('Jane Doe'); // ✅
superBasic.getFullName(); // ✅

// Incorrecto
const newBasic = new Basic('Jane Doe'); // ❌
newBasic.getName(); // ❌

// ## Otros usos

// Correcto
class SimpleBasic {} // ✅

// Correcto
const printName = (info: Basic) => {
    console.log(info.getName());
}; // ✅

// Correcto
printName(superBasic); // ✅

// Correcto
const newSimpleBasic = new SimpleBasic(); // ✅

// Incorrecto
printName(newSimpleBasic); // ❌
```

- **Constructores privados**: casi no se utiliza pero sirve para controlar la manera en que las instancias son ejecutadas «es común usarla con el ```patrón singleton```».   [⬆️](#navegación)
```ts
// Correcto
// Patrón singleton
class Basic {
    static instance: Basic;
    private constructor(private name: string) {}
    static getInstance(name: string): Basic {
        if (!Basic.instance) {
            Basic.instance = new Basic(name);
        }
        return Basic.instance;
    }
    get fullName(): string {
        return this.name
    }
} // ✅

// Correcto
const basic1 = Basic.getInstance('Jane Doe'); // ✅
basic1.fullName; // = 'Jane Doe' ✅
const basic2 = Basic.getInstance('John Doe'); // ✅
basic1.fullName; // = 'John Doe' ✅
basic2.fullName; // = 'John Doe' ✅

// Incorrecto
const basic = new Basic('Jane Doe'); // ❌
```