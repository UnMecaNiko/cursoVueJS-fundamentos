# Curso de Vue.js - Fundamentos

Aprende los fundamentos de Vue.js, el framework progresivo. Conoce cómo funciona la reactividad dentro de Vue.js y cómo podemos usarlo para empezar a construir aplicaciones frontend con esta librería. Descubre las ventajas de Vue junto con tu profesora @nerddiana.

- Descubrir por qué Vue es “el framework progresivo”
- Aprender cómo Vue maneja los eventos
- Descubrir el renderizado declarativo
- Aprender cómo funciona la reactividad con Vue.js

Lo que verás a continuacion son mis notas del curso, si ves errores conceptuales o de redacción no dudes en cotribuir con tus aportes 💚

# Introducción

- [Chrome extension Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
- Comando para instalación: `npm install -g @vue/cli`

## Using Vue from CDN
You can use Vue directly from a CDN via a script tag:

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```
Vue.js devuelve en sí una aplicación con todos los parámetros necesarios, este es un framework progresivo.

# Rederizado declarativo

## Interpolación de datos
Se hace referencia a la forma en como se trabaja con varibles dentro de los componentes.

Las llaves dobles `{{}}` se usan para agregar expresiones de javaScript

Lo que generalmente más se usa es generar un template por cada aplicación.

Generalmente las directivas de Vue.js empiezan con v-

Usando la directiva v-once: 
```js
template: `<div v-once v-text="text"><div/>` 
```
Se puede asegurar que el texto "text" sólo se actualice una vez.

La directiva `v-html` permite usar html dentro de las variables, hay que tener cuidado para que no se pueda ejecutar código a través de por ejemplo un input de usuario. No e recomienda que los ingresos en hmtl tengan que ver con ingresos de usuario.

# Input de usuario

Ahora, un ejemplo de un botón en un formulario usando Vue.js:

```js
const vm = Vue.createApp({
            data(){
                return{
                    text: ""
                    counter: 0
                };
            },
            methods: {
                increment(){
                    console.log("click!")
                    this.counter++;
                }
            },

            template: `
            <form v-on:submit.prevent="increment">
                <button >{{ counter }}</button>
            </form>
            `
        }).mount("#app")
```
En la línea después de template, donde se hace `.prevent`, se puede concatenar estos eventos como también `.stop` 

Una de las principales características de Vue.js es ser reactivo.

Para hablar de reactividad se puede hacer un ejemplo de un espacio de texto que imprime el contenido de una variable y que a su vez es editable para cambiar así el valor de esta:

```js
input(e){
    this.text = e.target.value;
}
<p>{{ text }}</p>
<input 
    type="text" 
    @input="input"
    :value="text"  
/>
```

Este proceso se puede simplificar con: `v-model="text"`

Con el atributo `:value` podemos asignar el valor inicial de alguna variable a algún elemento, si la variable cambia este atributo también.

- :, Atributos
- @, Eventos

# Reactividad

**Propiedades Computadas:** Son reactivas, y permiten separar bloques de código del template para tener un código más limpio y más fácil de leer.

Ejemplo:

```js
computed:{
    fullName(){
        return this.firstName +
            " " + this.lastName
    },
    today(){
        return this.now.toLocaleDateString();
    }
},
template: `
    <div>{{ fullName }}</div>
    <div>{{ today }}</div>
`
```


# Fuentes de información

- [Introduction | Vue.js](https://vuejs.org/guide/introduction.html)
- [Conceptos Básicos de Componentes](https://es.vuejs.org/v2/guide/components.html#Ejemplo-base)
- [Frontend a profundidad con Vue.js - Platzi](https://platzi.com/vue/)
- [gitignore.io](https://www.toptal.com/developers/gitignore)
- [Basic Formatting sintax - GitHub](https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
