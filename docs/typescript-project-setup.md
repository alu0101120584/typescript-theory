# Creación de un proyecto inicial para trabajar con TypeScript

Este apartado ilustra la creación de un proyecto inicial mínimo con el que poder
empezar a trabajar con TypeScript.

Lo primero que haremos es abrir una terminal, ya sea a través del sistema operativo o
de VSCode. En la misma crearemos un directorio con el nombre de nuestro proyecto:

```bash
[~()]$mkdir theory-examples
[~()]$cd theory-examples/
[~/theory-examples()]$
```

A continuación, ejecutaremos el siguiente comando para generar un fichero `package.json` en
la raíz de nuestro proyecto, el cual va a permitir que gestionemos las dependencias del mismo,
además de otras características:

```bash
[~/theory-examples()]$npm init --yes
Wrote to /home/usuario/theory-examples/package.json:

{
  "name": "theory-examples",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Seguidamente, también crearemos un fichero denominado `tsconfig.json` en la raíz de nuestro
proyecto, el cual va a contener las opciones del compilador de TypeScript. Lo editaremos
incluyendo el siguiente contenido:

```bash
[~/theory-examples()]$cat tsconfig.json 
{
  "compilerOptions": {
    "target": "ES2020",
    "outDir": "./dist",
    "rootDir": "./src"
  }
}
```

La opción `"target"` le indica al compilador de TypeScript que debe generar código JavaScript
compatible con la última versión del estándar. Las opciones `"outDir"` y `"rootDir"` permiten
establecer los directorios donde el compilador de TypeScript generará el código JavaScript y
donde podrá encontrar el código fuente en TypeScript, respectivamente. En este punto, podemos
crear dichos directorios en la raíz del proyecto, aunque la creación del directorio `dist` no
es necesaria (se crea automáticamente durante la primera compilación):

```bash
[~/theory-examples()]$mkdir src
[~/theory-examples()]$mkdir dist
```

Otra opción a la hora de crear el fichero `tsconfig.json` sería ejecutar el siguiente comando
para generarlo automáticamente y, posteriormente, proceder a editarlo:

```bash
[~/theory-examples()]$tsc --init
message TS6071: Successfully created a tsconfig.json file
```

El siguiente paso consistirá en instalar un paquete que permitirá detectar los cambios realizados
sobre los ficheros con código fuente en TypeScript (aquellos almacenados en el directorio `src`),
recompilarlos para generar el código JavaScript correspondiente y ejecutarlo, todo ello de manera
automática:

```bash
[~/theory-examples()]$npm install --save-dev tsc-watch

added 21 packages, and audited 22 packages in 4s

found 0 vulnerabilities
```

Para hacer uso de la funcionalidad de dicho paquete, debemos modificar la propiedad `"scripts"` del
fichero `package.json`, eliminando la línea correspondiente a la propiedad `"test"` y añadiendo una
propiedad que denominaremos `"start"`. El contenido del fichero después de ser modificado debe ser
el siguiente:

```bash
[~/theory-examples()]$cat package.json 
{
  "name": "theory-examples",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "tsc-watch --onSuccess \"node dist/index.js\""
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "tsc-watch": "^4.2.9"
  }
}
```

Tal y como puede observarse, solo se ejecutará el código JavaScript generado y almacenado en
el fichero `dist/index.js` en caso de que el proceso de compilación a través del compilador
de TypeScript haya sido satisfactorio, es decir, libre de errores.

Para que lo anterior funcione correctamente, debemos tener algún fichero con código escrito
en TypeScript en nuestro proyecto. Dado que el código JavaScript generado se encontrará en el
fichero `index.js` del directorio `dist`, tendremos que crear un fichero con código TypeScript
denominado `index.ts` en el directorio `src` de nuestro proyecto:

```bash
[~/theory-examples()]$cd src/
[~/theory-examples/src()]$cat index.ts 
console.log("Hola Mundo");
```

Por último, ejecutaremos el siguiente comando, el cual ejecutará el script definido en la propiedad
`"start"` del fichero `package.json`:

```bash
[~/theory-examples/src()]$npm start

...

21:47:30 - Starting compilation in watch mode...


21:47:35 - Found 0 errors. Watching for file changes.
Hola Mundo
```

Cada vez que modifiquemos algo en el directorio `src` de nuestro proyecto, se recompilarán
todos los ficheros con código TypeScript almacenados en dicho directorio y se ejecutará el
fichero `index.js` generado de manera automática en el directorio `dist`.
