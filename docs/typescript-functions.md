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

Por lo tanto, para evitar este tipo de errores, se puede combinar la funcionalidad de las funciones en una única
función o se puede asignar nombres diferentes a las funciones. El siguiente ejemplo ilustra la segunda opción:

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
console.log(`mySum = ${mySum}`)
```

