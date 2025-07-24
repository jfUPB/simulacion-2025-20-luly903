# Unidad 1

## 🤔 Fase: Reflect

### Actividad 9

*Parte 1: recuperación de conocimiento (Retrieval Practice)*

1. Describe la diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del Ruido Perlin (noise()). ¿En qué tipo de situación usarías cada una?
- con el random(), la aletoriedad es "uniforme", osea todos los eventos tienen la misma probabilidad de ocurrir, mientras que con noise(), esa probabilidad tiene una tendencia a favorecer aquellos eventos cercanos al evento que haya sucedido anteriormente o que el mismo programador especifique.
   
2. Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal?
- la distribucion de probabilidad es la forma en la que la probabilidad de que los eventos ocurran en una aleatoriedad estan distribuidos. Por ejemplo, cuando todos los eventos posibles tienen la misma probabilidad de ocurrir, a eso se le llama una distribucion uniforme. Por otro lado, cuando hay ciertos eventos que tienen una probabilidad mayor de ocurrir sobre el resto, a eso se le llama una distribucion normal.
   
3. ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple
- la aletoriedad en el arte genertivo ayuda a darle un toque unico y dinamico a las obras. Una forma en la que lo hace es permitir que ninguna obra de arte generativa sea la misma, pues la aleatoriedad permite que cada experiencia sea unica y diferente al interactuar con ella, ademas, esto permite que sean dinamicas e impredecibles al no seguir una estructura rigida con patrones repetitivos.
   
4. Piensa en tu obra final (Actividad 08). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas.
- uno de los conceptos que use fue el del randomGaussian(), en el caso de mi obra, lo use para que los cambios de velocidad del trazo variaran de forma impredecible, hice esto con el proposito de ponerle un reto al usuario ya que tendria que pensar muy bien cuando acelerar o desacelerar y en que puntos del lienzo, quize de alguna forma representar la forma en la que nosotros como humanos nunca tenemos un verdadero control total sobre lo que pasa en nuestras vidas y como las manejamos a partir de lo que SI podemos controlar.
   
5. ¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?
- no estoy muy segura si entendi la pregunta bien, pero segun lo que entendi, voy a responder que la caminata en el contexto de la simulacion es el desplazamiento del trazo a lo largo del mapa, una caracteristica particular de una caminata de tipo "levi flight" es que la mayoria de veces caminara uno o muy pocos pasos a la vez con una probabilidad muy pequeña de que pegue un salto gigante.




*Parte 2: reflexión sobre tu proceso (Metacognición)*

1. ¿Cuál fue el concepto más abstracto o difícil de “visualizar” para ti en esta unidad? ¿Qué hiciste para finalmente comprenderlo?
-para mi fue el ruido de perlin, principalmente porque a veces retornaba un valor, y a veces unas coordenadas y no comprendia muy bien como funcionaba y porque no funcionaba como queria a veces, para resolverlo ei mucho la guia de p5.js y vi ejemplos de como lo empleaban, aun no logro entenderlo por completo pero he avanzado desde la primera vez que vi el concepto. Mi mayor problema mas que entender el concepto, fue saber como se implementaba, entiendo la teoria detras pero al momento de aplicarlo en la programacion aun tengo problemas.

2. Describe un momento durante el desarrollo de tu obra final (Actividad 08) en el que un “error” o un resultado inesperado te llevó a una idea creativa interesante.
- originalmente iba a implementar el ruido de perlin en la aleatoriedad del cambio de color del trazo cada vez que oprimiera y soltara la tecla ENTER, pero no funcionaba como queria o de lleno no funcionaba, originalmente planeaba usarlo para que los cambios de colores no fueran tan exagerados; sin embargo, cuando empece a investigar mas sobre como aplicar el ruido de perlin en el sketch, me tope con un ejemplo donde usaban el ruido de perlin para que el fondo inicial del sketch pareciera una niebla gris que me parecio chevere, por lo que lo aplique en mi propio sketch y me gusto como esa niebla ayuda a que el fondo no se sienta muy monotono, ademas de agregar un toque unico al sketch ya que el fondo nunca es el mismo.

3. Al combinar diferentes técnicas de aleatoriedad, ¿Cuál fue el mayor desafío? ¿La interacción entre los sistemas, el control de los parámetros, el rendimiento?
-el mayor desafio para mi fue lograr implementarlos de una forma que aun permitiera al usuario interactuar con el sketch sin quitar completamente el control de lo que pudiera hacer dentro de el, ya que la idea principal es que el usuario aun estuviera en su mayoria en control sobre lo que haria dentro de el.
   
4. Si tuvieras que empezar la Actividad 08 de nuevo, ¿Qué harías de manera diferente basándote en lo que sabes ahora?
- no haria que el trazo siempre estuviera desplazandose hacia adelantes, sino que mas bien haria que funcionara similar a la serpiente de los minijuegos en el que siempre se esta moviendo pero tu puedes elegir la direccion en la que va, y cambiar por ende los controles para subir o bajar la velocidad del trazo.


### Actividad 10
*compañero: Daniel Betancourt*
- 5/5, la obra generativa hace uso de 3 elementos de aleatoriedad (ruido de perlin, randomGaussian y random), tiene descripcion, el codigo del sketch, captura y link al mismo, la obra es entretenida de observar e interactuar, algo que me pareceria interesante seria ver como cambiaria la obra si los puntos se desplazaran de forma continua al mantener oprimido las flechas derecha/izquierda, pero me gusta el efecto que da cuando solo se presionan varias veces, me recuerda aun copo de nieve o una flor cuando la hago girar a la izquierda o a la derecha.


 ### Actividad 11
1. ¿Qué actividad, ejemplo o explicación de esta unidad te resultó más reveladora o útil para tu aprendizaje? ¿Qué deberíamos mantener sí o sí?
- Me gusto la actividad del salto de levy, a pesar de que no la aplique en la actividad 8, si realice 2 ejemplos del mismo porque me parecia chevere de aplicar, algo que podrian mantener es la forma en la que se realizan actividades pequeñas y enfocadas para familiarizarse con los conceptos nuevos que introducen en la unidad.
   
2. ¿Hubo alguna actividad o concepto que te pareció redundante, confuso o menos útil? ¿Hay algo que eliminarías o cambiarías por completo?
- el ruido de perlin, aunque esto fue mas que nada en mi parte ya que tuve muchos problemas aplicandolo en practica, por ahora no cambiaria nada.

3. ¿Qué te faltó en esta unidad? ¿Quizás más ejemplos de artistas, más desafíos técnicos, más teoría? ¿Qué idea tienes para hacerla mejor?
- quizas mas desafios tecnicos para comprender bien como aplicar lo aprendido en diferentes contextos, se podrian hacer mas desafios tecnicos, no en mayor dificultad, p

4. Balance Teoría-Práctica: ¿Cómo sentiste el equilibrio entre analizar los ejemplos del texto guía y ponerte a programar tus propios sketches? Explica tu respuesta.


5. Comentario Adicional: ¿Hay algo más que quieras compartir sobre tu experiencia en esta unidad?




