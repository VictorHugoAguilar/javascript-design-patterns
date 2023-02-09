# Principios S.O.L.I.D.

## Introducción

La invención de los principios SOLID comenzó a finales de los años 80. Robert C. Martin comenzó a desarrollar estos principios mientras defendía el principio del diseño de software en USENET (un tipo temprano de Facebook). Después de la suma y la resta, Robert C. Martin formuló los principios a principios de la década de 2000. Fue hasta el año 2004 que se ordenaron los principios y se denominaron principios SÓLIDOS. Es un acrónimo que representa cinco principios de diseño específicos.

- **S** representa el principio de Responsabilidad Única [(SRP)](https://github.com/VictorHugoAguilar/javascript-design-patterns/blob/main/solid/SRP.md)	
- **O** representa el principio Abierto Cerrado [(OCP)](https://github.com/VictorHugoAguilar/javascript-design-patterns/blob/main/solid/OCP.md)
- **L** representa el principio de sustitución de Liskov [(Liskov)](https://github.com/VictorHugoAguilar/javascript-design-patterns/blob/main/solid/LISKOV.md)
- **I** representa el principio de segregación de interfaz [(ISP)](https://github.com/VictorHugoAguilar/javascript-design-patterns/blob/main/solid/ISP.md)
- **D** representa el principio de inversión de dependencia [(DIP)](https://github.com/VictorHugoAguilar/javascript-design-patterns/blob/main/solid/DIP.md)

Los principios SOLID son útiles cuando se construyen módulos individuales o arquitecturas más grandes. Entonces, vamos a explorar cada principio junto con ejemplos en JavaScript.

# Otra mirada a los principios SOLID

Los principios SOLID están estrictamente relacionados con **los patrones de diseño** . Es importante conocer los patrones de diseño porque es un tema candente para una entrevista. Si los conoce, comprenderá fácilmente los paradigmas de programación más sofisticados, los patrones arquitectónicos y las características del lenguaje, como la *programación reactiva* , la *arquitectura de flujo (Redux)* , los *generadores en JavaScript* , etc.

## ¿Qué son los principios SOLID?

**SÓLIDO** significa

- **S — Principio de responsabilidad única**
- **O — Principio cerrado abierto**
- **L — Principio de sustitución de Liskov**
- **I — Principio de segregación de interfaces**
- **D — Principio de inversión de dependencia**

Estos 5 principios lo guiarán sobre cómo escribir un mejor código. Aunque provienen de la programación orientada a objetos. Sé que es muy atrevido llamar a JavaScript un lenguaje orientado a objetos :) Independientemente, prometo que si comprende estos principios, cuando diseñe sus próximas soluciones, definitivamente se preguntará: "Oye, ¿estoy violando el principio de responsabilidad única ?".

Vamos a empezar

## **S — Principio de responsabilidad única**

Es probablemente el principio más fácil y, al mismo tiempo, el más incomprendido.

> Un módulo debe ser responsable de un solo actor. En consecuencia, sólo tiene una razón para cambiar
> 

### Ejemplo

Echemos un vistazo al siguiente código:

```tsx
class TodoList {
  constructor() {
    this.items = []
  }

  addItem(text) {
    this.items.push(text)
  }

  removeItem(index) {
    this.items = items.splice(index, 1)
  }

  toString() {
    return this.items.toString()
  }

  save(filename) {
    fs.writeFileSync(filename, this.toString())
  }

  load(filename) {
    // Some implementation
  }
}
```

Ups. Aunque a primera vista, esta clase parece estar bien, viola el principio de responsabilidad única. Agregamos una segunda responsabilidad a nuestra clase TodoList, que es la gestión de nuestra base de datos.

Arreglemos el código para que cumpla con el principio "S".

```tsx
class TodoList {
  constructor() {
    this.items = []
  }

  addItem(text) {
    this.items.push(text)
  }

  removeItem(index) {
    this.items = items.splice(index, 1)
  }

  toString() {
    return this.items.toString()
  }
}

class DatabaseManager {
  saveToFile(data, filename) {
    fs.writeFileSync(filename, data.toString())
  }

  loadFromFile(filename) {
    // Some implementation
  }
}
```

Por lo tanto, nuestro código se ha vuelto más escalable. Por supuesto, no es tan obvio cuando buscamos soluciones pequeñas. Cuando se aplica a una arquitectura compleja, este principio adquiere mucho más significado.

# SOLID

---

# Introducción

La invención de los principios SOLID comenzó a finales de los años 80. Robert C. Martin comenzó a desarrollar estos principios mientras defendía el principio del diseño de software en USENET (un tipo temprano de Facebook). Después de la suma y la resta, Robert C. Martin formuló los principios a principios de la década de 2000. Fue hasta el año 2004 que se ordenaron los principios y se denominaron principios SÓLIDOS. Es un acrónimo que representa cinco principios de diseño específicos.

- **S** representa el principio de Responsabilidad Única
- **O** representa el principio Abierto Cerrado
- **L** representa el principio de sustitución de Liskov
- **I** representa el principio de segregación de interfaz
- **D** representa el principio de inversión de dependencia

Los principios SOLID son útiles cuando se construyen módulos individuales o arquitecturas más grandes. Entonces, vamos a explorar cada principio junto con ejemplos en JavaScript.

# El principio de responsabilidad única

Este principio fue descrito en el trabajo de Tom DeMarco¹ y Meilir Page-Jones². Lo llamaron cohesión. Definieron la cohesión como la relación funcional de los elementos de un módulo.

## ¿Qué falla con el SRP?

De hecho, este principio podría ser el menos entendido porque tiene un nombre particularmente inapropiado. Muchos desarrolladores entendieron que cada módulo debería hacer solo una cosa. No se equivoquen, hay un principio como ese. Pero no es uno de los principios SOLID y en realidad no es el SRP.

## Entonces, ¿qué es el SRP?

Se ha descrito de la siguiente manera: “ *Cada módulo de software tiene una, y sólo una, razón para cambiar* ”. Porque los sistemas de software se modifican para cumplir con las demandas de los usuarios, para satisfacer a las partes interesadas, por lo que podemos reformular el principio para decir esto: " *Cada módulo debe ser responsable de uno, y solo uno, usuario o parte interesada* " *.* Pero es probable que haya más de un usuario o parte interesada que quiera que el sistema cambie de la misma manera, los llamamos actor o grupo, por lo que la versión final es: “ *Cada módulo debe ser responsable de uno, y solo uno* ”. *, actor* ”.

## Miramos algunos ejemplos para entender qué es el SRS

Supongamos que tenemos un objeto Empleado, tiene tres funciones: calcularPago(), informarHoras() y guardar().

```tsx
function Employee(name, pos, hours){
	this.name = name;
	this.pos = pos;
	this.hours = hours;

	this.calculatePay = function() {
		...
	}
	this.reportHours = function() {
		...
	}
	this.save = function() {
		...
	}
}
```

Desafortunadamente, está violando el SRP porque esas tres funciones están a cargo de tres actores diferentes.

- La función de calcularPago() es responsable del departamento de contabilidad.
- El departamento de recursos humanos utiliza la función reportHours().
- La función save() la especifican los administradores de la base de datos.

Entonces, la forma de evitar este problema es separar el código que admite diferentes actores.

El objeto **EmployData** para guardar una estructura de datos simple compartida, utilizada por los tres actores.

```tsx
function EmployData(name, pos, hours){
	this.name = name;
	this.pos = pos;
	this.hours = hours;
	...
}
```

El objeto **PayCalculator** tiene el `calculatePay()` método.

```tsx
function PayCalculator(employData){
	this.employData = employData;
	this.calculatePay = function(){
		...
	}
}
```

El objeto **HourReporter** tiene el `reportHours()` método.

```tsx
function HourReporter(employData){
	this.emplyData = employData;
	this.reportHours = function() {
		...
	}
}
```

El objeto **EmployeeServer** tiene el `save()` método.

```tsx
function EmployeeServer(employData) {
	this.emplyData = employData;
	this.save = function(){
		...
	}
}
```

Así es como usamos el SRS para refactorizar códigos incorrectos. Cada función es responsable de un actor específico. El SRP es uno de los principios más simples y uno de los más difíciles de acertar.

# El principio abierto-cerrado

Bertrand Meyer hizo famoso este principio en la década de 1980, que apareció en su libro Object-Oriented Software Construction³. Los sistemas de software se diseñen para permitir que se cambie el comportamiento de esos sistemas agregando un nuevo código, en lugar de cambiar el código existente.

## ¿Qué es el principio abierto-cerrado (OCP)?

El OCP establece lo siguiente: " *Las entidades de software (clases, módulos, funciones, etc.) deben estar abiertas para la extensión, pero cerradas para la modificación"* por Meyer, Bertrand *.* Este principio nos aconseja refactorizar el sistema para que más cambios de ese tipo no causen más modificaciones. La mayoría de los desarrolladores reconocen el OCP como un principio que los guía en el diseño de clases y módulos.

Hay dos atributos principales en el OCP, son.

- Abierto para extensión: podemos extender lo que hace el módulo.
- Cerrado para modificaciones: extender el comportamiento de un módulo no genera cambios en el código fuente o binario del módulo.

Parecería que estos dos atributos están en desacuerdo porque la forma normal de extender el comportamiento de un módulo es hacer cambios en el código fuente de ese módulo.

## Entonces, ¿cómo implementar el OCP?

Supongamos que cada empleado tiene un rol y privilegios otorgados. Pero, ¿qué pasa si introducimos un nuevo rol en el sistema y no modificamos las cosas existentes? Entonces, podemos hacer como el siguiente ejemplo para que pase el OCP.{}

```tsx
let allowedRoles = ['ceo', 'cto', 'cfo', 'staff']

function checkPrivilege(employee) {
	if(allowedRoles.includes(employee.role){
		return true;
	}
	return false;
}

function addRoles(role){
	allowedRoles.push(role);
}
```

Entonces, como en el ejemplo anterior, no tenemos que modificar el código existente, sino que podemos extenderlo para agregar un nuevo rol. El OCP es uno de los motores de la arquitectura de sistemas. El objetivo es hacer que el sistema sea fácil de extender sin incurrir en un alto impacto de cambio.

# El principio de sustitución de Liskov

La famosa definición de subtipos de Barbara Liskov, de 1988 en un discurso de apertura de una conferencia titulado Data Abstraction and Hierarchy. En resumen, este principio dice que para construir sistemas de software a partir de partes intercambiables, esas partes deben adherirse a un contrato que permita que esas partes se sustituyan entre sí.

## Puedes entenderlo de la siguiente manera

Uno de los ejemplos clásicos de este principio es un rectángulo que tiene cuatro lados. La altura de un rectángulo puede ser cualquier valor y el ancho puede ser cualquier valor. Un cuadrado es un rectángulo con igual ancho y alto. Entonces podemos decir que podemos extender las propiedades de la clase rectángulo a la clase cuadrada. Para hacer eso, debe intercambiar la clase secundaria (cuadrado) con la clase principal (rectángulo) para que se ajuste a la definición de un cuadrado que tiene cuatro lados iguales, pero una clase derivada no afecta el comportamiento de la clase principal, así que si lo hace que violará el principio de sustitución de Liskov.

## ¿Ves lo que va mal?

Considere que tenemos una aplicación que usa un objeto rectángulo definido de la siguiente manera.

Basándonos en el conocimiento de que un cuadrado es un rectángulo cuyos lados tienen la misma longitud, decidimos crear un objeto cuadrado para usar en lugar de un rectángulo.

```tsx
var square = {};
(function () {
  var length = 0,
    width = 0;

  Object.defineProperties(square, "length", {
    get: function () {
      return length;
    },
    set: function () {
      length = width = value;
    },
  });

  Object.defineProperties(square, "width", {
    get: function () {
      return width;
    },
    set: function () {
      length = width = value;
    },
  });
})();
```

Desafortunadamente, se descubre un problema cuando la aplicación intenta usar nuestro cuadrado en lugar de un rectángulo. Resulta que uno de los métodos calcula el área del rectángulo así.

```tsx
const area = function(rectangle){
	return (rectangle.length * rectangle.width)
}
```

Cuando se invoca el método con un cuadrado, el producto es 16 en lugar del valor esperado de 12. Nuestro objeto cuadrado viola el principio de sustitución de Liskov con respecto a la función de área. En este caso, la presencia de las propiedades de largo y ancho fue una pista de que nuestro cuadrado podría no ser 100 % compatible con el rectángulo, pero no siempre tendremos pistas tan obvias.

# El principio de segregación de la interfaz

Este principio aconseja a los diseñadores de software que eviten depender de cosas que no usan.

## Tomando un ejemplo simple para entender el ISP

Supongamos que entras en un restaurante y eres vegetariano puro. El mesero en ese restaurante le dio la carta del menú que incluye artículos vegetarianos, artículos no vegetarianos, bebidas y dulces. En este caso, como cliente, debe tener una tarjeta de menú que incluya solo elementos vegetarianos, no todo lo que no come en su comida.

Aquí el menú debe ser diferente para diferentes tipos de clientes. La tarjeta de menú común o general para todos se puede dividir en varias tarjetas en lugar de solo una. El uso de este principio ayuda a reducir los efectos secundarios y la frecuencia de los cambios necesarios.

## ¿Cómo implementar el ISP en JavaScript?

Porque no tenemos una interfaz por defecto en JavaScript. Pero todos nos habríamos enfrentado a situaciones en las que queremos hacer muchas cosas en el constructor de una clase. Entonces, ¿cómo implementar el ISP ahora?

Digamos algunas configuraciones que tenemos que hacer en el constructor. Las configuraciones que hacemos deben separarse de las otras configuraciones no deseadas en el constructor.

```tsx
class User {
	constructor(uer){
		this.user = user
		this.initUser()
	}
	initUser(){
		this.name = this.user.name
		this.getMenu()
	}
}
const user = new User({userProperties, getMenu(){})
```

Aquí, la función validarUser() se invocará en la llamada del constructor initialUser() aunque no se necesite todo el tiempo. Podemos traer esto a ISP con el siguiente código.

```tsx
class User {
	constructor(uer){
		this.user = user
		this.initUser()
		this.setupOptions = user.options
	}
	initUser(){
		this.name = this.user.name
		this.setupOptions()
	}
}
const user = new User({userProperties, options: { getMenu()}{}})
```

Como el código anterior, estamos segregando la lógica no deseada de la función del contratista.

# El principio de inversión de dependencia

Antes de discutir este tema, tenga en cuenta que la inversión de dependencia y la inyección de dependencia son conceptos diferentes. La mayoría de las personas se confunden al respecto y consideran que ambos son iguales. Ahora los puntos clave están aquí para tener en cuenta acerca de este principio.

## No confundir con el principio de inyección de dependencia

El Principio de Inversión de Dependencia (DIP) nos dice que los sistemas más flexibles son aquellos en los que las dependencias del código fuente se refieren solo a abstracciones, no a concreciones. Más bien, los detalles deberían depender de las políticas.

## Mirando un ejemplo de la vida real

Puede considerar el ejemplo de la vida real de una batería remota de TV. Su control remoto necesita una batería, pero no depende de la marca de la batería. Puede usar cualquier marca XYZ que desee y funcionará. Entonces podemos decir que el control remoto del televisor está vagamente asociado con el nombre de la marca. La inversión de dependencia hace que su código sea más reutilizable.

## ¿Cómo implementar el DIP en JavaScript?

En un lenguaje de tipo estático, como Java, esto significa que las declaraciones de uso, importación e inclusión deben referirse solo a los módulos fuente que contienen interfaces, clases abstractas o algún otro tipo de declaración abstracta. No se debe depender de nada concreto. En caso de JavaScript ¿Cómo podemos implementar el DIP?

Tomemos un ejemplo simple para entender cómo podemos hacerlo fácilmente.

Quiero contactar al servidor para algunos datos. Sin aplicar DIP, esto podría tener el siguiente aspecto.

```tsx
$.get("/address/to/data", function(data){
	$("#thingy1).text(data.property1)
	$("#thingy2).text(data.property2)
})
```

Con DIP, podría escribir código como.

```tsx
fillFromServer( "/address/to/data" , thingyView)
```

donde la abstracción `fillFromServer.` La función puede, para el caso particular en el que queremos usar Ajax de jQuery, implementarse de la siguiente manera.

```tsx
function fillFromServer(url, view){
	$.get(url, function(data) {
		view.setValues(data)
	}
}
```

La vista de abstracción se puede implementar para el caso particular de una vista basada en elementos con ID **cosa1** y **cosa2** de la siguiente manera.

```tsx
var thinyView = 
	setValues: function(data){ 
		$("#thingy1).text(data.property1)
		$("#thingy2).text(data.property2)
})
```

Fácil, ¿verdad? Espero que esto le brinde una comprensión básica de cómo aplicar los principios SOLID en JavaScript.

# Otra mirada a los principios SOLID

Los principios SOLID están estrictamente relacionados con **los patrones de diseño** . Es importante conocer los patrones de diseño porque es un tema candente para una entrevista. Si los conoce, comprenderá fácilmente los paradigmas de programación más sofisticados, los patrones arquitectónicos y las características del lenguaje, como la *programación reactiva* , la *arquitectura de flujo (Redux)* , los *generadores en JavaScript* , etc.

## ¿Qué son los principios SOLID?

**SÓLIDO** significa

- **S — Principio de responsabilidad única**
- **O — Principio cerrado abierto**
- **L — Principio de sustitución de Liskov**
- **I — Principio de segregación de interfaces**
- **D — Principio de inversión de dependencia**

Estos 5 principios lo guiarán sobre cómo escribir un mejor código. Aunque provienen de la programación orientada a objetos. Sé que es muy atrevido llamar a JavaScript un lenguaje orientado a objetos :) Independientemente, prometo que si comprende estos principios, cuando diseñe sus próximas soluciones, definitivamente se preguntará: "Oye, ¿estoy violando el principio de responsabilidad única ?".

Vamos a empezar

## **S — Principio de responsabilidad única**

Es probablemente el principio más fácil y, al mismo tiempo, el más incomprendido.

> Un módulo debe ser responsable de un solo actor. En consecuencia, sólo tiene una razón para cambiar
> 

### Ejemplo

Echemos un vistazo al siguiente código:

```tsx
class TodoList {
  constructor() {
    this.items = []
  }

  addItem(text) {
    this.items.push(text)
  }

  removeItem(index) {
    this.items = items.splice(index, 1)
  }

  toString() {
    return this.items.toString()
  }

  save(filename) {
    fs.writeFileSync(filename, this.toString())
  }

  load(filename) {
    // Some implementation
  }
}
```

Ups. Aunque a primera vista, esta clase parece estar bien, viola el principio de responsabilidad única. Agregamos una segunda responsabilidad a nuestra clase TodoList, que es la gestión de nuestra base de datos.

Arreglemos el código para que cumpla con el principio "S".

```tsx
class TodoList {
  constructor() {
    this.items = []
  }

  addItem(text) {
    this.items.push(text)
  }

  removeItem(index) {
    this.items = items.splice(index, 1)
  }

  toString() {
    return this.items.toString()
  }
}

class DatabaseManager {
  saveToFile(data, filename) {
    fs.writeFileSync(filename, data.toString())
  }

  loadFromFile(filename) {
    // Some implementation
  }
}
```

Por lo tanto, nuestro código se ha vuelto más escalable. Por supuesto, no es tan obvio cuando buscamos soluciones pequeñas. Cuando se aplica a una arquitectura compleja, este principio adquiere mucho más significado.

## **O — Principio cerrado abierto**

> Los módulos deben estar abiertos para la extensión pero cerrados para la modificación
> 

Eso significa que si desea extender el comportamiento de un módulo, no necesitará modificar el código existente de ese módulo.

### Ejemplo

```tsx
class Coder {
  constructor(fullName, language, hobby, education, workplace, position) {
    this.fullName = fullName
    this.language = language
    this.hobby = hobby
    this.education = education
    this.workplace = workplace
    this.position = position
  }
}

class CoderFilter {
  filterByName(coders, fullName) {
    return coders.filter(coder => coder.fullName === fullName)
  }

  filterBySize(coders, language) {
    return coders.filter(coder => coder.language === language)
  }

  filterByHobby(coders, hobby) {
    return coders.filter(coder => coder.hobby === hobby)
  }
}
```

El problema `CoderFilter`es que si queremos filtrar por alguna otra propiedad nueva tenemos que cambiar `CodeFilter`el código. Resolvamos este problema creando una `filterByProp`función.

```tsx
const filterByProp = (array, propName, value) =>
  array.filter(element => element[propName] === value)
```

## **L — Principio de sustitución de Liskov**

Un principio con el nombre más confuso. ¿Qué significa?

> Si tiene una función que funciona para un tipo base, debería funcionar para un tipo derivado
> 

Vamos con un ejemplo clásico

### Ejemplo

```tsx
class Rectangle {
  constructor(width, height) {
    this._width = width
    this._height = height
  }

  get width() {
    return this._width
  }
  get height() {
    return this._height
  }

  set width(value) {
    this._width = value
  }
  set height(value) {
    this._height = value
  }

  getArea() {
    return this._width * this._height
  }
}

class Square extends Rectangle {
  constructor(size) {
    super(size, size)
  }
}

const square = new Square(2)
square.width = 3
console.log(square.getArea())
```

Adivina qué se imprimirá en la consola. Si tu respuesta es `6`, tienes razón. Por supuesto, la respuesta deseada es 9. Aquí podemos ver una violación clásica del principio de sustitución de Liskov.

Por cierto, para solucionar el problema se puede definir de `Square`esta manera:

```tsx
class Square extends Rectangle {
  constructor(size) {
    super(size, size)
  }

  set width(value) {
    this._width = this._height = value
  }

  set height(value) {
    this._width = this._height = value
  }
}
```

## **I — Principio de segregación de interfaces**

> No se debe obligar a los clientes a depender de interfaces que no utilizan
> 

No hay interfaces en JavaScript. Hay una manera de imitar su comportamiento, pero no creo que tenga mucho sentido. Adaptemos mejor el principio al mundo js.

### Ejemplo

Definamos una `Phone`clase "abstracta" que desempeñará el papel de la interfaz en nuestro caso:

```tsx
class Phone {
  constructor() {
    if (this.constructor.name === 'Phone')
      throw new Error('Phone class is absctract')
  }

  call(number) {}

  takePhoto() {}

  connectToWifi() {}
}
```

¿Podemos usarlo para definir un iPhone?

```tsx
class IPhone extends Phone {
  call(number) {
    // Implementation
  }

  takePhoto() {
    // Implementation
  }

  connectToWifi() {
    // Implementation
  }
}
```

De acuerdo, pero para un Nokia 3310 antiguo, esta interfaz violará el principio "I".

```tsx
class Nokia3310 extends Phone {
  call(number) {
    // Implementation
  }

  takePhoto() {
    // Argh, I don't have a camera
  }

  connectToWifi() {
    // Argh, I don't know what wifi is
  }
}
```

## **D — Principio de inversión de dependencia**

> Los módulos de alto nivel no deben depender de los módulos de bajo nivel
> 

Echemos un vistazo al siguiente ejemplo:

### Ejemplo

```tsx
class FileSystem {
  writeToFile(data) {
    // Implementation
  }
}

class ExternalDB {
  writeToDatabase(data) {
    // Implementation
  }
}

class LocalPersistance {
  push(data) {
    // Implementation
  }
}

class PersistanceManager {
  saveData(db, data) {
    if (db instanceof FileSystem) {
      db.writeToFile(data)
    }

    if (db instanceof ExternalDB) {
      db.writeToDatabase(data)
    }

    if (db instanceof LocalPersistance) {
      db.push(data)
    }
  }
}
```

En este caso, un módulo de alto nivel `PersistanceManager`depende de los módulos de bajo nivel, que son `FileSystem`, `ExternalDB`y `LocalPersistance`.

Para evitar el problema en este caso simple, probablemente deberíamos hacer algo como esto:

```tsx
class FileSystem {
  save(data) {
    // Implementation
  }
}

class ExternalDB {
  save(data) {
    // Implementation
  }
}

class LocalPersistance {
  save(data) {
    // Implementation
  }
}

class PersistanceManager {
  saveData(db, data) {
    db.save(data)
  }
}
```

Por supuesto, este es un ejemplo demasiado simplificado, pero entiendes el punto.

## Conclusión

El valor de los principios SOLID no es obvio. Pero si te preguntas "¿Estoy violando los principios SOLID" cuando diseñas tu arquitectura, te prometo que la calidad y la escalabilidad de tu código serán mucho mejores.
