# Command Pattern

---

## **Intención**

**El comando** es un patrón de diseño de comportamiento que convierte una solicitud en un objeto independiente que contiene toda la información sobre la solicitud. Esta transformación le permite pasar solicitudes como argumentos de método, retrasar o poner en cola la ejecución de una solicitud y admitir operaciones que se pueden deshacer.

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled.png)

## **Problema**

Imagina que estás trabajando en una nueva aplicación de edición de texto. Su tarea actual es crear una barra de herramientas con un montón de botones para varias operaciones del editor. Ha creado una clase muy ordenada `Button`que se puede usar para botones en la barra de herramientas, así como para botones genéricos en varios cuadros de diálogo.

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%201.png)

*Todos los botones de la aplicación se derivan de la misma clase.*

Si bien todos estos botones se ven similares, se supone que todos deben hacer cosas diferentes. ¿Dónde colocaría el código para los distintos controladores de clic de estos botones? La solución más simple es crear toneladas de subclases para cada lugar donde se usa el botón. Estas subclases contendrían el código que tendría que ejecutarse al hacer clic en un botón.

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%202.png)

*Muchas subclases de botones. ¿Qué puede ir mal?*

En poco tiempo, te das cuenta de que este enfoque es profundamente defectuoso. Primero, tiene una enorme cantidad de subclases, y eso estaría bien si no se arriesgara a romper el código en estas subclases cada vez que modifica la `Button`clase base. En pocas palabras, su código GUI se ha vuelto torpemente dependiente del código volátil de la lógica empresarial.

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%203.png)

*Varias clases implementan la misma funcionalidad.*

Y aquí está la parte más fea. Algunas operaciones, como copiar/pegar texto, deberían invocarse desde varios lugares. Por ejemplo, un usuario podría hacer clic en un pequeño botón "Copiar" en la barra de herramientas, o copiar algo a través del menú contextual, o simplemente presionar `Ctrl+C`el teclado.

Inicialmente, cuando nuestra aplicación solo tenía la barra de herramientas, estaba bien colocar la implementación de varias operaciones en las subclases de botones. En otras palabras, tener el código para copiar texto dentro de la `CopyButton`subclase estaba bien. Pero luego, cuando implementa menús contextuales, accesos directos y otras cosas, debe duplicar el código de la operación en muchas clases o hacer que los menús dependan de los botones, lo cual es una opción aún peor.

## **Solución**

Un buen diseño de software a menudo se basa en el *principio de separación de preocupaciones* , que generalmente resulta en dividir una aplicación en capas. El ejemplo más común: una capa para la interfaz gráfica de usuario y otra capa para la lógica empresarial. La capa GUI es responsable de representar una imagen hermosa en la pantalla, capturar cualquier entrada y mostrar los resultados de lo que están haciendo el usuario y la aplicación. Sin embargo, cuando se trata de hacer algo importante, como calcular la trayectoria de la luna o redactar un informe anual, la capa GUI delega el trabajo a la capa subyacente de lógica empresarial.

En el código podría verse así: un objeto GUI llama a un método de un objeto de lógica empresarial, pasándole algunos argumentos. Este proceso generalmente se describe como un objeto que envía una *solicitud* a otro .

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%204.png)

*Los objetos de la GUI pueden acceder directamente a los objetos de la lógica empresarial.*

El patrón Command sugiere que los objetos GUI no deberían enviar estas solicitudes directamente. En su lugar, debe extraer todos los detalles de la solicitud, como el objeto al que se llama, el nombre del método y la lista de argumentos en una clase de *comando* separada con un solo método que activa esta solicitud.

Los objetos de comando sirven como enlaces entre varias GUI y objetos de lógica empresarial. De ahora en adelante, el objeto GUI no necesita saber qué objeto de lógica empresarial recibirá la solicitud y cómo se procesará. El objeto GUI solo activa el comando, que maneja todos los detalles.

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%205.png)

*Acceder a la capa de lógica empresarial a través de un comando.*

El siguiente paso es hacer que sus comandos implementen la misma interfaz. Por lo general, tiene un solo método de ejecución que no toma parámetros. Esta interfaz le permite usar varios comandos con el mismo remitente de solicitud, sin acoplarlo a clases concretas de comandos. Como beneficio adicional, ahora puede cambiar los objetos de comando vinculados al remitente, cambiando efectivamente el comportamiento del remitente en tiempo de ejecución.

Es posible que haya notado que falta una pieza del rompecabezas, que son los parámetros de solicitud. Un objeto GUI podría haber proporcionado algunos parámetros al objeto de la capa empresarial. Dado que el método de ejecución del comando no tiene ningún parámetro, ¿cómo pasaríamos los detalles de la solicitud al receptor? Resulta que el comando debe estar preconfigurado con estos datos o ser capaz de obtenerlos por sí solo.

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%206.png)

*Los objetos GUI delegan el trabajo a los comandos.*

Volvamos a nuestro editor de texto. Después de aplicar el patrón Comando, ya no necesitamos todas esas subclases de botones para implementar varios comportamientos de clic. Es suficiente poner un solo campo en la `Button`clase base que almacena una referencia a un objeto de comando y hacer que el botón ejecute ese comando con un clic.

Implementará un montón de clases de comando para cada operación posible y las vinculará con botones particulares, según el comportamiento previsto de los botones.

Otros elementos de la GUI, como menús, accesos directos o cuadros de diálogo completos, se pueden implementar de la misma manera. Estarán vinculados a un comando que se ejecuta cuando un usuario interactúa con el elemento GUI. Como ya habrás adivinado, los elementos relacionados con las mismas operaciones estarán vinculados a los mismos comandos, evitando cualquier duplicación de código.

Como resultado, los comandos se convierten en una capa intermedia conveniente que reduce el acoplamiento entre la GUI y las capas de lógica empresarial. ¡Y eso es solo una fracción de los beneficios que puede ofrecer el patrón Command!

## **Analogía del mundo real**

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%207.png)

*Hacer un pedido en un restaurante.*

Después de un largo paseo por la ciudad, llegas a un buen restaurante y te sientas en la mesa junto a la ventana. Un amable mesero se acerca a ti y rápidamente toma tu orden, anotándola en una hoja de papel. El camarero va a la cocina y pega el pedido en la pared. Después de un tiempo, el pedido llega al chef, quien lo lee y cocina la comida en consecuencia. El cocinero coloca la comida en una bandeja junto con el pedido. El mesero descubre la bandeja, revisa el pedido para asegurarse de que todo esté como usted quería y lo lleva todo a su mesa.

El pedido en papel sirve como comando. Permanece en una cola hasta que el chef está listo para servirlo. El pedido contiene toda la información relevante requerida para cocinar la comida. Le permite al chef comenzar a cocinar de inmediato en lugar de correr aclarando los detalles del pedido directamente.

## **Estructura**

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%208.png)

1. La clase **Sender** (también conocida como *invocador* ) es responsable de iniciar las solicitudes. Esta clase debe tener un campo para almacenar una referencia a un objeto de comando. El remitente activa ese comando en lugar de enviar la solicitud directamente al receptor. Tenga en cuenta que el remitente no es responsable de crear el objeto de comando. Por lo general, recibe un comando creado previamente del cliente a través del constructor.
2. La interfaz **de comando** generalmente declara un solo método para ejecutar el comando.
3. **Los comandos concretos** implementan varios tipos de solicitudes. No se supone que un comando concreto realice el trabajo por sí mismo, sino que pase la llamada a uno de los objetos de lógica empresarial. Sin embargo, en aras de simplificar el código, estas clases se pueden fusionar.
    
    Los parámetros necesarios para ejecutar un método en un objeto receptor se pueden declarar como campos en el comando concreto. Puede hacer que los objetos de comando sean inmutables al permitir solo la inicialización de estos campos a través del constructor.
    
4. La clase **Receiver** contiene algo de lógica empresarial. Casi cualquier objeto puede actuar como receptor. La mayoría de los comandos solo manejan los detalles de cómo se pasa una solicitud al receptor, mientras que el propio receptor hace el trabajo real.
5. El **Cliente** crea y configura objetos de comando concretos. El cliente debe pasar todos los parámetros de solicitud, incluida una instancia de receptor, al constructor del comando. Después de eso, el comando resultante puede asociarse con uno o varios remitentes.

## **pseudocódigo**

En este ejemplo, el patrón **Command** ayuda a rastrear el historial de operaciones ejecutadas y permite revertir una operación si es necesario.

![Untitled](Command%20Pattern%20195b1c8ce4fb4187b1e80246d00773dc/Untitled%209.png)

*Operaciones que se pueden deshacer en un editor de texto.*

Los comandos que resultan en cambiar el estado del editor (por ejemplo, cortar y pegar) hacen una copia de respaldo del estado del editor antes de ejecutar una operación asociada con el comando. Después de ejecutar un comando, se coloca en el historial de comandos (una pila de objetos de comando) junto con la copia de seguridad del estado del editor en ese momento. Más tarde, si el usuario necesita revertir una operación, la aplicación puede tomar el comando más reciente del historial, leer la copia de seguridad asociada del estado del editor y restaurarla.

El código del cliente (elementos GUI, historial de comandos, etc.) no está acoplado a clases de comandos concretas porque funciona con comandos a través de la interfaz de comandos. Este enfoque le permite introducir nuevos comandos en la aplicación sin romper ningún código existente.

```jsx
// The base command class defines the common interface for all
// concrete commands.
abstract class Command is
    protected field app: Application
    protected field editor: Editor
    protected field backup: text

    constructor Command(app: Application, editor: Editor) is
        this.app = app
        this.editor = editor

    // Make a backup of the editor's state.
    method saveBackup() is
        backup = editor.text

    // Restore the editor's state.
    method undo() is
        editor.text = backup

    // The execution method is declared abstract to force all
    // concrete commands to provide their own implementations.
    // The method must return true or false depending on whether
    // the command changes the editor's state.
    abstract method execute()

// The concrete commands go here.
class CopyCommand extends Command is
    // The copy command isn't saved to the history since it
    // doesn't change the editor's state.
    method execute() is
        app.clipboard = editor.getSelection()
        return false

class CutCommand extends Command is
    // The cut command does change the editor's state, therefore
    // it must be saved to the history. And it'll be saved as
    // long as the method returns true.
    method execute() is
        saveBackup()
        app.clipboard = editor.getSelection()
        editor.deleteSelection()
        return true

class PasteCommand extends Command is
    method execute() is
        saveBackup()
        editor.replaceSelection(app.clipboard)
        return true

// The undo operation is also a command.
class UndoCommand extends Command is
    method execute() is
        app.undo()
        return false

// The global command history is just a stack.
class CommandHistory is
    private field history: array of Command

    // Last in...
    method push(c: Command) is
        // Push the command to the end of the history array.

    // ...first out
    method pop():Command is
        // Get the most recent command from the history.

// The editor class has actual text editing operations. It plays
// the role of a receiver: all commands end up delegating
// execution to the editor's methods.
class Editor is
    field text: string

    method getSelection() is
        // Return selected text.

    method deleteSelection() is
        // Delete selected text.

    method replaceSelection(text) is
        // Insert the clipboard's contents at the current
        // position.

// The application class sets up object relations. It acts as a
// sender: when something needs to be done, it creates a command
// object and executes it.
class Application is
    field clipboard: string
    field editors: array of Editors
    field activeEditor: Editor
    field history: CommandHistory

    // The code which assigns commands to UI objects may look
    // like this.
    method createUI() is
        // ...
        copy = function() { executeCommand(
            new CopyCommand(this, activeEditor)) }
        copyButton.setCommand(copy)
        shortcuts.onKeyPress("Ctrl+C", copy)

        cut = function() { executeCommand(
            new CutCommand(this, activeEditor)) }
        cutButton.setCommand(cut)
        shortcuts.onKeyPress("Ctrl+X", cut)

        paste = function() { executeCommand(
            new PasteCommand(this, activeEditor)) }
        pasteButton.setCommand(paste)
        shortcuts.onKeyPress("Ctrl+V", paste)

        undo = function() { executeCommand(
            new UndoCommand(this, activeEditor)) }
        undoButton.setCommand(undo)
        shortcuts.onKeyPress("Ctrl+Z", undo)

    // Execute a command and check whether it has to be added to
    // the history.
    method executeCommand(command) is
        if (command.execute)
            history.push(command)

    // Take the most recent command from the history and run its
    // undo method. Note that we don't know the class of that
    // command. But we don't have to, since the command knows
    // how to undo its own action.
    method undo() is
        command = history.pop()
        if (command != null)
            command.undo()
```

## **Aplicabilidad**

### **Utilice el patrón Comando cuando desee parametrizar objetos con operaciones.**

El patrón Command puede convertir una llamada de método específico en un objeto independiente. Este cambio abre muchos usos interesantes: puede pasar comandos como argumentos de método, almacenarlos dentro de otros objetos, cambiar comandos vinculados en tiempo de ejecución, etc.

He aquí un ejemplo: está desarrollando un componente de GUI, como un menú contextual, y desea que sus usuarios puedan configurar elementos de menú que activen operaciones cuando un usuario final haga clic en un elemento.

### **Utilice el patrón Command cuando desee poner en cola operaciones, programar su ejecución o ejecutarlas de forma remota.**

Al igual que con cualquier otro objeto, un comando se puede serializar, lo que significa convertirlo en una cadena que se puede escribir fácilmente en un archivo o una base de datos. Posteriormente, la cadena se puede restaurar como el objeto de comando inicial. Por lo tanto, puede retrasar y programar la ejecución de comandos. ¡Pero aún hay más! De la misma manera, puede poner en cola, registrar o enviar comandos a través de la red.

### **Utilice el patrón Command cuando desee implementar operaciones reversibles.**

Aunque hay muchas formas de implementar deshacer/rehacer, el patrón Comando es quizás el más popular de todos.

Para poder revertir operaciones, debe implementar el historial de operaciones realizadas. El historial de comandos es una pila que contiene todos los objetos de comandos ejecutados junto con las copias de seguridad relacionadas del estado de la aplicación.

Este método tiene dos inconvenientes. En primer lugar, no es tan fácil guardar el estado de una aplicación porque parte de ella puede ser privada. Este problema se puede mitigar con el patrón **[Memento .](https://refactoring.guru/pattern/memento)**

En segundo lugar, las copias de seguridad de estado pueden consumir bastante RAM. Por lo tanto, en ocasiones se puede recurrir a una implementación alternativa: en lugar de restaurar el estado anterior, el comando realiza la operación inversa. La operación inversa también tiene un precio: puede resultar difícil o incluso imposible de implementar.

### ****Cómo implementar****

1. Declare la interfaz de comandos con un único método de ejecución.
2. Comience a extraer solicitudes en clases de comando concretas que implementan la interfaz de comando. Cada clase debe tener un conjunto de campos para almacenar los argumentos de la solicitud junto con una referencia al objeto receptor real. Todos estos valores deben inicializarse a través del constructor del comando.
3. Identifique las clases que actuarán como *remitentes* . Agregue los campos para almacenar comandos en estas clases. Los remitentes deben comunicarse con sus comandos solo a través de la interfaz de comandos. Los remitentes generalmente no crean objetos de comando por su cuenta, sino que los obtienen del código del cliente.
4. Cambie los remitentes para que ejecuten el comando en lugar de enviar una solicitud directamente al receptor.
5. El cliente debe inicializar los objetos en el siguiente orden:
    ◦ Crear receptores.
    ◦ Cree comandos y asócielos con receptores si es necesario.
    ◦ Cree remitentes y asócielos con comandos específicos.

## **Pros**

- *Principio de responsabilidad única*. Puede desacoplar las clases que invocan operaciones de las clases que realizan estas operaciones.
- *Principio abierto/cerrado*. Puede introducir nuevos comandos en la aplicación sin romper el código de cliente existente.
- Puede implementar deshacer/rehacer.
- Puede implementar la ejecución diferida de operaciones.
- Puede ensamblar un conjunto de comandos simples en uno complejo.

## C**ontras**

- El código puede volverse más complicado ya que está introduciendo una capa completamente nueva entre remitentes y receptores.

## Ejemplo Ts:

```jsx
/**
 * The Command interface declares a method for executing a command.
 */
interface Command {
    execute(): void;
}

/**
 * Some commands can implement simple operations on their own.
 */
class SimpleCommand implements Command {
    private payload: string;

    constructor(payload: string) {
        this.payload = payload;
    }

    public execute(): void {
        console.log(`SimpleCommand: See, I can do simple things like printing (${this.payload})`);
    }
}

/**
 * However, some commands can delegate more complex operations to other objects,
 * called "receivers."
 */
class ComplexCommand implements Command {
    private receiver: Receiver;

    /**
     * Context data, required for launching the receiver's methods.
     */
    private a: string;

    private b: string;

    /**
     * Complex commands can accept one or several receiver objects along with
     * any context data via the constructor.
     */
    constructor(receiver: Receiver, a: string, b: string) {
        this.receiver = receiver;
        this.a = a;
        this.b = b;
    }

    /**
     * Commands can delegate to any methods of a receiver.
     */
    public execute(): void {
        console.log('ComplexCommand: Complex stuff should be done by a receiver object.');
        this.receiver.doSomething(this.a);
        this.receiver.doSomethingElse(this.b);
    }
}

/**
 * The Receiver classes contain some important business logic. They know how to
 * perform all kinds of operations, associated with carrying out a request. In
 * fact, any class may serve as a Receiver.
 */
class Receiver {
    public doSomething(a: string): void {
        console.log(`Receiver: Working on (${a}.)`);
    }

    public doSomethingElse(b: string): void {
        console.log(`Receiver: Also working on (${b}.)`);
    }
}

/**
 * The Invoker is associated with one or several commands. It sends a request to
 * the command.
 */
class Invoker {
    private onStart: Command;

    private onFinish: Command;

    /**
     * Initialize commands.
     */
    public setOnStart(command: Command): void {
        this.onStart = command;
    }

    public setOnFinish(command: Command): void {
        this.onFinish = command;
    }

    /**
     * The Invoker does not depend on concrete command or receiver classes. The
     * Invoker passes a request to a receiver indirectly, by executing a
     * command.
     */
    public doSomethingImportant(): void {
        console.log('Invoker: Does anybody want something done before I begin?');
        if (this.isCommand(this.onStart)) {
            this.onStart.execute();
        }

        console.log('Invoker: ...doing something really important...');

        console.log('Invoker: Does anybody want something done after I finish?');
        if (this.isCommand(this.onFinish)) {
            this.onFinish.execute();
        }
    }

    private isCommand(object): object is Command {
        return object.execute !== undefined;
    }
}

/**
 * The client code can parameterize an invoker with any commands.
 */
const invoker = new Invoker();
invoker.setOnStart(new SimpleCommand('Say Hi!'));
const receiver = new Receiver();
invoker.setOnFinish(new ComplexCommand(receiver, 'Send email', 'Save report'));

invoker.doSomethingImportant();
```

**Resultado**:

```jsx
Invoker: Does anybody want something done before I begin?
SimpleCommand: See, I can do simple things like printing (Say Hi!)
Invoker: ...doing something really important...
Invoker: Does anybody want something done after I finish?
ComplexCommand: Complex stuff should be done by a receiver object.
Receiver: Working on (Send email.)
Receiver: Also working on (Save report.)
```