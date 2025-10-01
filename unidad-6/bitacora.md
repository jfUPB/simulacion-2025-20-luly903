# Evidencias de la unidad 6

## SET

### Actividad 01
- <img width="807" height="798" alt="image" src="https://github.com/user-attachments/assets/f70b2f0d-b974-4edd-9c18-2d75fdde14b4" />
esta obra me llama la atencion por la forma en la que pareciera que la obra fue hecha de forma tradicional a pinceladas, combinado con el contraste de blanco y negro me parece que la obra tiene un toque tradicional incluso si fue hecha a punta de programacion y algortimos.


- <img width="800" height="797" alt="image" src="https://github.com/user-attachments/assets/f539aa7e-7b04-4e2a-8697-7c93119a529f" />
Me gusta como las lineas blancas parecieran ser grietas que pasan por todo el lienzo como si fuera tierra seca o ramas de arboles abstractos.

- lo que me inspira de su trabajo es lo fluido y organico que se ve y siente su trabajo como si el mismo lo hubiera pintado a mano, aunque en verdad sea arte generativo que el programo y genero a partir de codigo.

## SEEK

### Actividad 02

**1. ¿Qué es una fuerza de dirección (steering force)?**
- Es una fuerza artificial que guia un agente (ej. particula) hacia una direccion especifica, se puede usar para crear un trayecto especifico para realizar acciones como ir hacia un objetivo, evitar obstaculos, etc.

**2. ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?**
- La fuerza de direccion no aplica las leyes de la fisica de la vida real, sino mas bien es una construccion computacional diseñada para controlar el movimiento de agentes de forma mas "inteligente" y estructurada.

**3. ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?**
- Craig Reynold fue quien introdujo el concepto de steering behaviors en su trabajo de 1987 sobre flocking.

### Actividad 03

**1. Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.**
- El campo de flujo es una matriz bidimensional donde cada celda contiene un vector de direccion, la direccion de estos vectores se determina de forma  aleatoria usando el ruido de perlin para que las direcciones sigan un "flujo" y no varien de forma tan abrupta entre casillas.

**2. Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.**
- El agente primero observa en que celda del campo se encuentra y el vector de direccion que contiene, despues lo multiplica por su velocidad maxima y calcula la steering force del agente restando el vector de la celda con el vector de velocidad del agente, ya por ultimo el resultado de esa resta (que se convierte en la steering force) que se le aplica al vector aceleracion del agente.

**3. Lista los parámetros clave identificados (resolución, maxspeed, maxforce).**
- resolution: define el tamaño de cada celda del campo de flujo, afectando la “grilla” de vectores.

maxspeed: la velocidad máxima a la que el agente puede moverse.

maxforce: la magnitud máxima de la fuerza de dirección que puede aplicar el agente en cada paso (para mantener un movimiento suave y realista).

**4. Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes.** 
- En la clase flowfield modifique los valores de cols y rows para que las celdas fueran muchos mas anchas y delgadas, ademas de modificar el ruido de perlin, que contenia un random normal, a valores fijos. Estas modificaciones provocaron que el flowfield ya no tenga "caminos" o "corrientes" fluidas, variadas y que dan una sensacion de curva, sino que ahora todas las celdas apuntan hacia la misma direccion que se genere, por lo que ahora los agentes siguen una misma direccion.
- como un extra, hice que los boids ahora tengan colores aleatorios cambiando el valor fijo del fill por 3 radom() que dieran valores rgb diferentes aleatorios.


**5. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.**
- fragmentos del codigo modificado:
  vehicle
    ```js
    show() {
    // Draw a triangle rotated in the direction of velocity
    let theta = this.velocity.heading();
    fill(random(255),random(255),random(255));
    stroke(0);
    strokeWeight(1);
    push();
    translate(this.position.x, this.position.y);
    rotate(theta);
    beginShape();
    vertex(this.r * 2, 0);
    vertex(-this.r * 2, -this.r);
    vertex(-this.r * 2, this.r);
    endShape(CLOSE);
    pop();
    }
    ```
  flowfield
    ```js
    class FlowField {
  constructor(r) {
    this.resolution = r;
    //{!2} Determine the number of columns and rows.
    this.cols = 100 / this.resolution;
    this.rows = 600 / this.resolution;
    //{!4} A flow field is a two-dimensional array of vectors. The example includes as         separate function to create that array
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
     }
    
    
      init() {
    // Reseed noise for a new flow field each time
    noiseSeed(random(10000));
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        //{.code-wide} In this example, use Perlin noise to create the vectors.
        let angle = map(noise(0, 200), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);
        yoff += 0.1;
      }
      xoff += 0.1;
    }
  }
    ```
  
- <img width="713" height="282" alt="image" src="https://github.com/user-attachments/assets/28e48b2e-6a5a-45a8-b922-ae6b91cdd64f" />


### Actividad 04

**1. Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).**
- Separacion:
    Objetivo: Evitar que los boids se choquen
    logica: cada agente busca vecinos muy cercanos dentro de un radio pequeño; si los  encuentra, genera una fuerza que lo empuja en dirección contraria

Alineacion:
    Objetivo: Moverse en la misma direccion que otros boids cercanos
    logica: calcula la velocidad promedio de los agentes dentro del radio de percepción y ajusta la suya para alinearse con esa dirección.

Cohesion:
    Objetivo: Evitar que los boids se separen mucho de los otros boids cercanos
    logica: calcula el centro de masa de los vecinos cercanos y genera una fuerza que empuja al agente hacia ese punto, promoviendo la agrupación.

**2. Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).**
- Radio de percepción: define qué tan lejos ve un agente para considerar a otros como vecinos en cada regla. Puede variar según la regla (ej. separación suele tener un radio más pequeño).

Pesos de las reglas: cada regla (separación, alineación, cohesión) tiene un "peso" que ajusta su influencia en el movimiento final.

maxspeed: la velocidad máxima que un boid puede alcanzar.

maxforce: la fuerza máxima de dirección que un boid puede aplicar al ajustar su movimiento.

**3. Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el comportamiento colectivo del enjambre (¿Se dispersan? ¿Forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.**
- 











