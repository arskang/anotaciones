###### Navegación
| ◀︎ | 🏠 |
| - | - |
| [Acrónimo - STUPID](./stupid.md) | [Inicio](./README.md) |

##### Glosario

- **Segregar**: separar una cosa de otra de la que forma parte para que siga existiendo con independencia
- **Dependencia**: en programación, significa que un módulo o componente requiere de otro para poder realizar su trabajo

### Principios SOLID

Los principios SOLID nos indican cómo organizar nuestras funciones y estructuras de datos en componentes y cómo dichos componentes deben estar interconectados

«Son recomendaciones y no reglas»

- **S - Single Responsibility Principle (SRP)**: tener una única responsabilidad no siempre significa hacer una única cosa [⬆️](#navegación)

    *Detectar violaciones*:
    - Nombres de clases y módulos demasiado génericos
    - Cambios en el código suelen afectar la clase o módulo
    - La clase involucra múltiples capas
    - Número elevado de importaciones
    - Cantidad elevada de métodos públicos
    - Excesivo número de línes de código

«Nunca debería haber más de un motivo por el cual cambiar una clase o un módulo» - Robert C. Martin

```ts
// Ejemplo:

// Este servicio es responsable de las acciones básicas de un CRUD
class UserService {

    get( name: string ) {
        console.log('Obteniendo usuario', name );
    }

    create( name: string ) {
        console.log('Creando usuario', name );
    }

    update( name: string ) {
        console.log('Actualizando usuario', name );
    }

    delete( name: string ) {
        console.log('Eliminando usuario', name );
    }

    // El envío del correo NO debería ser responsabilidad de este servicio
    sendMail( name: string ) {
        console.log('Enviando correo a', name );
    } // ❌

}
```

- **O - Open/Closed Principle (OCP)**: es un principio que depende mucho del contexto. Establece que las entidades del software (clases, módulos, métodos, etc.) deben estar abiertas para la extensión, pero cerradas para la modificación [⬆️](#navegación)

    *Detectar violaciones*:
    - Cambios normalmente afectan nuestra clase o módulo
    - Cuando una clase o módulo afecta muchas capas

```ts
// Ejemplo:

// En lugar de importar axios cada que se requiera,
// se puede hacer una clase para utilizar la dependencia una sola vez
import axios from 'axios'; // ❌

//--------------------------------------------------------------------//

// De esta forma al realizar una actualización o cambio,
// solo nos enfocaríamos en que la clase siempre haga lo mismo
// y el código dependiente no se vería afectado
import axios from 'axios'; // ✅

class HttpRequest {

    // El método puede extenderse sin afectar su uso
    async get(url: string) {
        return await axios.get(url);
    }

} // ✅
```

- **L - Liskov Substitution Principle (LSP)**: si una clase A es extendida por una clase B, deberíamos ser capaces de sustituir cualquier instancia de A por cualquier objeto de B sin que el sistema deje de funcionar [⬆️](#navegación)

«Las funciones que utilicen punteros o referencias a clases base deben ser capaces de usar objetos de clases derivadas sin saberlo» - Robert C. Martin

```ts
// Normalmente utilizamos las clases abstractas
// para aplicar herencia
abstract class Vehicle {
    abstract getNumberOfSeats(): number;
} // ✅

class Tesla extends Vehicle {

    constructor( private numberOfSeats: number ) {
        super();
    }

    getNumberOfSeats() {
        return this.numberOfSeats;
    }
} // ✅

class Audi extends Vehicle {

    constructor( private numberOfSeats: number ) {
        super();
    }

    getNumberOfSeats() {
        return this.numberOfSeats;
    }
} // ✅

class Toyota extends Vehicle {

    constructor( private numberOfSeats: number ) {
        super();
    }

    getNumberOfSeats() {
        return this.numberOfSeats;
    }
} // ✅

// De esta forma se pueden agregar n clases que extiendan de Vehicle
const printCarSeats = ( cars: Vehicle[] ) => {
    cars.forEach( car => {
        console.log( car.constructor.name, car.getNumberOfSeats() );
    });
} // ✅
```

- **I - Interface Segregation Principle (ISP)**: este principio establece que los clientes no deberían verse forzados a depender de interfaces que no usan [⬆️](#navegación)

    *Detectar violaciones*:
    - Si las interfaces que diseñamos nos obligan a violar los principios de responsabilidad única y sustitución de Liskov

 «Los clientes no deberían estar obligados a depender de interfaces que no utilicen» - Robert C. Martin

```ts
// Segregación de interfaz

interface Bird {
    eat(): number;
}

interface FlyingBird {
    fly(): void;
}

interface RunningBird {
    run(): void;
}

interface SwimmingBird {
    swim(): void;
}

// Únicamente se implementan las interfaces necesarias,
// así se evita que el código sea vulnerable a cambios

class Tucan implements Bird, FlyingBird, RunningBird {
    public fly(): void {}
    public eat(): void {}
    public run(): void {}
}

class Hummingbird implements Bird, FlyingBird {
    public fly(): void {}
    public eat(): void {}
}

class Ostrich implements Bird, SwimmingBird, RunningBird {
    public swim(): void {}
    public eat(): void {}
    public run(): void {}
}
class Penguin implements Bird, SwimmingBird {
    public swim(): void {}
    public eat(): void {}
}
```

- **D - Dependency Inversion Principle (DIP)**: depender de abstracciones (clases abstractas o interfaces). Uno de los motivos más importantes por el cual las reglas de negocio o capa de dominio deben depender de estas y no de concreciones es que aumenta su tolerancia al cambio [⬆️](#navegación)
    **¿Por qué obtenemos este beneficio?**
    - Cada cambio en un componente abstracto implica un cambio en su implementación
    - Por el contrario, los cambios en implementaciones concretas, la mayoría de las veces, no requieren cambios en las interfaces que implementa

    **Inyección de dependencias**: En algún momento nuestro programa o aplicación llegará a estar formado por muchos módulos. Cuando esto pase, es cuando debemos usar inyección de dependencias

    **Principio de inversión de dependencias**
    - Los módulos de alto nivel no deberían depender de módulos de bajo nivel
    - Ambos deberían depender de abstracciones
    - Las abstracciones no deberían depender de detalles
    - Los detalles deberían depender de abstracciones
    - Los componentes más importantes son aquellos centrados en resolver el problema subyacente al negocio, es decir, la capa de dominio
    - Los menos importantes son los que están próximos a la infraestructura, es decir, aquellos relacionados con la UI, la persistencia, la comunicación con API externas, etc.

«Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones. Las abstracciones no deben depender de concreciones. Los detalles deben depender de abstracciones» - Robert C. Martin

```ts
// Por buena práctica se recomienda generar
// la interface de nuestras respuestas HTTP
interface Post {
    body:   string;
    id:     number;
    title:  string;
    userId: number;
} // ✅

// Generamos una clase abstracta
// para poder definir los métodos a seguir
abstract class PostProvider {
    abstract getPosts(): Promise<Post[]>;
}

// La clase abstracta debe implementarse y no extenderse,
// en este caso no buscamos la herencia

// Esta primera clase implementa PostProvider,
// por lo que se ve obligada a implementar el método getPosts
class JsonDatabaseService implements PostProvider {

    async getPosts() {
        return [
            {
                userId: 1,
                id: 1,
                title: "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
                body: "quia et suscipit suscipit recusandae consequuntur expedita et cum reprehenderit molestiae ut ut quas totam nostrum rerum est autem sunt rem eveniet architecto"
            },
            {
                userId: 1,
                id: 2,
                title: "qui est esse",
                body: "est rerum tempore vitae sequi sint nihil reprehenderit dolor beatae ea dolores neque fugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis qui aperiam non debitis possimus qui neque nisi nulla"
            },
        ];
    }
}

// Esta segunda clase implementa PostProvider,
// por lo que se ve obligada a implementar el método getPosts
class WebApiPostService implements PostProvider {
    
    async getPosts() {
        const resp = await fetch('https://jsonplaceholder.typicode.com/posts');
        if (!resp.ok) {
            throw new Error('Error fetching posts');
        }
        const data: Post[] = await resp.json();
        return data;
    }
}

// Nuestra clase principal utiliza la inyección de dependencias,
// de esta forma siempre espera recibir PostProvider.
// Esta clase se ve mínimamente afectada por cualquier cambio
class PostService {
 
    private posts: Post[] = [];

    // Inyectamos la dependencia PostProvider
    constructor(private postProvider: PostProvider) {}

    // Rara vez nuestro método se vera afectado por algún cambio
    async getPosts() {
        this.posts = await this.postProvider.getPosts();

        return this.posts;
    }
}

(async () => {

    // Al inyectar la dependencia,
    // podemos cambiar entre las dos clases sin afectar el código

    // const postProvider = new JsonDatabaseService();
    const postProvider = new WebApiPostService();

    const postService = new PostService(postProvider);

    const posts = await postService.getPosts();

    console.log({ posts });

})();
```