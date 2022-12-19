# Builder Pattern con Javascript

El patrón Builder es uno de los patrones de los cuales más variantes e implementaciones diferentes he visto por internet sobre todo con Java. Antes de empezar mencionar que este artículo es el segundo artículo acerca de Patrones de diseño, puedes leer aquí acerca del patrón Singleton en Javascript.

La verdad me ha costado ordenar mi cabeza para ver cuál era la mejor forma de enseñar este patrón y al final me he decidido por la forma más simple que conozco de modo que se entienda tanto si eres un pro de Javascript como si te estas iniciando. Pero primero paso a citar la intención por la cual existe este patrón.

El patrón Builder consiste en separar la complejidad durante la construcción de un objeto de su representación de manera que con el mismo constructor puedas crear diferentes representaciones.

La forma de crear objetos a partir de su clase la gran mayoría de las veces no es algo muy descriptivo. Por lo general consiste en pasarle un montón de argumentos al constructor de la clase sin saber muy bien lo que le pasas y para qué se lo pasas.

En este ejemplo como veis estamos creando una nueva instancia de Pizza pero ¿qué son todos esos (true, true, true, 3, 4, [‘pepperoni’, ‘sausages’])?

````js
const Pizza = require("./pizza");
const pizza = new Pizza(true, true, true, 3, 4, ["pepperoni", "sausages"]);
````

Solamente si echamos un vistazo a la clase constructora podremos llegar a entender para qué sirve cada parámetro que le pasamos.

````js
export default class Pizza {
  constructor() {
    this.tomato = false;
    this.cheese = false;
    this.thinDough = false;
    this.pineappleSlices = 0;
    this.baconStrips = 0;
    this.otherIngredients = [];
  }
}
````

Pero, ¿es este código mantenible y reusable a largo plazo?🤔 Yo creo que no. Pero podemos hacerlo mucho mas mantenible y reusable gracias al patrón Builder.

Para eso lo que haremos es crear una clase Pizza y un método diferente por cada argumento que necesitamos pasarle al constructor devolviendo this en cada uno de ellos. Con lo cual nuestro código quedaría de la siguiente forma:

````js
class Pizza {
  constructor() {
    this.tomato = false;
    this.cheese = false;
    this.thinDough = false;
    this.pineappleSlices = 0;
    this.baconStrips = 0;
    this.otherIngredients = [];
  }
setTomato() {
    this.tomato = true;
    return this;
  }
setCheese() {
    this.cheese = true;
    return this;
  }
setThinDough() {
    this.thinDough = true;
    return this;
  }
setPineappleSlices(slices) {
    this.pineappleSlices = slices;
    return this;
  }
setBaconStrips(strips) {
    this.baconStrips = strips;
    return this;
  }
addOtherIngredients(ingredient) {
    this.otherIngredients.push(ingredient);
    return this;
  }
build() {
    return {
      tomato: this.tomato,
      cheese: this.cheese,
      thinDough: this.thinDough,
      pineappleSlices: this.pineappleSlices,
      baconStrips: this.baconStrips,
      otherIngredients: this.otherIngredients
    };
  }
}
````

Como veis, en el constructor de la clase Pizza hemos instanciado todos los valores por defecto que queremos de los ingredientes de nuestra pizza. (En serio, estoy escribiendo esto casi a la hora de cenar y me esta entrando un hambre…) 😋

````js
this.tomato = false
this.cheese = false
this.thinDough = false
this.pineappleSlices = 0
this.baconStrips = 0
this.otherIngredients = []
````

Después, hemos creado un método para seleccionar si queremos queso, otro para indicar si queremos masa fina, otro para si queremos añadir rodajas de piña a nuestra pizza… (se que más de uno va a dejar de leer aquí 😂)

Y al final utilizaremos nuestro método build que construirá nuestra nueva instancia de Pizza.

🍕 Creación de una pizza mediante la llamada de todos los métodos
Ahora sí quisiéramos crear una nueva instancia de Pizza a partir del patrón Builder, lo haríamos de la siguiente manera, de manera que estaríamos indicando uno a uno de los ingredientes que se compone.

````js

const pizza = new Pizza()
    .setTomato()
    .setCheese()
    .setThinDough()
    .setPineappleSlices(3)
    .setBaconStrips(4)
    .setOtherIngredients(['Pepperoni', 'Sausages'])
    .build()
    
 ````
 
 El valor de nuestro objeto pizza sería algo así como este:

````js

{
    tomato: true, 
    cheese: true, 
    thinDough: true, 
    pineappleSlices: 3, 
    baconStrips: 4, 
    otherIngredients: [‘Pepperoni’, ‘Sausages’]
}

````

🍕 Creación de una pizza mediante la llamada parcial de los métodos
Una de las ventajas de trabajar con el patrón Builder es que podemos crear representaciones de los objetos como nos venga en gana. No hace falta que pasemos argumentos con valor null para los ingredientes que no queremos añadir ni nada por el estilo. Solamente llamamos a los métodos que nos interesan para crear nuestra pizza y punto ¿a qué es fácil?

En esta ocasión solo vamos a querer una pizza con masa fina y 4 trozos de bacon:

````js

const pizza = new Pizza()
    .setThinDough()
    .setBaconStrips(4)
    .build()
    
    ````
    
 La representación del objeto con nuestra pizza ‘al gusto’ sería el siguiente:


````js

{
    tomato: false, 
    cheese: false, 
    thinDough: true, 
    pineappleSlices: 0, 
    baconStrips: 4, 
    otherIngredients: []
}

````

¡Así de fácil es crear objetos con el patrón builder y Javascript! ¡Espero que te haya gustado este artículo, compártelo en tus redes sociales para que otros compañeros conozcan este patrón y nos leemos en el próximo! 🤟
