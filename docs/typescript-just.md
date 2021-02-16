# ¿Por qué debería aprender y utilizar TypeScript?

TypeScript es un superconjunto de JavaScript. No se trata de un lenguaje de programación completamente
nuevo, sino que ha sido diseñado partiendo del lenguaje JavaScript, tomándolo como base y añadiéndole
nuevas características y funcionalidades, lo que permite desarrollar código en JavaScript de un modo
más sencillo, limpio y libre de errores.

No obstante, los programas desarrollados en TypeScript no pueden ejecutarse en entornos JavaScript,
como pueden ser navegadores Web o Node.js, por ejemplo. Respecto a lo anterior, es importante
resaltar que TypeScript no es solo un lenguaje de programación, sino un compilador que permite
generar código fuente en JavaScript a partir de código fuente escrito en TypeScript. Una vez generado
dicho código fuente en JavaScript, el mismo se podrá ejecutar en navegadores Web y otros entornos de
ejecución como Node.js.

Haciendo uso de TypeScript, básicamente lo que conseguimos es escribir código fuente de una manera más
sencilla y cómoda para que, luego, el compilador de TypeScript genere código JavaScript mucho más
complejo y difícil de escribir, algo que tendríamos que hacer a mano sin la existencia de TypeScript.

Además, como su nombre indica, TypeScript permite utilizar tipos de datos estáticos. Lo anterior, como
desarrolladores, es muy importante, dado que los tipos de datos estáticos permiten detectar errores en el
código en fases más tempranas del desarrollo del software y no, por ejemplo, en tiempo de ejecución, cuando
dicho software ya se encuentra en producción.

Un ejemplo de código fuente JavaScript que ilustra lo anterior sería el siguiente:

```javascript
function add(num1, num2) {
  return num1 + num2;
}

console.log(add('1', '7'));
```

Tal y como puede observarse, el resultado de invocar la función `add`, al pasarle dos argumentos
que son cadenas de caracteres, es la cadena '17', dado que en JavaScript el operador `+` permite
concatenar dos cadenas de caracteres. El resultado más esperado, sin embargo, sería el valor numérico 8.
Un error como el anterior es muy común y, muy posiblemente, no nos daríamos cuenta del mismo hasta que
el software estuviera desplegado en un entorno de producción.

Una posible solución, sería utilizar sentencias condicionales en la función `add` para validar los
posibles argumentos que podría recibir dicha función en sus parámetros:


```javascript
function add(num1, num2) {
  if (typeof num1 === 'number' && typeof num2 === 'number') {
    return num1 + num2;
  } else {
    return +num1 + +num2;
  }
}

console.log(add('1', '7'));
console.log(add(1, 7));
console.log(add('1', 7));
console.log(add(1, '7'));
```

Tal y como indicamos antes, TypeScript proporciona nuevas características y ventajas respecto
a JavaScript que permiten desarrollar de un modo más rápido y libre de errores y una de ellas
son los tipos estáticos que se definen mediante anotaciones de tipos. El mismo ejemplo anterior
escrito en TypeScript sería algo como lo siguiente:

```typescript
function add(num1: number, num2: number) {
    return num1 + num2;
}

console.log(add(1, 7));
console.log(add('1', 7));
```

Se puede observar como se han utilizado anotaciones de tipos para ambos parámetros de la función
`add`. Lo anterior no solo elimina la lógica innecesaria de comprobación de tipos de datos de los
argumentos pasados a la función `add`, sino que, además, una invocación de dicha función como
la llevada a cabo en la última línea del ejemplo anterior haría que el compilador de TypeScript
emitiese un error indicándonos que el primer argumento de la función no es válido, dado que se
trata de una cadena de caracteres y se espera un valor numérico.

Para finalizar esta sección, entre las características y ventajas más destacables de TypeScript
caben mencionar las siguientes:

* **Adición de tipos estáticos**. De este modo, debemos ser más explícitos a la hora de especificar
cómo deben funcionar nuestros programas, pero conseguimos evitar errores innecesarios e inesperados.
* **Soporte para entornos de desarrollo modernos**. Con lo anterior podemos utilizar funcionalidades
de desarrollo avanzadas proporcionadas por dichos entornos, como pueden ser la detección de errores
en tiempo de desarrollo, completar código de manera automática o acceder a documentación inmediatamente,
entre otras.
* **Soporte de nuevas funcionalidades incluidas en versiones modernas de JavaScript**. TypeScript permite
utilizar las nuevas características y funcionalidades incluidas en las versiones más modernas de JavaScript
y obtener código JavaScript compatible, incluso, con entornos de ejecución antiguos.
* **Soporte de nuevas funcionalidades no existentes en JavaScript**. Algunas de esas funcionalidades son
las interfaces y los tipos genéricos, por ejemplo.
* **Alto grado de configuración**. El compilador de TypeScript es altamente configurable, lo cual permite
adaptar dicha configuración a las características particulares de cada desarrollo.
