# Mediator Pattern

---

## **Intención**

**Mediator** es un patrón de diseño de comportamiento que le permite reducir las dependencias caóticas entre objetos. El patrón restringe las comunicaciones directas entre los objetos y los obliga a colaborar solo a través de un objeto mediador.

![Untitled](Mediator%20Pattern%204eadea2ded3a48fa89d3bccab4040b09/Untitled.png)

## **Problema**

Digamos que tiene un cuadro de diálogo para crear y editar perfiles de clientes. Consiste en varios controles de formulario, como campos de texto, casillas de verificación, botones, etc.

![Untitled](Mediator%20Pattern%204eadea2ded3a48fa89d3bccab4040b09/Untitled%201.png)

*Las relaciones entre los elementos de la interfaz de usuario pueden volverse caóticas a medida que evoluciona la aplicación.*

Algunos de los elementos del formulario pueden interactuar con otros. Por ejemplo, seleccionar la casilla de verificación "Tengo un perro" puede revelar un campo de texto oculto para ingresar el nombre del perro. Otro ejemplo es el botón de enviar que tiene que validar los valores de todos los campos antes de guardar los datos.

![Untitled](Mediator%20Pattern%204eadea2ded3a48fa89d3bccab4040b09/Untitled%202.png)

*Los elementos pueden tener muchas relaciones con otros elementos. Por lo tanto, los cambios en algunos elementos pueden afectar a los demás.*

Al implementar esta lógica directamente dentro del código de los elementos del formulario, hace que las clases de estos elementos sean mucho más difíciles de reutilizar en otras formas de la aplicación. Por ejemplo, no podrá usar esa clase de casilla de verificación dentro de otro formulario, porque está acoplada al campo de texto del perro. Puede usar todas las clases involucradas en la representación del formulario de perfil o ninguna.

## **Solución**

El patrón Mediador sugiere que debe cesar toda comunicación directa entre los componentes que desea independizar entre sí. En su lugar, estos componentes deben colaborar indirectamente llamando a un objeto mediador especial que redirige las llamadas a los componentes apropiados. Como resultado, los componentes dependen solo de una sola clase de mediadores en lugar de estar acoplados a docenas de sus colegas.

En nuestro ejemplo con el formulario de edición de perfil, la propia clase de diálogo puede actuar como mediador. Lo más probable es que la clase de diálogo ya conozca todos sus subelementos, por lo que ni siquiera necesitará introducir nuevas dependencias en esta clase.

![Untitled](Mediator%20Pattern%204eadea2ded3a48fa89d3bccab4040b09/Untitled%203.png)

*Los elementos de la interfaz de usuario deben comunicarse indirectamente, a través del objeto mediador.*

El cambio más significativo se produce en los elementos de formulario reales. Consideremos el botón de enviar. Anteriormente, cada vez que un usuario hacía clic en el botón, tenía que validar los valores de todos los elementos de formulario individuales. Ahora su único trabajo es notificar al cuadro de diálogo sobre el clic. Al recibir esta notificación, el propio diálogo realiza las validaciones o pasa la tarea a los elementos individuales. Por lo tanto, en lugar de estar vinculado a una docena de elementos de formulario, el botón solo depende de la clase de diálogo.

Puede ir más allá y hacer que la dependencia sea aún más flexible extrayendo la interfaz común para todos los tipos de diálogos. La interfaz declararía el método de notificación que todos los elementos del formulario pueden usar para notificar al cuadro de diálogo sobre los eventos que suceden en esos elementos. Por lo tanto, nuestro botón de envío ahora debería poder funcionar con cualquier cuadro de diálogo que implemente esa interfaz.

De esta forma, el patrón Mediator le permite encapsular una red compleja de relaciones entre varios objetos dentro de un único objeto mediador. Cuantas menos dependencias tenga una clase, más fácil será modificar, ampliar o reutilizar esa clase.

## **Analogía del mundo real**

![Untitled](Mediator%20Pattern%204eadea2ded3a48fa89d3bccab4040b09/Untitled%204.png)

*Los pilotos de aviones no se hablan directamente cuando deciden quién será el siguiente en aterrizar su avión. Toda la comunicación pasa por la torre de control.*

Los pilotos de aeronaves que se acercan o salen del área de control del aeropuerto no se comunican directamente entre sí. En cambio, hablan con un controlador de tráfico aéreo, que se sienta en una torre alta en algún lugar cerca de la pista de aterrizaje. Sin el controlador de tráfico aéreo, los pilotos tendrían que estar al tanto de todos los aviones en las cercanías del aeropuerto, discutiendo las prioridades de aterrizaje con un comité de docenas de otros pilotos. Eso probablemente dispararía las estadísticas de accidentes aéreos.

La torre no necesita controlar todo el vuelo. Solo existe para hacer cumplir las restricciones en el área de la terminal porque la cantidad de actores involucrados allí puede ser abrumadora para un piloto.

## **Estructura**

![Untitled](Mediator%20Pattern%204eadea2ded3a48fa89d3bccab4040b09/Untitled%205.png)

1. **Los componentes** son varias clases que contienen alguna lógica empresarial. Cada componente tiene una referencia a un mediador, declarado con el tipo de la interfaz del mediador. El componente no reconoce la clase real del mediador, por lo que puede reutilizar el componente en otros programas vinculándolo a un mediador diferente.
2. La interfaz **Mediator** declara métodos de comunicación con los componentes, que normalmente incluyen un único método de notificación. Los componentes pueden pasar cualquier contexto como argumentos de este método, incluidos sus propios objetos, pero solo de tal manera que no se produzca ningún acoplamiento entre un componente receptor y la clase del remitente.
3. **Los mediadores concretos** encapsulan las relaciones entre varios componentes. Los mediadores concretos a menudo mantienen referencias a todos los componentes que administran y, a veces, incluso administran su ciclo de vida.
4. Los componentes no deben ser conscientes de otros componentes. Si algo importante sucede dentro oa un componente, sólo debe notificar al mediador. Cuando el mediador recibe la notificación, puede identificar fácilmente al remitente, lo que podría ser suficiente para decidir qué componente debe activarse a cambio.
    
    Desde la perspectiva de un componente, todo parece una caja negra total. El remitente no sabe quién terminará manejando su solicitud y el receptor no sabe quién envió la solicitud en primer lugar.
    

## **pseudocódigo**

En este ejemplo, el patrón **Mediator** lo ayuda a eliminar las dependencias mutuas entre varias clases de IU: botones, casillas de verificación y etiquetas de texto.

![Untitled](Mediator%20Pattern%204eadea2ded3a48fa89d3bccab4040b09/Untitled%206.png)

*Estructura de las clases de diálogo de la interfaz de usuario.*

Un elemento, activado por un usuario, no se comunica directamente con otros elementos, incluso si parece que debería hacerlo. En cambio, el elemento solo necesita informar a su mediador sobre el evento, pasando cualquier información contextual junto con esa notificación.

En este ejemplo, todo el cuadro de diálogo de autenticación actúa como mediador. Sabe cómo se supone que deben colaborar los elementos concretos y facilita su comunicación indirecta. Al recibir una notificación sobre un evento, el cuadro de diálogo decide qué elemento debe abordar el evento y redirige la llamada en consecuencia.

```jsx
// The mediator interface declares a method used by components
// to notify the mediator about various events. The mediator may
// react to these events and pass the execution to other
// components.
interface Mediator is
    method notify(sender: Component, event: string)

// The concrete mediator class. The intertwined web of
// connections between individual components has been untangled
// and moved into the mediator.
class AuthenticationDialog implements Mediator is
    private field title: string
    private field loginOrRegisterChkBx: Checkbox
    private field loginUsername, loginPassword: Textbox
    private field registrationUsername, registrationPassword,
                  registrationEmail: Textbox
    private field okBtn, cancelBtn: Button

    constructor AuthenticationDialog() is
        // Create all component objects by passing the current
        // mediator into their constructors to establish links.

    // When something happens with a component, it notifies the
    // mediator. Upon receiving a notification, the mediator may
    // do something on its own or pass the request to another
    // component.
    method notify(sender, event) is
        if (sender == loginOrRegisterChkBx and event == "check")
            if (loginOrRegisterChkBx.checked)
                title = "Log in"
                // 1. Show login form components.
                // 2. Hide registration form components.
            else
                title = "Register"
                // 1. Show registration form components.
                // 2. Hide login form components

        if (sender == okBtn && event == "click")
            if (loginOrRegister.checked)
                // Try to find a user using login credentials.
                if (!found)
                    // Show an error message above the login
                    // field.
            else
                // 1. Create a user account using data from the
                // registration fields.
                // 2. Log that user in.
                // ...

// Components communicate with a mediator using the mediator
// interface. Thanks to that, you can use the same components in
// other contexts by linking them with different mediator
// objects.
class Component is
    field dialog: Mediator

    constructor Component(dialog) is
        this.dialog = dialog

    method click() is
        dialog.notify(this, "click")

    method keypress() is
        dialog.notify(this, "keypress")

// Concrete components don't talk to each other. They have only
// one communication channel, which is sending notifications to
// the mediator.
class Button extends Component is
    // ...

class Textbox extends Component is
    // ...

class Checkbox extends Component is
    method check() is
        dialog.notify(this, "check")
    // ...
```

## **Aplicabilidad**

**Utilice el patrón Mediator cuando sea difícil cambiar algunas de las clases porque están estrechamente acopladas a un montón de otras clases.**

El patrón le permite extraer todas las relaciones entre clases en una clase separada, aislando cualquier cambio en un componente específico del resto de los componentes.

**Utilice el patrón cuando no pueda reutilizar un componente en un programa diferente porque depende demasiado de otros componentes.**

Después de aplicar el Mediador, los componentes individuales no se dan cuenta de los otros componentes. Todavía podían comunicarse entre sí, aunque indirectamente, a través de un objeto mediador. Para reutilizar un componente en una aplicación diferente, debe proporcionarle una nueva clase de mediador.

**Use el Mediador cuando se encuentre creando toneladas de subclases de componentes solo para reutilizar algunos comportamientos básicos en varios contextos.**

Dado que todas las relaciones entre los componentes están contenidas dentro del mediador, es fácil definir formas completamente nuevas para que estos componentes colaboren mediante la introducción de nuevas clases de mediadores, sin tener que cambiar los componentes mismos.

****Cómo implementar****
1. Identifique un grupo de clases estrechamente acopladas que se beneficiarían de ser más independientes (p. ej., para un mantenimiento más sencillo o una reutilización más sencilla de estas clases).
2. Declare la interfaz del mediador y describa el protocolo de comunicación deseado entre los mediadores y varios componentes. En la mayoría de los casos, basta con un único método para recibir notificaciones de los componentes.
Esta interfaz es crucial cuando desea reutilizar clases de componentes en diferentes contextos. Siempre que el componente funcione con su mediador a través de la interfaz genérica, puede vincular el componente con una implementación diferente del mediador.
3. Implemente la clase de mediador concreto. Considere almacenar referencias a todos los componentes dentro del mediador. De esta manera, podría llamar a cualquier componente desde los métodos del mediador.
4. Puede ir aún más lejos y hacer que el mediador sea responsable de la creación y destrucción de los objetos componentes. Después de esto, el mediador puede parecer una **[fábrica](https://refactoring.guru/pattern/abstract-factory)** o una **[fachada](https://refactoring.guru/pattern/facade)** .
5. Los componentes deben almacenar una referencia al objeto mediador. La conexión generalmente se establece en el constructor del componente, donde se pasa un objeto mediador como argumento.
6. Cambie el código de los componentes para que llamen al método de notificación del mediador en lugar de a los métodos de otros componentes. Extraiga el código que implica llamar a otros componentes a la clase mediadora. Ejecute este código cada vez que el mediador reciba notificaciones de ese componente.

## **Pros**

- *Principio de responsabilidad única*. Puede extraer las comunicaciones entre varios componentes en un solo lugar, lo que facilita su comprensión y mantenimiento.
- *Principio abierto/cerrado*. Puede introducir nuevos mediadores sin tener que cambiar los componentes reales.
- Puede reducir el acoplamiento entre varios componentes de un programa.
- Puede reutilizar componentes individuales más fácilmente.

## C**ontras**

- Con el tiempo, un mediador puede evolucionar hasta convertirse en un **[Objeto de Dios](https://refactoring.guru/antipatterns/god-object)**.

## Ejemplo Ts:

 

```jsx
/**
 * The Mediator interface declares a method used by components to notify the
 * mediator about various events. The Mediator may react to these events and
 * pass the execution to other components.
 */
interface Mediator {
    notify(sender: object, event: string): void;
}

/**
 * Concrete Mediators implement cooperative behavior by coordinating several
 * components.
 */
class ConcreteMediator implements Mediator {
    private component1: Component1;

    private component2: Component2;

    constructor(c1: Component1, c2: Component2) {
        this.component1 = c1;
        this.component1.setMediator(this);
        this.component2 = c2;
        this.component2.setMediator(this);
    }

    public notify(sender: object, event: string): void {
        if (event === 'A') {
            console.log('Mediator reacts on A and triggers following operations:');
            this.component2.doC();
        }

        if (event === 'D') {
            console.log('Mediator reacts on D and triggers following operations:');
            this.component1.doB();
            this.component2.doC();
        }
    }
}

/**
 * The Base Component provides the basic functionality of storing a mediator's
 * instance inside component objects.
 */
class BaseComponent {
    protected mediator: Mediator;

    constructor(mediator?: Mediator) {
        this.mediator = mediator!;
    }

    public setMediator(mediator: Mediator): void {
        this.mediator = mediator;
    }
}

/**
 * Concrete Components implement various functionality. They don't depend on
 * other components. They also don't depend on any concrete mediator classes.
 */
class Component1 extends BaseComponent {
    public doA(): void {
        console.log('Component 1 does A.');
        this.mediator.notify(this, 'A');
    }

    public doB(): void {
        console.log('Component 1 does B.');
        this.mediator.notify(this, 'B');
    }
}

class Component2 extends BaseComponent {
    public doC(): void {
        console.log('Component 2 does C.');
        this.mediator.notify(this, 'C');
    }

    public doD(): void {
        console.log('Component 2 does D.');
        this.mediator.notify(this, 'D');
    }
}

/**
 * The client code.
 */
const c1 = new Component1();
const c2 = new Component2();
const mediator = new ConcreteMediator(c1, c2);

console.log('Client triggers operation A.');
c1.doA();

console.log('');
console.log('Client triggers operation D.');
c2.doD();
```

**Resultado**:

```jsx
Client triggers operation A.
Component 1 does A.
Mediator reacts on A and triggers following operations:
Component 2 does C.

Client triggers operation D.
Component 2 does D.
Mediator reacts on D and triggers following operations:
Component 1 does B.
Component 2 does C.
```