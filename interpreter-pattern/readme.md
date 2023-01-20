# Interpreter Pattern (básico)

---

**El poder del patrón de diseño de intérprete en JavaScript**

![Untitled](Interpreter%20Pattern%20(ba%CC%81sico)%2027bf35f5669940d7bb2cefc0f9a68bc6/Untitled.png)

En esta publicación, repasaremos el patrón de diseño de intérprete en JavaScript. Implementaremos un Intérprete así como representaciones gramaticales básicas de un código fuente. Crearemos una interfaz para que la usen los códigos de los clientes (como analizadores, por ejemplo)

Los patrones de diseño se dividen en tres categorías: conductuales, creacionales y estructurales. El Intérprete pertenece al grupo de comportamiento. De todos los patrones, el intérprete parece ser el más confuso para las personas, pero en mi experiencia, pensar en una perspectiva de nivel superior (fuera de la caja) ayudará a que esa bombilla en nuestras mentes de repente ilumine todo.

## ¿Cuándo necesitamos aplicar el Patrón Intérprete?

A veces, podemos encontrarnos con situaciones en las que necesitamos alguna interfaz para decirle a un intérprete cómo interpretar en función de un contexto particular. Este patrón también se usa ampliamente en el análisis de SQL, motores de procesamiento de símbolos, etc.

Piense en ello como si creara un " *lenguaje* " de secuencias de comandos .

Por ejemplo, las matrices pueden contener más matrices en uno de sus índices que pueden contener más matrices, etc.:

```tsx
const items = [
  {
    profile: {
      username: 'bob',
      members: [
        {
          username: 'mike',
        },
        {
          username: 'sally123',
        },
        {
          username: 'panera',
          members: [
            {
              username: 'sonOfPanera',
            },
          ],
        },
      ],
    },
  },
]
```

El objetivo es crear algún tipo de *representación " gramática* " que en la mayoría de los casos sea capaz de *recursividad* .

Cuando observamos su estructura, cada parte de la representación gramatical es un compuesto o una hoja de un árbol. En una perspectiva de nivel superior, observe cómo estas representaciones gramaticales forman la estructura del patrón de diseño compuesto:

![Untitled](Interpreter%20Pattern%20(ba%CC%81sico)%2027bf35f5669940d7bb2cefc0f9a68bc6/Untitled%201.png)

Fíjate como hay algunos hijos que contienen más hijos y la profundidad aumenta tanto como se necesita. Aquí es donde la recursividad es crucial y se vuelve necesaria en los algoritmos transversales.

El principal participante en este patrón es el propio intérprete.

## Implementando las Representaciones de Intérprete y Gramática

Avancemos y creemos el intérprete. El intérprete tendrá un `interpret`método que se encargará de ejecutar nuestro código y producir tokens a partir de ellos. Estos tokens contendrán reglas que el intérprete necesitará para crear expresiones combinatorias para que el código del cliente, como los analizadores, las entienda:

Veamos la primera línea (iniciaremos esto como una matriz vacía para que sea más fácil de entender a medida que avanzamos):

```tsx
const items = []
```

Si tuviéramos que leer esta línea en el código, ¿cómo manipulamos el nombre `items`para `collection`?

Podemos leerlo línea por línea y hacerlo con algo como esto:

```tsx
let srcCode = `const items = [`

let prevChar
let nextChar

for (let index = 0; index < srcCode.length; index++) {
  nextChar = srcCode[index + 1]

  const char = srcCode[index]

  if (
    char === 'i' &&
    nextChar === 't' &&
    srcCode[index + 2] === 'e' &&
    srcCode[index + 3] === 'm' &&
    srcCode[index + 4] === 's'
  ) {
    srcCode = srcCode.split('')
    srcCode = [
      ...srcCode.slice(0, index),
      'collection',
      ...srcCode.slice(index + 'items'.length),
    ].join('')
  }

  prevChar = char
}
```

Sin embargo, esto no es muy eficiente porque no hay una manera confiable de ver los índices anteriores/siguientes, así como de saber qué línea o columna comienza con ciertas expresiones, declaraciones, etc. Tampoco tenemos una manera para hacer cosas útiles como poder actualizar el tipo de declarador ( `var`, `let`, `const`).

Cuando utilizamos el patrón Intérprete, creamos una interfaz real que tomará ese código, conectará objetos relacionados entre sí que representan partes de la gramática en consecuencia e instruirá al intérprete sobre qué hacer con ellos (recuerde, cada uno de esos objetos tiene sus propias reglas que guían al intérprete) y producen una salida que puede ser entendida por el código del cliente como los analizadores.

Entonces, volviendo a esta línea:

```tsx
const items = []
```

En su lugar, podemos crear una interfaz. Ahora tenga en cuenta que esta interfaz es la parte más poderosa de este patrón, ya que podemos personalizar cada clase para hacer lo que queramos, como anular el `toString`método predeterminado:

```tsx
class VariableDeclaration {
  constructor() {
    this.kind = null
    this.declarations = []
  }
  toString() {
    let output = ''

    output += `${this.kind} `

    this.declarations.forEach((declaration) => {
      output += declaration.toString()
    })

    return output
  }
}

class VariableDeclarator {
  constructor() {
    this.id = null
    this.init = null
  }
  interpret() {
    return this.init.interpret()
  }
  toString() {
    let output = ''
    output += this.id.toString()
    output += ' = '
    output += this.init.toString()
    return output
  }
}

class ArrayExpression {
  constructor() {
    this.elements = []
  }
  toString() {
    let output = ''

    output += '['

    this.elements.forEach((elem) => {
      output += elem.toString()
    })

    output += ']'

    return output
  }
}

class Identifier {
  constructor(name) {
    this.name = name
  }
  toString() {
    return this.name
  }
}
```

¡Perfecto! ¡Este es el patrón de diseño de intérprete en acción! Ahora tenemos algunas representaciones para nuestra gramática.

Este patrón es *muy poderoso* . Con esta interfaz en su lugar, podemos ver que sobrescribimos todos los `toString`métodos predeterminados en cada clase a `structure for our domain`.

A continuación, necesitamos un intérprete para tomar el código fuente y leerlo. Nuestro intérprete será una versión mucho más simplificada de los que se utilizan en la práctica real. Por el bien de esta publicación, solo incluí las partes necesarias para representar completamente nuestro trazador de líneas:

```tsx
class Interpreter {
  interpret(srcCode = '') {
    let nodes = []
    let words = srcCode.split(/(\s|\r|\n|\t\|=|\.|\]|\[)/)

    for (let index = 0; index < words.length; index++) {
      const word = words[index]
      if (/var|let|const/.test(word)) {
        const kind = ['var', 'let', 'const'].find((char) => word.includes(char))
        const variableName = words[index + 2]

        words.shift()
        words.shift()
        words.shift()
        words.shift()
        words.shift()
        words.shift()
        words.shift()

        const declaration = new VariableDeclaration()
        const declarator = new VariableDeclarator()
        const variable = new Identifier(variableName)

        if (words[0] === '[') {
          declarator.init = new ArrayExpression()
        }

        declaration.kind = kind
        declaration.declarations = [declarator]
        declarator.id = variable

        nodes.push(declaration)
      }
    }

    return nodes
  }

  toString(nodes) {
    let output = ''

    for (const node of nodes) {
      output += node.toString()
    }

    return output
  }
}
```

Con esto en su lugar, el código del cliente puede utilizar nuestro intérprete y manipular nuestra línea de código:

```tsx
const interpreter = new Interpreter()
const interpreted = interpreter.interpret(srcCode)
const newCode = interpreter.toString(interpreted) // `const items = []`
```

```tsx
const interpreter = new Interpreter()
const interpreted = interpreter.interpret(srcCode)

if (interpreted[0] instanceof VariableDeclaration) {
  interpreted[0].declarations[0].id.name = 'collection'
}

const newCode = interpreter.toString(interpreted) // `const collection = []`
```

## Contexto

Los objetos de contexto se usan comúnmente en combinación con los intérpretes. Si podemos anular `toString`todos nuestros objetos de representaciones gramaticales, definitivamente podemos mejorar el poder de nuestro intérprete ingresando información de estado útil en un objeto llamado `context`.

Por ejemplo, podemos darle al código del cliente la capacidad de omitir `undefined`valores al construir salidas de expresión de matriz:

![Untitled](Interpreter%20Pattern%20(ba%CC%81sico)%2027bf35f5669940d7bb2cefc0f9a68bc6/Untitled%202.png)

![Untitled](Interpreter%20Pattern%20(ba%CC%81sico)%2027bf35f5669940d7bb2cefc0f9a68bc6/Untitled%203.png)

## Consejos con otros patrones

- *El patrón Intérprete es más potente cuando se trabaja con estructuras compuestas. En otras palabras, combinar el patrón compuesto con el patrón intérprete es imprescindible cuando se trata de estructuras compuestas.*
- *En el patrón de constructor, se puede usar un constructor para crear estructuras jerárquicas como representaciones gramaticales de un idioma y permitir que el cliente las use con el patrón de intérprete.*
- *La fábrica abstracta se puede usar para crear objetos complejos (por ejemplo, en javascript, esta puede ser una declaración de devolución de función muy larga con muchas expresiones binarias)*
- *Al igual que el patrón compuesto, el patrón de visitante e iterador también es poderoso con el intérprete. Los visitantes a menudo se implementan con un algoritmo transversal recursivo que implementa el patrón iterador en sí mismo. Los intérpretes pueden utilizar a los visitantes para atravesar árboles compuestos.*

![Untitled](Interpreter%20Pattern%20(ba%CC%81sico)%2027bf35f5669940d7bb2cefc0f9a68bc6/Untitled%204.png)