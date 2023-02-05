
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
