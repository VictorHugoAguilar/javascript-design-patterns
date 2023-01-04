# Visitor Pattern

---

## **Intención**

**Visitor** es un patrón de diseño de comportamiento que le permite separar los algoritmos de los objetos en los que operan.

![Untitled](Visitor%20Pattern%20be97f8bd167a43bf8f8b93ec3694c5dc/Untitled.png)

## **Problema**

Imagina que tu equipo desarrolla una aplicación que funciona con información geográfica estructurada como un gráfico colosal. Cada nodo del gráfico puede representar una entidad compleja como una ciudad, pero también cosas más granulares como industrias, áreas turísticas, etc. Los nodos están conectados con otros si hay un camino entre los objetos reales que representan. Bajo el capó, cada tipo de nodo está representado por su propia clase, mientras que cada nodo específico es un objeto.

![Untitled](Visitor%20Pattern%20be97f8bd167a43bf8f8b93ec3694c5dc/Untitled%201.png)

*Exportación del gráfico a XML.*

En algún momento, tuvo la tarea de implementar la exportación del gráfico a formato XML. Al principio, el trabajo parecía bastante sencillo. Planeó agregar un método de exportación a cada clase de nodo y luego aprovechar la recursividad para revisar cada nodo del gráfico, ejecutando el método de exportación. La solución fue simple y elegante: gracias al polimorfismo, no estabas acoplando el código que llamaba al método de exportación a clases concretas de nodos.

Desafortunadamente, el arquitecto del sistema se negó a permitirle modificar las clases de nodos existentes. Dijo que el código ya estaba en producción y que no quería arriesgarse a romperlo debido a un posible error en sus cambios.

![Untitled](Visitor%20Pattern%20be97f8bd167a43bf8f8b93ec3694c5dc/Untitled%202.png)

*El método de exportación XML tuvo que agregarse a todas las clases de nodos, lo que conllevaba el riesgo de romper toda la aplicación si se presentaba algún error junto con el cambio.*

Además, cuestionó si tiene sentido tener el código de exportación XML dentro de las clases de nodos. El trabajo principal de estas clases era trabajar con geodatos. El comportamiento de exportación XML parecería extraño allí.

Había otra razón para la negativa. Es muy probable que después de implementar esta función, alguien del departamento de marketing le pida que proporcione la capacidad de exportar a un formato diferente, o solicite otras cosas extrañas. Esto te obligaría a cambiar de nuevo esas preciadas y frágiles clases.

## **Solución**

El patrón Visitor sugiere que coloque el nuevo comportamiento en una clase separada llamada *visitante* , en lugar de intentar integrarlo en las clases existentes. El objeto original que tenía que realizar el comportamiento ahora se pasa a uno de los métodos del visitante como argumento, proporcionando acceso al método a todos los datos necesarios contenidos en el objeto.

Ahora, ¿qué pasa si ese comportamiento se puede ejecutar sobre objetos de diferentes clases? Por ejemplo, en nuestro caso con la exportación XML, la implementación real probablemente será un poco diferente en varias clases de nodos. Por lo tanto, la clase visitante puede definir no uno, sino un conjunto de métodos, cada uno de los cuales podría tomar argumentos de diferentes tipos, como este:

```jsx
class ExportVisitor implements Visitor is
    method doForCity(City c) { ... }
    method doForIndustry(Industry f) { ... }
    method doForSightSeeing(SightSeeing ss) { ... }
    // ...
```

Pero, ¿cómo llamaríamos exactamente a estos métodos, especialmente cuando se trata de todo el gráfico? Estos métodos tienen firmas diferentes, por lo que no podemos usar polimorfismo. Para elegir un método de visitante adecuado que pueda procesar un objeto determinado, debemos verificar su clase. ¿No suena esto como una pesadilla?

```jsx
foreach (Node node in graph)
    if (node instanceof City)
        exportVisitor.doForCity((City) node)
    if (node instanceof Industry)
        exportVisitor.doForIndustry((Industry) node)
    // ...
}
```

Podría preguntarse, ¿por qué no usamos la sobrecarga de métodos? Ahí es cuando le da a todos los métodos el mismo nombre, incluso si admiten diferentes conjuntos de parámetros. Desafortunadamente, incluso suponiendo que nuestro lenguaje de programación lo admita (como lo hacen Java y C#), no nos ayudará. Dado que la clase exacta de un objeto de nodo se desconoce de antemano, el mecanismo de sobrecarga no podrá determinar el método correcto para ejecutar. Por defecto usará el método que toma un objeto de la `Node`clase base.

Sin embargo, el patrón Visitor soluciona este problema. Utiliza una técnica llamada **[Double Dispatch](https://refactoring.guru/pages/design-patterns/visitor-double-dispatch)** , que ayuda a ejecutar el método adecuado en un objeto sin condicionales engorrosos. En lugar de permitir que el cliente seleccione una versión adecuada del método para llamar, ¿qué tal si delegamos esta elección a los objetos que estamos pasando al visitante como argumento? Dado que los objetos conocen sus propias clases, podrán elegir un método adecuado para el visitante con menos dificultad. Ellos “aceptan” a un visitante y le dicen qué método de visita debe ejecutarse.

```jsx
// Client code
foreach (Node node in graph)
    node.accept(exportVisitor)

// City
class City is
    method accept(Visitor v) is
        v.doForCity(this)
    // ...

// Industry
class Industry is
    method accept(Visitor v) is
        v.doForIndustry(this)
    // ...
```

Yo confieso. Después de todo, tuvimos que cambiar las clases de nodos. Pero al menos el cambio es trivial y nos permite agregar más comportamientos sin alterar el código una vez más.

Ahora, si extraemos una interfaz común para todos los visitantes, todos los nodos existentes pueden funcionar con cualquier visitante que introduzca en la aplicación. Si se encuentra introduciendo un nuevo comportamiento relacionado con los nodos, todo lo que tiene que hacer es implementar una nueva clase de visitante.

## **Analogía del mundo real**

![Untitled](Visitor%20Pattern%20be97f8bd167a43bf8f8b93ec3694c5dc/Untitled%203.png)

*Un buen agente de seguros siempre está listo para ofrecer diferentes pólizas a varios tipos de organizaciones.*

Imagine un agente de seguros experimentado que está ansioso por conseguir nuevos clientes. Puede visitar todos los edificios de un vecindario, tratando de vender seguros a todos los que conoce. Dependiendo del tipo de organización que ocupe el edificio, puede ofrecer pólizas de seguro especializadas:

- Si es un edificio residencial, vende seguros médicos.
- Si es un banco, vende seguros de robo.
- Si es una cafetería, vende seguros contra incendios e inundaciones.

## **Estructura**

![Untitled](Visitor%20Pattern%20be97f8bd167a43bf8f8b93ec3694c5dc/Untitled%204.png)

1. La interfaz **Visitor** declara un conjunto de métodos de visita que pueden tomar elementos concretos de una estructura de objeto como argumentos. Estos métodos pueden tener los mismos nombres si el programa está escrito en un lenguaje que admite sobrecarga, pero el tipo de sus parámetros debe ser diferente.
2. Cada **Visitante concreto** implementa varias versiones de los mismos comportamientos, adaptados para diferentes clases de elementos concretos.
3. La interfaz de **Element** declara un método para "aceptar" visitantes. Este método debe tener un parámetro declarado con el tipo de interfaz de visitante.
4. Cada **Elemento de Concreto** debe implementar el método de aceptación. El propósito de este método es redirigir la llamada al método de visitante adecuado correspondiente a la clase de elemento actual. Tenga en cuenta que incluso si una clase de elemento base implementa este método, todas las subclases aún deben anular este método en sus propias clases y llamar al método apropiado en el objeto visitante.
5. El **Cliente** suele representar una colección o algún otro objeto complejo (por ejemplo, un árbol **[compuesto ).](https://refactoring.guru/pattern/composite)** Por lo general, los clientes no conocen todas las clases de elementos concretos porque trabajan con objetos de esa colección a través de alguna interfaz abstracta.

## **pseudocódigo**

En este ejemplo, el patrón **Visitor** agrega soporte de exportación XML a la jerarquía de clases de formas geométricas.

![Untitled](Visitor%20Pattern%20be97f8bd167a43bf8f8b93ec3694c5dc/Untitled%205.png)

*Exportación de varios tipos de objetos en formato XML a través de un objeto de visitante.*

```jsx
// The element interface declares an `accept` method that takes
// the base visitor interface as an argument.
interface Shape is
    method move(x, y)
    method draw()
    method accept(v: Visitor)

// Each concrete element class must implement the `accept`
// method in such a way that it calls the visitor's method that
// corresponds to the element's class.
class Dot implements Shape is
    // ...

    // Note that we're calling `visitDot`, which matches the
    // current class name. This way we let the visitor know the
    // class of the element it works with.
    method accept(v: Visitor) is
        v.visitDot(this)

class Circle implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitCircle(this)

class Rectangle implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitRectangle(this)

class CompoundShape implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitCompoundShape(this)

// The Visitor interface declares a set of visiting methods that
// correspond to element classes. The signature of a visiting
// method lets the visitor identify the exact class of the
// element that it's dealing with.
interface Visitor is
    method visitDot(d: Dot)
    method visitCircle(c: Circle)
    method visitRectangle(r: Rectangle)
    method visitCompoundShape(cs: CompoundShape)

// Concrete visitors implement several versions of the same
// algorithm, which can work with all concrete element classes.
//
// You can experience the biggest benefit of the Visitor pattern
// when using it with a complex object structure such as a
// Composite tree. In this case, it might be helpful to store
// some intermediate state of the algorithm while executing the
// visitor's methods over various objects of the structure.
class XMLExportVisitor implements Visitor is
    method visitDot(d: Dot) is
        // Export the dot's ID and center coordinates.

    method visitCircle(c: Circle) is
        // Export the circle's ID, center coordinates and
        // radius.

    method visitRectangle(r: Rectangle) is
        // Export the rectangle's ID, left-top coordinates,
        // width and height.

    method visitCompoundShape(cs: CompoundShape) is
        // Export the shape's ID as well as the list of its
        // children's IDs.

// The client code can run visitor operations over any set of
// elements without figuring out their concrete classes. The
// accept operation directs a call to the appropriate operation
// in the visitor object.
class Application is
    field allShapes: array of Shapes

    method export() is
        exportVisitor = new XMLExportVisitor()

        foreach (shape in allShapes) do
            shape.accept(exportVisitor)
```

Si se pregunta por qué necesitamos el `accept`método en este ejemplo, mi artículo **[Visitor and Double Dispatch](https://refactoring.guru/pages/design-patterns/visitor-double-dispatch)** aborda esta pregunta en detalle.

## **Aplicabilidad**

**Utilice el Visitante cuando necesite realizar una operación en todos los elementos de una estructura de objeto compleja (por ejemplo, un árbol de objetos).**

El patrón Visitor le permite ejecutar una operación sobre un conjunto de objetos con diferentes clases haciendo que un objeto visitante implemente varias variantes de la misma operación, que corresponden a todas las clases de destino.

**Utilice el Visitante para limpiar la lógica empresarial de los comportamientos auxiliares.**

El patrón le permite hacer que las clases primarias de su aplicación se centren más en sus trabajos principales al extraer todos los demás comportamientos en un conjunto de clases de visitantes.

**Utilice el patrón cuando un comportamiento tenga sentido solo en algunas clases de una jerarquía de clases, pero no en otras.**

Puede extraer este comportamiento en una clase de visitante separada e implementar solo aquellos métodos de visita que acepten objetos de clases relevantes, dejando el resto vacío.

****Cómo implementar****
1. Declare la interfaz de visitante con un conjunto de métodos de "visita", uno por cada clase de elemento concreto que existe en el programa.
2. Declarar la interfaz del elemento. Si está trabajando con una jerarquía de clases de elementos existente, agregue el método abstracto de "aceptación" a la clase base de la jerarquía. Este método debería aceptar un objeto visitante como argumento.
3. Implementar los métodos de aceptación en todas las clases de elementos de hormigón. Estos métodos simplemente deben redirigir la llamada a un método visitante en el objeto visitante entrante que coincida con la clase del elemento actual.
4. Las clases de elementos solo deberían funcionar con los visitantes a través de la interfaz de visitantes. Los visitantes, sin embargo, deben conocer todas las clases de elementos concretos, a los que se hace referencia como tipos de parámetros de los métodos de visita.
5. Para cada comportamiento que no se pueda implementar dentro de la jerarquía de elementos, cree una nueva clase de visitante concreta e implemente todos los métodos de visita.
Puede encontrar una situación en la que el visitante necesite acceso a algunos miembros privados de la clase de elemento. En este caso, puede hacer públicos estos campos o métodos, violando la encapsulación del elemento, o anidar la clase de visitante en la clase de elemento. Esto último solo es posible si tiene la suerte de trabajar con un lenguaje de programación que admita clases anidadas.
6. El cliente debe crear objetos de visitante y convertirlos en elementos a través de métodos de "aceptación".

## **Pros y**

- *Principio abierto/cerrado*. Puede introducir un nuevo comportamiento que puede funcionar con objetos de diferentes clases sin cambiar estas clases.
- *Principio de responsabilidad única*. Puede mover varias versiones del mismo comportamiento a la misma clase.
- Un objeto visitante puede acumular información útil mientras trabaja con varios objetos. Esto puede ser útil cuando desea atravesar una estructura de objeto compleja, como un árbol de objetos, y aplicar el visitante a cada objeto de esta estructura.

## C**ontras**

- Debe actualizar a todos los visitantes cada vez que se agrega o elimina una clase de la jerarquía de elementos.
- Los visitantes pueden carecer del acceso necesario a los campos y métodos privados de los elementos con los que se supone que deben trabajar.

## Ejemplo Ts:

```jsx
/**
 * The Component interface declares an `accept` method that should take the base
 * visitor interface as an argument.
 */
interface Component {
    accept(visitor: Visitor): void;
}

/**
 * Each Concrete Component must implement the `accept` method in such a way that
 * it calls the visitor's method corresponding to the component's class.
 */
class ConcreteComponentA implements Component {
    /**
     * Note that we're calling `visitConcreteComponentA`, which matches the
     * current class name. This way we let the visitor know the class of the
     * component it works with.
     */
    public accept(visitor: Visitor): void {
        visitor.visitConcreteComponentA(this);
    }

    /**
     * Concrete Components may have special methods that don't exist in their
     * base class or interface. The Visitor is still able to use these methods
     * since it's aware of the component's concrete class.
     */
    public exclusiveMethodOfConcreteComponentA(): string {
        return 'A';
    }
}

class ConcreteComponentB implements Component {
    /**
     * Same here: visitConcreteComponentB => ConcreteComponentB
     */
    public accept(visitor: Visitor): void {
        visitor.visitConcreteComponentB(this);
    }

    public specialMethodOfConcreteComponentB(): string {
        return 'B';
    }
}

/**
 * The Visitor Interface declares a set of visiting methods that correspond to
 * component classes. The signature of a visiting method allows the visitor to
 * identify the exact class of the component that it's dealing with.
 */
interface Visitor {
    visitConcreteComponentA(element: ConcreteComponentA): void;

    visitConcreteComponentB(element: ConcreteComponentB): void;
}

/**
 * Concrete Visitors implement several versions of the same algorithm, which can
 * work with all concrete component classes.
 *
 * You can experience the biggest benefit of the Visitor pattern when using it
 * with a complex object structure, such as a Composite tree. In this case, it
 * might be helpful to store some intermediate state of the algorithm while
 * executing visitor's methods over various objects of the structure.
 */
class ConcreteVisitor1 implements Visitor {
    public visitConcreteComponentA(element: ConcreteComponentA): void {
        console.log(`${element.exclusiveMethodOfConcreteComponentA()} + ConcreteVisitor1`);
    }

    public visitConcreteComponentB(element: ConcreteComponentB): void {
        console.log(`${element.specialMethodOfConcreteComponentB()} + ConcreteVisitor1`);
    }
}

class ConcreteVisitor2 implements Visitor {
    public visitConcreteComponentA(element: ConcreteComponentA): void {
        console.log(`${element.exclusiveMethodOfConcreteComponentA()} + ConcreteVisitor2`);
    }

    public visitConcreteComponentB(element: ConcreteComponentB): void {
        console.log(`${element.specialMethodOfConcreteComponentB()} + ConcreteVisitor2`);
    }
}

/**
 * The client code can run visitor operations over any set of elements without
 * figuring out their concrete classes. The accept operation directs a call to
 * the appropriate operation in the visitor object.
 */
function clientCode(components: Component[], visitor: Visitor) {
    // ...
    for (const component of components) {
        component.accept(visitor);
    }
    // ...
}

const components = [
    new ConcreteComponentA(),
    new ConcreteComponentB(),
];

console.log('The client code works with all visitors via the base Visitor interface:');
const visitor1 = new ConcreteVisitor1();
clientCode(components, visitor1);
console.log('');

console.log('It allows the same client code to work with different types of visitors:');
const visitor2 = new ConcreteVisitor2();
clientCode(components, visitor2);
```

**Resultado**: 

```jsx
The client code works with all visitors via the base Visitor interface:
A + ConcreteVisitor1
B + ConcreteVisitor1

It allows the same client code to work with different types of visitors:
A + ConcreteVisitor2
B + ConcreteVisitor2
```