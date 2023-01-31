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
