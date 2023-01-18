# Strategy Pattern

---

## **Intención**

**La estrategia** es un patrón de diseño de comportamiento que le permite definir una familia de algoritmos, colocar cada uno de ellos en una clase separada y hacer que sus objetos sean intercambiables.

![Untitled](Strategy%20Pattern%20363f3d0faa9649feaef20cc6ee26640b/Untitled.png)

## **Problema**

Un día decidiste crear una aplicación de navegación para viajeros ocasionales. La aplicación se centraba en un hermoso mapa que ayudaba a los usuarios a orientarse rápidamente en cualquier ciudad.

Una de las funciones más solicitadas para la aplicación fue la planificación automática de rutas. Un usuario debe poder ingresar una dirección y ver la ruta más rápida a ese destino que se muestra en el mapa.

La primera versión de la aplicación solo podía construir rutas sobre carreteras. La gente que viajaba en coche rebosaba de alegría. Pero aparentemente, no a todo el mundo le gusta conducir en sus vacaciones. Entonces, con la próxima actualización, agregó una opción para crear rutas para caminar. Inmediatamente después de eso, agregó otra opción para permitir que las personas usen el transporte público en sus rutas.

Sin embargo, eso fue solo el comienzo. Más tarde planeó agregar la creación de rutas para ciclistas. Y más adelante, otra opción para construir rutas por todos los atractivos turísticos de una ciudad.

![Untitled](Strategy%20Pattern%20363f3d0faa9649feaef20cc6ee26640b/Untitled%201.png)

*El código del navegador se infló.*

Si bien desde una perspectiva comercial la aplicación fue un éxito, la parte técnica te causó muchos dolores de cabeza. Cada vez que agregaba un nuevo algoritmo de enrutamiento, la clase principal del navegador se duplicaba en tamaño. En algún momento, la bestia se volvió demasiado difícil de mantener.

Cualquier cambio en uno de los algoritmos, ya fuera una simple corrección de errores o un ligero ajuste de la puntuación de la calle, afectó a toda la clase, lo que aumentó la posibilidad de crear un error en el código que ya funcionaba.

Además, el trabajo en equipo se volvió ineficiente. Sus compañeros de equipo, que habían sido contratados justo después del lanzamiento exitoso, se quejan de que pasan demasiado tiempo resolviendo conflictos de combinación. La implementación de una nueva función requiere que cambie la misma clase enorme, lo que entra en conflicto con el código producido por otras personas.

## **Solución**

El patrón de estrategia sugiere que tome una clase que haga algo específico de muchas maneras diferentes y extraiga todos estos algoritmos en clases separadas llamadas *estrategias* .

La clase original, llamada *contexto* , debe tener un campo para almacenar una referencia a una de las estrategias. El contexto delega el trabajo a un objeto de estrategia vinculado en lugar de ejecutarlo por sí solo.

El contexto no es responsable de seleccionar un algoritmo apropiado para el trabajo. En cambio, el cliente pasa la estrategia deseada al contexto. De hecho, el contexto no sabe mucho sobre estrategias. Funciona con todas las estrategias a través de la misma interfaz genérica, que solo expone un único método para activar el algoritmo encapsulado dentro de la estrategia seleccionada.

De esta manera el contexto se independiza de estrategias concretas, por lo que puedes agregar nuevos algoritmos o modificar los existentes sin cambiar el código del contexto u otras estrategias.

![Untitled](Strategy%20Pattern%20363f3d0faa9649feaef20cc6ee26640b/Untitled%202.png)

*Estrategias de planificación de rutas.*

En nuestra aplicación de navegación, cada algoritmo de enrutamiento se puede extraer a su propia clase con un solo `buildRoute`método. El método acepta un origen y un destino y devuelve una colección de los puntos de control de la ruta.

Aunque con los mismos argumentos, cada clase de enrutamiento puede crear una ruta diferente, a la clase de navegador principal realmente no le importa qué algoritmo se selecciona, ya que su trabajo principal es representar un conjunto de puntos de control en el mapa. La clase tiene un método para cambiar la estrategia de enrutamiento activa, por lo que sus clientes, como los botones en la interfaz de usuario, pueden reemplazar el comportamiento de enrutamiento seleccionado actualmente por otro.

## **Analogía del mundo real**

![Untitled](Strategy%20Pattern%20363f3d0faa9649feaef20cc6ee26640b/Untitled%203.png)

*Diversas estrategias para llegar al aeropuerto.*

Imagina que tienes que llegar al aeropuerto. Puedes tomar un autobús, pedir un taxi o subirte a tu bicicleta. Estas son sus estrategias de transporte. Puede elegir una de las estrategias según factores como el presupuesto o las limitaciones de tiempo.

## **Estructura**

![Untitled](Strategy%20Pattern%20363f3d0faa9649feaef20cc6ee26640b/Untitled%204.png)

1. El **contexto** mantiene una referencia a una de las estrategias concretas y se comunica con este objeto solo a través de la interfaz de estrategia.
2. La interfaz de **Estrategia es común a todas las estrategias concretas.** Declara un método que utiliza el contexto para ejecutar una estrategia.
3. **Las estrategias concretas** implementan diferentes variaciones de un algoritmo que utiliza el contexto.
4. El contexto llama al método de ejecución en el objeto de estrategia vinculado cada vez que necesita ejecutar el algoritmo. El contexto no sabe con qué tipo de estrategia trabaja o cómo se ejecuta el algoritmo.
5. El **Cliente** crea un objeto de estrategia específico y lo pasa al contexto. El contexto expone un setter que permite a los clientes reemplazar la estrategia asociada con el contexto en tiempo de ejecución.

## **pseudocódigo**

En este ejemplo, el contexto usa múltiples **estrategias** para ejecutar varias operaciones aritméticas.

```jsx
// The strategy interface declares operations common to all
// supported versions of some algorithm. The context uses this
// interface to call the algorithm defined by the concrete
// strategies.
interface Strategy is
    method execute(a, b)

// Concrete strategies implement the algorithm while following
// the base strategy interface. The interface makes them
// interchangeable in the context.
class ConcreteStrategyAdd implements Strategy is
    method execute(a, b) is
        return a + b

class ConcreteStrategySubtract implements Strategy is
    method execute(a, b) is
        return a - b

class ConcreteStrategyMultiply implements Strategy is
    method execute(a, b) is
        return a * b

// The context defines the interface of interest to clients.
class Context is
    // The context maintains a reference to one of the strategy
    // objects. The context doesn't know the concrete class of a
    // strategy. It should work with all strategies via the
    // strategy interface.
    private strategy: Strategy

    // Usually the context accepts a strategy through the
    // constructor, and also provides a setter so that the
    // strategy can be switched at runtime.
    method setStrategy(Strategy strategy) is
        this.strategy = strategy

    // The context delegates some work to the strategy object
    // instead of implementing multiple versions of the
    // algorithm on its own.
    method executeStrategy(int a, int b) is
        return strategy.execute(a, b)

// The client code picks a concrete strategy and passes it to
// the context. The client should be aware of the differences
// between strategies in order to make the right choice.
class ExampleApplication is
    method main() is
        Create context object.

        Read first number.
        Read last number.
        Read the desired action from user input.

        if (action == addition) then
            context.setStrategy(new ConcreteStrategyAdd())

        if (action == subtraction) then
            context.setStrategy(new ConcreteStrategySubtract())

        if (action == multiplication) then
            context.setStrategy(new ConcreteStrategyMultiply())

        result = context.executeStrategy(First number, Second number)

        Print result.
```

## **Aplicabilidad**

**Utilice el patrón de estrategia cuando desee utilizar diferentes variantes de un algoritmo dentro de un objeto y poder cambiar de un algoritmo a otro durante el tiempo de ejecución.**

El patrón de estrategia le permite alterar indirectamente el comportamiento del objeto en tiempo de ejecución al asociarlo con diferentes subobjetos que pueden realizar subtareas específicas de diferentes maneras.

**Use la estrategia cuando tenga muchas clases similares que solo difieren en la forma en que ejecutan algún comportamiento.**

El patrón de estrategia le permite extraer el comportamiento variable en una jerarquía de clases separada y combinar las clases originales en una sola, reduciendo así el código duplicado.

**Utilice el patrón para aislar la lógica empresarial de una clase de los detalles de implementación de los algoritmos que pueden no ser tan importantes en el contexto de esa lógica.**

El patrón de estrategia le permite aislar el código, los datos internos y las dependencias de varios algoritmos del resto del código. Varios clientes obtienen una interfaz simple para ejecutar los algoritmos y cambiarlos en tiempo de ejecución.

**Use el patrón cuando su clase tenga una declaración condicional masiva que cambie entre diferentes variantes del mismo algoritmo.**

El patrón de estrategia le permite eliminar dicho condicional al extraer todos los algoritmos en clases separadas, todas las cuales implementan la misma interfaz. El objeto original delega la ejecución a uno de estos objetos, en lugar de implementar todas las variantes del algoritmo.

### ****Cómo implementar****

1. En la clase de contexto, identifique un algoritmo que sea propenso a cambios frecuentes. También puede ser un condicional masivo que selecciona y ejecuta una variante del mismo algoritmo en tiempo de ejecución.
2. Declarar la interfaz de estrategia común a todas las variantes del algoritmo.
3. Uno por uno, extraiga todos los algoritmos en sus propias clases. Todos deberían implementar la interfaz de estrategia.
4. En la clase de contexto, agregue un campo para almacenar una referencia a un objeto de estrategia. Proporcione un setter para reemplazar los valores de ese campo. El contexto debe funcionar con el objeto de estrategia solo a través de la interfaz de estrategia. El contexto puede definir una interfaz que permita a la estrategia acceder a sus datos.
5. Los clientes del contexto deben asociarlo con una estrategia adecuada que coincida con la forma en que esperan que el contexto realice su trabajo principal.

## **Pros**

- Puede intercambiar algoritmos utilizados dentro de un objeto en tiempo de ejecución.
- Puede aislar los detalles de implementación de un algoritmo del código que lo usa.
- Puede reemplazar la herencia con la composición.
- *Principio abierto/cerrado*. Puede introducir nuevas estrategias sin tener que cambiar el contexto.

## C**ontras**

- Si solo tiene un par de algoritmos y rara vez cambian, no hay una razón real para complicar demasiado el programa con nuevas clases e interfaces que vienen con el patrón.
- Los clientes deben ser conscientes de las diferencias entre las estrategias para poder seleccionar una adecuada.
- Muchos lenguajes de programación modernos tienen soporte de tipo funcional que le permite implementar diferentes versiones de un algoritmo dentro de un conjunto de funciones anónimas. Entonces podría usar estas funciones exactamente como hubiera usado los objetos de estrategia, pero sin inflar su código con clases e interfaces adicionales.

## Ejemplo Ts:

```jsx
/**
 * The Context defines the interface of interest to clients.
 */
class Context {
    /**
     * @type {Strategy} The Context maintains a reference to one of the Strategy
     * objects. The Context does not know the concrete class of a strategy. It
     * should work with all strategies via the Strategy interface.
     */
    private strategy: Strategy;

    /**
     * Usually, the Context accepts a strategy through the constructor, but also
     * provides a setter to change it at runtime.
     */
    constructor(strategy: Strategy) {
        this.strategy = strategy;
    }

    /**
     * Usually, the Context allows replacing a Strategy object at runtime.
     */
    public setStrategy(strategy: Strategy) {
        this.strategy = strategy;
    }

    /**
     * The Context delegates some work to the Strategy object instead of
     * implementing multiple versions of the algorithm on its own.
     */
    public doSomeBusinessLogic(): void {
        // ...

        console.log('Context: Sorting data using the strategy (not sure how it\'ll do it)');
        const result = this.strategy.doAlgorithm(['a', 'b', 'c', 'd', 'e']);
        console.log(result.join(','));

        // ...
    }
}

/**
 * The Strategy interface declares operations common to all supported versions
 * of some algorithm.
 *
 * The Context uses this interface to call the algorithm defined by Concrete
 * Strategies.
 */
interface Strategy {
    doAlgorithm(data: string[]): string[];
}

/**
 * Concrete Strategies implement the algorithm while following the base Strategy
 * interface. The interface makes them interchangeable in the Context.
 */
class ConcreteStrategyA implements Strategy {
    public doAlgorithm(data: string[]): string[] {
        return data.sort();
    }
}

class ConcreteStrategyB implements Strategy {
    public doAlgorithm(data: string[]): string[] {
        return data.reverse();
    }
}

/**
 * The client code picks a concrete strategy and passes it to the context. The
 * client should be aware of the differences between strategies in order to make
 * the right choice.
 */
const context = new Context(new ConcreteStrategyA());
console.log('Client: Strategy is set to normal sorting.');
context.doSomeBusinessLogic();

console.log('');

console.log('Client: Strategy is set to reverse sorting.');
context.setStrategy(new ConcreteStrategyB());
context.doSomeBusinessLogic();
```

**Resultado:**

```jsx
Client: Strategy is set to normal sorting.
Context: Sorting data using the strategy (not sure how it'll do it)
a,b,c,d,e

Client: Strategy is set to reverse sorting.
Context: Sorting data using the strategy (not sure how it'll do it)
e,d,c,b,a
```