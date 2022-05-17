###### Navegación
| ◀︎ | 🏠 | ▶︎ |
| - | - | - |
| [Objetos y tipos personalizados](./objetos-tipos-personalizados.md) | [Inicio](./README.md) | [Clases](./clases.md) |

### Depuración de errores y el archivo tsconfig.json

- Documentación oficial de [tsconfig](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) «última consulta: **13/Mayo/2022**» [⬆️](#navegación)
```
https://www.typescriptlang.org/docs/handbook/tsconfig-json.html
```

- **Depuración**: se recomienda activar la siguiente configuración para generar los archivos ```.map``` correspondientes y dar depurar el código de ```typescript``` [⬆️](#navegación)
```js
/* Create source map files for emitted JavaScript files. */
```

```json
{
    "compilerOptions": {
        "sourceMap": true,
    }
}
```

- **Remover comentarios**: se recomienda activar la siguiente configuración para eliminar los comentarios en los archivos generados de ```typescript``` a ```javascript``` [⬆️](#navegación)
```js
/* Disable emitting comments. */
```

```json
{
    "compilerOptions": {
        "removeComments": true
    }
}
```

- **Incluir y excluir carpetas**: es posible configurar que carpetas se pueden incluir o excluir del transpilado a ```javascript``` [⬆️](#navegación)

```json
{
    "compilerOptions": {},
    "include": ["src/**/*"],
    "exclude": ["node_modules", "**/*.spec.ts"]
}
```

- **Archivo de salida**: es recomendable generar un solo archivo de transpilado a ```javascript```, para esto es necesario definir nuestro archivo de salida [⬆️](#navegación)
```js
/*
Specify a file that bundles all outputs into one JavaScript file.
If `declaration` is true, also designates a file that bundles all .d.ts output.
*/
```

```json
{
    "compilerOptions": {
        "outFile": "./main.js"
    }
}
```