# Chain of Responsability Pattern

---

## **Intención**

**La cadena de responsabilidad** es un patrón de diseño de comportamiento que le permite pasar solicitudes a lo largo de una cadena de controladores. Al recibir una solicitud, cada controlador decide procesarla o pasarla al siguiente controlador de la cadena.

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled.png)

## **Problema**

Imagine que está trabajando en un sistema de pedidos en línea. Desea restringir el acceso al sistema para que solo los usuarios autenticados puedan crear pedidos. Además, los usuarios que tienen permisos administrativos deben tener acceso completo a todos los pedidos.

Después de un poco de planificación, se dio cuenta de que estas comprobaciones deben realizarse secuencialmente. La aplicación puede intentar autenticar a un usuario en el sistema siempre que reciba una solicitud que contenga las credenciales del usuario. Sin embargo, si esas credenciales no son correctas y la autenticación falla, no hay razón para continuar con otras verificaciones.

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%201.png)

*La solicitud debe pasar una serie de controles antes de que el propio sistema de pedidos pueda manejarla.*

Durante los siguientes meses, implementó varias de esas comprobaciones secuenciales.

- Uno de sus colegas sugirió que no es seguro pasar datos sin procesar directamente al sistema de pedidos. Entonces agregó un paso de validación adicional para desinfectar los datos en una solicitud.
- Más tarde, alguien notó que el sistema es vulnerable al descifrado de contraseñas por fuerza bruta. Para negar esto, agregó rápidamente una verificación que filtra las solicitudes fallidas repetidas que provienen de la misma dirección IP.
- Alguien más sugirió que podría acelerar el sistema devolviendo resultados almacenados en caché en solicitudes repetidas que contienen los mismos datos. Por lo tanto, agregó otra verificación que permite que la solicitud pase al sistema solo si no hay una respuesta adecuada en caché.

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%202.png)

*Cuanto más crecía el código, más desordenado se volvía.*

El código de los cheques, que ya parecía un desastre, se inflaba cada vez más a medida que agregaba cada nueva característica. Cambiar un cheque a veces afectaba a los demás. Lo peor de todo es que cuando intentó reutilizar las comprobaciones para proteger otros componentes del sistema, tuvo que duplicar parte del código, ya que esos componentes requerían algunas de las comprobaciones, pero no todas.

El sistema se volvió muy difícil de comprender y costoso de mantener. Luchaste con el código durante un tiempo, hasta que un día decidiste refactorizar todo.

## **Solución**

Como muchos otros patrones de diseño de comportamiento, la **Cadena de responsabilidad** se basa en transformar comportamientos particulares en objetos independientes llamados *controladores* . En nuestro caso, cada cheque debe extraerse a su propia clase con un solo método que realiza el cheque. La solicitud, junto con sus datos, se pasa a este método como argumento.

El patrón sugiere que enlace estos manipuladores en una cadena. Cada controlador vinculado tiene un campo para almacenar una referencia al siguiente controlador de la cadena. Además de procesar una solicitud, los controladores pasan la solicitud más adelante en la cadena. La solicitud viaja a lo largo de la cadena hasta que todos los controladores han tenido la oportunidad de procesarla.

Aquí está la mejor parte: un controlador puede decidir no pasar la solicitud más abajo en la cadena y detener efectivamente cualquier procesamiento posterior.

En nuestro ejemplo con sistemas de pedidos, un controlador realiza el procesamiento y luego decide si pasa la solicitud más abajo en la cadena. Suponiendo que la solicitud contiene los datos correctos, todos los controladores pueden ejecutar su comportamiento principal, ya sea comprobaciones de autenticación o almacenamiento en caché.

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%203.png)

*Los manipuladores se alinean uno por uno, formando una cadena.*

Sin embargo, existe un enfoque ligeramente diferente (y es un poco más canónico) en el que, al recibir una solicitud, un controlador decide si puede procesarla. Si puede, no pasa la solicitud más. Por lo tanto, es solo un controlador el que procesa la solicitud o ninguno en absoluto. Este enfoque es muy común cuando se trata de eventos en pilas de elementos dentro de una interfaz gráfica de usuario.

Por ejemplo, cuando un usuario hace clic en un botón, el evento se propaga a través de la cadena de elementos de la GUI que comienza con el botón, recorre sus contenedores (como formularios o paneles) y termina en la ventana principal de la aplicación. El evento es procesado por el primer elemento de la cadena que es capaz de manejarlo. Este ejemplo también es digno de mención porque muestra que una cadena siempre se puede extraer de un árbol de objetos.

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%204.png)

*Se puede formar una cadena a partir de una rama de un árbol de objetos.*

Es crucial que todas las clases de controlador implementen la misma interfaz. Cada manipulador de concreto solo debe preocuparse por el siguiente que tiene el `execute`método. De esta manera, puede componer cadenas en tiempo de ejecución, utilizando varios controladores sin acoplar su código a sus clases concretas.

## **Analogía del mundo real**

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%205.png)

*Una llamada al soporte técnico puede pasar por múltiples operadores.*

Acaba de comprar e instalar una nueva pieza de hardware en su computadora. Como eres un geek, la computadora tiene varios sistemas operativos instalados. Intenta iniciarlos todos para ver si el hardware es compatible. Windows detecta y habilita el hardware automáticamente. Sin embargo, su amado Linux se niega a trabajar con el nuevo hardware. Con un pequeño destello de esperanza, decide llamar al número de teléfono de soporte técnico escrito en la caja.

Lo primero que escuchas es la voz robótica del autorespondedor. Sugiere nueve soluciones populares para varios problemas, ninguno de los cuales es relevante para su caso. Después de un tiempo, el robot lo conecta con un operador en vivo.

Por desgracia, el operador tampoco puede sugerir nada específico. Sigue citando extensos extractos del manual y se niega a escuchar sus comentarios. Después de escuchar la frase "¿has probado a apagar y encender la computadora?" por décima vez, exige que lo conecten con un ingeniero adecuado.

Eventualmente, el operador pasa su llamada a uno de los ingenieros, quien probablemente había anhelado un chat humano en vivo durante horas mientras estaba sentado en su solitaria sala de servidores en el oscuro sótano de algún edificio de oficinas. El ingeniero le dice dónde descargar los controladores adecuados para su nuevo hardware y cómo instalarlos en Linux. ¡Finalmente, la solución! Terminas la llamada, rebosante de alegría.

## **Estructura**

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%206.png)

1. El **Manejador** declara la interfaz, común para todos los manejadores concretos. Por lo general, contiene un solo método para manejar solicitudes, pero a veces también puede tener otro método para configurar el siguiente controlador en la cadena.
2. El controlador **base** es una clase opcional en la que puede colocar el código repetitivo que es común a todas las clases de controlador.
    
    Por lo general, esta clase define un campo para almacenar una referencia al siguiente controlador. Los clientes pueden construir una cadena pasando un controlador al constructor o configurador del controlador anterior. La clase también puede implementar el comportamiento de manejo predeterminado: puede pasar la ejecución al siguiente controlador después de verificar su existencia.
    
3. **Los controladores de hormigón** contienen el código real para procesar las solicitudes. Al recibir una solicitud, cada manejador debe decidir si procesarla y, adicionalmente, si pasarla a lo largo de la cadena.
    
    Los controladores suelen ser autónomos e inmutables y aceptan todos los datos necesarios solo una vez a través del constructor.
    
4. El **Cliente** puede componer cadenas una sola vez o componerlas dinámicamente, según la lógica de la aplicación. Tenga en cuenta que se puede enviar una solicitud a cualquier controlador de la cadena, no tiene que ser el primero.

## **pseudocódigo**

En este ejemplo, el patrón **Cadena de responsabilidad** es responsable de mostrar información de ayuda contextual para elementos de GUI activos.

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%207.png)

*Las clases de GUI están construidas con el patrón Composite. Cada elemento está vinculado a su elemento contenedor. En cualquier punto, puede crear una cadena de elementos que comience con el propio elemento y atraviese todos sus elementos contenedores.*

La GUI de la aplicación suele estar estructurada como un árbol de objetos. Por ejemplo, la `Dialog`clase que muestra la ventana principal de la aplicación sería la raíz del árbol de objetos. El cuadro de diálogo contiene `Panels`, que puede contener otros paneles o elementos simples de bajo nivel como `Buttons`y `TextFields`.

Un componente simple puede mostrar información contextual breve sobre herramientas, siempre que el componente tenga algún texto de ayuda asignado. Pero los componentes más complejos definen su propia forma de mostrar ayuda contextual, como mostrar un extracto del manual o abrir una página en un navegador.

![Untitled](Chain%20of%20Responsability%20Pattern%20df108c97fcca42ad85a81b6f9f245f31/Untitled%208.png)

*Así es como una solicitud de ayuda atraviesa los objetos GUI.*

Cuando un usuario apunta el cursor del mouse a un elemento y presiona la `F1`tecla, la aplicación detecta el componente debajo del puntero y le envía una solicitud de ayuda. La solicitud asciende a través de todos los contenedores del elemento hasta que llega al elemento que es capaz de mostrar la información de ayuda.

```jsx
// The handler interface declares a method for executing a
// request.
interface ComponentWithContextualHelp is
    method showHelp()

// The base class for simple components.
abstract class Component implements ComponentWithContextualHelp is
    field tooltipText: string

    // The component's container acts as the next link in the
    // chain of handlers.
    protected field container: Container

    // The component shows a tooltip if there's help text
    // assigned to it. Otherwise it forwards the call to the
    // container, if it exists.
    method showHelp() is
        if (tooltipText != null)
            // Show tooltip.
        else
            container.showHelp()

// Containers can contain both simple components and other
// containers as children. The chain relationships are
// established here. The class inherits showHelp behavior from
// its parent.
abstract class Container extends Component is
    protected field children: array of Component

    method add(child) is
        children.add(child)
        child.container = this

// Primitive components may be fine with default help
// implementation...
class Button extends Component is
    // ...

// But complex components may override the default
// implementation. If the help text can't be provided in a new
// way, the component can always call the base implementation
// (see Component class).
class Panel extends Container is
    field modalHelpText: string

    method showHelp() is
        if (modalHelpText != null)
            // Show a modal window with the help text.
        else
            super.showHelp()

// ...same as above...
class Dialog extends Container is
    field wikiPageURL: string

    method showHelp() is
        if (wikiPageURL != null)
            // Open the wiki help page.
        else
            super.showHelp()

// Client code.
class Application is
    // Every application configures the chain differently.
    method createUI() is
        dialog = new Dialog("Budget Reports")
        dialog.wikiPageURL = "http://..."
        panel = new Panel(0, 0, 400, 800)
        panel.modalHelpText = "This panel does..."
        ok = new Button(250, 760, 50, 20, "OK")
        ok.tooltipText = "This is an OK button that..."
        cancel = new Button(320, 760, 50, 20, "Cancel")
        // ...
        panel.add(ok)
        panel.add(cancel)
        dialog.add(panel)

    // Imagine what happens here.
    method onF1KeyPress() is
        component = this.getComponentAtMouseCoords()
        component.showHelp()
```

## **Aplicabilidad**

### **Utilice el patrón Cadena de responsabilidad cuando se espera que su programa procese diferentes tipos de solicitudes de varias maneras, pero los tipos exactos de solicitudes y sus secuencias se desconocen de antemano.**

El patrón le permite vincular varios controladores en una cadena y, al recibir una solicitud, "preguntar" a cada controlador si puede procesarla. De esta manera, todos los controladores tienen la oportunidad de procesar la solicitud.

### **Use el patrón cuando sea esencial ejecutar varios controladores en un orden particular.**

Dado que puede vincular los controladores en la cadena en cualquier orden, todas las solicitudes pasarán por la cadena exactamente como lo planeó.

### **Utilice el patrón CoR cuando se supone que el conjunto de controladores y su orden deben cambiar en tiempo de ejecución.**

Si proporciona configuradores para un campo de referencia dentro de las clases de controlador, podrá insertar, quitar o reordenar controladores dinámicamente.

### ****Cómo implementar****

1. Declare la interfaz del controlador y describa la firma de un método para manejar solicitudes.
Decida cómo el cliente pasará los datos de la solicitud al método. La forma más flexible es convertir la solicitud en un objeto y pasarla al método de manejo como argumento.
2. Para eliminar el código repetitivo duplicado en controladores concretos, podría valer la pena crear una clase de controlador base abstracta, derivada de la interfaz del controlador.
Esta clase debe tener un campo para almacenar una referencia al siguiente controlador de la cadena. Considere hacer que la clase sea inmutable. Sin embargo, si planea modificar cadenas en tiempo de ejecución, debe definir un definidor para modificar el valor del campo de referencia.
También puede implementar el comportamiento predeterminado conveniente para el método de manejo, que es reenviar la solicitud al siguiente objeto a menos que no quede ninguno. Los controladores concretos podrán utilizar este comportamiento llamando al método principal.
3. Uno por uno crea subclases de manejadores concretos e implementa sus métodos de manejo. Cada controlador debe tomar dos decisiones al recibir una solicitud:
    ◦ Si procesará la solicitud.
    ◦ Si pasará la solicitud a lo largo de la cadena.
4. El cliente puede ensamblar cadenas por su cuenta o recibir cadenas preconstruidas de otros objetos. En este último caso, debe implementar algunas clases de fábrica para construir cadenas de acuerdo con la configuración o los ajustes del entorno.
5. El cliente puede activar cualquier controlador de la cadena, no solo el primero. La solicitud se pasará a lo largo de la cadena hasta que algún controlador se niegue a pasarla más o hasta que llegue al final de la cadena.
6. Debido a la naturaleza dinámica de la cadena, el cliente debe estar preparado para manejar los siguientes escenarios:
    ◦ La cadena puede consistir en un solo eslabón.
    ◦ Algunas solicitudes pueden no llegar al final de la cadena.
    ◦ Otros pueden llegar al final de la cadena sin control.

## **Pros y contras**

- Puede controlar el orden de gestión de solicitudes.
- *Principio de responsabilidad única*. Puede desacoplar las clases que invocan operaciones de las clases que realizan operaciones.
- *Principio abierto/cerrado*. Puede introducir nuevos controladores en la aplicación sin romper el código de cliente existente.
- Algunas solicitudes pueden terminar sin ser atendidas.

## Ejemplo Ts:

```jsx
/**
 * The Handler interface declares a method for building the chain of handlers.
 * It also declares a method for executing a request.
 */
interface Handler {
    setNext(handler: Handler): Handler;

    handle(request: string): string;
}

/**
 * The default chaining behavior can be implemented inside a base handler class.
 */
abstract class AbstractHandler implements Handler
{
    private nextHandler: Handler;

    public setNext(handler: Handler): Handler {
        this.nextHandler = handler;
        // Returning a handler from here will let us link handlers in a
        // convenient way like this:
        // monkey.setNext(squirrel).setNext(dog);
        return handler;
    }

    public handle(request: string): string {
        if (this.nextHandler) {
            return this.nextHandler.handle(request);
        }

        return null;
    }
}

/**
 * All Concrete Handlers either handle a request or pass it to the next handler
 * in the chain.
 */
class MonkeyHandler extends AbstractHandler {
    public handle(request: string): string {
        if (request === 'Banana') {
            return `Monkey: I'll eat the ${request}.`;
        }
        return super.handle(request);

    }
}

class SquirrelHandler extends AbstractHandler {
    public handle(request: string): string {
        if (request === 'Nut') {
            return `Squirrel: I'll eat the ${request}.`;
        }
        return super.handle(request);
    }
}

class DogHandler extends AbstractHandler {
    public handle(request: string): string {
        if (request === 'MeatBall') {
            return `Dog: I'll eat the ${request}.`;
        }
        return super.handle(request);
    }
}

/**
 * The client code is usually suited to work with a single handler. In most
 * cases, it is not even aware that the handler is part of a chain.
 */
function clientCode(handler: Handler) {
    const foods = ['Nut', 'Banana', 'Cup of coffee'];

    for (const food of foods) {
        console.log(`Client: Who wants a ${food}?`);

        const result = handler.handle(food);
        if (result) {
            console.log(`  ${result}`);
        } else {
            console.log(`  ${food} was left untouched.`);
        }
    }
}

/**
 * The other part of the client code constructs the actual chain.
 */
const monkey = new MonkeyHandler();
const squirrel = new SquirrelHandler();
const dog = new DogHandler();

monkey.setNext(squirrel).setNext(dog);

/**
 * The client should be able to send a request to any handler, not just the
 * first one in the chain.
 */
console.log('Chain: Monkey > Squirrel > Dog\n');
clientCode(monkey);
console.log('');

console.log('Subchain: Squirrel > Dog\n');
clientCode(squirrel);
```

**Resultado:**

```jsx
Chain: Monkey > Squirrel > Dog

Client: Who wants a Nut?
  Squirrel: I'll eat the Nut.
Client: Who wants a Banana?
  Monkey: I'll eat the Banana.
Client: Who wants a Cup of coffee?
  Cup of coffee was left untouched.

Subchain: Squirrel > Dog

Client: Who wants a Nut?
  Squirrel: I'll eat the Nut.
Client: Who wants a Banana?
  Banana was left untouched.
Client: Who wants a Cup of coffee?
  Cup of coffee was left untouched.
```