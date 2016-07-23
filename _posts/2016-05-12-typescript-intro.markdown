---
layout: post
title:  "Introducción a TypeScript"
date:   2016-05-12 12:32:10
categories: typescript
lvl: intro
comments: true
description: "TypeScript es un 'superset' de JavaScript que añade ciertas funcionalidades a nuestro código, como tipos de dato definidos e interfaces. Angular 2 está hecho en TypeScript, por lo tanto vale la pena aprender un poco. Esta guía está dirigida a personas que no han pasado por lenguajes como Java o C# (para ellos esto de TypeScript resulta muy familiar)."
image: https://cloud.githubusercontent.com/assets/7959823/15593025/0f57a2c8-236e-11e6-9670-eced2b4bc0cd.png
---


TypeScript es un 'superset' de JavaScript que añade ciertas funcionalidades a nuestro código como, tipos de dato definidos e interfaces. Angular 2 está hecho en TypeScript, por lo tanto vale la pena aprender un poco y ver de que se trata, lo más probable es que en el futuro cercano se popularice.

## Funcionamiento

Básicamente funciona así:

1. Creas un archivo de texto con extensión `ts` _miAplicacion.ts_
1. Escribes código en el formato de TypeScript dentro de tu archivo
1. Compilas tu archivo usando el compilador de TypeScript
1. Se genera un archivo con extensión `js` _miAplicacion.js_


## Setup

Existen [varios plugins para TypeSctipt](https://www.typescriptlang.org/index.html#download-links), en este ejemplo lo usaremos directamente desde node:

`$ npm install -g typescript`


### Ejecutando el compilador de TypeScript

Tras instalar TypeScript tendremos disponible en nuestra terminal el programa `tsc`, TypeScript Compiler, con el comando:

`$ tsc nombreDelArchivo.ts --watch`

Le decimos a TypeScript Compiler que compile `nombreDelArchivo.ts` a `nombreDelArchivo.js`. Así mismo con el parámetro `--watch` le decimos que esté atento a cualquier cambio y realice la compilación en cada caso.


## Tipos

Empecemos por lo más _Type_ de TypeScript, _Strong Typing_ o "tipado fuerte" (o estricto):

Si tenemos la siguiente función en JavaScript de toda la vida:


```js
let alertar = (mensaje) => {
  alert(mensaje);
}
```


TypeScript nos permite definir tipos estrictos:

Podemos llamar a la función `alertar` y pasar cualquier valor como mensaje. Sin embargo, tiene más sentido que la función reciba únicamente cadenas de texto (_strings_):


```js
let alertar = (mensaje: string) => {
  alert(mensaje);
}
```


Y eso es todo. Como te podrás imaginar si se quieren otros tipos de datos como números o valores _booleanos_ tan solo hay que reemplazarlo sobre el _string_ que usamos:


```js
let alertar = (mensaje: number) => {
  alert(mensaje);
}

// o

let alertar = (mensaje: boolean) => {
  alert(mensaje);
}
```


Incluso podemos definir valores con _union types_ para hacer que estos reciban más de un solo tipo de dato:


```js
let alertar = (mensaje: string | number) => {
  alert(mensaje);
}
```

Podemos ser incluso más flexibles y definir valores con el tipo _any_;


```js
let alertar = (mensaje: any) => {
  alert(mensaje);
}
```


De esta forma volvemos al ejemplo inicial con JavaScript de toda la vida en donde nuestra función recibe cualquier tipo de dato.


## Interfaces

Una _interfaz_ nos permite modelar la forma de un objeto:


```js
interface Zapato {
  marca: string,
  numero: number,
  deprotivo: boolean
}
```


Ahora podemos definir un objeto y especificar que debe tener la forma de `Zapato`, es decir, **extender** la _interfaz_.


Usando el ejemplo anterior con la función `alertar` limitamos el tipo del mensaje para validar que concuerde con la estructura que se define en la _interfaz_ `Zapato`:


```js
interface Zapato {
  marca: string,
  numero: number,
  deprotivo: boolean
}

let alertar = (mensaje: Zapato) => {
  alert(mensaje);
}

alertar({marca: 'Patito', numero: 42, deprotivo: true});
```


Si pasaramos un argumento de más o nos faltara uno, TypeScript nos lo hará saber con un error.


Podemos tener propiedades opcionales dentro de una _interfaz_:


```js
interface ITelefono {
  marca: string,
  precio: number,
  nfc: boolean,
  ranurasParaSIMCard?: number
}
```


Aquí podemos pasar un número en la propiedad `ranurasParaSIMCard` o no pasar un valor en absoluto. Algo a notar también es que en este ejemplo definí la _interfaz_ como `I` `NombreDeLaInterfaz`, esta nomenclatura suele encontrarse en Internet y es eso, sólo una forma de escribir los nombres de las interfaces.

Finalmente, cabe notar que la _interfaz_ por sí sola no hace nada en nuestro código final (js), es únicamente una restricción que establecemos durante el desarrollo de nuestro código TypeScript. Si intentaramos compilar la definición de la _interfaz_ terminaríamos con un archivo JavaScript vacío.


## Ejemplo

Ahora sí llegamos al ejemplo que tanto esperabas (🙄): código que integra lo que hemos visto hasta ahora. Este ejemplo usa el concepto de clases, si no lo tienes muy claro puedes visitar la [documentación de Mozilla para este tema](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Classes).

```js
interface IPokemon {
    nombre: string,
    sonido: string,
    dormido?: boolean
}

class Pokemon {
  
  pokemon;
    
  get (){
    return this.pokemon;
  }

  set (nuevoPokemon){
    this.pokemon = nuevoPokemon;
  }
  
  hacerHablar() {
    if (this.pokemon.dormido){
      console.log('Zzzzz');
    }else{
      console.log(this.pokemon.sonido);
    }
  }

  constructor(nuevoPokemon: IPokemon) {
    this.set(nuevoPokemon);
  }
}


let pikachu = new Pokemon({ nombre: "Pikachu", sonido: "¡Pika pika!" });

pikachu.hacerHablar();

let snorlax = new Pokemon({ nombre: "Snorlax", sonido: "Snoooor-laax", dormido: true });

snorlax.hacerHablar();
```

1. Definimos una _interfaz_ con una propiedad opcional
1. Definimos una clase que tiene:
    1. Una referencia a un objeto _principal_
    1. Un método _getter_ para consultar nuestro objeto _principal_
    1. Un método para escribir nuestro objeto _principal_
    1. Un método para ejecutar una acción
    1. Un constructor que:
        1. Recibe como parámetro un objeto que debe tener la forma especificada por la _interfaz_
        1. Invoca al método _set_ para escribir el objeto _principal_
1. Una instancia de la clase
1. La invocación del método destinado a ejecutar una acción
1. Una instancia de la clase
1. La invocación del método destinado a ejecutar una acción


Repo: [https://github.com/UnJavaScripter/typescript-basics](https://github.com/UnJavaScripter/typescript-basics)