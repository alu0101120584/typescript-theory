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

En TypeScript, los tipos de datos estáticos se definen utilizando *anotaciones de tipo*:

```typescript
function add(firstNum: number, secondNum: number): number {
  return firstNum + secondNum;
}

const myConst: number = 7;
let myResult: number = add(1, myConst);
console.log(myResult);
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
console.log(`myResult = ${myResult}`)
```

El compilador infiere que la constante `myConst` es de tipo `number` debido a la asignación
del valor numérico 7. Más tarde, detectará que se está intentando invocar a la función `add`
con un argumento de tipo `number`, a pesar de haberla declarado con parámetros de tipo `string`,
lo cual hará que informe de un error.

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
console.log(`myResult = ${myResult}`)
```

Para evitar lo anterior, podemos indicar, explícitamente, que la función `add` devuelve un valor
de tipo `any`:

```typescript
function add(firstNum: number, secondNum: number): any {
  return firstNum + secondNum;
}

const myConst = 7;
let myResult: string = add(1, myConst);
console.log(`myResult = ${myResult}`)
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
que la variable `myResult` toma el valor `NaN` o *Not a Number*.

Otro ejemplo que ilustra este tipo de problemas podría ser el siguiente:

```typescript
function add(firstNum: number, secondNum: number): any {
  return `Result is ${firstNum + secondNum}`;
}

let myResult: number = add(7, 8) / 2;
console.log(`myResult = ${myResult.toFixed(2)}`)
```

El resultado de la función `add`, que es de tipo `any`, puede asignarse a la variable `myResult`, que es
de tipo `number`, y el compilador de TypeScript no emite ningún tipo de aviso al respecto, tal y como se
esperaba. En tiempo de ejecución, no obstante, la función `add` devuelve un valor de tipo `string` que se
asigna a la variable `myResult` y, debido al tipado dinámico de JavaScript, en ese justo momento,
`myResult` pasa a ser de tipo `string`. La invocación del método `toFixed` implica un error en tiempo de
ejecución, dado que el método `toFixed` no se encuentra disponible para ser invocado.

Además de usar anotaciones de tipo explícitas para utilizar el tipo de datos `any`, TypeScript asignará
el tipo de datos `any`de manera implícita siempre y cuando no sea capaz de inferir un tipo de datos
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
console.log(`myResult = ${myResult}`)
```

En el ejemplo anterior, el compilador no es capaz de inferir un tipo de datos específico para los
parámetros de la función `add` y, tampoco, para su resultado, por lo que infiere que todos son de
tipo `any`. El tipo de datos inferido para la variable `myResult` también será `any`. De hecho,
se trata del código que escribiríamos directamente en JavaScript. El compilador de TypeScript, por
defecto, no indicará ningún tipo de error y, además, el código generado se ejecutará correctamente,
llevando a cabo, por supuesto, las correspondientes transformaciones de un tipo de datos a otro.

Utilizar el tipo de datos `any` explícitamente cuidadosamente podría llegar a beneficiarnos en
algunos casos. No obstante, permitir que el compilador de TypeScript infiera el tipo de datos
`any`de manera implícita implica la aparición de errores relacionados con los tipos de datos,
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
    format: boolean): number | string {
  return format ? (firstNum + secondNum).toFixed(2) : firstNum + secondNum;
}

let myResult = add(1, 7, false);
console.log(`myResult = ${myResult}`);
myResult = add(1, 7, true);
console.log(`myResult = ${myResult}`)
```

En este punto, es importante indicar que las propiedades de una unión de tipos son aquellas incluidas en
la intersección de las propiedades de los tipos que componen la unión. En el ejemplo anterior, el compilador
infiere que la variable `myResult` es de tipo `number | string` y no `number` o `string`.Para ilustrar lo
anterior, ejecute el siguiente código:

```typescript
function add(firstNum: number, secondNum: number,
    format: boolean): number | string {
  return format ? (firstNum + secondNum).toFixed(2) : firstNum + secondNum;
}

let myResult = add(1, 7, false);
console.log(`myResult = ${myResult}`);
console.log(myResult.toFixed(2));
myResult = add(1, 7, true);
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

