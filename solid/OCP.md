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
