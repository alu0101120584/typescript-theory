# Creación de un proyecto inicial para trabajar con TypeScript

Esta sección ilustra la creación de un proyecto inicial mínimo con el que poder
empezar a trabajar con TypeScript. También se describe la configuración de algunas
herramientas que podrían ser de utilidad a la hora de desarrollar.

## Gestión de dependencias y configuración del compilador de TypeScript

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

## Compilación y ejecución automática tras detección de cambios

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

Ahora, ejecutaremos el siguiente comando, el cual ejecutará el script definido en la propiedad
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

# Comprobación de sintaxis, búsqueda y resolución de problemas y aplicación de estilo en nuestro código

Si quisiéramos comprobar la sintaxis de nuestro código, encontrar y solucionar problemas en
el mismo o forzar algún tipo de estilo de programación, tendremos que utilizar un *linter*, y el más
conocido para trabajar con JavaScript y TypeScript es [Eslint](https://eslint.org/). Para
empezar a utilizarlo, pueden instalar el paquete `eslint` de manera global haciendo uso de
`npm`:

```bash
[~/theory-examples()]$npm install -g eslint

changed 110 packages, and audited 111 packages in 8s

13 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
[~/theory-examples()]$eslint --version
v7.20.0
```

A continuación, debemos crear un fichero de configuración de ESLint en base a una serie de
preguntas que nos va a ir realizando el script de inicialización del comando `eslint`:

```bash
[~/theory-examples()]$eslint --init
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · commonjs
✔ Which framework does your project use? · none
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · google
✔ What format do you want your config file to be in? · JSON
Checking peerDependencies of eslint-config-google@latest
Local ESLint installation not found.
The config that you've selected requires the following dependencies:

@typescript-eslint/eslint-plugin@latest eslint-config-google@latest eslint@>=5.16.0 @typescript-eslint/parser@latest
✔ Would you like to install them now with npm? · No / Yes
Installing @typescript-eslint/eslint-plugin@latest, eslint-config-google@latest, eslint@>=5.16.0, @typescript-eslint/parser@latest

added 134 packages, and audited 156 packages in 12s

24 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Successfully created .eslintrc.json file in /home/usuario/theory-examples
ESLint was installed locally. We recommend using this local copy instead of your globally-installed copy
```

El fichero de configuración de ESLint se encontrará en la raíz de nuestro proyecto con el nombre `.eslintrc.json`,
siempre y cuando hayamos elegido volcarlo en un fichero JSON durante el script de inicialización.

Si llegamos a instalar en una de nuestras prácticas anteriores la extensión de ESLint en VSCode, la integración con
dicho entorno de desarrollo será total una vez instalado el paquete `eslint` y llevada a cabo la configuración
correspondiente. No obstante, en algunos casos, VSCode podría pedirnos que habilitemos el uso de ESLint en nuestro
proyecto. Si queremos aprovechar la funcionalidad que otorga, deberemos acceder a habilitarlo.

Una vez habilitado, podremos comprobar que la instalación y configuración de ESLint ha sido adecuada, abriendo el
fichero `src/index.ts` en VSCode. Si, por ejemplo, se especificó la guía de estilo de Google, ESLint informará
de un problema con la cadena de caracteres `"Hola Mundo"`. El error, en concreto, se encuentra asociado a una regla
denominada `"quotes"`, que indica que las cadenas de caracteres deben delimitarse entre comillas simples y no dobles.

Si queremos solucionar el problema detectado, podremos hacer clic en el icono de la bombilla que aparece cuando
hacemos clic sobre la cadena de caracteres y elegir alguna de las propuestas sugeridas como, por ejemplo, sustituir
las comillas dobles por comillas simples o desactivar la regla en esa sentencia de código concreta, entre otras.

Por último, para desactivar una regla en ESLint, en general, debemos editar el fichero de configuración
`.eslintrc.json`, en concreto, la propiedad denominada `"rules"`. Por ejemplo, para desactivar la regla
`"quotes"`, deberemos incluir lo siguiente en la propiedad anteriormente mencionada:

```bash
[~/theory-examples()]$cat .eslintrc.json 
{
    "env": {
        "browser": true,
        "commonjs": true,
        "es2021": true
    },
    "extends": [
        "google"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 12
    },
    "plugins": [
        "@typescript-eslint"
    ],
    "rules": {
        "quotes": "off"
    }
}
```

Para cada regla, además del valor `"off"`, que permite desactivarla, también se pueden usar los valores
`"warn"` y `"error"`, que permiten definir el nivel de severidad de la regla en caso de estar habilitada.
Se vuelven a echar un vistazo al fichero `src/index.ts`, podrán observar como ahora ESLint no informa sobre
problemas con las cadenas de caracteres con comillas dobles.
