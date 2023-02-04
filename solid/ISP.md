# El principio de segregación de la interfaz ISP

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
