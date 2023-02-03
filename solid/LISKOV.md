# El principio de sustitución de Liskov

La famosa definición de subtipos de Barbara Liskov, de 1988 en un discurso de apertura de una conferencia titulado Data Abstraction and Hierarchy. En resumen, este principio dice que para construir sistemas de software a partir de partes intercambiables, esas partes deben adherirse a un contrato que permita que esas partes se sustituyan entre sí.

## Puedes entenderlo de la siguiente manera

Uno de los ejemplos clásicos de este principio es un rectángulo que tiene cuatro lados. La altura de un rectángulo puede ser cualquier valor y el ancho puede ser cualquier valor. Un cuadrado es un rectángulo con igual ancho y alto. Entonces podemos decir que podemos extender las propiedades de la clase rectángulo a la clase cuadrada. Para hacer eso, debe intercambiar la clase secundaria (cuadrado) con la clase principal (rectángulo) para que se ajuste a la definición de un cuadrado que tiene cuatro lados iguales, pero una clase derivada no afecta el comportamiento de la clase principal, así que si lo hace que violará el principio de sustitución de Liskov.

## ¿Ves lo que va mal?

Considere que tenemos una aplicación que usa un objeto rectángulo definido de la siguiente manera.

Basándonos en el conocimiento de que un cuadrado es un rectángulo cuyos lados tienen la misma longitud, decidimos crear un objeto cuadrado para usar en lugar de un rectángulo.

```tsx
var square = {};
(function () {
  var length = 0,
    width = 0;

  Object.defineProperties(square, "length", {
    get: function () {
      return length;
    },
    set: function () {
      length = width = value;
    },
  });

  Object.defineProperties(square, "width", {
    get: function () {
      return width;
    },
    set: function () {
      length = width = value;
    },
  });
})();
```

Desafortunadamente, se descubre un problema cuando la aplicación intenta usar nuestro cuadrado en lugar de un rectángulo. Resulta que uno de los métodos calcula el área del rectángulo así.

```tsx
const area = function(rectangle){
	return (rectangle.length * rectangle.width)
}
```

Cuando se invoca el método con un cuadrado, el producto es 16 en lugar del valor esperado de 12. Nuestro objeto cuadrado viola el principio de sustitución de Liskov con respecto a la función de área. En este caso, la presencia de las propiedades de largo y ancho fue una pista de que nuestro cuadrado podría no ser 100 % compatible con el rectángulo, pero no siempre tendremos pistas tan obvias.
