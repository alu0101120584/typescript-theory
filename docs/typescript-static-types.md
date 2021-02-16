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

Tal y como se ha ilustrado en la sección de justificación del uso de TypeScript, el
tipado dinámico puede llevar a la aparición de problemas. TypeScript, que es un
lenguaje con tipos de datos estáticos, requiere de la especificación explícita de los
tipos de datos, lo que permite al compilador detectar posibles errores cuando se
intentan utilizar tipos de datos diferentes a los esperados.

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
