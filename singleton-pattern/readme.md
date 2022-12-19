# El patrón singleton en JavaScript

## Introducción

Los patrones de diseño suelen utilizarse como herramientas para afrontar aquellos problemas más comunes que se nos presentan a diario.

Ahora vamos a repasar una estructura muy conocida para aquellos desarrolladores que provienen de lenguajes como Java y que tiene por nombre singleton.

## Patrón Singleton

Según la siempre sabia Wikipedia, el patrón de diseño singleton (instancia única) está diseñado para restringir la creación de objetos pertenecientes a una clase o el valor de un tipo a un único objeto. Su intención consiste en garantizar que una clase sólo tenga una instancia y proporcionar un punto de acceso global a ella.

Este tipo de patrón se implementa creando en nuestra clase un método que crea a su vez una instancia del objeto sólo si añun no existe alguna. Para asegurar que la clase no puede ser instanciada nuevamente se regula el alcance del constructor (con atributos como protegido o privado).

En lenguajes como el mencionado Java, no se puede crear un objeto directamente, sino que se necesita una clase para hacerlo; es cierto que existen excepciones pero para ilustrar este artículo las pasaremos por alto.

En definitiva, con el patrón singleton, se prentende que la clase corresponda con una única instancia estableciéndose una relación uno-a-uno entre ambas.

A continuación se muestra un ejemplo de este esquema en Java:

````java
public class Singleton {
 
  public static final Singleton INSTANCE = new Singleton();
 
  // Private constructor prevents external instantiation
  private Singleton() {
  }
 
  public void amethod() {
    // ...
  }
}
````

## El singleton en Javascript

Javascript permite la creación directa de objetos. De hecho, es de los pocos lenguajes de programación que poseen esta característica, por lo que un esquema de este tipo no es estrictamente necesario. Obsérvese el siguiente ejemplo:

````js
var namespace = {
  singleton: {
    amethod: function() {
      console.log("amethod");
    }
  }
};
// Invoke: namespace.singleton.amethod()
````

El contexto creado con namespace no es imprescindible, pero si una buena práctica para aislar el objeto. Por lo demás, como vemos, las características de Javascript hacen que este esquema no tenga una portabilidad demasiado útil. De hecho, tiene muchas similitudes con el esquema del módulo que mencionamos antes y sus expansiones.

Si necesitamos crear el singleton bajo demanda (lo que conocemos generalmente con el término lazily), el código se complica un poco:

````js
var namespace = {
  _singleton: null,
  getSingleton: function() {
    if (!this._singleton) {
      this._singleton = {
        amethod: function() {
          console.log("amethod");
        }
      }
    }
    return this._singleton;
  }
};
// Invoke: namespace.getSingleton().amethod()
````

A simple vista, el código es más complejo de lo que debería. En navegadores recientes, gracias a la nueva especificación ECMAScript 5, este ejemplo podría reducirse considerablemente a través del uso del método getter.

````js
var namespace = {
  _singleton: null,
  get singleton() {
    if (!this._singleton) {
      this._singleton = {
        amethod: function() {
          console.log("amethod");
        }
      }
    }
    return this._singleton;
  }
};
// Invoke: namespace.singleton.amethod()
````

## Consideraciones sobre seguridad

Las soluciones presentadas arriba pueden parecer demasiado simples, pero son son relativamente fáciles de entender y con un funcionamiento obvio. Sin embargo, en determinados escenarios, es posible que sea necesario mejorar la estructura anterior para ofrecer funcionalidades semejantes a las que encontramos en otros lenguajes.

Algunos de estos requisitos exigibles podrían ser los siguientes:

Prevenir la creación de copias: los que provienen de Java, tienen una conciencia especial sobre este aspecto y es posible conseguirlo escondiendo el constructor.
Prevenir que el patrón pueda ser reemplazado: un atacante puede tratar de cambiar el singleton de una aplicación por otro implementado por él mismo. Mediante los nuevos atributos ECMAScript 5 de solo lectura (read-only) y de no-configuración (non-configurable), se puede conseguir fácilmente:

````js
  Object.defineProperty( namespace, "singleton",
    { writable: false, configurable: false, value: { ... } } );
````

Prevenir que el singleton pueda ser modificado: Los métodos methodsObject.preventExtensions(), Object.seal(), y Object.freeze() nos pueden permitir esa granularidad deseada.
Mantener los datos privados: De nuevo, son los programadores Java los que más buscarán este comportamiento. Conseguirlo es sencillo: basta con crear una función autoejecutable que envuelva al singleton para esconder sus variables al exterior:

````js
var namespace = {
    getSingleton: (function() { // BEGIN iife
      var singleton;
      return function() {
        if (!singleton) {
          singleton = {
            amethod: function() {
              console.log("amethod");
            }
          }
        }
        return singleton;
      };
    }()) // END iife
  };
  // Invoke: namespace.getSingleton().amethod()
````

## Conclusión

Aunque debido a las características de Javascript un patrón singleton no es estrictamente necesario, ni especialmente útil, los desarrolladores que provienen de otros lenguajes como Java se encontrarán mucho más cómodos con una estructura que les resulta muy familiar. Mediante algunos artificios y giros del lenguaje, podemos conseguir una funcionalidad similar a algo que, reitero una vez más, no tiene mayor razón de ser: debería quedar únicamente como un juego o ejercicio de estructura POO antes que como un código eficaz.

A tal respecto, en el libro Essential JAvaScript & jQuery Design Patterns, Addy Osmani concluye con que este patrón, tal y como lo hemos tratado anteriormente, puede resultar útil únicamente cuando necesitamos que un objeto concreto y exacto coordine otras implementaciones de patrones a lo largo de un sistema.

En mi opinión, podemos prescindir de esta estructura sin miedo.

  
