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
