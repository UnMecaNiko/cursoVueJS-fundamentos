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

La directiva `v-html` permite usar html dentro de las variables, hay que tener cuidado para que no se pueda ejecutar código a través de por ejemplo un input de usuario. No se recomienda que los ingresos en hmtl tengan que ver con ingresos de usuario.

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
**Watcher:** Vigila el cambio de una variable, se recomiendo no involucrar procesos complejos dentro de lo llamados si no llamar a un method.

Ejemplo simple:

```js
watch:{
    text(value, old){
        console.log("watcher!",value,old)
    }
},
template: `
    {{ text }}
```

Los estilos reactivos son la forma de cambiar valores en la visualización al tiempo que se hacen cambios en la aplicación, gneralmente se combinan clases ya creadas co asignacione dinámicas con vue.

# Listas y condicionales

Nos permiten hacer cambios directos en el template en función de alguna variable, en el ejemplo se comprueba la variable open para saber si ya se ha accedido a una cuenta o no:

```html
<div class="container" :class="styles">
    <h2>{{ text }}</h2>
    <div v-if="open">
        <p>Hola, {{ username }}</p>
    </div>
    <div v-else>
        <label>Username </label>
        <input type="text" v-model="username" > 
    </div>
    <button @click="open= !open">
        <div v-if="!open">Acceder</div>
        <div v-else>Salir</div>
    </button>
</div>
```

Se usa el atributo `:key` para dar un identificador único a elementos que se generan a partir de `v-for` 

Un ejemplo de `v-for`:

```js
posts: [
{
    title: "Titulo 1",
    description: "Lorem pmisum..."
},
{
    title: "Titulo 2",
    description: "Lorem pmisum..."
},
{
    title: "Titulo 3",
    description: "Lorem pmisum..."
}
]
```
```html
<div class="list">
    <div v-for="(item,i) in posts" :key="i" class="item">
        <div class="title">item.title</div>
        <p>item.description</p>
    </div>
</div>
```
# Componentes personalizados

Cuando el código en el template crece mucho lo mejor es comenzar a separar todo en pequeños módulos de código.

Con la función `app.component(nombre, descripción en json)` podemos agregar componentes, aislando distintos elementos

El componente raíz es el que contiene a todos los demás, el número de hijos es ilimitado y cada hijo puede tener múltiples hijos.

**Slots - REutilización de código**

Los slots nos permiten acceder a distintos espacios designados de los componentes diseñados. De esta manera se pueden colocar los estilos de forma más fácil y preocuparse sólo por el contenido. Así queda todo de una forma modular y más reutilizable.

# Comunicación entre componentes

La directiva `v-bind` permite utilizar variables dinámicas, reactividad de valores en tiempo real, permite cualquier tipo de dato

La sintaxis de props permite enviar datos de componentes padre a componentes hijo de forma reactiva, se utiliza como una variable local que viene desde una parte externa.

Es una mala práctica cambiar el valor del prop sin avisar al componente padre

**Ejemplo:**

```js
app.component("v-item",{
    props: {
        text: {
            default: "Texto vacío"
        }
    },
    template:`
    <li>
        {{text}}
    </li>
    `
} )
```

La sintaxis en `props` se puede simplificar a una lista, sin embargo se considera una buena práctica escribirlo como el ejemplo

> Recuerden: Los errores son nuestros amigos, así que, siempre podemos utilizarlos a nuestro favor 💚.

- Usar `v-on` o `@` es equivalente.

Para la comunicación de hijo a padre e pueden crear eventos que retornen valores y ejecuten código, en el ejemplo a continuación se crea un método en el componente hijo que es llamado al hacer click, el método dispara un evento que es escuchado desde el componente padre:

```js
//dentro de "createApp"
template:`
<ul>
    <v-item 
        v-for="(item, i) in items" 
        v-bind:text="item"
        @remove="remove(i)"
    />
</ul>
`
```
```js
 app.component("v-item",{
props: {
    text: String
},
methods:{
    rm(){
        this.$emit("remove");
    }
},
template:`
<li @click="rm">
    {{ text }}
</li>
`
} )
```

### 🔄 v-model
Permite una comunicacion en dos sentidos entre el padre y el hijo, se debe especificar el prop del componente y la variable involucrada: `v-model:propValue="variableValue"`

ejemplo:
```js
app.component("v-input",{
props: {
    inputValue: String
},
methods:{
    changeInput(e){
        this.$emit("update:inputValue",e.target.value)
    }
},
template:`
<input
    type="text"
    v-bind:value="inputValue"
    v-on:input="changeInput"
/>
`
} )
```
Se crea un componente para ser llamado en el template principal:
```js
template:`
<div>
    <p>{{ text }}</p>
    <v-input 
        v-model:inputValue="text"
    />    
</div>
`
```
Nótese cómo se usa `v-model`

Un componente profundo no es hijo directamente del componente que provee los datos, es decir hay uno a más entre este y el padre principal.

`provide` permite pasar variables a componentes hijo cuando estos usan `inject`

Para lograr reactividad, las variables no se declaran en el apartado `provide`, se declaran en el apartado `data` y se llaman con el prefijo `this.`

Ejemplo:

```js
const app = Vue.createApp({
    data(){
        return{
            text: "Hola Vue"
        }
    },
    provide(){
        return{
            otroTexto: this.text
        }
    },
    m
    template:`
    <p>Padre: {{text}}</p>
    <v-primerHijo/>
    `
})
app.component("v-primerHijo",{
    inject: ["otroTexto"],
    template:`
    <p>Primer hijo: {{otroTexto}}</p>
    <v-tercer/>
    `
} )
app.component("v-tercer",{
    inject: ["otroTexto"],
    template:`
    <div>segundo hijo: {{otroTexto}}</div>
    `
} )
const vm =app.mount("#app")
console.log(vm)
```

# Componentes en el virtual DOM

**Instancias de componentes:** Una instancia es una copia de un componente de Vue 

Usando `vm.$root` devuelve el objet raiz, el ue contiene todo el árbol de componentes de la aplicación

Usando `vm.$el` se accede al componente html de la aplicación

Para acceder a referencias se usa `vm.$refs`. Para props, `vm.$props` de esta forma se puede interactuar con Vue y JavaScript Vanila de forma progresiva.

En caso de querer usar Vue como un framework completo también es posible, esta es una de las principales ventajas.

> Un proxy es un elemento reactivo

# Vue progresivo

EN este curso hemos aprendido que Vue es una herramienta progresiva, para usarlo como framework debemos acer uso de nodeJs
Los comandos que utilizaremos son:
```bash
npm install -g @vue/cli
vue create hello-world
npm run serve
```

# Fuentes de información

- [Frontend a profundidad con Vue.js - Platzi](https://platzi.com/vue/)
- [Vue CLI](https://cli.vuejs.org/)
- [Introduction | Vue.js](https://vuejs.org/guide/introduction.html)
- [Conceptos Básicos de Componentes](https://es.vuejs.org/v2/guide/components.html#Ejemplo-base)
- [Component Events](https://vuejs.org/guide/components/events.html)
- [Conditional Rendering | Vue.js](https://vuejs.org/guide/essentials/conditional.html)
- [gitignore.io](https://www.toptal.com/developers/gitignore)
- [Basic Formatting sintax - GitHub](https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
