# Facade Pattern

# ****Propósito****

**Facade** es un patrón de diseño estructural que proporciona una interfaz simplificada a una biblioteca, un framework o cualquier otro grupo complejo de clases.

![Untitled](Facade%20Pattern%204154ebf601de4ef49e6783de5403b020/Untitled.png)

# 😟 ****Problema****

Imagina que debes lograr que tu código trabaje con un amplio grupo de objetos que pertenecen a una sofisticada biblioteca o *framework*. Normalmente, debes inicializar todos esos objetos, llevar un registro de las dependencias, ejecutar los métodos en el orden correcto y así sucesivamente.

Como resultado, la lógica de negocio de tus clases se vería estrechamente acoplada a los detalles de implementación de las clases de terceros, haciéndola difícil de comprender y mantener.

# 🙂 Solución

Una fachada es una clase que proporciona una interfaz simple a un subsistema complejo que contiene muchas partes móviles. Una fachada puede proporcionar una funcionalidad limitada en comparación con trabajar directamente con el subsistema. Sin embargo, tan solo incluye las funciones realmente importantes para los clientes.

Tener una fachada resulta útil cuando tienes que integrar tu aplicación con una biblioteca sofisticada con decenas de funciones, de la cual sólo necesitas una pequeña parte.

Por ejemplo, una aplicación que sube breves vídeos divertidos de gatos a las redes sociales, podría potencialmente utilizar una biblioteca de conversión de vídeo profesional. Sin embargo, lo único que necesita en realidad es una clase con el método simple `codificar(nombreDelArchivo, formato)`. Una vez que crees dicha clase y la conectes con la biblioteca de conversión de vídeo, tendrás tu primera fachada.

# 🚙 ****Analogía en el mundo real****

![Untitled](Facade%20Pattern%204154ebf601de4ef49e6783de5403b020/Untitled%201.png)

Cuando llamas a una tienda para hacer un pedido por teléfono, un operador es tu fachada a todos los servicios y departamentos de la tienda. El operador te proporciona una sencilla interfaz de voz al sistema de pedidos, pasarelas de pago y varios servicios de entrega.

# ****Estructura****

![Untitled](Facade%20Pattern%204154ebf601de4ef49e6783de5403b020/Untitled%202.png)

1. El patrón **Facade** proporciona un práctico acceso a una parte específica de la funcionalidad del subsistema. Sabe a dónde dirigir la petición del cliente y cómo operar todas las partes móviles.
2. Puede crearse una clase **Fachada Adicional** para evitar contaminar una única fachada con funciones no relacionadas que podrían convertirla en otra estructura compleja. Las fachadas adicionales pueden utilizarse por clientes y por otras fachadas.
3. El **Subsistema Complejo** consiste en decenas de objetos diversos. Para lograr que todos hagan algo significativo, debes profundizar en los detalles de implementación del subsistema, que pueden incluir inicializar objetos en el orden correcto y suministrarles datos en el formato adecuado. Las clases del subsistema no conocen la existencia de la fachada. Operan dentro del sistema y trabajan entre sí directamente.
4. El **Cliente** utiliza la fachada en lugar de invocar directamente los objetos del subsistema.
    
    # ****Pseudocódigo****
    
    En este ejemplo, el patrón **Facade** simplifica la interacción con un framework complejo de conversión de vídeo.
    
    ![Untitled](Facade%20Pattern%204154ebf601de4ef49e6783de5403b020/Untitled%203.png)
    
    *Un ejemplo de aislamiento de múltiples dependencias dentro de una única clase fachada.*
    
    En lugar de hacer que tu código trabaje con decenas de las clases del framework directamente, creas una clase fachada que encapsula esa funcionalidad y la esconde del resto del código. Esta estructura también te ayuda a minimizar el esfuerzo de actualizar a futuras versiones del framework o de sustituirlo por otro. Lo único que tendrías que cambiar en la aplicación es la implementación de los métodos de la fachada.
    
    ```jsx
    // Estas son algunas de las clases de un framework de conversión
    // de video de un tercero. No controlamos ese código, por lo que
    // no podemos simplificarlo.
    
    class VideoFile
    // ...
    
    class OggCompressionCodec
    // ...
    
    class MPEG4CompressionCodec
    // ...
    
    class CodecFactory
    // ...
    
    class BitrateReader
    // ...
    
    class AudioMixer
    // ...
    
    // Creamos una clase fachada para esconder la complejidad del
    // framework tras una interfaz simple. Es una solución de
    // equilibrio entre funcionalidad y simplicidad.
    class VideoConverter is
        method convert(filename, format):File is
            file = new VideoFile(filename)
            sourceCodec = (new CodecFactory).extract(file)
            if (format == "mp4")
                destinationCodec = new MPEG4CompressionCodec()
            else
                destinationCodec = new OggCompressionCodec()
            buffer = BitrateReader.read(filename, sourceCodec)
            result = BitrateReader.convert(buffer, destinationCodec)
            result = (new AudioMixer()).fix(result)
            return new File(result)
    
    // Las clases Application no dependen de un millón de clases
    // proporcionadas por el complejo framework. Además, si decides
    // cambiar los frameworks, sólo tendrás de volver a escribir la
    // clase fachada.
    class Application is
        method main() is
            convertor = new VideoConverter()
            mp4 = convertor.convert("funny-cats-video.ogg", "mp4")
            mp4.save()
    ```
    
    # ****Aplicabilidad****
    
    **Utiliza el patrón Facade cuando necesites una interfaz limitada pero directa a un subsistema complejo.**
    
    A menudo los subsistemas se vuelven más complejos con el tiempo. Incluso la aplicación de patrones de diseño suele conducir a la creación de un mayor número de clases. Un subsistema puede hacerse más flexible y más fácil de reutilizar en varios contextos, pero la cantidad de código de configuración que exige de un cliente, crece aún más. El patrón Facade intenta solucionar este problema proporcionando un atajo a las funciones más utilizadas del subsistema que mejor encajan con los requisitos del cliente.
    
    **Utiliza el patrón Facade cuando quieras estructurar un subsistema en capas.**
    
    Crea fachadas para definir puntos de entrada a cada nivel de un subsistema. Puedes reducir el acoplamiento entre varios subsistemas exigiéndoles que se comuniquen únicamente mediante fachadas.
    
    Por ejemplo, regresemos a nuestro framework de conversión de vídeo. Puede dividirse en dos capas: la relacionada con el vídeo y la relacionada con el audio. Puedes crear una fachada para cada capa y hacer que las clases de cada una de ellas se comuniquen entre sí a través de esas fachadas. Este procedimiento es bastante similar al patrón **[Mediator](https://refactoring.guru/es/design-patterns/mediator)**.
    
    # ****Cómo implementarlo****
    
    1. Comprueba si es posible proporcionar una interfaz más simple que la que está proporcionando un subsistema existente. Estás bien encaminado si esta interfaz hace que el código cliente sea independiente de muchas de las clases del subsistema.
    2. Declara e implementa esta interfaz en una nueva clase fachada. La fachada deberá redireccionar las llamadas desde el código cliente a los objetos adecuados del subsistema. La fachada deberá ser responsable de inicializar el subsistema y gestionar su ciclo de vida, a no ser que el código cliente ya lo haga.
    3. Para aprovechar el patrón al máximo, haz que todo el código cliente se comunique con el subsistema únicamente a través de la fachada. Ahora el código cliente está protegido de cualquier cambio en el código del subsistema. Por ejemplo, cuando se actualice un subsistema a una nueva versión, sólo tendrás que modificar el código de la fachada.
    4. Si la fachada se vuelve **[demasiado grande](https://refactoring.guru/es/smells/large-class)**, piensa en extraer parte de su comportamiento y colocarlo dentro de una nueva clase fachada refinada.
    
    # Pros
    
    Puedes aislar tu código de la complejidad de un subsistema.
    
    # Contras
    
    Una fachada puede convertirse en **[un objeto todopoderoso](https://refactoring.guru/es/antipatterns/god-object)** acoplado a todas las clases de una aplicación.
    
    # Ejemplo Typescript
    
    ```jsx
    /**
     * The Facade class provides a simple interface to the complex logic of one or
     * several subsystems. The Facade delegates the client requests to the
     * appropriate objects within the subsystem. The Facade is also responsible for
     * managing their lifecycle. All of this shields the client from the undesired
     * complexity of the subsystem.
     */
    class Facade {
        protected subsystem1: Subsystem1;
    
        protected subsystem2: Subsystem2;
    
        /**
         * Depending on your application's needs, you can provide the Facade with
         * existing subsystem objects or force the Facade to create them on its own.
         */
        constructor(subsystem1?: Subsystem1, subsystem2?: Subsystem2) {
            this.subsystem1 = subsystem1 || new Subsystem1();
            this.subsystem2 = subsystem2 || new Subsystem2();
        }
    
        /**
         * The Facade's methods are convenient shortcuts to the sophisticated
         * functionality of the subsystems. However, clients get only to a fraction
         * of a subsystem's capabilities.
         */
        public operation(): string {
            let result = 'Facade initializes subsystems:\n';
            result += this.subsystem1.operation1();
            result += this.subsystem2.operation1();
            result += 'Facade orders subsystems to perform the action:\n';
            result += this.subsystem1.operationN();
            result += this.subsystem2.operationZ();
    
            return result;
        }
    }
    
    /**
     * The Subsystem can accept requests either from the facade or client directly.
     * In any case, to the Subsystem, the Facade is yet another client, and it's not
     * a part of the Subsystem.
     */
    class Subsystem1 {
        public operation1(): string {
            return 'Subsystem1: Ready!\n';
        }
    
        // ...
    
        public operationN(): string {
            return 'Subsystem1: Go!\n';
        }
    }
    
    /**
     * Some facades can work with multiple subsystems at the same time.
     */
    class Subsystem2 {
        public operation1(): string {
            return 'Subsystem2: Get ready!\n';
        }
    
        // ...
    
        public operationZ(): string {
            return 'Subsystem2: Fire!';
        }
    }
    
    /**
     * The client code works with complex subsystems through a simple interface
     * provided by the Facade. When a facade manages the lifecycle of the subsystem,
     * the client might not even know about the existence of the subsystem. This
     * approach lets you keep the complexity under control.
     */
    function clientCode(facade: Facade) {
        // ...
    
        console.log(facade.operation());
    
        // ...
    }
    
    /**
     * The client code may have some of the subsystem's objects already created. In
     * this case, it might be worthwhile to initialize the Facade with these objects
     * instead of letting the Facade create new instances.
     */
    const subsystem1 = new Subsystem1();
    const subsystem2 = new Subsystem2();
    const facade = new Facade(subsystem1, subsystem2);
    clientCode(facade);
    ```
    
    # Salida
    
    ```jsx
    Facade initializes subsystems:
    Subsystem1: Ready!
    Subsystem2: Get ready!
    Facade orders subsystems to perform the action:
    Subsystem1: Go!
    Subsystem2: Fire!
    ```
    
    # Ejemplo Javascript Facade Pattern
    
    Al crear una aplicación, a menudo nos enfrentamos a problemas con las API externas. Uno tiene métodos simples, otro los tiene muy complicados. Unificarlos bajo una interfaz común es uno de los usos del patrón de fachada.
    
    Imaginemos que estamos creando una aplicación que muestra información sobre películas, programas de televisión, música y libros. Para cada uno de estos tenemos un proveedor diferente. Se implementan utilizando varios métodos, tienen varios requisitos, etc. Tenemos que recordar o tener en cuenta cómo consultar cada tipo.
    
    ¿O nosotros?
    
    El patrón de fachada resuelve tales problemas. Esta es una interfaz común que tiene los mismos métodos sin importar lo que se use debajo.
    
    He preparado cuatro implementaciones diferentes de un servicio de recursos:
    
    ```jsx
    class FetchMusic {
      get resources() {
        return [
          { id: 1, title: "The Fragile" },
          { id: 2, title: "Alladin Sane" },
          { id: 3, title: "OK Computer" }
        ];
      }
    
      fetch(id) {
        return this.resources.find(item => item.id === id);
      }
    }
    
    class GetMovie {
      constructor(id) {
        return this.resources.find(item => item.id === id);
      }
    
      get resources() {
        return [
          { id: 1, title: "Apocalypse Now" },
          { id: 2, title: "Die Hard" },
          { id: 3, title: "Big Lebowski" }
        ];
      }
    }
    
    const getTvShow = function(id) {
      const resources = [
        { id: 1, title: "Twin Peaks" },
        { id: 2, title: "Luther" },
        { id: 3, title: "The Simpsons" }
      ];
    
      return resources.find(item => item.id === 1);
    };
    
    const booksResource = [
      { id: 1, title: "Ulysses" },
      { id: 2, title: "Ham on Rye" },
      { id: 3, title: "Quicksilver" }
    ];
    ```
    
    Se nombran usando diferentes patrones, se implementan mejor, peor, requieren más o menos trabajo. Como no quería complicarme demasiado, usé ejemplos simples con un formato de respuesta común. Sin embargo, esto ilustra bien el problema.
    
    # **Diseño de nuestra fachada**
    
    Para crear una fachada, lo primero que necesitamos es conocer todos los aspectos de cada proveedor. Si se requiere una autorización adicional, más parámetros, etc., esto debe implementarse. Este es un extra y se puede descartar cuando se usa con un proveedor que no lo necesita.
    
    **El componente básico de una fachada es la interfaz común** . Independientemente del recurso que desee consultar, solo debe utilizar un método. Por supuesto, debajo puede haber más, pero el acceso público debe ser limitado y fácil de usar.
    
    Primero, debemos decidir la forma de la API pública. Para este ejemplo, un solo captador debería ser suficiente. La única distinción aquí es el tipo de medio: libro, película, etc. Así que el tipo será nuestra base.
    
    A continuación, las cosas comunes entre los recursos. Cada uno es consultable por ID. Entonces, nuestro getter debería aceptar un parámetro, una ID.
    
    # **Construyendo nuestra fachada**
    
    He decidido usar una clase para esto, pero esto no es un requisito. El módulo que consiste en un objeto literal o incluso una colección de funciones probablemente sería suficiente. Sin embargo, me gusta esta notación
    
    ```jsx
    class CultureFasade {
      constructor(type) {
        this.type = type;
      }
    }
    ```
    
    Para empezar, definimos el tipo en el constructor. Esto significa que cada una de las instancias de fachada devolverá una diferente. Sé que esto puede parecer redundante, pero es más conveniente que usar una sola instancia de función y pasar más argumentos cada vez.
    
    Bien, lo siguiente es definir nuestros métodos públicos y privados. Para señalar los "privados", usé el famoso `_`en lugar del `#`, porque CodePen aún no lo admite.
    
    Como dijimos anteriormente, el único método público debería ser nuestro captador.
    
    ```jsx
    class CultureFacade {
      constructor(type) {
        this.type = type;
      }
    
      get(id) {
        return id;
      }
    }
    ```
    
    La implementación base (un esqueleto) está ahí. Ahora, pasemos a la *carne* real de nuestra clase: captadores privados.
    
    En primer lugar, debemos identificar cómo se consulta cada recurso:
    
    - La música requiere una nueva instancia y luego pasar una identificación dentro del método `get`;
    - Cada instancia de la película devuelve los datos, requiere ID durante la inicialización;
    - TV Show es solo una función única que acepta una identificación y devuelve los datos;
    - Los libros son solo un recurso, necesitamos consultarlo nosotros mismos.
    
    Sé que este paso parecía tedioso e innecesario, pero tenga en cuenta que ahora realmente no tenemos que resolver nada. **La fase conceptual es muy importante durante el proceso de diseño y construcción** .
    
    Está bien, música, vamos.
    
    ```jsx
    class CultureFacade {
      ...
    
      _findMusic(id) {
        const db = new FetchMusic();
        return db.fetch(id);
      }
    }
    ```
    
    Hemos creado un método simple que hace exactamente lo que describimos anteriormente. Los tres restantes serán solo un trámite.
    
    ```jsx
    class CultureFacade {
      ...
    
      _findMusic(id) {
        const db = new FetchMusic();
        return db.fetch(id);
      }
    
      _findMovie(id) {
        return new GetMovie(id);
      }
    
      _findTVShow(id) {
        return getTvShow(id);
      }
    
      _findBook(id) {
        return booksResource.find(item => item.id === id);
      }
    }
    ```
    
    Ahí, ahora tenemos todos los métodos para consultar nuestras bases de datos.
    
    # **Obtener la API pública**
    
    Una de las cosas más importantes que he aprendido cuando trabajo como programador es nunca depender de sus proveedores. Nunca sabes lo que puede pasar. Pueden ser atacados, cerrados, su empresa puede dejar de pagar por el servicio, etc.
    
    Sabiendo esto, nuestro getter también debería usar una especie de fachada. Debería *intentar* obtener los datos, sin asumir que tendrá éxito.
    
    Entonces, escribamos tal método.
    
    ```jsx
    class CultureFacade {
      ...
    
      get _error() {
        return { status: 404, error: `No item with this id found` };
      }
    
      _tryToReturn(func, id) {
        const result = func.call(this, id);
    
        return new Promise((ok, err) => !!result
          ? ok(result)
          : err(this._error));
      }
    }
    ```
    
    Detengámonos aquí por un minuto. Como puede ver, este método también es privado. ¿Por qué? El público no se beneficia de ello. Requiere el conocimiento de otros métodos privados. A continuación, requiere dos parámetros: `func`y `id`. Mientras que lo último es bastante obvio, lo primero no lo es. Bien, entonces esto aceptará una función (o más bien el método de nuestra clase) para ejecutar. Como puede ver, la ejecución se asigna a la `result`variable. A continuación, comprobamos si tuvo éxito y devolvemos un archivo `Promise`. ¿Por qué tal construcción barroca? Las promesas son muy fáciles de depurar y ejecutar, con la sintaxis `async/await`o incluso simple .`then/catch`
    
    Ah, y el error. Nada grande, solo un captador que devuelve un mensaje. Esto puede ser más elaborado, tiene más información, etc. No implementé nada sofisticado, ya que esto realmente no lo requiere, y nuestros proveedores tampoco tienen ningún error en el que basarse.
    
    Bien, entonces, ¿qué tenemos ahora? Los métodos privados para consultar proveedores. Nuestra fachada interior para tratar de consultar. Y nuestro esqueleto captador público. Expandámoslo en un ser vivo.
    
    Dado que confiamos en tipos predefinidos, usaremos la `switch`declaración siempre tan poderosa.
    
    ```jsx
    class CultureFacade {
      constructor(type) {
        this.type = type;
      }
    
      get(id) {
        switch (this.type) {
          case "music": {
            return this._tryToReturn(this._findMusic, id);
          }
    
          case "movie": {
            return this._tryToReturn(this._findMovie, id);
          }
    
          case "tv": {
            return this._tryToReturn(this._findTVShow, id);
          }
    
          case "book": {
            return this._tryToReturn(this._findBook, id);
          }
    
          default: {
            throw new Error("No type set!");
          }
        }
      }
    }
    ```
    
    # **Una nota sobre la definición de tipos de cadena**
    
    Nuestros tipos están escritos a mano. Esta no es la mejor práctica. Debe definirse a un lado, por lo que ningún error tipográfico causará el error. Por qué no, hagámoslo.
    
    ```jsx
    const TYPE_MUSIC = "music";
    const TYPE_MOVIE = "movie";
    const TYPE_TV = "tv";
    const TYPE_BOOK = "book";
    
    class CultureFacade {
      constructor(type) {
        this.type = type;
      }
    
      get(id) {
        switch (this.type) {
          case TYPE_MUSIC: {
            return this._tryToReturn(this._findMusic, id);
          }
    
          case TYPE_MOVIE: {
            return this._tryToReturn(this._findMovie, id);
          }
    
          case TYPE_TV: {
            return this._tryToReturn(this._findTVShow, id);
          }
    
          case TYPE_BOOK: {
            return this._tryToReturn(this._findBook, id);
          }
    
          default: {
            throw new Error("No type set!");
          }
        }
      }
    }
    ```
    
    Estos tipos deben exportarse y luego usarse en toda la aplicación.
    
    # **Uso**
    
    Entonces, parece que hemos terminado aquí. ¡Vamos a darle una vuelta!
    
    ```jsx
    const music = new CultureFacade(TYPE_MUSIC);
    music.get(3)
        .then(data => console.log(data))
        .catch(e => console.error(e));
    ```
    
    Implementación muy simple usando `then/catch`. Simplemente cierra la sesión del álbum que estábamos buscando, *OK Computer* de Radiohead en este caso. Gran escucha, por cierto.
    
    Está bien, pero intentemos obtener un error también. Ninguno de nuestros proveedores realmente puede decir nada cuando no tiene el recurso solicitado. ¡Pero nosotros podemos!
    
    ```jsx
    const movies = new CultureFacade(TYPE_MOVIE);
    movie.get(5)
        .then(data => console.log(data))
        .catch(e => console.log(e));
    ```
    
    ¿Y qué tenemos aquí? Oh, la consola arroja un error que dice "No se encontró ningún elemento con esta identificación". En realidad, ¡es un objeto compatible con JSON! ¡Sí!
    
    ---
    
    El patrón de fachada, como puede ver, puede ser muy poderoso cuando se usa correctamente. Puede ser realmente beneficioso cuando tiene varias fuentes similares, operaciones similares, etc., y desea unificar el uso.