# Observer Pattern

---

## **Propósito**

**Observer** es un patrón de diseño de comportamiento que te permite definir un mecanismo de suscripción para notificar a varios objetos sobre cualquier evento que le suceda al objeto que están observando.

![Untitled](Observer%20Pattern%20d3161883803843fba87653a8a0eac808/Untitled.png)

## **Problema**

Imagina que tienes dos tipos de objetos: un objeto `Cliente` y un objeto `Tienda`. El cliente está muy interesado en una marca particular de producto (digamos, un nuevo modelo de iPhone) que estará disponible en la tienda muy pronto.

El cliente puede visitar la tienda cada día para comprobar la disponibilidad del producto. Pero, mientras el producto está en camino, la mayoría de estos viajes serán en vano.

![Untitled](Observer%20Pattern%20d3161883803843fba87653a8a0eac808/Untitled%201.png)

*Visita a la tienda vs. envío de spam*

Por otro lado, la tienda podría enviar cientos de correos (lo cual se podría considerar spam) a todos los clientes cada vez que hay un nuevo producto disponible. Esto ahorraría a los clientes los interminables viajes a la tienda, pero, al mismo tiempo, molestaría a otros clientes que no están interesados en los nuevos productos.

Parece que nos encontramos ante un conflicto. O el cliente pierde tiempo comprobando la disponibilidad del producto, o bien la tienda desperdicia recursos notificando a los clientes equivocados.

## **Solución**

El objeto que tiene un estado interesante suele denominarse *sujeto*, pero, como también va a notificar a otros objetos los cambios en su estado, le llamaremos *notificador* (en ocasiones también llamado *publicador*). El resto de los objetos que quieren conocer los cambios en el estado del notificador, se denominan *suscriptores*.

El patrón Observer sugiere que añadas un mecanismo de suscripción a la clase notificadora para que los objetos individuales puedan suscribirse o cancelar su suscripción a un flujo de eventos que proviene de esa notificadora. ¡No temas! No es tan complicado como parece. En realidad, este mecanismo consiste en: 1) un campo matriz para almacenar una lista de referencias a objetos suscriptores y 2) varios métodos públicos que permiten añadir suscriptores y eliminarlos de esa lista.

![Untitled](Observer%20Pattern%20d3161883803843fba87653a8a0eac808/Untitled%202.png)

*Un mecanismo de suscripción permite a los objetos individuales suscribirse a notificaciones de eventos.*

Ahora, cuando le sucede un evento importante al notificador, recorre sus suscriptores y llama al método de notificación específico de sus objetos.

Las aplicaciones reales pueden tener decenas de clases suscriptoras diferentes interesadas en seguir los eventos de la misma clase notificadora. No querrás acoplar la notificadora a todas esas clases. Además, puede que no conozcas algunas de ellas de antemano si se supone que otras personas pueden utilizar tu clase notificadora.

Por eso es fundamental que todos los suscriptores implementen la misma interfaz y que el notificador únicamente se comunique con ellos a través de esa interfaz. Esta interfaz debe declarar el método de notificación junto con un grupo de parámetros que el notificador puede utilizar para pasar cierta información contextual con la notificación.

![Untitled](Observer%20Pattern%20d3161883803843fba87653a8a0eac808/Untitled%203.png)

*El notificador notifica a los suscriptores invocando el método de notificación específico de sus objetos.*

Si tu aplicación tiene varios tipos diferentes de notificadores y quieres hacer a tus suscriptores compatibles con todos ellos, puedes ir más allá y hacer que todos los notificadores sigan la misma interfaz. Esta interfaz sólo tendrá que describir algunos métodos de suscripción. La interfaz permitirá a los suscriptores observar los estados de los notificadores sin acoplarse a sus clases concretas.

## **Analogía en el mundo real**

![Untitled](Observer%20Pattern%20d3161883803843fba87653a8a0eac808/Untitled%204.png)

*Suscripciones a revistas y periódicos.*

Si te suscribes a un periódico o una revista, ya no necesitarás ir a la tienda a comprobar si el siguiente número está disponible. En lugar de eso, el notificador envía nuevos números directamente a tu buzón justo después de la publicación, o incluso antes.

El notificador mantiene una lista de suscriptores y sabe qué revistas les interesan. Los suscriptores pueden abandonar la lista en cualquier momento si quieren que el notificador deje de enviarles nuevos números.

## **Estructura**

![Untitled](Observer%20Pattern%20d3161883803843fba87653a8a0eac808/Untitled%205.png)

1. El **Notificador** envía eventos de interés a otros objetos. Esos eventos ocurren cuando el notificador cambia su estado o ejecuta algunos comportamientos. Los notificadores contienen una infraestructura de suscripción que permite a nuevos y antiguos suscriptores abandonar la lista.
2. Cuando sucede un nuevo evento, el notificador recorre la lista de suscripción e invoca el método de notificación declarado en la interfaz suscriptora en cada objeto suscriptor.
3. La interfaz **Suscriptora** declara la interfaz de notificación. En la mayoría de los casos, consiste en un único método `actualizar`. El método puede tener varios parámetros que permitan al notificador pasar algunos detalles del evento junto a la actualización.
4. Los **Suscriptores Concretos** realizan algunas acciones en respuesta a las notificaciones emitidas por el notificador. Todas estas clases deben implementar la misma interfaz de forma que el notificador no esté acoplado a clases concretas.
5. Normalmente, los suscriptores necesitan cierta información contextual para manejar correctamente la actualización. Por este motivo, a menudo los notificadores pasan cierta información de contexto como argumentos del método de notificación. El notificador puede pasarse a sí mismo como argumento, dejando que los suscriptores extraigan la información necesaria directamente.
6. El **Cliente** crea objetos tipo notificador y suscriptor por separado y después registra a los suscriptores para las actualizaciones del notificador.

## **Pseudocódigo**

En este ejemplo, el patrón **Observer** permite al objeto editor de texto notificar a otros objetos tipo servicio sobre los cambios en su estado.

![Untitled](Observer%20Pattern%20d3161883803843fba87653a8a0eac808/Untitled%206.png)

*Notificar a objetos sobre eventos que suceden a otros objetos.*

La lista de suscriptores se compila dinámicamente: los objetos pueden empezar o parar de escuchar notificaciones durante el tiempo de ejecución, dependiendo del comportamiento que desees para tu aplicación.

En esta implementación, la clase editora no mantiene la lista de suscripción por sí misma. Delega este trabajo al objeto ayudante especial dedicado justo a eso. Puedes actualizar ese objeto para que sirva como despachador centralizado de eventos, dejando que cualquier objeto actúe como notificador.

Añadir nuevos suscriptores al programa no requiere cambios en clases notificadoras existentes, siempre y cuando trabajen con todos los suscriptores a través de la misma interfaz.

```tsx
// La clase notificadora base incluye código de gestión de
// suscripciones y métodos de notificación.
class EventManager is
    private field listeners: hash map of event types and listeners

    method subscribe(eventType, listener) is
        listeners.add(eventType, listener)

    method unsubscribe(eventType, listener) is
        listeners.remove(eventType, listener)

    method notify(eventType, data) is
        foreach (listener in listeners.of(eventType)) do
            listener.update(data)

// El notificador concreto contiene lógica de negocio real, de
// interés para algunos suscriptores. Podemos derivar esta clase
// de la notificadora base, pero esto no siempre es posible en
// el mundo real porque puede que la notificadora concreta sea
// ya una subclase. En este caso, puedes modificar la lógica de
// la suscripción con composición, como hicimos aquí.
class Editor is
    public field events: EventManager
    private field file: File

    constructor Editor() is
        events = new EventManager()

    // Los métodos de la lógica de negocio pueden notificar los
    // cambios a los suscriptores.
    method openFile(path) is
        this.file = new File(path)
        events.notify("open", file.name)

    method saveFile() is
        file.write()
        events.notify("save", file.name)

    // ...

// Aquí está la interfaz suscriptora. Si tu lenguaje de
// programación soporta tipos funcionales, puedes sustituir toda
// la jerarquía suscriptora por un grupo de funciones.

interface EventListener is
    method update(filename)

// Los suscriptores concretos reaccionan a las actualizaciones
// emitidas por el notificador al que están unidos.
class LoggingListener implements EventListener is
    private field log: File
    private field message: string

    constructor LoggingListener(log_filename, message) is
        this.log = new File(log_filename)
        this.message = message

    method update(filename) is
        log.write(replace('%s',filename,message))

class EmailAlertsListener implements EventListener is
    private field email: string
    private field message: string

    constructor EmailAlertsListener(email, message) is
        this.email = email
        this.message = message

    method update(filename) is
        system.email(email, replace('%s',filename,message))

// Una  aplicación puede configurar notificadores y suscriptores
// durante el tiempo de ejecución.
class Application is
    method config() is
        editor = new Editor()

        logger = new LoggingListener(
            "/path/to/log.txt",
            "Someone has opened the file: %s")
        editor.events.subscribe("open", logger)

        emailAlerts = new EmailAlertsListener(
            "admin@example.com",
            "Someone has changed the file: %s")
        editor.events.subscribe("save", emailAlerts)
```

## **Aplicabilidad**

**Utiliza el patrón Observer cuando los cambios en el estado de un objeto puedan necesitar cambiar otros objetos y el grupo de objetos sea desconocido de antemano o cambie dinámicamente.**

Puedes experimentar este problema a menudo al trabajar con clases de la interfaz gráfica de usuario. Por ejemplo, si creaste clases personalizadas de botón y quieres permitir al cliente colgar código cliente de tus botones para que se active cuando un usuario pulse un botón.

El patrón Observer permite que cualquier objeto que implemente la interfaz suscriptora pueda suscribirse a notificaciones de eventos en objetos notificadores. Puedes añadir el mecanismo de suscripción a tus botones, permitiendo a los clientes acoplar su código personalizado a través de clases suscriptoras personalizadas.

**Utiliza el patrón cuando algunos objetos de tu aplicación deban observar a otros, pero sólo durante un tiempo limitado o en casos específicos.**

La lista de suscripción es dinámica, por lo que los suscriptores pueden unirse o abandonar la lista cuando lo deseen.

****Cómo implementarlo****
1. Repasa tu lógica de negocio e intenta dividirla en dos partes: la funcionalidad central, independiente del resto de código, actuará como notificador; el resto se convertirá en un grupo de clases suscriptoras.
2. Declara la interfaz suscriptora. Como mínimo, deberá declarar un único método `actualizar`.
3. Declara la interfaz notificadora y describe un par de métodos para añadir y eliminar de la lista un objeto suscriptor. Recuerda que los notificadores deben trabajar con suscriptores únicamente a través de la interfaz suscriptora.
4. Decide dónde colocar la lista de suscripción y la implementación de métodos de suscripción. Normalmente, este código tiene el mismo aspecto para todos los tipos de notificadores, por lo que el lugar obvio para colocarlo es en una clase abstracta derivada directamente de la interfaz notificadora. Los notificadores concretos extienden esa clase, heredando el comportamiento de suscripción.
Sin embargo, si estás aplicando el patrón a una jerarquía de clases existentes, considera una solución basada en la composición: coloca la lógica de la suscripción en un objeto separado y haz que todos los notificadores reales la utilicen.
5. Crea clases notificadoras concretas. Cada vez que suceda algo importante dentro de una notificadora, deberá notificar a todos sus suscriptores.
6. Implementa los métodos de notificación de actualizaciones en clases suscriptoras concretas. La mayoría de las suscriptoras necesitarán cierta información de contexto sobre el evento, que puede pasarse como argumento del método de notificación.
Pero hay otra opción. Al recibir una notificación, el suscriptor puede extraer la información directamente de ella. En este caso, el notificador debe pasarse a sí mismo a través del método de actualización. La opción menos flexible es vincular un notificador con el suscriptor de forma permanente a través del constructor.
7. El cliente debe crear todos los suscriptores necesarios y registrarlos con los notificadores adecuados.

## **Pros y contras**

- *Principio de abierto/cerrado*. Puedes introducir nuevas clases suscriptoras sin tener que cambiar el código de la notificadora (y viceversa si hay una interfaz notificadora).
- Puedes establecer relaciones entre objetos durante el tiempo de ejecución.
- Los suscriptores son notificados en un orden aleatorio.

## Ejemplo Ts:

```tsx
/**
 * The Subject interface declares a set of methods for managing subscribers.
 */
interface Subject {
    // Attach an observer to the subject.
    attach(observer: Observer): void;

    // Detach an observer from the subject.
    detach(observer: Observer): void;

    // Notify all observers about an event.
    notify(): void;
}

/**
 * The Subject owns some important state and notifies observers when the state
 * changes.
 */
class ConcreteSubject implements Subject {
    /**
     * @type {number} For the sake of simplicity, the Subject's state, essential
     * to all subscribers, is stored in this variable.
     */
    public state: number;

    /**
     * @type {Observer[]} List of subscribers. In real life, the list of
     * subscribers can be stored more comprehensively (categorized by event
     * type, etc.).
     */
    private observers: Observer[] = [];

    /**
     * The subscription management methods.
     */
    public attach(observer: Observer): void {
        const isExist = this.observers.includes(observer);
        if (isExist) {
            return console.log('Subject: Observer has been attached already.');
        }

        console.log('Subject: Attached an observer.');
        this.observers.push(observer);
    }

    public detach(observer: Observer): void {
        const observerIndex = this.observers.indexOf(observer);
        if (observerIndex === -1) {
            return console.log('Subject: Nonexistent observer.');
        }

        this.observers.splice(observerIndex, 1);
        console.log('Subject: Detached an observer.');
    }

    /**
     * Trigger an update in each subscriber.
     */
    public notify(): void {
        console.log('Subject: Notifying observers...');
        for (const observer of this.observers) {
            observer.update(this);
        }
    }

    /**
     * Usually, the subscription logic is only a fraction of what a Subject can
     * really do. Subjects commonly hold some important business logic, that
     * triggers a notification method whenever something important is about to
     * happen (or after it).
     */
    public someBusinessLogic(): void {
        console.log('\nSubject: I\'m doing something important.');
        this.state = Math.floor(Math.random() * (10 + 1));

        console.log(`Subject: My state has just changed to: ${this.state}`);
        this.notify();
    }
}

/**
 * The Observer interface declares the update method, used by subjects.
 */
interface Observer {
    // Receive update from subject.
    update(subject: Subject): void;
}

/**
 * Concrete Observers react to the updates issued by the Subject they had been
 * attached to.
 */
class ConcreteObserverA implements Observer {
    public update(subject: Subject): void {
        if (subject instanceof ConcreteSubject && subject.state < 3) {
            console.log('ConcreteObserverA: Reacted to the event.');
        }
    }
}

class ConcreteObserverB implements Observer {
    public update(subject: Subject): void {
        if (subject instanceof ConcreteSubject && (subject.state === 0 || subject.state >= 2)) {
            console.log('ConcreteObserverB: Reacted to the event.');
        }
    }
}

/**
 * The client code.
 */

const subject = new ConcreteSubject();

const observer1 = new ConcreteObserverA();
subject.attach(observer1);

const observer2 = new ConcreteObserverB();
subject.attach(observer2);

subject.someBusinessLogic();
subject.someBusinessLogic();

subject.detach(observer2);

subject.someBusinessLogic();
```

**Resultado**:

```tsx
Subject: Attached an observer.
Subject: Attached an observer.

Subject: I'm doing something important.
Subject: My state has just changed to: 6
Subject: Notifying observers...
ConcreteObserverB: Reacted to the event.

Subject: I'm doing something important.
Subject: My state has just changed to: 1
Subject: Notifying observers...
ConcreteObserverA: Reacted to the event.
Subject: Detached an observer.

Subject: I'm doing something important.
Subject: My state has just changed to: 5
Subject: Notifying observers...
```

## **Entendiendo el patrón observador (Observer Pattern) en Javascript**

Entendiendo el patrón observador (Observer Pattern) en Javascript En este post aprenderemos como implementar el patrón observer en Javascript y en que situaciones lo podemos usar.

El patrón observador es uno de los patrones de diseño de software más usado en Javascript. De el se extienden importantes aplicaciones que pueden ayudar a definir mejores arquitecturas en aplicaciones web, por lo que su uso y estudio es altamente recomendado. En este post aprenderemos como implementarlo en Javascript y en que situaciones lo podemos usar.

En primer lugar el patrón observador se define como un patrón de comportamiento es decir que dentro del mundo de la programación orientada a objetos es un patrón responsable por la comunicación entre objetos.

En segundo lugar el patrón observador también puedes encontrarlo como el patrón publicador-subscriptor o modelo-patrón y nos da una idea básica de lo que hace. En términos simples este patrón permite la notificación a objetos (subscriptores u observador) al cambio de otro objeto (publicador).

¿Cómo podemos entonces implementarlo en Javascript?, empecemos implementando una clase llamada Publicador que contenga los métodos subscribe(), unsubscribe(), y notify().

```jsx
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
```

Como verás hemos manejado la lista de subscriptores con un array de Javascript como propiedad de la clase así es posible fácilmente subscribir y des-subscribir subscriptores. También la función notify itera sobre cada uno de los subscriptores y se encarga de invocarlos con el evento.

Ahora imaginemos que usaremos esta definición de Publicador para un periódico que regularmente publica nuevas ediciones.

```jsx
const periodico = new Publicador();
```

Bien, pensemos un poco más acerca de los clientes. Estos van a necesitar saber cuando llegue una nueva versión del periódico. Inicialmente pensemos en que los clientes son funciones:

```jsx
function Observer(edicion) {
  console.log("LLegó una nueva edición con el nombre de: " + edicion);
}

periodico.subscribe(Observer);
periodico.subscribe(Observer);
periodico.notify("Nueva edicion");
```

Con esta definición anterior al ejecutarlo obtenemos algo así:

"LLegó una nueva edición con el nombre de: Nueva edicion" "LLegó una nueva edición con el nombre de: Nueva edicion"

Si queremos ser conscientes de que cliente recibió que edición del periódico podemos retocar un poco las definiciones de la función notify y de la función observer:

```jsx
class Publicador {
      ...
      notify(event) {
        let index = 0;
        this.subscriptors.forEach( item => {
          item.call(this, index, event);
          index++;
        });
      }
      ...
    }
    ...
    function Observer(index, edicion) {
      console.log("Al Observador #" +
                  index + " le llegó una nueva edición con el nombre de: " +
                  edicion);
    }
```

De esta manera tenemos como output algo mucho mas entendible:

```jsx
"Al Observador #0 le llegó una nueva edición con el nombre de: Nueva edicion";
"Al Observador #1 le llegó una nueva edición con el nombre de: Nueva edicion";
```

Sin embargo como un mejor acercamiento podríamos definir una clase para los clientes que nos permita crear instancias de ella y tener un control mas granular. También debemos definir un método únicamente diseñado para escuchar por nuevas decisiones del periódico.

```jsx
class Observador {
  constructor(id) {
    this.id = id;
    console.log("Se ha creado el subscriptor #: " + id);
  }
  buzon(edicion) {
    console.log(
      "Subscriptor # " + this.id + " recibió una nueva edición: " + edicion
    );
  }
}

const subscriptor1 = new Subscriptor(1);
const subscriptor2 = new Subscriptor(2);
const subscriptor3 = new Subscriptor(3);
```

De esta forma podemos tener más control sobre los subscriptores y podemos subscribirlos y des-subscribirlos de mejor manera. A continuación verás como publicar multiples ediciones de un perioido así como la habilidad suscribir y des-suscribir clientes:

```jsx
periodico.subscribe(subscriptor1);
periodico.subscribe(subscriptor2);
periodico.notify("Nueva edicion");
periodico.subscribe(subscriptor3);
periodico.notify("Segunda edicion");
periodico.unsubscribe(subscriptor1);
periodico.notify("Tercera edicion");
periodico.subscribe(subscriptor2);
periodico.notify("Nueva edicion");
periodico.subscribe(subscriptor3);
periodico.notify("Segunda edicion");
periodico.unsubscribe(subscriptor1);
periodico.notify("Tercera edicion");
```

Y obtenemos la siguiente salida:

```jsx
"--- Primera edición ---";

"Subscriptor # 1 recibió una nueva edición: Nueva edicion";

"Subscriptor # 2 recibió una nueva edición: Nueva edicion";

"--- Segunda edición ---";

"Subscriptor # 1 recibió una nueva edición: Segunda edicion";

"Subscriptor # 2 recibió una nueva edición: Segunda edicion";

"Subscriptor # 3 recibió una nueva edición: Segunda edicion";

"--- Tercera edición ---";

"Subscriptor # 2 recibió una nueva edición: Tercera edicion";

"Subscriptor # 3 recibió una nueva edición: Tercera edicion";
```

De esta manera queda totalmente completo el patrón observer. Como vez es muy fácil implementar el patrón observer y su utilidad es casi inmediata. A continuación podrás observar todo el código completo de este ejemplo:

```jsx
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
      item.buzon.call(item, event);
    });
  }
}

class Subscriptor {
  constructor(id) {
    this.id = id;
    console.log("Se ha creado el subscriptor #: " + id);
  }
  buzon(edicion) {
    console.log(
      "Subscriptor # " + this.id + " recibió una nueva edición: " + edicion
    );
  }
}

const periodico = new Publicador();

const subscriptor1 = new Subscriptor(1);
const subscriptor2 = new Subscriptor(2);
const subscriptor3 = new Subscriptor(3);

console.log("--- Primera edición ---");

periodico.subscribe(subscriptor1);

periodico.subscribe(subscriptor2);

periodico.notify("Nueva edicion");

console.log("--- Segunda edición ---");

periodico.subscribe(subscriptor3);

periodico.notify("Segunda edicion");

console.log("--- Tercera edición ---");

periodico.unsubscribe(subscriptor1);

periodico.notify("Tercera edicion");
```

Eso es todo, espero que este post te sea de utilidad y lo puedas aplicar a algún proyecto que tengas en mente y que simplemente te haya ayudado a entender la naturaleza del patrón observer. déjame un comentario si lograste implementarlo, si quieres añadir alguna otra funcionalidad o si tienes alguna duda no dudes en dejarme un comentario en la parte de abajo, recuerda que si te gustó también puedes compartir usando los links a las redes sociales en la parte de abajo.