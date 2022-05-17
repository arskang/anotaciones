###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Genéricos](./generics.md) | [Inicio](./README.md) | [Usando librerías que no están en Typescript](./librerias.md) |

### Decoradores

-  Más información: [Decoradores](https://www.typescriptlang.org/docs/handbook/decorators.html) «última consulta: **17/Mayo/2022**»

- **Concepto decoradores**: un decorador no es más que una función que se ejecuta en el momento de la transpilación de ```typescript``` a ```javascript```; se utiliza para expandir o añadir funcionalidades a un objeto. Se pueden anidar ```n```número de decoradores. Para utilizar un decorador se debe usar ```@``` y posicionarlo antes del método, propiedad o clase en donde se desea ejecutar [⬆️](#navegación)

- **Activar decoradores**: para poder utilizar los decoradores se deben activar en el archivo ```tsconfig.json``` [⬆️](#navegación)
```js
/* Enables experimental support for ES7 decorators. */
```

```json
{
    "compilerOptions": {
       "experimentalDecorators": true
    }
}
```

- **Decoradores de clases**: es posible utilizar decoradores en clases. Ejemplo de como imprimir una clase en la consola: [⬆️](#navegación)
```ts
function printToConsole(constructor: Function) {
    console.log(constructor);
}

@printToConsole
export class Pokemon {
    public publicApi: string = 'https://pokeapi.co';
    constructor(
        public name: string,
    ){}
}
```

- **Decoradores de métodos**: es posible utilizar decoradores en funciones o métodos de clases. Ejemplo de como validar un id: [⬆️](#navegación)
```ts
function CheckValidPokemonID() {
    return function(target: any, propertyKey: string, descriptor: PropertyDescriptor){
        const originalMethod = descriptor.value;
        if (propertyKey === 'savePokemonDB') {
            descriptor.value = function(id: number) {
                if (id <= 0 || id > 800) {
                    console.error('Invalid Pokemon ID');
                }
                return originalMethod.apply(this, [id]);
            }
        }
    }
}

export class Pokemon {
    public publicApi: string = 'https://pokeapi.co';
    constructor(
        public name: string,
    ){}

    @CheckValidPokemonID()
    savePokemonDB(id: number) {
        console.log(`Saving pokemon ${id}`);
    }
}
```

- **Decoradores de propiedades**: es posible utilizar decoradores en propiedades, ejemplo de como generar un readOnly [⬆️](#navegación)
```ts
function readOnly(isWriteable: boolean = true): Function {
    return function(target: any, propertyKey: string){
        const descriptor: PropertyDescriptor = {
            get() {},
            set(this, value) {
                Object.defineProperty(this, propertyKey, {
                    value,
                    writable: !isWriteable,
                    enumerable: false,
                })
            }
        }
        return descriptor;
    }
}

export class Pokemon {
    @readOnly()
    public publicApi: string = 'https://pokeapi.co';

    constructor(
        public name: string,
    ){}
}
```