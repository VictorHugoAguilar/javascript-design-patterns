# El principio de inversión de dependencia DIP

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
