# Prototype Pattern

# ****Propósito****

**Prototype**
 es un patrón de diseño creacional que nos permite copiar objetos existentes sin que el código dependa de sus clases.

![Untitled](Prototype%20Pattern%20053504eac4804ac098dd390230d45a6a/Untitled.png)

# 😟 ****Problema****

Digamos que tienes un objeto y quieres crear una copia exacta de él. ¿Cómo lo harías? En primer lugar, debes crear un nuevo objeto de la misma clase. Después debes recorrer todos los campos del objeto original y copiar sus valores en el nuevo objeto.

¡Bien! Pero hay una trampa. No todos los objetos se pueden copiar de este modo, porque algunos de los campos del objeto pueden ser privados e invisibles desde fuera del propio objeto.

![Untitled](Prototype%20Pattern%20053504eac4804ac098dd390230d45a6a/Untitled%201.png)

Hay otro problema con el enfoque directo. Dado que debes conocer la clase del objeto para crear un duplicado, el código se vuelve dependiente de esa clase. Si esta dependencia adicional no te da miedo, todavía hay otra trampa. En ocasiones tan solo conocemos la interfaz que sigue el objeto, pero no su clase concreta, cuando, por ejemplo, un parámetro de un método acepta cualquier objeto que siga cierta interfaz.

# 🙂 ****Solución****

El patrón Prototype delega el proceso de clonación a los propios objetos que están siendo clonados. El patrón declara una interfaz común para todos los objetos que soportan la clonación. Esta interfaz nos permite clonar un objeto sin acoplar el código a la clase de ese objeto. Normalmente, dicha interfaz contiene un único método `clonar`.

La implementación del método `clonar` es muy parecida en todas las clases. El método crea un objeto a partir de la clase actual y lleva todos los valores de campo del viejo objeto, al nuevo. Se puede incluso copiar campos privados, porque la mayoría de los lenguajes de programación permite a los objetos acceder a campos privados de otros objetos que pertenecen a la misma clase.

Un objeto que soporta la clonación se denomina *prototipo*. Cuando tus objetos tienen decenas de campos y miles de configuraciones posibles, la clonación puede servir como alternativa a la creación de subclases.

![Untitled](Prototype%20Pattern%20053504eac4804ac098dd390230d45a6a/Untitled%202.png)

Funciona así: se crea un grupo de objetos configurados de maneras diferentes. Cuando necesites un objeto como el que has configurado, clonas un prototipo en lugar de construir un nuevo objeto desde cero.

# ****Analogía del mundo real****

En la vida real, los prototipos se utilizan para realizar pruebas de todo tipo antes de comenzar con la producción en masa de un producto. Sin embargo, en este caso, los prototipos no forman parte de una producción real, sino que juegan un papel pasivo.

![Untitled](Prototype%20Pattern%20053504eac4804ac098dd390230d45a6a/Untitled%203.png)

Ya que los prototipos industriales en realidad no se copian a sí mismos, una analogía más precisa del patrón es el proceso de la división mitótica de una célula (biología, ¿recuerdas?). Tras la división mitótica, se forma un par de células idénticas. La célula original actúa como prototipo y asume un papel activo en la creación de la copia.

# ****Estructura****

## ****Implementación básica****

![Untitled](Prototype%20Pattern%20053504eac4804ac098dd390230d45a6a/Untitled%204.png)

## ****Implementación del registro de prototipos****

![Untitled](Prototype%20Pattern%20053504eac4804ac098dd390230d45a6a/Untitled%205.png)

# ****Aplicabilidad****

- **Utiliza el patrón Prototype cuando tu código no deba depender de las clases concretas de objetos que necesites copiar.**
- Esto sucede a menudo cuando tu código funciona con objetos pasados por código de terceras personas a través de una interfaz. Las clases concretas de estos objetos son desconocidas y no podrías depender de ellas aunque quisieras.
    
    El patrón Prototype proporciona al código cliente una interfaz general para trabajar con todos los objetos que soportan la clonación. Esta interfaz hace que el código cliente sea independiente de las clases concretas de los objetos que clona.
    
- **Utiliza el patrón cuando quieras reducir la cantidad de subclases que solo se diferencian en la forma en que inicializan sus respectivos objetos. Puede ser que alguien haya creado estas subclases para poder crear objetos con una configuración específica.**
- El patrón Prototype te permite utilizar como prototipos un grupo de objetos prefabricados, configurados de maneras diferentes.
    
    En lugar de instanciar una subclase que coincida con una configuración, el cliente puede, sencillamente, buscar el prototipo adecuado y clonarlo.
    

# ****Cómo implementarlo****

1. Crea la interfaz del prototipo y declara el método `clonar` en ella, o, simplemente, añade el método a todas las clases de una jerarquía de clase existente, si la tienes.
2. Una clase de prototipo debe definir el constructor alternativo que acepta un objeto de dicha clase como argumento. El constructor debe copiar los valores de todos los campos definidos en la clase del objeto que se le pasa a la instancia recién creada. Si deseas cambiar una subclase, debes invocar al constructor padre para permitir que la superclase gestione la clonación de sus campos privados.
    
    Si el lenguaje de programación que utilizas no soporta la sobrecarga de métodos, puedes definir un método especial para copiar la información del objeto. El constructor es el lugar más adecuado para hacerlo, porque entrega el objeto resultante justo después de invocar el operador `new`.
    
3. Normalmente, el método de clonación consiste en una sola línea que ejecuta un operador `new` con la versión prototípica del constructor. Observa que todas las clases deben sobreescribir explícitamente el método de clonación y utilizar su propio nombre de clase junto al operador `new`. De lo contrario, el método de clonación puede producir un objeto a partir de una clase madre.
4. Opcionalmente, puedes crear un registro de prototipos centralizado para almacenar un catálogo de prototipos de uso frecuente.
    
    Puedes implementar el registro como una nueva clase de fábrica o colocarlo en la clase base de prototipo con un método estático para buscar el prototipo. Este método debe buscar un prototipo con base en el criterio de búsqueda que el código cliente pase al método. El criterio puede ser una etiqueta tipo *string* o un grupo complejo de parámetros de búsqueda. Una vez encontrado el prototipo adecuado, el registro deberá clonarlo y devolver la copia al cliente.
    
    Por último, sustituye las llamadas directas a los constructores de las subclases por llamadas al método de fábrica del registro de prototipos.
    
    ## ****Pros****
    
    - Puedes clonar objetos sin acoplarlos a sus clases concretas.
    - Puedes evitar un código de inicialización repetido clonando prototipos prefabricados.
    - Puedes crear objetos complejos con más facilidad.
    - Obtienes una alternativa a la herencia al tratar con preajustes de configuración para objetos complejos.
    
    ## ****Contras****
    
    - Clonar objetos complejos con referencias circulares puede resultar complicado.
    
    ```tsx
    /**
     * The example class that has cloning ability. We'll see how the values of field
     * with different types will be cloned.
     */
    class Prototype {
        public primitive: any;
        public component: object;
        public circularReference: ComponentWithBackReference;
    
        public clone(): this {
            const clone = Object.create(this);
    
            clone.component = Object.create(this.component);
    
            // Cloning an object that has a nested object with backreference
            // requires special treatment. After the cloning is completed, the
            // nested object should point to the cloned object, instead of the
            // original object. Spread operator can be handy for this case.
            clone.circularReference = {
                ...this.circularReference,
                prototype: { ...this },
            };
    
            return clone;
        }
    }
    
    class ComponentWithBackReference {
        public prototype;
    
        constructor(prototype: Prototype) {
            this.prototype = prototype;
        }
    }
    
    /**
     * The client code.
     */
    function clientCode() {
        const p1 = new Prototype();
        p1.primitive = 245;
        p1.component = new Date();
        p1.circularReference = new ComponentWithBackReference(p1);
    
        const p2 = p1.clone();
        if (p1.primitive === p2.primitive) {
            console.log('Primitive field values have been carried over to a clone. Yay!');
        } else {
            console.log('Primitive field values have not been copied. Booo!');
        }
        if (p1.component === p2.component) {
            console.log('Simple component has not been cloned. Booo!');
        } else {
            console.log('Simple component has been cloned. Yay!');
        }
    
        if (p1.circularReference === p2.circularReference) {
            console.log('Component with back reference has not been cloned. Booo!');
        } else {
            console.log('Component with back reference has been cloned. Yay!');
        }
    
        if (p1.circularReference.prototype === p2.circularReference.prototype) {
            console.log('Component with back reference is linked to original object. Booo!');
        } else {
            console.log('Component with back reference is linked to the clone. Yay!');
        }
    }
    
    clientCode();
    ```
    
    ## Resultado de la ejecución
    
    ```tsx
    Primitive field values have been carried over to a clone. Yay!
    Simple component has been cloned. Yay!
    Component with back reference has been cloned. Yay!
    Component with back reference is linked to the clone. Yay!
    ```
    
    # **Patrones creacionales: [Prototype](https://es.wikipedia.org/wiki/Prototype_(patr%C3%B3n_de_dise%C3%B1o)) (Prototipo)**
    
    🌱 Una instancia completamente inicializada para ser copiada o clonada
    
    ![Untitled](Prototype%20Pattern%20053504eac4804ac098dd390230d45a6a/Untitled%206.png)
    
    > Este patrón resulta útil en escenarios donde es preciso abstraer la lógica que decide qué tipos de objetos utilizará una aplicación, de la lógica que luego usarán esos objetos en su ejecución. Los motivos de esta separación pueden ser variados, por ejemplo, puede ser que la aplicación deba basarse en alguna configuración o parámetro en tiempo de ejecución para decidir el tipo de objetos que se debe crear. En ese caso, la aplicación necesitará crear nuevos objetos a partir de modelos. Estos modelos, o prototipos, son clonados y el nuevo objeto será una copia exacta de los mismos, con el mismo estado. Como decimos, esto resulta interesante para crear, en tiempo de ejecución, copias de objetos concretos inicialmente fijados, o también cuando sólo existe un número pequeño de combinaciones diferentes de estado para las instancias de una clase.
    > 
    
    > Dicho de otro modo, este patrón propone la creación de distintas variantes de objetos que nuestra aplicación necesite, en el momento y contexto adecuado. Toda la lógica necesaria para la decisión sobre el tipo de objetos que usará la aplicación en su ejecución se hace independiente, de manera que el código que utiliza estos objetos solicitará una copia del objeto que necesite. En este contexto, una copia significa otra instancia del objeto. El único requisito que debe cumplir este objeto es suministrar la funcionalidad de clonarse.
    > 
    
    **Claves:**
    
    - Muy popular en JavaScript
    - En JavaScript podemos hacer uso de `Object.create()`, que internamente ya implementa este patrón.
    
    **Clonación simple con `Object.create()`**
    
    ```tsx
    const coche = {
      marca: "Seat",
      modelo: "Panda",
      antiguedad: 20,
      color: "azul",
      tipo: "turismo"
    };
    
    const clonCoche = Object.create(coche);
    console.log(`${clonCoche.marca} ${clonCoche.modelo}`);
    ```
    
    **Clonación compleja con `Object.create()`**
    
    ```tsx
    const coche = {
      marca: "Land Rover",
      modelo: "Santana Aníbal",
      antiguedad: 35,
      color: "Marrón tierra",
      tipo: "4x4",
      detalles: dameDetalles
    };
    
    const furgon = {
      taraMinima: 1200,
      cargaUtil: 768,
      volumenCarga: 4.5,
      detalles: detallesTecnicos
    };
    
    const conductor = {
      nombre: "Yo",
      apellido: "Mismo",
      experiencia: 10000,
      limite: 120,
      detalles() {
        console.log(`El conductor es ${this.nombre} ${this.apellido}. Con ${this.experiencia} horas de experiencia y una restricción a ${this.limite}Km/h.`);
      }
    };
    
    function dameDetalles(){
      console.log(`Tu coche es un ${this.marca} ${this.modelo} con ${this.antiguedad} años, clase ${this.tipo} y color ${this.color}`);
    };
    
    function detallesTecnicos(){
      console.warn(`Tu coche tiene una Tara mínima de ${this.taraMinima}. Carga útil de ${this.cargaUtil} y un volumen de carga de ${this.volumenCarga}m3`);
    };
    
    // Patrón de Prototype
    const miPickup = Object.create(coche, {
        'conductor': { value: conductor },
        'carga': { value: furgon}
      });
    
    miPickup.detalles();
    miPickup.carga.detalles();
    miPickup.conductor.detalles();
    console.log(`Es "coche" prototipo de "miPickup" ? ${coche.isPrototypeOf(miPickup)}`);
    console.log(`Es "conductor" prototipo de "miPickup" ? ${conductor.isPrototypeOf(miPickup)}`);
    console.log(`Es "furgon" prototipo de "miPickup" ? ${furgon.isPrototypeOf(miPickup)}`);
    ```
    
    **Sin `Object.create()`**
    
    ```tsx
    class constructorCoches {
      constructor(modelo, color) {
        this.marca = "Seat";
        this.modelo = modelo || "Ibiza";
        this.antiguedad = 20;
        this.color = color || "rojo";
        this.detalles = function (){
          console.log(`Tu coche es un ${this.marca} ${this.modelo} con ${this.antiguedad} años y color ${this.color}`);
        }
      }
    
      clone() {
          return new constructorCoches(this.modelo, this.marca);
      }
    }
    
    // Comprobaciones
    const cocheRojo = new constructorCoches();
    const otroCoche = constructorCoches.prototype.clone( "Azul" );
    console.log(`¿Es "cocheRojo" una instancia de "constructorCoches"? ${cocheRojo instanceof constructorCoches}`); //true
    console.log(`¿Es "otroCoche" una instancia de "constructorCoches"? ${otroCoche instanceof constructorCoches}`); //true
    ```