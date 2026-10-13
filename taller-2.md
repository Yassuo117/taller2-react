# Taller 2 React

**Asignatura:** Desarrollo Front End  
**Estudiante:** Carlos Antonio Ramirez Galvis  
**Código:** T00066021  
**Tema:** React  
**Fecha:** 12 de octubre de 2026

## Introducción

En el desarrollo web moderno, una página ya no se limita a mostrar información estática. Muchas aplicaciones deben responder a clics, búsquedas, formularios, mensajes, cambios de sesión y datos que llegan desde una API. Cuando todo eso se programa únicamente manipulando el DOM paso por paso, el código puede crecer muy rápido y se vuelve difícil mantener la interfaz de acuerdo con los datos reales de la aplicación.

React propone una manera diferente de enfrentar ese problema. En vez de indicar cada modificación que debe hacerse sobre el documento HTML, el desarrollador describe la interfaz que corresponde a un estado determinado. React se encarga de actualizar la pantalla cuando los datos cambian. Además, permite dividir la aplicación en componentes pequeños que se pueden entender, probar y reutilizar.

En este trabajo explico qué es React, cuál es el problema que busca resolver, cómo funcionan sus conceptos principales y por qué sigue siendo importante en la industria. También presento casos concretos de empresas que lo han utilizado en productos de gran escala.

## Punto 1 Qué es React

### Definición y utilidad en el desarrollo web

React es una librería de JavaScript para construir interfaces de usuario en la web y en otras plataformas. Una interfaz de usuario es la parte con la que una persona interactúa: botones, formularios, menús, tarjetas, listas, mensajes y demás elementos visibles. La documentación oficial explica que React permite formar pantallas completas a partir de piezas individuales llamadas componentes.[^1]

En mis palabras, React sirve para organizar la parte visual y el comportamiento de una aplicación. Por ejemplo, en una tienda virtual se podrían crear componentes separados para la barra de navegación, la tarjeta de un producto, el carrito y el formulario de pago. Cada componente tiene una responsabilidad concreta, pero todos se combinan para formar la aplicación completa.

React no reemplaza HTML, CSS y JavaScript. En realidad, se apoya en esos conocimientos. HTML sigue representando la estructura, CSS se encarga de los estilos y JavaScript aporta la lógica. React ayuda a relacionar esos tres elementos de una forma más ordenada cuando la interfaz cambia constantemente.

### Origen y año de aparición

React fue creado originalmente por Jordan Walke mientras trabajaba en Facebook, empresa que hoy se llama Meta.[^2] La librería fue presentada y publicada como proyecto de código abierto el 29 de mayo de 2013.[^3] Por eso normalmente se afirma que React apareció en 2013, aunque antes ya se había utilizado de forma interna dentro de Facebook.

El origen de React es importante porque no nació solamente como un experimento académico. Surgió para responder a problemas reales de interfaces complejas y con muchos datos. Con el tiempo, otras empresas y desarrolladores empezaron a adoptarlo, aportar al proyecto y construir herramientas alrededor de él.

### React como librería y no como framework completo

React se considera una librería porque su propósito principal es construir y actualizar interfaces. No obliga a usar una única solución para todas las partes de una aplicación. Por ejemplo, el equipo puede escoger otras herramientas para el enrutamiento, el acceso a datos, las pruebas, los formularios o la generación del proyecto. La propia documentación de React lo presenta como una librería y aclara que, para una aplicación completa, se puede combinar con un framework como Next.js o React Router.[^1]

Un framework suele ofrecer una estructura más amplia y define muchas decisiones del proyecto. Normalmente incluye reglas sobre la organización de carpetas, enrutamiento, compilación, obtención de datos y construcción para producción. En ese caso, el desarrollador trabaja dentro de una estructura ya establecida. Una librería es más específica: se usa para resolver una parte del problema y deja más decisiones abiertas.

La diferencia se puede resumir de esta forma: React aporta las piezas para construir la interfaz, mientras que un framework basado en React puede aportar la estructura completa del proyecto. Esto da flexibilidad, aunque también significa que el equipo debe escoger correctamente las herramientas que acompañarán a React.

### Significado de un enfoque declarativo

Decir que React es declarativo significa que se describe qué debe aparecer en la pantalla según los datos actuales, sin escribir manualmente cada paso necesario para modificar el DOM. Si una variable indica que el usuario inició sesión, el componente devuelve la vista correspondiente. Si la variable cambia, React vuelve a calcular la interfaz y actualiza lo necesario.

Por ejemplo, esta expresión describe directamente el resultado esperado:

```jsx
function Mensaje({ conectado }) {
  return (
    <p>{conectado ? "Sesión iniciada" : "Debes iniciar sesión"}</p>
  );
}
```

El componente no busca el párrafo para cambiar su texto mediante instrucciones separadas. Solo establece qué texto corresponde al valor de `conectado`. La documentación oficial contrasta esta idea con el enfoque imperativo, donde se habilitan, deshabilitan, muestran u ocultan elementos uno por uno.[^4]

## Punto 2 El problema que resuelve React

### Dificultad de manipular el DOM a mano

El DOM es la representación que el navegador crea de los elementos de una página. Con JavaScript tradicional se puede buscar un elemento, crear nodos, cambiar textos, agregar clases y conectar eventos. El problema no es que este método sea incorrecto, sino que se vuelve difícil de controlar cuando la aplicación crece.

Imaginemos una lista de tareas con un contador de pendientes. Al eliminar una tarea habría que borrar el elemento de la lista, actualizar el contador y tal vez cambiar un mensaje que dice que no existen tareas. Si el programador olvida uno de esos pasos, los datos pueden indicar una cosa y la pantalla mostrar otra. Este tipo de desincronización es más probable cuando existen muchas interacciones y varios elementos dependen del mismo dato.

Un ejemplo imperativo simplificado sería el siguiente:

```javascript
const boton = document.querySelector("#sumar");
const resultado = document.querySelector("#resultado");
let cantidad = 0;

boton.addEventListener("click", () => {
  cantidad = cantidad + 1;
  resultado.textContent = cantidad;
});
```

Aquí el programador administra por separado la variable y el texto del DOM. En una interfaz pequeña es manejable, pero en una aplicación grande aparecen muchos elementos y dependencias que deben actualizarse de manera coordinada.

### Enfoque imperativo y enfoque declarativo

En el enfoque imperativo se explica cómo alcanzar el resultado. Las instrucciones pueden ser: buscar el elemento, borrar su contenido, crear un nodo, agregar una clase y anexarlo al documento. El programa describe la secuencia exacta de acciones.

En el enfoque declarativo se expresa el resultado que corresponde al estado actual. En React, el componente puede indicar que, si una lista tiene elementos, deben mostrarse; si está vacía, debe aparecer un mensaje. Cuando cambia la lista, React actualiza la vista. La lógica se concentra en la relación entre datos e interfaz y no en una colección de operaciones manuales sobre el DOM.

```jsx
function ListaTareas({ tareas }) {
  if (tareas.length === 0) {
    return <p>No hay tareas pendientes.</p>;
  }

  return (
    <ul>
      {tareas.map((tarea) => (
        <li key={tarea.id}>{tarea.nombre}</li>
      ))}
    </ul>
  );
}
```

Este código describe dos posibles vistas. No contiene instrucciones como “borra el mensaje vacío” o “crea tres elementos `li`”. Esas operaciones quedan bajo la responsabilidad de React.

### Sincronización entre el estado y la interfaz

React trabaja con la idea de que la interfaz es una representación de los datos actuales. Los datos que cambian durante el uso de un componente se guardan en el estado. Cuando el estado se modifica mediante su función de actualización, React vuelve a ejecutar el componente y calcula qué debe mostrarse.

La actualización no significa que React recargue toda la página. React compara el resultado anterior con el nuevo y aplica los cambios necesarios a la interfaz. Desde la perspectiva del desarrollador, lo más importante es mantener un estado correcto y describir la vista que depende de él. La documentación recomienda evitar estados duplicados o redundantes, porque son una fuente común de errores.[^5]

Un contador muestra esta relación de forma sencilla:

```jsx
import { useState } from "react";

function Contador() {
  const [cantidad, setCantidad] = useState(0);

  return (
    <button onClick={() => setCantidad(cantidad + 1)}>
      Total de clics: {cantidad}
    </button>
  );
}
```

Cuando el usuario hace clic, `setCantidad` cambia el estado. Después React vuelve a representar el botón con el nuevo valor. No es necesario localizar el texto del botón para actualizarlo manualmente.

### Componentes reutilizables como parte de la solución

Los componentes permiten dividir una pantalla grande en unidades pequeñas. Cada unidad puede recibir datos y devolver una parte de la interfaz. Esto disminuye la repetición y hace que las responsabilidades sean más claras.

Por ejemplo, una red social no necesita programar desde cero cada botón “Me gusta”. Puede crear un componente y reutilizarlo en cientos de publicaciones. Cada instancia recibe información distinta, como el identificador de la publicación o la cantidad de reacciones. Si después cambia el diseño del botón, la modificación se realiza en el componente y se refleja en todos los lugares donde se utiliza.

La reutilización no consiste solamente en copiar menos código. También ayuda a mantener comportamientos consistentes, facilita las pruebas y permite que distintos integrantes de un equipo trabajen en partes separadas de la aplicación.

## Punto 3 Elementos principales de React

### Componentes

Un componente es una función de JavaScript que devuelve la interfaz que se debe mostrar. Su nombre comienza con mayúscula para que React pueda diferenciarlo de una etiqueta HTML normal. Los componentes pueden contener otros componentes y, de esa forma, formar un árbol cuya raíz suele ser `App`.

Características principales:

- Representan una parte concreta de la interfaz.
- Se pueden reutilizar varias veces.
- Pueden recibir datos por medio de props.
- Pueden tener estado propio cuando necesitan recordar información.
- Se pueden combinar y anidar para construir pantallas completas.

```jsx
function TarjetaCurso() {
  return (
    <article className="tarjeta">
      <h3>Desarrollo Front End</h3>
      <p>Curso sobre interfaces web modernas.</p>
    </article>
  );
}

function App() {
  return (
    <main>
      <TarjetaCurso />
      <TarjetaCurso />
    </main>
  );
}
```

`TarjetaCurso` se usa dos veces como si fuera una etiqueta. Sin embargo, no es una etiqueta propia de HTML, sino un componente creado por el desarrollador.

### JSX

JSX es una extensión de sintaxis para JavaScript que permite escribir una estructura parecida a HTML dentro del código. El navegador no interpreta JSX directamente; una herramienta de construcción lo transforma en instrucciones de JavaScript que React puede utilizar.

Sus reglas básicas incluyen:

- El componente debe devolver un solo elemento raíz. También se puede usar un fragmento `<>...</>` para agrupar varios elementos.
- Las etiquetas deben cerrarse, incluso cuando no tienen contenido, como `<img />`.
- Se usa `className` en lugar de `class`.
- Los atributos escritos con varias palabras suelen usar camelCase, como `onClick`.
- Las llaves permiten insertar expresiones de JavaScript dentro del marcado.

```jsx
function Saludo() {
  const nombre = "Laura";
  const tareas = 3;

  return (
    <section className="saludo">
      <h2>Hola, {nombre}</h2>
      <p>Tienes {tareas} tareas pendientes.</p>
    </section>
  );
}
```

Las llaves no reciben cualquier bloque de instrucciones, sino expresiones que producen un valor. En el ejemplo se insertan dos variables dentro del contenido visible.

### Props

Las props son datos que un componente padre envía a un componente hijo. Se parecen a los argumentos de una función porque permiten reutilizar la misma estructura con información diferente. El componente que recibe una prop no debe modificarla directamente; si el dato debe cambiar, el cambio se administra desde el estado correspondiente.

Características principales:

- Permiten personalizar cada uso de un componente.
- Viajan normalmente del componente padre hacia el hijo.
- Pueden contener textos, números, arreglos, objetos, funciones u otros valores.
- Se consideran de solo lectura dentro del componente que las recibe.

```jsx
function Producto({ nombre, precio }) {
  return (
    <article>
      <h3>{nombre}</h3>
      <p>Precio: ${precio}</p>
    </article>
  );
}

function App() {
  return (
    <main>
      <Producto nombre="Teclado" precio={85000} />
      <Producto nombre="Mouse" precio={45000} />
    </main>
  );
}
```

El componente `Producto` mantiene la misma estructura, pero muestra datos diferentes en cada instancia. Las props hacen que el componente sea reutilizable sin duplicar su código.

### Estado y useState

El estado es la memoria de un componente. Guarda información que puede cambiar mientras la persona utiliza la aplicación, como el contenido de un formulario, un contador, la pestaña seleccionada o la respuesta de una API. El hook `useState` permite declarar una variable de estado y obtener una función para actualizarla.

Características principales:

- Se declara dentro de un componente mediante `useState`.
- Tiene un valor inicial.
- Se actualiza con la función que devuelve el hook.
- Al actualizarse, React vuelve a representar el componente.
- No se debe cambiar directamente, porque React necesita recibir la actualización mediante la función correspondiente.

```jsx
import { useState } from "react";

function Interruptor() {
  const [encendido, setEncendido] = useState(false);

  return (
    <section>
      <p>Estado: {encendido ? "Encendido" : "Apagado"}</p>
      <button onClick={() => setEncendido(!encendido)}>
        Cambiar estado
      </button>
    </section>
  );
}
```

El valor inicial es `false`. Cada clic llama a `setEncendido` con el valor contrario. Como el texto depende de `encendido`, React actualiza el párrafo automáticamente. Es incorrecto hacer una asignación directa como `encendido = true`, porque esa acción no sigue el mecanismo de estado de React.

### Eventos

Los eventos permiten responder a acciones del usuario. React utiliza nombres como `onClick`, `onChange` y `onSubmit`. A esos atributos se les pasa una función, no el resultado de ejecutarla. El evento puede cambiar el estado y provocar que la interfaz muestre nueva información.

Características principales:

- Usan camelCase, por ejemplo `onClick` y no `onclick`.
- Reciben una función manejadora.
- Pueden acceder al objeto del evento para conocer el elemento o valor involucrado.
- Se combinan con el estado para crear interfaces interactivas.

```jsx
import { useState } from "react";

function FormularioNombre() {
  const [nombre, setNombre] = useState("");

  function manejarCambio(evento) {
    setNombre(evento.target.value);
  }

  return (
    <section>
      <label>
        Nombre
        <input value={nombre} onChange={manejarCambio} />
      </label>
      <p>Vista previa: {nombre || "Sin nombre"}</p>
    </section>
  );
}
```

`onChange` se activa cuando cambia el contenido del campo. La función guarda el nuevo valor en el estado y el párrafo se actualiza en la misma interacción. Este es un ejemplo de un campo controlado, porque su valor visible depende del estado de React.

## Punto 4 Importancia de React en la industria

### Razones por las que las empresas lo eligen

Una razón importante es la posibilidad de dividir interfaces grandes en componentes. En productos con muchas pantallas, varios equipos pueden trabajar con piezas independientes y compartir componentes comunes. Esto favorece la consistencia visual y evita repetir soluciones para botones, formularios, tarjetas o menús.

También influye su modelo declarativo. Relacionar la interfaz con el estado permite razonar sobre lo que debe mostrarse en cada situación. La documentación oficial señala que React puede incorporarse de manera gradual, incluso en una página existente, por lo que una empresa no siempre necesita reconstruir todo su producto para comenzar a utilizarlo.[^1]

Otro punto es que React se ha probado a gran escala. El sitio oficial indica que los cambios del proyecto se validan en superficies críticas usadas por más de mil millones de personas y en más de cien mil componentes de Meta.[^1] Esto no elimina la necesidad de hacer pruebas propias, pero muestra que el proyecto recibe validación en aplicaciones muy exigentes.

### Ecosistema de desarrollo

El ecosistema de React incluye herramientas que cubren necesidades distintas. Vite permite crear proyectos, ejecutar un servidor de desarrollo y generar archivos optimizados para producción. Su recarga rápida ayuda a ver los cambios casi inmediatamente mientras se programa.[^6] React Router aporta navegación entre vistas. Frameworks como Next.js agregan enrutamiento, renderizado en el servidor y otras decisiones para aplicaciones completas. También existen herramientas de pruebas, bibliotecas de componentes y soluciones para formularios o manejo de datos.

Esta variedad es una ventaja porque el equipo puede escoger según el proyecto. Sin embargo, también puede ser una dificultad para alguien que está empezando, ya que React por sí solo no toma todas las decisiones. Por eso es necesario entender primero sus fundamentos antes de instalar muchas bibliotecas.

### Comunidad y mantenimiento actual

React fue creado dentro de Facebook y durante muchos años estuvo estrechamente relacionado con Meta. Esa historia todavía es visible en el equipo técnico y en el uso interno de la librería. No obstante, el mantenimiento institucional cambió recientemente. El 24 de febrero de 2026 se lanzó oficialmente la React Foundation, una fundación independiente alojada por la Linux Foundation. React, React Native y proyectos relacionados como JSX pasaron a ser propiedad de esta fundación. Entre sus miembros fundadores se encuentran Meta, Microsoft, Amazon y Vercel, entre otras organizaciones.[^7]

La dirección técnica continúa en manos de las personas que contribuyen y mantienen el proyecto, separada de la junta de la fundación. Además, React cuenta con un equipo dedicado y con más de mil contribuidores de código abierto reconocidos por su documentación oficial.[^2] Esta combinación de equipo técnico, empresas participantes y comunidad abierta ayuda a que la librería tenga continuidad.

### Relevancia para un desarrollador front end

Aprender React es relevante porque obliga a practicar conceptos que aparecen en muchos proyectos modernos: componentes, composición, flujo de datos, estado, eventos y renderizado condicional. Incluso si después se utiliza otra herramienta, esas ideas son transferibles.

Su presencia en la industria también hace que sea una habilidad útil para buscar prácticas profesionales o empleo. En la Encuesta de Desarrolladores de Stack Overflow de 2026, el 41,5 % de los 10.281 participantes de la sección de tecnologías web indicó haber trabajado ampliamente con React durante el último año. En esa categoría quedó detrás de Node.js y por encima de otras alternativas consultadas.[^8] El dato no significa que React sea la única opción válida, pero sí demuestra que sigue teniendo un uso considerable.

Para mí, la importancia de aprender React no consiste solamente en memorizar `useState` o copiar componentes. Lo valioso es aprender a pensar una interfaz como un conjunto de estados y piezas relacionadas. Ese enfoque ayuda a escribir código más organizado y a comprender aplicaciones front end de mayor tamaño.

## Punto 5 Empresas que utilizan React

### Meta

Meta es el caso más directo porque React nació dentro de Facebook. En sus productos, React se utiliza para construir interfaces de Facebook e Instagram. Un ejemplo concreto es la mensajería de Instagram para computadores: el equipo de ingeniería explicó que fue la primera parte del sitio escrita completamente con componentes funcionales y Hooks de React.[^9]

Este caso es relevante porque una función de mensajería debe manejar conversaciones, nuevos mensajes, estados de carga, notificaciones e interacciones en tiempo real. Es una interfaz donde muchos datos cambian sin recargar la página. El modelo de componentes y estado de React resulta apropiado para mantener esas vistas organizadas y actualizadas.

Meta también ha explicado que sus equipos reutilizan infraestructura y funciones construidas para Facebook.com, como el News Feed y los comentarios, en experiencias para otras plataformas.[^10] Esto demuestra una de las ideas principales de React: crear piezas que puedan compartirse y adaptarse sin repetir toda la lógica.

### Discord

Discord también utiliza React en su aplicación de escritorio. En una publicación técnica de 2025, la empresa indicó que su cliente de escritorio está construido con React y su cliente móvil con React Native.[^11] En otro análisis, Discord describió un problema de memoria que apareció durante una actualización de React, lo que confirma que la librería forma parte directa de su código de producto y no solamente de una página informativa.[^12]

El caso es relevante porque Discord es una aplicación con conversaciones, canales, listas de usuarios, llamadas y cambios en tiempo real. La interfaz debe responder constantemente a eventos y datos nuevos. React permite separar esas áreas en componentes, compartir lógica y actualizar partes de la pantalla a medida que cambia el estado.

Además, Discord ha aprovechado la cercanía entre React y React Native para compartir lógica y herramientas entre sus equipos de web y dispositivos móviles. Esto muestra que el conocimiento de React puede servir como base para trabajar en productos que van más allá de una sola página web.

### Comparación de los casos

Meta y Discord tienen productos distintos, pero comparten una necesidad: administrar interfaces grandes que cambian de acuerdo con datos y acciones del usuario. En Instagram la mensajería debe reflejar conversaciones y eventos en tiempo real. En Discord ocurre algo parecido con canales, mensajes y llamadas. Los dos casos demuestran que React no se limita a proyectos pequeños de aprendizaje; puede formar parte de aplicaciones usadas por millones de personas.

## Conclusión

React es una librería de JavaScript enfocada en la construcción de interfaces mediante componentes. Su aporte principal no es crear etiquetas nuevas ni reemplazar los fundamentos de la web, sino ofrecer un modelo más ordenado para relacionar datos e interfaz. El desarrollador declara qué debe verse y React actualiza la pantalla cuando cambia el estado.

Los componentes, JSX, las props, el estado y los eventos trabajan juntos. Los componentes dividen la aplicación; JSX expresa su estructura; las props llevan datos desde otros componentes; el estado conserva información cambiante; y los eventos conectan las acciones de la persona con las actualizaciones de la interfaz. Comprender esa relación es más importante que aprender cada concepto de forma aislada.

React sigue siendo relevante por su adopción, su ecosistema y la comunidad que lo sostiene. La creación de la React Foundation en 2026 también muestra que el proyecto entró en una nueva etapa de gobierno independiente. Los ejemplos de Meta y Discord confirman que su modelo se aplica a productos complejos y con cambios en tiempo real. Por estas razones, considero que React es una habilidad importante para un desarrollador front end, pero debe estudiarse sobre una base sólida de HTML, CSS y JavaScript.

## Fuentes consultadas

[^1]: React. [React, la librería para interfaces web y nativas](https://react.dev/). Consultado el 12 de octubre de 2026.
[^2]: React. [Acknowledgements](https://react.dev/community/acknowledgements). Consultado el 12 de octubre de 2026.
[^3]: React. [React Versions](https://react.dev/versions). La página registra que React fue publicado como código abierto el 29 de mayo de 2013. Consultado el 12 de octubre de 2026.
[^4]: React. [Reacting to Input with State](https://react.dev/learn/reacting-to-input-with-state). Consultado el 12 de octubre de 2026.
[^5]: React. [Managing State](https://react.dev/learn/managing-state). Consultado el 12 de octubre de 2026.
[^6]: Vite. [Why Vite](https://vite.dev/guide/why). Consultado el 12 de octubre de 2026.
[^7]: React. [The React Foundation A New Home for React Hosted by the Linux Foundation](https://react.dev/blog/2026/02/24/the-react-foundation). Publicado el 24 de febrero de 2026.
[^8]: Stack Overflow. [Developer Survey 2026 Web Frameworks and Technologies](https://survey.stackoverflow.co/2026/technology/data/web-framework). Consultado el 12 de octubre de 2026.
[^9]: Engineering at Meta. [Launching Instagram Messaging on Desktop](https://engineering.fb.com/2022/07/26/web/launching-instagram-messaging-on-desktop/). Publicado el 26 de julio de 2022.
[^10]: Engineering at Meta. [React at Meta Connect 2024](https://engineering.fb.com/2024/10/02/android/react-at-meta-connect-2024/). Publicado el 2 de octubre de 2024.
[^11]: Discord. [Supercharging Discord Mobile Our Journey to a Faster App](https://discord.com/blog/supercharging-discord-mobile-our-journey-to-a-faster-app). Publicado el 5 de marzo de 2025.
[^12]: Discord. [Investigating Discord's React Memory Leak](https://discord.com/blog/investigating-discords-react-memory-leak). Publicado el 8 de agosto de 2019.
