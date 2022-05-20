###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Clean Code y deuda técnica](./deuda-tecnica.md) | [Inicio](./README.md) | [Acrónimo - STUPID](./stupid.md) |

### Clean Code - clases y comentarios

- **Principio de responsabilidad única**: se debe priorizar la composición frente a la herencia; es importante que nuestras clases no abusen de la herencia [⬆️](#navegación)
    + Abuso de la herencia y solución:

```ts
// Para evitar cualquier cancelación:
// solo se tomaron los siguientes dos generos para un ejemplo rápido
type Gender = 'M' | 'H'; // ✅

// La clase «Person» tiene una responsabilidad única
class Person {
    constructor(
        public name: string, 
        public gender: Gender, 
        public birthdate: Date
    ){}
} // ✅

// La clase «User» al heredar de la clase «Person»
// ahora necesita los argumentos del constructor «Person».
// Cualquier adición al constructor de «Person» afectaría a la clase «User»,
// lo que hace que sea más complicado mantenerla
class User extends Person {
    
    public lastAccess: Date;

    constructor(
        public email: string,
        public role: string,
        // Parámetros que necesita la clase «Person»
        name: string,
        gender: Gender,
        birthdate: Date,
    ) {
        super( name, gender, birthdate );
        this.lastAccess = new Date();
    }

    checkCredentials() {
        return true;
    }
} // ❌

// La clase «UserSettings» aún lo tiene más complicado;
// ya que hereda de «User» que esta a su vez hereda de «Person»,
// por lo que se ve afectada por cambios en estas dos clases
class UserSettings extends User {
    constructor(
        public workingDirectory: string,
        public lastOpenFolder  : string,
        // Parámetros que necesita la clase «User»
        email                  : string,
        role                   : string,
        // Parámetros que necesita la clase «Person»
        name                   : string,
        gender                 : Gender,
        birthdate              : Date
    ) {
        super(email, role, name, gender, birthdate );
    }
} // ❌

// Los argumentos podrían ser más semánticos
const userSettings = new UserSettings(
    '/usr/home',
    '/home',
    'hello@gmail.com',
    'Admin',
    'John Doe',
    'H',
    new Date('1987-07-02T00:00:00')
); // ❌
```
```js
// La siguiente solución es un ejemplo
// y no es una regla obligatoría solucionar todo de esta forma
```
```js
// Aplicando el principio de responsabilidad única:
```
```ts
// Para evitar cualquier cancelación:
// solo se tomaron los siguientes dos generos para un ejemplo rápido
type Gender = 'M' | 'H'; // ✅

// A simple vista a diferencia del ejemplo anterior, aquí hay más código.

// La razón por la que se decidió generar una interface se verá al final
interface PersonProps {
    name: string;
    gender: Gender;
    birthdate: Date;
} // ✅

// En esta refactorización el comportamiento observable de la clase «Person» ha cambiado, pero se vuelve más semántico
// Pensando en la posibilidad de que las propiedades de la clase aumenten,
// la responsabilidad de esos cambios ahora recae sobre la clase «Person»,
// evitando que la clase «UserSettings» se vea afectada
class Person {
    public name: string;
    public gender: Gender;
    private birthdate: Date;

    constructor({ name, gender, birthdate }: PersonProps){
        this.name = name;
        this.gender = gender;
        this.birthdate = birthdate;
    }
}// ✅

// La razón por la que se decidió generar una interface se verá al final
interface UserProps {
    email: string;
    role: string;
} // ✅

// Aquí se evita la herencia a «Person» y se aplica la misma refactorización.
class User {
    public email: string;
    public role: string;
    private lastAccess: Date;

    constructor({ email, role }: UserProps){
        this.email = email;
        this.role = role;
        this.lastAccess = new Date();
    }

    checkCredentials() {
        return true;
    }
} // ✅

// La razón por la que se decidió generar una interface se verá al final
interface SettingsProps {
    workingDirectory: string;
    lastOpenFolder: string;
} // ✅

// Se crea una nueva clase que solo se encarga de las configuraciones
class Settings {
    public workingDirectory: string;
    public lastOpenFolder: string;

    constructor({ workingDirectory, lastOpenFolder }: SettingsProps) {
        this.workingDirectory = workingDirectory;
        this.lastOpenFolder = lastOpenFolder;
    }
} // ✅

// ¿Aquí nos encontramos con una contradicción al abusar de la herencia?
// En este caso, la herencia nos evita modificar nuestro contructor cuando existen modificaciones en ellas; esta es la razón por la cual todos los parametros de las clases se pasaron a una interface
interface UserSettingsProps extends SettingsProps, UserProps, PersonProps {} // ✅

// De esta forma los cambios que se apliquen en las clases anteriores
// rara vez afectarán nuestra clase «UserSettings»
class UserSettings {

    public person: Person;
    public user: User;
    public settings: Settings;

    // Como «UserSettingsProps» hereda a las demás interfaces se puede pasar directamente en los argumentos
    constructor(userSettingsProps: UserSettingsProps) {
        this.person = new Person(userSettingsProps);
        this.user = new User(userSettingsProps);
        this.settings = new Settings(userSettingsProps);
    }
} // ✅

// Finalmente la refactorización afecto el comportamiento observable de «UserSettings», pero ahora es más semántico
const newUserSettings = new UserSettings({
    name: 'John Doe',
    gender: 'H',
    birthdate: new Date('1987-07-02T00:00:00'),
    email: 'hello@gmail.com',
    role: 'Admin',
    workingDirectory: '/usr/home',
    lastOpenFolder: '/home',
}); // ✅
```

- **Estructura de una clase**: a continuación recomendaciones de la estructura interna de una clase; también es necesario seguir las recomendaciones y buenas prácticas del lenguaje utilizado [⬆️](#navegación)

```ts
class HtmlElement {

    // Comenzar con lista de propiedades
    // 1. Estáticas
    // 2. Públicas
    // 3. Privadas

    public static domReady: boolean = false;

    private _id: string;
    private type: string;
    private updatedAt: string;

    // Métodos
    // 1. Constructores estáticos
    // 2. Constructor
    // 3. Contructor privado «si es el caso»
    // 4. Métodos estáticos
    // 5. Resto de los métodos de instancia ordenados de mayor a menor importancia
    // 6. Getters y Setters al final

    // Este método estático retorna una instancia de la clase
    static createInput(id: string) {
        return new HtmlElement(id, 'input');
    }

    constructor(id: string, type: string) {
        this._id = id;
        this.type = type;
        this.updatedAt = new Date().toISOString();
    }

    setType(type: string) {
        this.type = type;
        this.updatedAt = new Date().toISOString();
    }

    get id(): string {
        return this._id;
    }
    
}
```

- **Comentarios en el código**: hay que evitar utilizar comentarios; si necesitas agregar comentarios al código es porque no es lo suficiente autoexplicativo. Pero, cuando usamos librerías de terceros, APIS, frameworks, etc. nos encontraremos ante situaciones en las que escribir un comentario será mejor que dejar la solución compleja o un hack sin explicación. Los comentarios deberían ser la excepción, no la regla [⬆️](#navegación)
«No comentes el código mal escrito, reescribelo» - Brian W. Kernighan
    + Nuestro código debe ser suficientemente autoexplicativo
    + Un comentario debe responder ¿el por qué? en lugar del ¿qué? o ¿cómo?

```ts
// No necesario
const name = 'John Doe';
// Si name es igual a 'John Doe'
if (name === 'John Doe') {
    // entonces...
} // ❗️
```

- **Uniformidad en el proyecto**: es recomendable tratar de mantener la misma consistencia de estilos en todo el proyecto; al igual que los nombres de los métodos, clases o estructuras de las carpetas   [⬆️](#navegación)

```ts
// Por ejemplo si tienes:
createUser();
updateUser();
deleteUser();

// Incorrecto
insertProduct(); // ❌
modifyProduct(); // ❌
removeProduct(); // ❌

// Correcto
createProduct(); // ✅
updateProduct(); // ✅
deleteProduct(); // ✅
```