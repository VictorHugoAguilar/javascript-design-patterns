# State Pattern

---

## **Intención**

**El estado** es un patrón de diseño de comportamiento que permite que un objeto altere su comportamiento cuando cambia su estado interno. Parece como si el objeto cambiara su clase.

![Untitled](State%20Pattern%20b9f611d6919640fbbe614d5c76dc08c9/Untitled.png)

## **Problema**

El patrón de estado está estrechamente relacionado con el concepto de *máquina de estados finitos.*

![Untitled](State%20Pattern%20b9f611d6919640fbbe614d5c76dc08c9/Untitled%201.png)

*Máquina de estados finitos.*

La idea principal es que, en un momento dado, hay un número *finito de estados* en los que puede estar un programa. Dentro de cualquier estado único, el programa se comporta de manera diferente, y el programa puede cambiar de un estado a otro instantáneamente. Sin embargo, dependiendo del estado actual, el programa puede cambiar o no a otros estados determinados. Estas reglas de cambio, llamadas *transiciones* , también son finitas y predeterminadas.

También puede aplicar este enfoque a los objetos. Imagina que tenemos una `Document`clase. Un documento puede estar en uno de tres estados: `Draft`, `Moderation`y `Published`. El `publish`método del documento funciona un poco diferente en cada estado:

- En `Draft`, mueve el documento a la moderación.
- En `Moderation`, hace público el documento, pero solo si el usuario actual es administrador.
- En `Published`, no hace nada en absoluto.
    
    ![Untitled](State%20Pattern%20b9f611d6919640fbbe614d5c76dc08c9/Untitled%202.png)
    

*Posibles estados y transiciones de un objeto de documento.*

Las máquinas de estado generalmente se implementan con muchas declaraciones condicionales ( `if`o `switch`) que seleccionan el comportamiento apropiado según el estado actual del objeto. Por lo general, este "estado" es solo un conjunto de valores de los campos del objeto. Incluso si nunca antes ha oído hablar de las máquinas de estado finito, probablemente haya implementado un estado al menos una vez. ¿Te suena familiar la siguiente estructura de código?

**`clase**  **Documento**  **es campo**  estado :  cadena // ... **método**  publicar ( )  **es** interruptor  ( estado ) " d r a f t " : estado  =  " m o d e r a t i o n " break " m o d e r a c i ó n " : **if**  ( usuario actual. role  = =  " a re m i n " ) state  =  " p u b l i s h e d " break " p u b l i s h e d " : // No hacer nada. descanso // ...`

La mayor debilidad de una máquina de estados basada en condicionales se revela una vez que comenzamos a agregar más y más estados y comportamientos dependientes del estado a la `Document`clase. La mayoría de los métodos contendrán condicionales monstruosos que elegirán el comportamiento adecuado de un método de acuerdo con el estado actual. Un código como este es muy difícil de mantener porque cualquier cambio en la lógica de transición puede requerir cambios en los condicionales de estado en cada método.

El problema tiende a hacerse más grande a medida que evoluciona un proyecto. Es bastante difícil predecir todos los estados y transiciones posibles en la etapa de diseño. Por lo tanto, una máquina de estado esbelta construida con un conjunto limitado de condicionales puede convertirse en un desastre inflado con el tiempo.

## **Solución**

El patrón State sugiere que cree nuevas clases para todos los estados posibles de un objeto y extraiga todos los comportamientos específicos del estado en estas clases.

En lugar de implementar todos los comportamientos por sí solo, el objeto original, llamado *contexto* , almacena una referencia a uno de los objetos de estado que representa su estado actual y delega todo el trabajo relacionado con el estado a ese objeto.

![Untitled](State%20Pattern%20b9f611d6919640fbbe614d5c76dc08c9/Untitled%203.png)

*El documento delega el trabajo a un objeto de estado.*

Para hacer la transición del contexto a otro estado, reemplace el objeto de estado activo con otro objeto que represente ese nuevo estado. Esto es posible solo si todas las clases de estado siguen la misma interfaz y el propio contexto funciona con estos objetos a través de esa interfaz.

Esta estructura puede parecer similar al patrón de **[estrategia](https://refactoring.guru/pattern/strategy)** , pero hay una diferencia clave. En el patrón de Estado, los estados particulares pueden ser conscientes unos de otros e iniciar transiciones de un estado a otro, mientras que las estrategias casi nunca se conocen entre sí.

## **Analogía del mundo real**

Los botones e interruptores de su teléfono inteligente se comportan de manera diferente según el estado actual del dispositivo:

- Cuando el teléfono está desbloqueado, al presionar los botones se ejecutan varias funciones.
- Cuando el teléfono está bloqueado, al presionar cualquier botón se accede a la pantalla de desbloqueo.
- Cuando la carga del teléfono es baja, al presionar cualquier botón se muestra la pantalla de carga.

## **Estructura**

![Untitled](State%20Pattern%20b9f611d6919640fbbe614d5c76dc08c9/Untitled%204.png)

1. **El contexto** almacena una referencia a uno de los objetos de estado concretos y le delega todo el trabajo específico del estado. El contexto se comunica con el objeto de estado a través de la interfaz de estado. El contexto expone un setter para pasarle un nuevo objeto de estado.
2. La interfaz **State** declara los métodos específicos del estado. Estos métodos deberían tener sentido para todos los estados concretos porque no desea que algunos de sus estados tengan métodos inútiles que nunca se llamarán.
3. **Los estados concretos** proporcionan sus propias implementaciones para los métodos específicos del estado. Para evitar la duplicación de código similar en varios estados, puede proporcionar clases abstractas intermedias que encapsulen algún comportamiento común.
    
    Los objetos de estado pueden almacenar una referencia inversa al objeto de contexto. A través de esta referencia, el estado puede obtener cualquier información requerida del objeto de contexto, así como iniciar transiciones de estado.
    
4. Tanto el contexto como los estados concretos pueden establecer el siguiente estado del contexto y realizar la transición del estado real reemplazando el objeto de estado vinculado al contexto.

## **pseudocódigo**

En este ejemplo, el patrón **State** permite que los mismos controles del reproductor multimedia se comporten de manera diferente, según el estado de reproducción actual.

![Untitled](State%20Pattern%20b9f611d6919640fbbe614d5c76dc08c9/Untitled%205.png)

*Ejemplo de cómo cambiar el comportamiento de un objeto con objetos de estado.*

El objeto principal del jugador siempre está vinculado a un objeto de estado que realiza la mayor parte del trabajo para el jugador. Algunas acciones reemplazan el objeto de estado actual del jugador con otro, lo que cambia la forma en que el jugador reacciona a las interacciones del usuario.

```jsx
// The AudioPlayer class acts as a context. It also maintains a
// reference to an instance of one of the state classes that
// represents the current state of the audio player.
class AudioPlayer is
    field state: State
    field UI, volume, playlist, currentSong

    constructor AudioPlayer() is
        this.state = new ReadyState(this)

        // Context delegates handling user input to a state
        // object. Naturally, the outcome depends on what state
        // is currently active, since each state can handle the
        // input differently.
        UI = new UserInterface()
        UI.lockButton.onClick(this.clickLock)
        UI.playButton.onClick(this.clickPlay)
        UI.nextButton.onClick(this.clickNext)
        UI.prevButton.onClick(this.clickPrevious)

    // Other objects must be able to switch the audio player's
    // active state.
    method changeState(state: State) is
        this.state = state

    // UI methods delegate execution to the active state.
    method clickLock() is
        state.clickLock()
    method clickPlay() is
        state.clickPlay()
    method clickNext() is
        state.clickNext()
    method clickPrevious() is
        state.clickPrevious()

    // A state may call some service methods on the context.
    method startPlayback() is
        // ...
    method stopPlayback() is
        // ...
    method nextSong() is
        // ...
    method previousSong() is
        // ...
    method fastForward(time) is
        // ...
    method rewind(time) is
        // ...

// The base state class declares methods that all concrete
// states should implement and also provides a backreference to
// the context object associated with the state. States can use
// the backreference to transition the context to another state.
abstract class State is
    protected field player: AudioPlayer

    // Context passes itself through the state constructor. This
    // may help a state fetch some useful context data if it's
    // needed.
    constructor State(player) is
        this.player = player

    abstract method clickLock()
    abstract method clickPlay()
    abstract method clickNext()
    abstract method clickPrevious()

// Concrete states implement various behaviors associated with a
// state of the context.
class LockedState extends State is

    // When you unlock a locked player, it may assume one of two
    // states.
    method clickLock() is
        if (player.playing)
            player.changeState(new PlayingState(player))
        else
            player.changeState(new ReadyState(player))

    method clickPlay() is
        // Locked, so do nothing.

    method clickNext() is
        // Locked, so do nothing.

    method clickPrevious() is
        // Locked, so do nothing.

// They can also trigger state transitions in the context.
class ReadyState extends State is
    method clickLock() is
        player.changeState(new LockedState(player))

    method clickPlay() is
        player.startPlayback()
        player.changeState(new PlayingState(player))

    method clickNext() is
        player.nextSong()

    method clickPrevious() is
        player.previousSong()

class PlayingState extends State is
    method clickLock() is
        player.changeState(new LockedState(player))

    method clickPlay() is
        player.stopPlayback()
        player.changeState(new ReadyState(player))

    method clickNext() is
        if (event.doubleclick)
            player.nextSong()
        else
            player.fastForward(5)

    method clickPrevious() is
        if (event.doubleclick)
            player.previous()
        else
            player.rewind(5)
```

## **Aplicabilidad**

**Utilice el patrón de estado cuando tenga un objeto que se comporte de manera diferente dependiendo de su estado actual, la cantidad de estados es enorme y el código específico del estado cambia con frecuencia.**

El patrón sugiere que extraiga todo el código específico del estado en un conjunto de clases distintas. Como resultado, puede agregar nuevos estados o cambiar los existentes de forma independiente, lo que reduce el costo de mantenimiento.

**Utilice el patrón cuando tenga una clase contaminada con condicionales masivos que alteren el comportamiento de la clase de acuerdo con los valores actuales de los campos de la clase.**

El patrón State te permite extraer ramas de estos condicionales en métodos de clases de estado correspondientes. Mientras lo hace, también puede limpiar los campos temporales y los métodos auxiliares involucrados en el código específico del estado de su clase principal.

**Use Estado cuando tenga mucho código duplicado en estados y transiciones similares de una máquina de estado basada en condiciones.**

El patrón State le permite componer jerarquías de clases de estado y reducir la duplicación mediante la extracción de código común en clases base abstractas.

****Cómo implementar****
1. Decida qué clase actuará como contexto. Podría ser una clase existente que ya tiene el código dependiente del estado; o una nueva clase, si el código específico del estado se distribuye en varias clases.
2. Declarar la interfaz de estado. Aunque puede reflejar todos los métodos declarados en el contexto, apunte solo a aquellos que pueden contener un comportamiento específico del estado.
3. Para cada estado real, cree una clase que derive de la interfaz de estado. Luego revise los métodos del contexto y extraiga todo el código relacionado con ese estado en su clase recién creada.
Mientras mueve el código a la clase de estado, puede descubrir que depende de los miembros privados del contexto. Hay varias soluciones:
    ◦ Haga públicos estos campos o métodos.
    ◦ Convierta el comportamiento que está extrayendo en un método público en el contexto y llámelo desde la clase de estado. Esta forma es fea pero rápida, y siempre puedes arreglarla más tarde.
    ◦ Anide las clases de estado en la clase de contexto, pero solo si su lenguaje de programación admite clases anidadas.
4. En la clase de contexto, agregue un campo de referencia del tipo de interfaz de estado y un setter público que permita anular el valor de ese campo.
5. Repase el método del contexto nuevamente y reemplace los condicionales de estado vacíos con llamadas a los métodos correspondientes del objeto de estado.
6. Para cambiar el estado del contexto, cree una instancia de una de las clases de estado y pásela al contexto. Puede hacer esto dentro del propio contexto, o en varios estados, o en el cliente. Dondequiera que se haga esto, la clase se vuelve dependiente de la clase de estado concreta que instancia.

## **Pros**

- *Principio de responsabilidad única*. Organice el código relacionado con estados particulares en clases separadas.
- *Principio abierto/cerrado*. Introduzca nuevos estados sin cambiar las clases de estado existentes o el contexto.
- Simplifique el código del contexto eliminando los condicionales voluminosos de la máquina de estado.

## C**ontras**

- Aplicar el patrón puede ser excesivo si una máquina de estado tiene solo unos pocos estados o rara vez cambia.

## Ejemplo Ts:

```jsx
/**
 * The Context defines the interface of interest to clients. It also maintains a
 * reference to an instance of a State subclass, which represents the current
 * state of the Context.
 */
class Context {
    /**
     * @type {State} A reference to the current state of the Context.
     */
    private state: State;

    constructor(state: State) {
        this.transitionTo(state);
    }

    /**
     * The Context allows changing the State object at runtime.
     */
    public transitionTo(state: State): void {
        console.log(`Context: Transition to ${(<any>state).constructor.name}.`);
        this.state = state;
        this.state.setContext(this);
    }

    /**
     * The Context delegates part of its behavior to the current State object.
     */
    public request1(): void {
        this.state.handle1();
    }

    public request2(): void {
        this.state.handle2();
    }
}

/**
 * The base State class declares methods that all Concrete State should
 * implement and also provides a backreference to the Context object, associated
 * with the State. This backreference can be used by States to transition the
 * Context to another State.
 */
abstract class State {
    protected context: Context;

    public setContext(context: Context) {
        this.context = context;
    }

    public abstract handle1(): void;

    public abstract handle2(): void;
}

/**
 * Concrete States implement various behaviors, associated with a state of the
 * Context.
 */
class ConcreteStateA extends State {
    public handle1(): void {
        console.log('ConcreteStateA handles request1.');
        console.log('ConcreteStateA wants to change the state of the context.');
        this.context.transitionTo(new ConcreteStateB());
    }

    public handle2(): void {
        console.log('ConcreteStateA handles request2.');
    }
}

class ConcreteStateB extends State {
    public handle1(): void {
        console.log('ConcreteStateB handles request1.');
    }

    public handle2(): void {
        console.log('ConcreteStateB handles request2.');
        console.log('ConcreteStateB wants to change the state of the context.');
        this.context.transitionTo(new ConcreteStateA());
    }
}

/**
 * The client code.
 */
const context = new Context(new ConcreteStateA());
context.request1();
context.request2();
```

**Resultado:**

```jsx
Context: Transition to ConcreteStateA.
ConcreteStateA handles request1.
ConcreteStateA wants to change the state of the context.
Context: Transition to ConcreteStateB.
ConcreteStateB handles request2.
ConcreteStateB wants to change the state of the context.
Context: Transition to ConcreteStateA.
```