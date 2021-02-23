# Funciones

Aunque ya hemos visto que se pueden utilizar anotaciones de tipo explícitas para los parámetros y valor de
retorno de una función, y que también TypeScript es capaz de inferir implícitamente sus tipos de datos, en
este apartado vamos a profundizar en la funcionalidad que nos proporciona TypeScript con respecto a las funciones.

Para comenzar, puede partir de un proyecto ya existente o [configurar uno nuevo](typescript-project-setup.md).
Lo realmente importante es que el fichero con las opciones del compilador `tsconfig.json` incluya lo siguiente:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "declaration": true,
    "outDir": "./dist",
    "rootDir": "./src" 
  }
}
```

Además, partiremos del siguiente ejemplo que deberá contener el fichero `index.ts`:

```typescript
function add(firstNum, secondNum) {
  return firstNum + secondNum;
}

const mySum = add(1, 7);
console.log(`mySum = ${mySum}`);
```

Recuerde utilizar, si lo estima oportuno, `tsc-watch` para habilitar la compilación y ejecución automáticas
cuando se detecten cambios en el fichero `index.ts`.

## Definición de funciones

En JavaScript, una función se puede definir más de una vez y, la implementación más reciente, esto es,
la ubicada más abajo en un fichero con código fuente, es la que se utiliza cuando se invoca dicha
función.

Una consecuencia de lo anterior es que en JavaScript no se puede llevar a cabo sobrecarga de funciones,
es decir, definir funciones que comparten el mismo nombre, pero que se diferencian por su número y/o
tipo de parámetros.

Si se define una función con el mismo nombre que una función previamente definida, con independencia de
sus parámetros, la última definición prevalecerá sobre las anteriores.

Además de lo anterior, es importante recordar que el número de argumentos con los que se invoca una función
en JavaScript no es determinante. Si la invocación se lleva a cabo con menos argumentos que parámetros definidos
en la función, los parámetros sobrantes tomarán el valor `undefined`. Si la invocación se lleva a cabo con más
argumentos que parámetros, o bien podremos hacer que la función los ignore, o bien podremos tenerlos en cuenta
haciendo uso de `arguments`, lo cual nos permite acceder a todos los argumentos utilizados para invocar a la
función.

Para ejemplificar lo anterior, redefinamos la función `add`:

```typescript
function add(firstNum, secondNum) {
  return firstNum + secondNum;
}

function add(firstNum, secondNum, thirdNum) {
  return firstNum + secondNum + thirdNum;
}

const mySum = add(1, 7);
console.log(`mySum = ${mySum}`);
```

El compilador de TypeScript trata de evitar el problema de la redefinición de funciones informando de un error:

```bash
src/index.ts(1,10): error TS2393: Duplicate function implementation.
src/index.ts(5,10): error TS2393: Duplicate function implementation.
```

Por lo tanto, para evitar este tipo de errores, se puede combinar la funcionalidad de las funciones del mismo
nombre en una única función o se pueden asignar nombres diferentes a las funciones. El siguiente ejemplo
ilustra la segunda opción:

```typescript
function addTwoNumbers(firstNum, secondNum) {
  return firstNum + secondNum;
}

function addThreeNumbers(firstNum, secondNum, thirdNum) {
  return firstNum + secondNum + thirdNum;
}

let mySum = addTwoNumbers(1, 7);
console.log(`mySum = ${mySum}`);

mySum = addThreeNumbers(1, 7, 15);
console.log(`mySum = ${mySum}`);
```

# Número de parámetros y argumentos

Anteriormente se indicó que el número de argumentos en JavaScript a la hora de invocar una función
no es determinante. Sin embargo, TypeScript espera que una función se invoque con un número de argumentos
igual al número de parámetros especificados en la definición de la función. Si modificamos el ejemplo
anterior:

```typescript
function addTwoNumbers(firstNum, secondNum) {
  return firstNum + secondNum;
}

function addThreeNumbers(firstNum, secondNum, thirdNum) {
  return firstNum + secondNum + thirdNum;
}

let mySum = addTwoNumbers(1, 7, 8);
console.log(`mySum = ${mySum}`);

mySum = addThreeNumbers(1, 7);
console.log(`mySum = ${mySum}`);
```

El compilador de TypeScript informa de los siguientes errores:

```bash
src/index.ts(9,33): error TS2554: Expected 2 arguments, but got 3.
src/index.ts(12,9): error TS2554: Expected 3 arguments, but got 2.
```

Indicando que la invocación a la primera función esperaba dos argumentos pero ha sido invocada con tres,
al mismo tiempo que la invocación a la segunda función esperaba tres argumentos pero ha sido invocada con dos.

# Parámetros opcionales

Para introducir un poco de flexibilidad en lo que respecta a la coincidencia del número de parámetros y de
argumentos de una función, TypeScript permite indicar que un parámetro sea opcional. Los parámetros opcionales
siempre se deben definir después de los parámetros obligatorios:

```typescript
function add(firstNum, secondNum, thirdNum?) {
  return firstNum + secondNum + (thirdNum || 0);
}

let mySum = add(1, 7);
console.log(`mySum = ${mySum}`);

mySum = add(1, 7, 8);
console.log(`mySum = ${mySum}`);
```

Tal y como puede observarse, hemos unificado la funcionalidad de las dos funciones utilizadas en ejemplos anteriores
en una sola, indicando que el parámetro `thirdNum` es opcional añadiendo el caracter `?` después de su nombre.
Una vez dentro de la función, si el valor de `thirdNum` está definido, se utilizará para calcular la suma. En caso
contrario, si es `undefined`, se sumará el valor cero. Si modifica el ejemplo anterior para que el segundo parámetro
sea opcional, en lugar del tercero:

```typescript
function add(firstNum, secondNum?, thirdNum) {
  return firstNum + secondNum + (thirdNum || 0);
}

let mySum = add(1, 7);
console.log(`mySum = ${mySum}`);

mySum = add(1, 7, 8);
console.log(`mySum = ${mySum}`);
```

El compilador de TypeScript informará de los siguientes errores:

```bash
src/index.ts(1,36): error TS1016: A required parameter cannot follow an optional parameter.
src/index.ts(5,13): error TS2554: Expected 3 arguments, but got 2.
```

## Parámetros con valores por defecto

En el ejemplo anterior hemos definido un parámetro como opcional, que ha implicado tener que introducir
cierta lógica en la función `add` para actuar en consecuencia de que el argumento correspondiente a dicho
parámetro opcional no se especifique en la invocación de la función, es decir, que el valor de ese
argumento sea `undefined`.

En ese ejemplo concreto, la introducción de esa lógica adicional podría evitarse haciendo uso de un valor
por defecto que toma el argumento en caso de que no se especifique dicho valor, de manera explícita,
durante la invocación de la función:

```typescript
function add(firstNum, secondNum, thirdNum = 0) {
  return firstNum + secondNum + thirdNum;
}

let mySum = add(1, 7);
console.log(`mySum = ${mySum}`);

mySum = add(1, 7, 8);
console.log(`mySum = ${mySum}`);
```

En el ejemplo anterior, el parámetro `thirdNum` se inicializa por defecto al valor cero. Para definir un valor
por defecto de un parámetro se debe utilizar el carácter `=` seguido de dicho valor por defecto. De este modo, la
función `add` se puede invocar con dos o tres argumentos. En el primer caso, se utilizará el valor cero para llevar
a cabo la suma y, tal y como se comentó anteriormente, se ha podido eliminar del cuerpo de la función toda la lógica
utilizada para comprobar posibles valores `undefined`.

Cabe mencionar en este punto que un parámetro inicializado por defecto sigue siendo un parámetro opcional y,
por lo tanto, debe definirse después de los parámetros obligatorios.

## Parámetros *rest*

Mediante un *parámetro rest*, una función puede recibir un número variable de argumentos. Una función solo puede tener
un único parámetro rest y sus argumentos se agrupan en un array:

```typescript
function add(firstNum, secondNum = 0, ...remainingNums) {
  return firstNum + secondNum +
         remainingNums.reduce((total, value) => total + value, 0);
}

let mySum = add(1);
console.log(`mySum = ${mySum}`);

mySum = add(1, 7);
console.log(`mySum = ${mySum}`);

mySum = add(1, 7, 8);
console.log(`mySum = ${mySum}`);

mySum = add(1, 7, 8, 10, 23);
console.log(`mySum = ${mySum}`);
```

Se puede observar como el parámetro rest `remainingNums` viene precedido de una elipsis, esto es, `...`.
El array de argumentos apuntado por dicho parámetro siempre se inicializa y puede llegar a no contener
ningún valor, en el caso de que el parámetro rest no se utilice, como en el caso de la primera y segunda
invocaciones de la función `add` del ejemplo anterior. En dicho ejemplo, la función `add` debe invocarse,
al menos, con un argumento, el correspondiente al parámetro obligatorio `firstNum`. El segundo parámetro
`secondNum` es opcional, dado que recibe el valor cero por defecto. Los restantes argumentos con los que
se invoca la función van agrupados en el parámetro rest. Por último, cabe mencionar que el parámetro rest
de una función se debe definir después de los parámetros opcionales, que a su vez, se deben definir después
de los parámetros obligatorios.

## 