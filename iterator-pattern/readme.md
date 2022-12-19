Entendiendo el patrón observador (Observer Pattern) en Javascript
En este post aprenderemos como implementar el patrón observer en Javascript y en que situaciones lo podemos usar.

El patrón observador es uno de los patrones de diseño de software más usado en Javascript. De el se extienden importantes aplicaciones que pueden ayudar a definir mejores arquitecturas en aplicaciones web, por lo que su uso y estudio es altamente recomendado. En este post aprenderemos como implementarlo en Javascript y en que situaciones lo podemos usar.

En primer lugar el patrón observador se define como un patrón de comportamiento es decir que dentro del mundo de la programación orientada a objetos es un patrón responsable por la comunicación entre objetos.

En segundo lugar el patrón observador también puedes encontrarlo como el patrón publicador-subscriptor o modelo-patrón y nos da una idea básica de lo que hace. En términos simples este patrón permite la notificación a objetos (subscriptores u observador) al cambio de otro objeto (publicador).

¿Cómo podemos entonces implementarlo en Javascript?, empecemos implementando una clase llamada Publicador que contenga los métodos subscribe(), unsubscribe(), y notify().

````js
class Publicador {
  constructor() {
    this.subscriptors = \[\];
  }


  subscribe(subscriptor) {
    this.subscriptors.push(subscriptor);
  }


  unsubscribe(subscriptor) {
    this.subscriptors = this.subscriptors.filter(
      (item) => item !== subscriptor
    );
  }


  notify(event) {
    this.subscriptors.forEach((item) => {
      item.call(this, event);
    });
  }
}
````

Como verás hemos manejado la lista de subscriptores con un array de Javascript como propiedad de la clase así es posible fácilmente subscribir y des-subscribir subscriptores. También la función notify itera sobre cada uno de los subscriptores y se encarga de invocarlos con el evento.

Ahora imaginemos que usaremos esta definición de Publicador para un periódico que regularmente publica nuevas ediciones.

````javascript
const periodico = new Publicador();
````

