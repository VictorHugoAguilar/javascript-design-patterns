# Memento Pattern

---

## **Intención**

**Memento** es un patrón de diseño de comportamiento que le permite guardar y restaurar el estado anterior de un objeto sin revelar los detalles de su implementación.

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled.png)

## **Problema**

Imagina que estás creando una aplicación de edición de texto. Además de la edición de texto simple, su editor puede formatear texto, insertar imágenes en línea, etc.

En algún momento, decidió permitir a los usuarios deshacer cualquier operación realizada en el texto. Esta característica se ha vuelto tan común a lo largo de los años que hoy en día la gente espera que todas las aplicaciones la tengan. Para la implementación, optó por adoptar el enfoque directo. Antes de realizar cualquier operación, la aplicación registra el estado de todos los objetos y lo guarda en algún almacenamiento. Más tarde, cuando un usuario decide revertir una acción, la aplicación obtiene la última instantánea del historial y la usa para restaurar el estado de todos los objetos.

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled%201.png)

*Antes de ejecutar una operación, la aplicación guarda una instantánea del estado de los objetos, que luego se puede usar para restaurar los objetos a su estado anterior.*

Pensemos en esas instantáneas estatales. ¿Cómo exactamente producirías uno? Probablemente necesite revisar todos los campos en un objeto y copiar sus valores en el almacenamiento. Sin embargo, esto solo funcionaría si el objeto tuviera restricciones de acceso bastante relajadas a su contenido. Desafortunadamente, la mayoría de los objetos reales no permitirán que otros miren dentro de ellos tan fácilmente, ocultando todos los datos importantes en campos privados.

Ignore ese problema por ahora y supongamos que nuestros objetos se comportan como hippies: prefieren relaciones abiertas y mantienen su estado público. Si bien este enfoque resolvería el problema inmediato y le permitiría producir instantáneas de los estados de los objetos a voluntad, todavía tiene algunos problemas serios. En el futuro, puede decidir refactorizar algunas de las clases del editor, o agregar o eliminar algunos de los campos. Suena fácil, pero esto también requeriría cambiar las clases responsables de copiar el estado de los objetos afectados.

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled%202.png)

*¿Cómo hacer una copia del estado privado del objeto?*

Pero hay más Consideremos las "instantáneas" reales del estado del editor. ¿Qué datos contiene? Como mínimo, debe contener el texto real, las coordenadas del cursor, la posición de desplazamiento actual, etc. Para hacer una instantánea, debe recopilar estos valores y colocarlos en algún tipo de contenedor.

Lo más probable es que almacene muchos de estos objetos contenedores dentro de alguna lista que represente el historial. Por lo tanto, los contenedores probablemente terminen siendo objetos de una clase. La clase casi no tendría métodos, pero sí muchos campos que reflejan el estado del editor. Para permitir que otros objetos escriban y lean datos desde y hacia una instantánea, probablemente necesite hacer públicos sus campos. Eso expondría todos los estados del editor, privados o no. Otras clases se volverían dependientes de cada pequeño cambio en la clase instantánea, lo que de otro modo sucedería dentro de los campos y métodos privados sin afectar las clases externas.

Parece que hemos llegado a un callejón sin salida: expone todos los detalles internos de las clases, haciéndolas demasiado frágiles, o restringe el acceso a su estado, haciendo imposible producir instantáneas. ¿Hay alguna otra forma de implementar el "deshacer"?

## **Solución**

Todos los problemas que acabamos de experimentar son causados por una encapsulación rota. Algunos objetos intentan hacer más de lo que se supone que deben hacer. Para recopilar los datos necesarios para realizar alguna acción, invaden el espacio privado de otros objetos en lugar de permitir que estos objetos realicen la acción real.

El patrón Memento delega la creación de instantáneas de estado al propietario real de ese estado, el objeto *originador* . Por lo tanto, en lugar de que otros objetos intenten copiar el estado del editor desde "afuera", la clase del editor en sí puede hacer la instantánea ya que tiene acceso total a su propio estado.

El patrón sugiere almacenar la copia del estado del objeto en un objeto especial llamado *recuerdo* . El contenido del memento no es accesible a ningún otro objeto excepto al que lo produjo. Otros objetos deben comunicarse con los recuerdos mediante una interfaz limitada que puede permitir obtener los metadatos de la instantánea (hora de creación, el nombre de la operación realizada, etc.), pero no el estado del objeto original contenido en la instantánea.

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled%203.png)

*El autor tiene acceso completo al recuerdo, mientras que el cuidador solo puede acceder a los metadatos.*

Una política tan restrictiva le permite almacenar recuerdos dentro de otros objetos, generalmente llamados *cuidadores* . Dado que el cuidador trabaja con el recuerdo solo a través de la interfaz limitada, no puede manipular el estado almacenado dentro del recuerdo. Al mismo tiempo, el creador tiene acceso a todos los campos dentro del memento, lo que le permite restaurar su estado anterior a voluntad.

En nuestro ejemplo de editor de texto, podemos crear una clase de historial separada para que actúe como cuidador. Una pila de recuerdos almacenados dentro del cuidador crecerá cada vez que el editor esté a punto de ejecutar una operación. Incluso podría representar esta pila dentro de la interfaz de usuario de la aplicación, mostrando el historial de operaciones realizadas anteriormente a un usuario.

Cuando un usuario activa el deshacer, el historial toma el recuerdo más reciente de la pila y lo devuelve al editor, solicitando una reversión. Dado que el editor tiene acceso total al memento, cambia su propio estado con los valores tomados del memento.

## **Estructura**

### **Implementación basada en clases anidadas**

La implementación clásica del patrón se basa en la compatibilidad con clases anidadas, disponibles en muchos lenguajes de programación populares (como C++, C# y Java).

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled%204.png)

1. La clase **Originator** puede producir instantáneas de su propio estado, así como restaurar su estado a partir de instantáneas cuando sea necesario.
2. El **Memento** es un objeto de valor que actúa como una instantánea del estado del creador. Es una práctica común hacer que el recuerdo sea inmutable y pasarle los datos solo una vez, a través del constructor.
3. El **Guardián** no solo sabe "cuándo" y "por qué" capturar el estado del originador, sino también cuándo se debe restaurar el estado.
    
    Un cuidador puede realizar un seguimiento del historial del originador almacenando una pila de recuerdos. Cuando el creador tiene que viajar atrás en la historia, el cuidador busca el recuerdo más alto de la pila y lo pasa al método de restauración del creador.
    
4. En esta implementación, la clase memento está anidada dentro del originador. Esto le permite al creador acceder a los campos y métodos del recuerdo, aunque se declaren privados. Por otro lado, el cuidador tiene un acceso muy limitado a los campos y métodos del recuerdo, lo que le permite almacenar recuerdos en una pila pero no alterar su estado.

### **Implementación basada en una interfaz intermedia**

Hay una implementación alternativa, adecuada para lenguajes de programación que no admiten clases anidadas (sí, PHP, estoy hablando de ti).

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled%205.png)

1. En ausencia de clases anidadas, puede restringir el acceso a los campos del recuerdo estableciendo una convención de que los cuidadores pueden trabajar con un recuerdo solo a través de una interfaz intermedia declarada explícitamente, que solo declararía métodos relacionados con los metadatos del recuerdo.
2. Por otro lado, los creadores pueden trabajar directamente con un objeto memento, accediendo a campos y métodos declarados en la clase memento. La desventaja de este enfoque es que necesita declarar públicos a todos los miembros del memento.

### **Implementación con una encapsulación aún más estricta**

Hay otra implementación que es útil cuando no desea dejar ni la más mínima posibilidad de que otras clases accedan al estado del originador a través del memento.

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled%206.png)

1. Esta implementación permite tener múltiples tipos de originadores y mementos. Cada creador trabaja con una clase de recuerdo correspondiente. Ni los creadores ni los recuerdos exponen su estado a nadie.
2. Los cuidadores ahora tienen restricciones explícitas para cambiar el estado almacenado en los recuerdos. Además, la clase cuidadora se vuelve independiente del originador porque el método de restauración ahora está definido en la clase memento.
3. Cada recuerdo se vincula con el creador que lo produjo. El originador se pasa al constructor del memento, junto con los valores de su estado. Gracias a la estrecha relación entre estas clases, un memento puede restaurar el estado de su originador, dado que este último ha definido los setters apropiados.

## **pseudocódigo**

Este ejemplo usa el patrón Memento junto con el patrón **[Command](https://refactoring.guru/pattern/command)** para almacenar instantáneas del estado del editor de texto complejo y restaurar un estado anterior a partir de estas instantáneas cuando sea necesario.

![Untitled](Memento%20Pattern%203a26b40ca3fe4c1ea17ddfc1f7301cba/Untitled%207.png)

*Guardar instantáneas del estado del editor de texto.*

La clase memento no declara campos públicos, getters o setters. Por lo tanto ningún objeto puede alterar su contenido. Los recuerdos están vinculados al objeto editor que los creó. Esto permite que un recuerdo restaure el estado del editor vinculado al pasar los datos a través de setters en el objeto del editor. Dado que los recuerdos están vinculados a objetos de editor específicos, puede hacer que su aplicación admita varias ventanas de editor independientes con una pila de deshacer centralizada.

```jsx
// The originator holds some important data that may change over
// time. It also defines a method for saving its state inside a
// memento and another method for restoring the state from it.
class Editor is
    private field text, curX, curY, selectionWidth

    method setText(text) is
        this.text = text

    method setCursor(x, y) is
        this.curX = x
        this.curY = y

    method setSelectionWidth(width) is
        this.selectionWidth = width

    // Saves the current state inside a memento.
    method createSnapshot():Snapshot is
        // Memento is an immutable object; that's why the
        // originator passes its state to the memento's
        // constructor parameters.
        return new Snapshot(this, text, curX, curY, selectionWidth)

// The memento class stores the past state of the editor.
class Snapshot is
    private field editor: Editor
    private field text, curX, curY, selectionWidth

    constructor Snapshot(editor, text, curX, curY, selectionWidth) is
        this.editor = editor
        this.text = text
        this.curX = x
        this.curY = y
        this.selectionWidth = selectionWidth

    // At some point, a previous state of the editor can be
    // restored using a memento object.
    method restore() is
        editor.setText(text)
        editor.setCursor(curX, curY)
        editor.setSelectionWidth(selectionWidth)

// A command object can act as a caretaker. In that case, the
// command gets a memento just before it changes the
// originator's state. When undo is requested, it restores the
// originator's state from a memento.
class Command is
    private field backup: Snapshot

    method makeBackup() is
        backup = editor.createSnapshot()

    method undo() is
        if (backup != null)
            backup.restore()
    // ...
```

## **Aplicabilidad**

**Utilice el patrón Memento cuando desee producir instantáneas del estado del objeto para poder restaurar un estado anterior del objeto.**

El patrón Memento le permite hacer copias completas del estado de un objeto, incluidos los campos privados, y almacenarlas por separado del objeto. Si bien la mayoría de la gente recuerda este patrón gracias al caso de uso "deshacer", también es indispensable cuando se trata de transacciones (es decir, si necesita revertir una operación por error).

**Utilice el patrón cuando el acceso directo a los campos/getters/setters del objeto viole su encapsulación.**

El Memento hace que el objeto mismo sea responsable de crear una instantánea de su estado. Ningún otro objeto puede leer la instantánea, lo que hace que los datos de estado del objeto original estén seguros y protegidos.

****Cómo implementar****
1. Determine qué clase desempeñará el papel del originador. Es importante saber si el programa utiliza un objeto central de este tipo o varios más pequeños.
2. Cree la clase de recuerdo. Uno por uno, declare un conjunto de campos que reflejen los campos declarados dentro de la clase originadora.
3. Haz que la clase de recuerdo sea inmutable. Un recuerdo debe aceptar los datos solo una vez, a través del constructor. La clase no debe tener setters.
4. Si su lenguaje de programación admite clases anidadas, anide el recuerdo dentro del originador. Si no, extraiga una interfaz en blanco de la clase memento y haga que todos los demás objetos la usen para referirse al memento. Puede agregar algunas operaciones de metadatos a la interfaz, pero nada que exponga el estado del creador.
5. Agrega un método para producir recuerdos a la clase originadora. El creador debe pasar su estado al memento a través de uno o varios argumentos del constructor del memento.
El tipo de retorno del método debe ser de la interfaz que extrajo en el paso anterior (suponiendo que la haya extraído). Bajo el capó, el método de producción de recuerdos debería funcionar directamente con la clase de recuerdos.
6. Agregue un método para restaurar el estado del originador a su clase. Debe aceptar un objeto de recuerdo como argumento. Si extrajo una interfaz en el paso anterior, conviértala en el tipo del parámetro. En este caso, debe encasillar el objeto entrante en la clase memento, ya que el creador necesita acceso total a ese objeto.
7. El cuidador, ya sea que represente un objeto de comando, una historia o algo completamente diferente, debe saber cuándo solicitar nuevos recuerdos del creador, cómo almacenarlos y cuándo restaurar al creador con un recuerdo en particular.
8. El vínculo entre los cuidadores y los creadores puede trasladarse a la clase memento. En este caso, cada recuerdo debe estar conectado con el autor que lo creó. El método de restauración también pasaría a la clase memento. Sin embargo, todo esto tendría sentido solo si la clase memento está anidada en originator o si la clase originator proporciona suficientes setters para anular su estado.

## **Pros**

- Puede producir instantáneas del estado del objeto sin violar su encapsulación.
- Puede simplificar el código del originador dejando que el cuidador mantenga el historial del estado del originador.

## C**ontras**

- La aplicación puede consumir mucha RAM si los clientes crean recuerdos con demasiada frecuencia.
- Los cuidadores deben rastrear el ciclo de vida del creador para poder destruir los recuerdos obsoletos.
- La mayoría de los lenguajes de programación dinámicos, como PHP, Python y JavaScript, no pueden garantizar que el estado dentro del recuerdo permanezca intacto.

## Ejemplo Ts:

```jsx
/**
 * The Originator holds some important state that may change over time. It also
 * defines a method for saving the state inside a memento and another method for
 * restoring the state from it.
 */
class Originator {
    /**
     * For the sake of simplicity, the originator's state is stored inside a
     * single variable.
     */
    private state: string;

    constructor(state: string) {
        this.state = state;
        console.log(`Originator: My initial state is: ${state}`);
    }

    /**
     * The Originator's business logic may affect its internal state. Therefore,
     * the client should backup the state before launching methods of the
     * business logic via the save() method.
     */
    public doSomething(): void {
        console.log('Originator: I\'m doing something important.');
        this.state = this.generateRandomString(30);
        console.log(`Originator: and my state has changed to: ${this.state}`);
    }

    private generateRandomString(length: number = 10): string {
        const charSet = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';

        return Array
            .apply(null, { length })
            .map(() => charSet.charAt(Math.floor(Math.random() * charSet.length)))
            .join('');
    }

    /**
     * Saves the current state inside a memento.
     */
    public save(): Memento {
        return new ConcreteMemento(this.state);
    }

    /**
     * Restores the Originator's state from a memento object.
     */
    public restore(memento: Memento): void {
        this.state = memento.getState();
        console.log(`Originator: My state has changed to: ${this.state}`);
    }
}

/**
 * The Memento interface provides a way to retrieve the memento's metadata, such
 * as creation date or name. However, it doesn't expose the Originator's state.
 */
interface Memento {
    getState(): string;

    getName(): string;

    getDate(): string;
}

/**
 * The Concrete Memento contains the infrastructure for storing the Originator's
 * state.
 */
class ConcreteMemento implements Memento {
    private state: string;

    private date: string;

    constructor(state: string) {
        this.state = state;
        this.date = new Date().toISOString().slice(0, 19).replace('T', ' ');
    }

    /**
     * The Originator uses this method when restoring its state.
     */
    public getState(): string {
        return this.state;
    }

    /**
     * The rest of the methods are used by the Caretaker to display metadata.
     */
    public getName(): string {
        return `${this.date} / (${this.state.substr(0, 9)}...)`;
    }

    public getDate(): string {
        return this.date;
    }
}

/**
 * The Caretaker doesn't depend on the Concrete Memento class. Therefore, it
 * doesn't have access to the originator's state, stored inside the memento. It
 * works with all mementos via the base Memento interface.
 */
class Caretaker {
    private mementos: Memento[] = [];

    private originator: Originator;

    constructor(originator: Originator) {
        this.originator = originator;
    }

    public backup(): void {
        console.log('\nCaretaker: Saving Originator\'s state...');
        this.mementos.push(this.originator.save());
    }

    public undo(): void {
        if (!this.mementos.length) {
            return;
        }
        const memento = this.mementos.pop();

        console.log(`Caretaker: Restoring state to: ${memento.getName()}`);
        this.originator.restore(memento);
    }

    public showHistory(): void {
        console.log('Caretaker: Here\'s the list of mementos:');
        for (const memento of this.mementos) {
            console.log(memento.getName());
        }
    }
}

/**
 * Client code.
 */
const originator = new Originator('Super-duper-super-puper-super.');
const caretaker = new Caretaker(originator);

caretaker.backup();
originator.doSomething();

caretaker.backup();
originator.doSomething();

caretaker.backup();
originator.doSomething();

console.log('');
caretaker.showHistory();

console.log('\nClient: Now, let\'s rollback!\n');
caretaker.undo();

console.log('\nClient: Once more!\n');
caretaker.undo();
```

**Resultado**:

```jsx
Originator: My initial state is: Super-duper-super-puper-super.

Caretaker: Saving Originator's state...
Originator: I'm doing something important.
Originator: and my state has changed to: qXqxgTcLSCeLYdcgElOghOFhPGfMxo

Caretaker: Saving Originator's state...
Originator: I'm doing something important.
Originator: and my state has changed to: iaVCJVryJwWwbipieensfodeMSWvUY

Caretaker: Saving Originator's state...
Originator: I'm doing something important.
Originator: and my state has changed to: oSUxsOCiZEnohBMQEjwnPWJLGnwGmy

Caretaker: Here's the list of mementos:
2019-02-17 15:14:05 / (Super-dup...)
2019-02-17 15:14:05 / (qXqxgTcLS...)
2019-02-17 15:14:05 / (iaVCJVryJ...)

Client: Now, let's rollback!

Caretaker: Restoring state to: 2019-02-17 15:14:05 / (iaVCJVryJ...)
Originator: My state has changed to: iaVCJVryJwWwbipieensfodeMSWvUY

Client: Once more!

Caretaker: Restoring state to: 2019-02-17 15:14:05 / (qXqxgTcLS...)
Originator: My state has changed to: qXqxgTcLSCeLYdcgElOghOFhPGfMxo
```