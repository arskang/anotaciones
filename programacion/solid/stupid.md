###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Clean Code - clases y comentarios](./clases-comentarios.md) | [Inicio](./README.md) | [Principios SOLID](./solid.md) |

##### Glosario

- **S**ingleton: patrón singleton [⬆️](#navegación)
- **T**ight Coupling: alto acoplamiento [⬆️](#navegación)
- **U**ntestability: código no portable (unit test) [⬆️](#navegación)
- **P**remature optimization: optimizaciones prematuras[⬆️](#navegación)
- **I**ndescriptive Naming: nombres poco descriptivos [⬆️](#navegación)
- **D**uplication: duplicidad de código, no aplicar el principio DRY [⬆️](#navegación)

### Acrónimo - STUPID

- **Patrón singleton**: garantiza una única instancia de la clase a lo largo de toda la aplicación [⬆️](#navegación)
    «¿Por qué code smell?»
    + Vive en el contexto global
    + Puede ser modificado por cualquiera y en cualquier momento
    + No es rastreable
    * Difícil de testear debido a su ubicación

```js
// Evitar su uso
const Singleton = (function () {

    let instance;

    function createInstance(value) {
        return new Object(value);
    }

    return {
        getInstance(value) {
            if (!instance) {
                instance = createInstance(value);
            }
            return instance;
        }
    };
})(); // 🚫

(function () {
    const instance1 = Singleton.getInstance('I am the instance');
    const instance2 = Singleton.getInstance('I am the second instance');
    console.log('¿Misma instancia? ', (instance1 === instance2));
    // ¿Misma instancia? true
})();
```

- **Acoplamiento y cohesión**: lo ideal es tener bajo acoplamiento y buena cohesión. Se refiere a cuán relacionados o dependientes son dos clases o módulos entre sí. En **bajo acoplamiento**, cambiar algo importante en una clase no debería afectar a otra. En **alto acoplamiento**, dificultaría el cambio y el mantenimiento de su código; dado que las clases están muy unidas, hacer un cambio podría requerir una renovación completa del sistema [⬆️](#navegación)
    *«Queremos diseñar componentes que sean auto-contenidos, auto suficientes e independientes. Con un objetivo y un propósito bien definido» - The Pragmatic Programmer*

    **Alto acoplamiento** - desventajas:
    + Un cambio en el módulo por lo general provoca un efecto dominó de los cambios en otros módulos
    + El ensamblaje de módulos puede requerir más esfuerzo y/o tiempo debido a la mayor dependencia entre módulos
    + Un módulo en particular puede ser más difícil de reutilizar y/o probar porque se deben incluir módulos dependientes

    **Cohesión**:
    + Se refiere a lo que la clase (o módulo) puede hacer
    + La baja cohesion significaría que la clase realiza una gran variedad de acciones: es amplia, no se enfoca en lo que debe hacer
    + Alta cohesión significa que la clase se enfoca en lo que debería estar haciendo, es decir, solo métodos relacionados con la intención de la clase

```ts
// Alto acoplamiento ❌
// No aplicando el principio de responsabilidad única
(()=>{
    type Gender = 'M'|'F';

    // Bajo Acoplamiento
    // Esta clase aplica el principio de responsabilidad única
    class Person {
        constructor(
            public name: string,
            public gender: Gender,
            public birthdate: Date,
        ){}
    }

    // Alto acoplamiento
    // Depende de la clase Person,
    // cualquier cambio afecta a esta clase
    class User extends Person {
        constructor(
            public email: string,
            public role: string,
            private lastAccess: Date,
            name: string,
            gender: Gender,
            birthdate: Date,
        ){
            super(name, gender, birthdate);
            this.lastAccess = new Date();
        }

        checkCredentials() {
            return true;
        }
    }

    // Alto acoplamiento
   // Depende de la clase User,
   // cualquier cambio afecta a esta clase
    class UserSettings extends User {
        constructor(
            public workingDirectory: string,
            public lastFolderOpen: string,
            email     : string,
            role      : string,
            name      : string,
            gender    : Gender,
            birthdate : Date,
        ){
            super(
                email,
                role,
                new Date(),
                name,
                gender,
                birthdate
            )
        }
    }
    
    // Alto acoplamiento
    const userSettings = new UserSettings(
        '/urs/home',
        '/development',
        'john.doe@gmail.com',
        'F',
        'John Doe',
        'M',
        new Date('1985-10-21')
    );

    console.log({ userSettings, credentials: userSettings.checkCredentials() });
    
})();
```

- **Código no probable**: código dificilmente testeable. Se debe tener en mente las pruebas desde la creación del código [⬆️](#navegación)
    + Código de alto acoplamiento
    + Código con muchas dependencias no inyectadas
    + Dependencias en el contexto global (tipo Singleton)

- **Optimizaciones prematuras**: mantener abiertas las opciones retrasando la toma de decisiones nos permite darle mayor relevancia a lo que es más importante en una aplicación. No debemos anticiparnos a los requisitos y desarrollar abstracciones innecesarias que puedan añadir complejidad accidental. Se debe encontrar un balance entre la complejidad esencial y la accidentel [⬆️](#navegación)
    + **Complejidad esencial**: la complejidad es inherente al problema
    + **Complejidad accidental**: cuando implementamos una solución compleja a la mínima indispensable

- **Nombres poco descriptivos**: aplicar buenas prácticas en los nombres y evitar: [⬆️](#navegación)
    + Nombres de variables mal nombradas
    + Nombres de clases genéricas
    + Nombres de funciones mal nombradas
    + Ser muy especifico o demasiado genérico

- **Duplicidad de código**: no aplicar el principio DRY [⬆️](#navegación)

    **Real**
    + Código es idéntico y cumple la misma función
    + Un cambio implicaría actualizar todo el código idéntico en varios lugares
    + Incrementa posibilidades de error humano al olvidar una parte para actualizar
    + Mayor cantidad de pruebas innecesarias

    **Accidental**
    + Código luce similar pero cumple funciones distintas
    + Cuando hay un cambio, sólo hay que modificar un sólo lugar
    + Este tipo de duplicidad se puede trabajar con parámetros u optimizaciones

- **Otros olores honoríficos**: [⬆️](#navegación)
    + **Inflación**: evitar que el código se incremente o que las clases hagan demasiadas cosas. Lo ideal es separar aunque pueda impactar el rendimiento
    + **Obsesión primitiva**: uso de variables primitivas o constantes para manejar información, por ejemplo: ```USER_ADMIN_ROLE = 1```
    + **Lista larga de parámetros**: tener más de tres parámetros en un método. Lo ideal es mandar un objeto como parámetro

- **Acopladores**: todos los olores de este grupo contribuyen al acoplamiento excesivo entre clases o muestran lo que sucede si el acoplamiento se reemplaza por una delegación excesiva [⬆️](#navegación)
    + **Feature envy**: cuando un método hace mucha referencia a un método de otro módulo se debe replantear si el método se encuentra en el lugar correcto
    + **Intimidad inapropiada**: cuando una clase usa campos y métodos de otra clase. Las buenas clases deben saber lo menos posible de otras clases
    + **Cadenas de mensajes**: cuando tenemos un método que llama en cadena a más métodos
    + **The middle man**: si una clase realiza una sola acción y esa acción es delegar el trabajo a otra clase