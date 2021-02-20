# Tipos de datos estáticos

JavaScript es un lenguaje de programación tipado dinámicamente, es decir,
es el valor que se asigna a una variable el que permite inferir dinámicamente
el tipo de dicha variable. En el siguiente ejemplo JavaScript, que ilustra lo
anterior, la variable `myVar` cambia su tipo de datos dependiendo del tipo del
valor que se le asigna:

```javascript
let myVar;
console.log(`${myVar} = ${typeof myVar}`);
myVar = 12;
console.log(`${myVar} = ${typeof myVar}`);
myVar = "Hello";
console.log(`${myVar} = ${typeof myVar}`);
myVar = true;
console.log(`${myVar} = ${typeof myVar}`);
```

Cabe mencionar la primera asignación, donde la variable `myVar` no ha recibido ningún
valor y, por lo tanto, su tipo no se puede inferir dinámicamente. Es por ello que es
de tipo `undefined` y su valor es `undefined` también.

A modo de recordatorio, JavaScript cuenta con los siguientes tipos de datos:

* `number`: representa valores numéricos.
* `string`: representa cadenas de caracteres.
* `boolean`: toma los valores lógicos `true` y `false`.
* `symbol`: representa valores constantes únicos.
* `null`: solo puede tomar el valor `null` e indica una referencia que no existe o no válida.
* `undefined`: este tipo (y valor) es el que toma una variable que se ha definido pero a la
   que no se ha asignado valor alguno.
* `object`: representa valores complejos, formados por pares propiedad--valor.

Tal y como se ha ilustrado en la sección de justificación del uso de TypeScript, el
tipado dinámico puede llevar a la aparición de problemas. TypeScript, que es un
lenguaje con tipos de datos estáticos, requiere de la especificación explícita de los
tipos de datos, lo que permite al compilador detectar posibles errores cuando se
intentan utilizar tipos de datos diferentes a los esperados. Los tipos de datos que
pueden explicitarse en TypeScript son los mismos que en JavaScript, como cabría esperar,
dado que TypeScript es un superconjunto de JavaScript.

## Anotaciones de tipo e inferencia de tipos

En TypeScript, los tipos de datos estáticos se definen explícitamente utilizando *anotaciones de tipo*:

```typescript
function add(firstNum: number, secondNum: number): number {
  return firstNum + secondNum;
}

const myConst: number = 7;
let myResult: number = add(1, myConst);
console.log(`myResult = ${myResult}`);
```

En el ejemplo anterior, se ha utilizado una anotación de tipo para los parámetros de la función
`add`, así como una anotación de tipo para el resultado que devuelve la misma. También se puede
observar como se pueden llevar a cabo anotaciones de tipo en constantes y variables.

A pesar de que se pueden indicar los tipos de datos estáticos de manera explícita,
el compilador de TypeScript también es capaz de inferir los tipos de datos de manera
implícita:

```typescript
function add(firstNum: number, secondNum: number) {
  return firstNum + secondNum;
}

const myConst = 7;
let myResult = add(1, myConst);
console.log(`myResult = ${myResult}`);
```

Se puede observar como no hemos indicado de manera explícita los tipos de la
constante `myConst` y la variable `myResult`. Tampoco se ha explicitado el tipo
que retorna la función `add`. Dado que los tipos se utilizan correctamente,
el compilador no detecta ningún tipo de error.

Sin embargo, si se modifica, por ejemplo, la definición de la función `add`:

```typescript
function add(firstNum: string, secondNum: string) {
  return firstNum + secondNum;
}

const myConst = 7;
let myResult = add(1, myConst);
console.log(`myResult = ${myResult}`);
```

El compilador infiere que la constante `myConst` es de tipo `number` debido a la asignación
del valor numérico 7. Más tarde, detectará que se está intentando invocar a la función `add`
con un argumento de tipo `number`, a pesar de haberla declarado con parámetros de tipo `string`,
lo cual hará que informe de un error:

```bash
src/index.ts(6,20): error TS2345: Argument of type 'number' is not assignable to parameter of type 'string'.
```

El compilador de TypeScript cuenta con una opción interesante que, si se habilita, permite conocer
los tipos de datos que se están utilizando en el código:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true
  }
}
```

Observando el contenido del directorio `/dist`, ya no solo existe un fichero `index.js`, sino también un
fichero `index.d.ts`, el cual contiene información sobre los tipos de datos, ya se hayan definido
explícitamente mediante anotaciones de tipo o inferido a través del proceso de compilación.

```typescript
declare function add(firstNum: string, secondNum: string): string;
declare const myConst = 7;
declare let myResult: string;
```

## El tipo de datos any

El tipo de datos `any` permite indicar que, por ejemplo, una constante o variable, un parámetro
de una función o un resultado devuelto por la misma sea de cualquier tipo. En el siguiente ejemplo,
el compilador de TypeScript informará de un error, dado que el resultado de la función `add`, que
es de tipo `number`, se está intentando asignar a la variable `myResult`, que es de tipo `string`.

```typescript
function add(firstNum: number, secondNum: number) {
  return firstNum + secondNum;
}

const myConst = 7;
let myResult: string = add(1, myConst);
console.log(`myResult = ${myResult}`);
```

Para evitar lo anterior, podemos indicar, explícitamente, que la función `add` devuelve un valor
de tipo `any`:

```typescript
function add(firstNum: number, secondNum: number): any {
  return firstNum + secondNum;
}

const myConst = 7;
let myResult: string = add(1, myConst);
console.log(`myResult = ${myResult}`);
```

Utilizar el tipo de datos `any` permite a TypeScript acceder a todas las funcionalidades proporcionadas
por el tipado dinámico de JavaScript. No obstante, su uso también suele implicar los mismos errores,
muchas veces inesperados, que surgen cuando programamos directamente en JavaScript, los cuales suelen
aparecer en tiempo de ejecución debido a la transformación automática de unos tipos de datos en otros.
El siguiente ejemplo ilustra lo anterior:

```typescript
function add(firstNum: number, secondNum: number): any {
  return `Result is ${firstNum + secondNum}`;
}

let myResult = add(7, 8) / 2;
console.log(`myResult = ${myResult}`);
```

La función `add` devuelve un valor de tipo `any`. En tiempo de ejecución, el valor devuelto es de tipo
`string` y consiste en una cadena de caracteres que comienza con *"Result is ..."*. Al intentar dividir
el resultado devuelto por la función `add`, el mismo no puede transformarse a un valor numérico, por lo
que la variable `myResult`, tras llevar a cabo la división, toma el valor `NaN` o *Not a Number*.

Otro ejemplo que ilustra este tipo de problemas podría ser el siguiente:

```typescript
function add(firstNum: number, secondNum: number): any {
  return `Result is ${firstNum + secondNum}`;
}

let myResult: number = add(7, 8);
console.log(`myResult = ${myResult.toFixed(2)}`);
```

El resultado de la función `add`, que es de tipo `any`, puede asignarse a la variable `myResult`, que es
de tipo `number`, y el compilador de TypeScript no emite ningún tipo de aviso al respecto, tal y como se
esperaba. En tiempo de ejecución, no obstante, la función `add` devuelve un valor de tipo `string` que se
asigna a la variable `myResult` y, debido al tipado dinámico de JavaScript, en ese justo momento,
`myResult` pasa a ser de tipo `string`. La invocación del método `toFixed` implica un error en tiempo de
ejecución, dado que el método `toFixed` no se encuentra disponible para ser invocado al pertenecer al tipo
`number` y no al tipo `string`.

Además de usar anotaciones de tipo explícitas para utilizar el tipo de datos `any`, TypeScript asignará
el tipo de datos `any` de manera implícita siempre y cuando no sea capaz de inferir un tipo de datos
más concreto. Lo anterior permite aplicar TypeScript en proyectos JavaScript de manera selectiva. También
permite simplificar el trabajo con paquetes de terceros desarrollados en JavaScript.

```typescript
function add(firstNum, secondNum) {
  return firstNum + secondNum;
}

let myResult = add(1, 7);
console.log(`myResult = ${myResult}`);
myResult = add('1', '7');
console.log(`myResult = ${myResult}`);
myResult = add(true, true);
console.log(`myResult = ${myResult}`);
```

En el ejemplo anterior, el compilador no es capaz de inferir un tipo de datos específico para los
parámetros de la función `add` y, tampoco, para su resultado, por lo que infiere que todos son de
tipo `any`. El tipo de datos inferido para la variable `myResult` también será `any`. De hecho,
se trata del código que escribiríamos directamente en JavaScript. El compilador de TypeScript, por
defecto, no indicará ningún tipo de error y, además, el código generado se ejecutará correctamente,
llevando a cabo, por supuesto, las correspondientes transformaciones de un tipo de datos a otro.

Utilizar el tipo de datos `any` de un modo explícito cuidadosamente podría llegar a beneficiarnos en
algunos casos. No obstante, permitir que el compilador de TypeScript infiera el tipo de datos `any`
de manera implícita implica la aparición de errores relacionados con los tipos de datos,
como ya ocurre con JavaScript. Es una buena práctica, por tanto, desactivar el uso implícito del
tipo de datos `any` habilitando la opción `noImplicitAny` del compilador de TypeScript:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "noImplicitAny": true
  }
}
```

Al activar dicha opción en el compilador, la siguiente vez que tratemos de compilar el último
ejemplo de código, obtendremos lo siguiente:

```bash
src/index.ts(1,14): error TS7006: Parameter 'firstNum' implicitly has an 'any' type.
src/index.ts(1,24): error TS7006: Parameter 'secondNum' implicitly has an 'any' type.
```

De este modo, será necesario especificar un tipo de datos en los parámetros de la función `add`,
el cual podría ser, de manera intencionada, `any`. Si por ejemplo los especificamos como `number`:

```typescript
function add(firstNum: number, secondNum: number) {
  return firstNum + secondNum;
}

let myResult = add(1, 7);
console.log(`myResult = ${myResult}`);
myResult = add('1', '7');
console.log(`myResult = ${myResult}`);
myResult = add(true, true);
console.log(`myResult = ${myResult}`);
```

Compilando el anterior código, ahora obtendremos lo siguiente desde el compilador:

```bash
src/index.ts(7,16): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
src/index.ts(9,16): error TS2345: Argument of type 'boolean' is not assignable to parameter of type 'number'.
```

En este caso, la segunda y tercera invocaciones a la función `add` dejarían de ser válidas, dado que no
estamos utilizando argumentos con el mismo tipo de datos especificado en la declaración de la función.

## Uniones de tipos

Teniendo en cuenta los posibles extremos en las anotaciones de tipos, por un lado, tenemos el tipo de datos
`any`, lo que permite una total libertad. En el otro extremo se encontraría un tipo de datos específico como,
por ejemplo, `boolean` o `number`. Entre ambos extremos se encuentran las uniones de tipos o *type unions*,
que permiten especificar un conjunto de tipos de datos. El siguiente ejemplo ilustra una unión de los tipos
`number` y `string` para el resultado de la función `add`:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myResult = add(1, 7, true);
console.log(`myResult = ${myResult}`);
myResult = add(1, 7, false);
console.log(`myResult = ${myResult}`);
```

En este punto, es importante indicar que las propiedades de una unión de tipos son aquellas incluidas en
la intersección de las propiedades de los tipos que componen la unión. En el ejemplo anterior, el compilador
infiere que la variable `myResult` es de tipo `number | string` y no `number` o `string`. Para ilustrar lo
anterior, ejecute el siguiente código:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myResult = add(1, 7, true);
console.log(`myResult = ${myResult}`);
console.log(myResult.toFixed(2));
myResult = add(1, 7, false);
console.log(`myResult = ${myResult}`);
console.log(myResult.charAt(0));
```

El resultado de la ejecución será algo como lo siguiente:

```bash
src/index.ts(8,22): error TS2339: Property 'toFixed' does not exist on type 'string | number'.
  Property 'toFixed' does not exist on type 'string'.
src/index.ts(11,22): error TS2339: Property 'charAt' does not exist on type 'string | number'.
  Property 'charAt' does not exist on type 'number'.
```

La propiedad (método) `toFixed` pertenece al tipo de datos `number`, mientras que la propiedad
(método) `charAt` pertenece al tipo `string`. Sin embargo, ninguna de esas propiedades pertenece
a la unión de tipos `number | string`, lo que se traduce en que el compilador informe de los
errores expuestos más arriba.

## Afirmaciones de tipos

Una afirmación de tipo o *type assertion* permite indicarle al compilador de TypeScript que trate
a un valor como de un tipo de datos concreto. Esto se conoce como *type narrowing*.

Una afirmación de tipo es una de las maneras de aplicar *type narrowing* a una unión de tipos:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myNumResult = add(1, 7, true) as number;
console.log(`myNumResult = ${myNumResult}`);
console.log(myNumResult.toFixed(2));
let myStrResult = add(1, 7, false) as string;
console.log(`myStrResult = ${myStrResult}`);
console.log(myStrResult.charAt(0));
```

Hemos especificado que el compilador debe tratar el valor asignado a la variable `myNumResult`
como un `number`, mientras que debe tratar el valor asignado a la variable `myStrResult` como
un `string`. Además, el compilador infiere, implícitamente, el tipo de la variable a través
del tipo utilizado para la afirmación: El siguiente ejemplo es equivalente al anterior:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myNumResult: number = add(1, 7, true) as number;
console.log(`myNumResult = ${myNumResult}`);
console.log(myNumResult.toFixed(2));
let myStrResult: string = add(1, 7, false) as string;
console.log(`myStrResult = ${myStrResult}`);
console.log(myStrResult.charAt(0));
```

Dado que una afirmación de tipo selecciona uno de los tipos de la unión, ahora, en tiempo
de compilación, todos los métodos y propiedades de dicho tipo están disponibles para ser
utilizados, evitando los errores que el compilador informaba. Es por ello que, ahora, si
podemos invocar los métodos `toFixed` y `charAt`.

¿Qué sucedería si tratásemos de utilizar una afirmación de tipo no esperada?

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myNumResult = add(1, 7, true) as number;
console.log(`myNumResult = ${myNumResult}`);
console.log(myNumResult.toFixed(2));
let myStrResult = add(1, 7, false) as string;
console.log(`myStrResult = ${myStrResult}`);
console.log(myStrResult.charAt(0));
let myBoolResult = add(1, 7, false) as boolean;
console.log(`myBoolResult = ${myBoolResult}`);
```

Obtenemos el siguiente error:

```bash
src/index.ts(12,20): error TS2352: Conversion of type 'string | number' to type 'boolean' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.
  Type 'number' is not comparable to type 'boolean'.
```

Para solucionar lo anterior, lo que suele hacerse es:

1. Utilizar un tipo de datos diferente en la afirmación de tipos que no arroje un error por el compilador.
2. Ampliar la unión de tipos con un tipo de datos nuevo.
3. Forzar la afirmación de tipos, utilizando una afirmación al tipo `any` e, inmediatamente, al tipo deseado.

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myNumResult = add(1, 7, true) as number;
console.log(`myNumResult = ${myNumResult}`);
console.log(myNumResult.toFixed(2));
let myStrResult = add(1, 7, false) as string;
console.log(`myStrResult = ${myStrResult}`);
console.log(myStrResult.charAt(0));
let myBoolResult = add(1, 7, false) as any as boolean;
console.log(`myBoolResult = ${myBoolResult}`);

myBoolResult ? console.log("I am a true value") :
               console.log("I am a false value");
```

Lo anterior, en tiempo de compilación, permite evitar que el compilador informe del error que obtuvimos más arriba.
No obstante y, siempre que utilizamos el tipo `any`, como ya hemos visto anteriormente, debemos asegurarnos de que,
en tiempo de ejecución, no vaya a producirse ningún error. En tiempo de ejecución, la función `add` devolverá un
`string` que será asignado a la variable `myBoolResult`, la cual pasará a ser de tipo `string` (tipado dinámico
de JavaScript). A la hora de utilizar un `string` en una sentencia condicional, solo la cadena vacía se evalúa como
falsa en JavaScript, mientras que una cadena como la que contiene `myBoolResult` se evaluará como verdadera.
Es por ello que en la terminal se mostrará el mensaje `"I am a true value"`.

## Guardianes de tipo

Un guardián de tipo o *type guard* implica el uso de `typeof` para averiguar un tipo de datos, que tiene que ser
uno de los tipos básicos de JavaScript. Es una alternativa a la utilización de afirmaciones de tipo:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myResult = add(1, 7, true);

if (typeof myResult === "number") {
  console.log(`myResult = ${myResult}`);
  console.log(myResult.toFixed(2));
} else if (typeof myResult === "string") {
  console.log(`myResult = ${myResult}`);
  console.log(myResult.charAt(0));
}
```

En tiempo de ejecución, `myResult` recibirá un valor de tipo `number`, y dicha variable pasará a ser
de ese tipo también. El guardián de tipo establecido a través del uso de `typeof` permitirá bifurcar
el flujo de ejecución correctamente. Además, el compilador de TypeScript no informará de ningún error
dado que confía en que las sentencias de un bloque condicional concreto solo se ejecutarán si el valor
comprobado es del tipo especificado en la condición. Es por ello que también podemos invocar al método
`toFixed` dentro del primer bloque condicional y al método `charAt` dentro del segundo bloque condicional.

El compilador de TypeScript también es capaz de detectar guardianes de tipo en sentencias `switch` y no
solo en sentencias `if-else`:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myResult = add(1, 7, true);

switch (typeof myResult) {
  case "number":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.toFixed(2));
    break;
  case "string":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.charAt(0));
    break;
}
```

## El tipo de datos never

TypeScript proporciona el tipo de datos `never` para aquellos casos en los que un guardián de tipos
ya ha comprobado todos los posibles tipos de datos de un valor concreto. Una vez ha hecho todas
esas comprobaciones, solo permitirá asignar el tipo `never` a un valor.

En el ejemplo que hemos estado desarrollando, la unión de tipos solo permite los tipos `number` o
`string` y la sentencia `switch` que utilizamos en el último ejemplo ya se encargó de llevar a cabo
las comprobaciones con ambos tipos:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myResult = add(1, 7, true);

switch (typeof myResult) {
  case "number":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.toFixed(2));
    break;
  case "string":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.charAt(0));
    break;
  default:
    let result: never = myResult;
    console.log(`Type was not expected: ${result}`);
}
```

Alcanzar el caso por defecto de la sentencia `switch` indicaría que ha ocurrido algún tipo de
error durante la ejecución. TypeScript proporciona el tipo de datos `never` para evitar
utilizar un valor una vez que se ha utilizado un guardián para comprobar todos sus posibles
tipos de datos.

## El tipo de datos unknown

Ya hemos visto anteriormente que el tipo de datos `any` permite acceder a la flexibilidad que
proporciona JavaScript respecto al tipado de datos dinámico, lo cual también podría llevarnos
a cometer ciertos errores.

El uso del tipo de datos `unknown` es una alternativa más segura al uso de `any`.
Un valor `unknown` solo puede asignarse al tipo de datos `any` o `unknown`, a no ser que
se utilice una afirmación o un guardián de tipo, lo que introduce un nivel de
comprobación adicional respecto a la utilización de `any`:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myResult = add(1, 7, true);

switch (typeof myResult) {
  case "number":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.toFixed(2));
    break;
  case "string":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.charAt(0));
    break;
  default:
    let result: never = myResult;
    console.log(`Type was not expected: ${result}`);
}

let mySecondResult: unknown = add(1, 7, false);
let myStringResult: string = mySecondResult;
console.log(`myStringResult = ${myStringResult}`);
console.log(`${myStringResult.charAt(0)}`);
```

El anterior ejemplo hace que el compilador de TypeScript informe del siguiente error,
algo que no ocurriría si `mySecondResult` fuera de tipo `any`:

```bash
src/index.ts(23,5): error TS2322: Type 'unknown' is not assignable to type 'string'.
```

El error anterior puede solucionarse, por ejemplo, haciendo uso de una afirmación de tipo
aplicada sobre `myStringResult`:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

let myResult = add(1, 7, true);

switch (typeof myResult) {
  case "number":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.toFixed(2));
    break;
  case "string":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.charAt(0));
    break;
  default:
    let result: never = myResult;
    console.log(`Type was not expected: ${result}`);
}

let mySecondResult: unknown = add(1, 7, false);
let myStringResult: string = mySecondResult as string;
console.log(`myStringResult = ${myStringResult}`);
console.log(`${myStringResult.charAt(0)}`);
```

## Tipos de datos null y undefined

Tal y como se ha mencionado con anterioridad, el valor `null`, que representa algo que no existe o
que no es válido, solo puede asignarse al tipo `null`. Al mismo tiempo, el valor `undefined`, que
se usa cuando se ha definido una variable a la que no se ha asignado un valor áun, solo puede
asignarse al tipo `undefined`.

El compilador de TypeScript, por defecto, permite que los valores `null` y `undefined` puedan asignarse
a todos los tipos:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

function div(numerator: number, denominator: number,
    isNumber: boolean): number | string {
  if (denominator === 0) {
    return null;
  }
  return isNumber ? numerator / denominator :
                    (numerator / denominator).toFixed(2);
}

let myResult: number | string = div(1, 0, true);

switch (typeof myResult) {
  case "number":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.toFixed(2));
    break;
  case "string":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.charAt(0));
    break;
  default:
    let result: never = myResult;
    console.log(`Type was not expected: ${result}`);
}
```

En el ejemplo anterior se ha añadido una función `div` que devuelve `null` en caso de que
alguno de los operandos sea negativo. Tal y como puede observarse, el compilador de TypeScript
no informa de ningún error, a pesar de que se ha indicado explícitamente que el resultado de
dicha función devuelve una unión de tipos `number | string`. Al mismo tiempo, tampoco informa
de que a la variable `myResult`, que también se ha anotado explícitamente con el tipo de dicha
unión, se le puede asignar el valor `null`, resultado de una división por cero.

En tiempo de ejecución, no obstante, el valor devuelto es `null`, lo que hará que la variable
`myResult` tome el tipo de datos `null`. Más tarde, en la sentencia `switch`, se alcanzará el
caso por defecto:

```bash
Type was not expected: null
```

Para evitar situaciones como las anteriores, el compilador de TypeScript proporciona la opción
`strictNullChecks`. Si se habilita dicha opción, el compilador no permitirá la asignación de
los valores `null` y `undefined` a otros tipos que no sean los correspondientes:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

Al habilitar dicha opción en el fichero `tsconfig.json` e intentar recompilar el ejemplo anterior,
el compilador informará del siguiente error:

```bash
src/index.ts(9,5): error TS2322: Type 'null' is not assignable to type 'string | number'.
```

Para solucionarlo, podríamos modificar el cuerpo de la función para evitar que la misma devuelva el
valor `null`, o podemos ampliar la unión de tipos utilizada para anotar el tipo del resultado de la
función `div`, indicando que el tipo `null` también forma parte de dicha unión:

```typescript
function add(firstNum: number, secondNum: number,
    isNumber: boolean): number | string {
  return isNumber ? firstNum + secondNum : (firstNum + secondNum).toFixed(2);
}

function div(numerator: number, denominator: number,
    isNumber: boolean): number | string | null {
  if (denominator === 0) {
    return null;
  }
  return isNumber ? numerator / denominator :
                    (numerator / denominator).toFixed(2);
}

let myResult: number | string | null = div(1, 0, true);

switch (typeof myResult) {
  case "number":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.toFixed(2));
    break;
  case "string":
    console.log(`myResult = ${myResult}`);
    console.log(myResult.charAt(0));
    break;
  default:
    if (myResult === null) {
      console.log(`myResult = ${myResult}`);
    } else {
      let result: never = myResult;
      console.log(`Type was not expected: ${result}`);
    }
}
```

No solo se debe modificar la anotación en la función, sino también en la declaración de la variable
`myResult`. Por último, debemos tener en cuenta también la posibilidad de obtener un valor `null` en
el guardián de tipos. La pregunta que podrían hacerse ahora mismo es ¿por qué no utilizar otro
`case` en dicho guardián para comprobar si `myResult` es de tipo `null`. Modifiquemos el ejemplo anterior
para incluir lo siguiente:

```typescript
let myResult: number | string | null = div(1, 0, true);
console.log(`${typeof myResult}`)
```

Al aplicar `typeof` al tipo `null` se obtiene como respuesta `"object"`. Es por ello que no se puede utilizar
otra sentencia `case` dentro del `switch` que haga referencia al tipo de datos `null`. Lo que se debe hacer
es comprobar explícitamente si el valor obtenido de la ejecución de la función `div` es `null` dentro de una
sentencia `if-else`, la cual es tratada como un guardián de tipos por el compilador de TypeScript.

Cabe mencionar en este punto, no obstante, que, al aplicar `typeof` al tipo `undefined`, si que se obtiene
como respuesta `"undefined"`.

