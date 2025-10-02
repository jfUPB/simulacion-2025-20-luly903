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
- Modifique drasticamente el peso de las 3 reglas (cohesion, separacion, alineacion). Disminui cohesion y separacion a 0.5 y aumente alineacion a 5.0, como resultado el enjambre ahora todos siguen una misma direccion en grupos muy juntos.

codigo modificado

  boid
  ```
 // We accumulate a new acceleration each time based on three rules
  flock(boids) {
    let sep = this.separate(boids); // Separation
    let ali = this.align(boids); // Alignment
    let coh = this.cohere(boids); // Cohesion
    // Arbitrarily weight these forces
    sep.mult(0.5);
    ali.mult(5.0);
    coh.mult(0.5);
    // Add the force vectors to acceleration
    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
  }
  ```

- <img width="718" height="287" alt="image" src="https://github.com/user-attachments/assets/4f692d89-9d5c-4f33-856a-237ea67d06df" />


## APPLY

### Actividad 05

**Proceso de diseño:** Para esta obra se me ocurrio jugar con el bass o frecuencias bajas de las canciones y como podrian infuenciar el comportamiento de un flock que siga el mouse todo el tiempo. En esta caso, quise hacer como si la potencia del bass provocara que los boids se dispersaran como si un terremoto los hiciera separarse.


**codigo fuente:**
```js
let flock = [];
let song;
let analyzer;
let rms = 0; 
let addingBoids = false;

function preload() {
  song = loadSound("LUA NA PRAÇA.mp4"); // cambia por tu archivo
}

function setup() {
  createCanvas(800, 600);
  analyzer = new p5.Amplitude();
  analyzer.setInput(song);
}

function draw() {
  background(10); // Fondo oscuro para resaltar el efecto neón

  // Potencia de la canción
  let level = analyzer.getLevel();
  rms = lerp(rms, level, 0.6);

  // Mapear potencia a dispersión
  let dispersion = map(rms, 0, 0.05, 0.01, 1); 
  dispersion = constrain(dispersion, 0.2, 8.0);

  // Dibujar y actualizar boids
  for (let boid of flock) {
    boid.run(flock, dispersion, rms);
  }

  // Generar boids con la barra espaciadora
  if (addingBoids) {
    flock.push(new Boid(width / 2, height / 2));
  }

  // Barra de potencia
  fill(0, 200, 0);
  noStroke();
  rect(20, height - 30, rms * 600, 20);
}

// ---------------------------------------------
// Controles
// ---------------------------------------------
function keyPressed() {
  if (key === ' ') {
    addingBoids = true;
  } else if (key === 'p') {
    if (song.isPlaying()) {
      song.pause();
    } else {
      song.play();
    }
  }
}

function keyReleased() {
  if (key === ' ') {
    addingBoids = false;
  }
}

// ---------------------------------------------
// Clase Boid
// ---------------------------------------------
class Boid {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.velocity.setMag(random(2, 4));
    this.acceleration = createVector();
    this.maxforce = 0.1;
    this.maxspeed = 8; // velocidad fija
  }

  run(boids, dispersion, rms) {
    this.flock(boids, dispersion);
    this.update();
    this.edges();
    this.show(rms);
  }

  flock(boids, dispersion) {
    let sep = this.separate(boids);
    let ali = this.align(boids);
    let coh = this.cohesion(boids);

    let sepWeight = map(dispersion, 0.2, 8.0, 0.01, 10.0);
    let cohWeight = map(dispersion, 0.2, 8.0, 4.0, 0.2);

    sep.mult(sepWeight);
    ali.mult(1.0);
    coh.mult(cohWeight);

    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);

    // Fuerza para seguir el mouse
    let seekForce = this.seek(createVector(mouseX, mouseY));
    seekForce.mult(1.5); 
    this.applyForce(seekForce);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  seek(target) {
    let desired = p5.Vector.sub(target, this.position);
    desired.setMag(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    return steer;
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  show(rms) {
    let w = map(rms, 0, 0.1, 2, 5);

    // Paleta neón → de blanco a rojo pasando por salmón, naranja y fucsia
    let palette = [
      color(255, 255, 255),   // blanco
      color(255, 160, 120),   // salmón
      color(255, 120, 40),    // naranja brillante
      color(255, 60, 180),    // fucsia neón
      color(255, 0, 60)       // rojo fuerte
    ];

    // Mapear rms a la paleta
    let t = constrain(map(rms, 0, 0.5, 0, palette.length - 1), 0, palette.length - 1);
    let idx = floor(t);
    let col = (idx < palette.length - 1) 
      ? lerpColor(palette[idx], palette[idx + 1], t - idx)
      : palette[palette.length - 1];

    // Efecto de neón estilo contornos brillantes
    noFill();
    for (let i = 6; i > 0; i--) {
      strokeWeight(w + i * 2);
      stroke(red(col), green(col), blue(col), 30 - i * 4); // capas más transparentes
      point(this.position.x, this.position.y);
    }

    // Núcleo brillante
    strokeWeight(w);
    stroke(col);
    point(this.position.x, this.position.y);
  }

  // ------------------------------
  // Reglas de flocking
  // ------------------------------
  separate(boids) {
    let desiredseparation = 25.0;
    let steer = createVector();
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if ((d > 0) && (d < desiredseparation)) {
        let diff = p5.Vector.sub(this.position, other.position);
        diff.normalize();
        diff.div(d);
        steer.add(diff);
        count++;
      }
    }
    if (count > 0) {
      steer.div(count);
    }
    if (steer.mag() > 0) {
      steer.setMag(this.maxspeed);
      steer.sub(this.velocity);
      steer.limit(this.maxforce);
    }
    return steer;
  }

  align(boids) {
    let neighbordist = 50;
    let sum = createVector();
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if ((d > 0) && (d < neighbordist)) {
        sum.add(other.velocity);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.setMag(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.velocity);
      steer.limit(this.maxforce);
      return steer;
    } else {
      return createVector();
    }
  }

  cohesion(boids) {
    let neighbordist = 50;
    let sum = createVector();
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.position, other.position);
      if ((d > 0) && (d < neighbordist)) {
        sum.add(other.position);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum);
    } else {
      return createVector();
    }
  }
}
```

**[Enlace](https://editor.p5js.org/luly903/sketches/3o5QcWAzH)**

**Imagen:**<img width="795" height="668" alt="image" src="https://github.com/user-attachments/assets/4a1694ef-e049-4639-94dd-1a082ba0fd3d" />

<img width="762" height="671" alt="image" src="https://github.com/user-attachments/assets/aa62306f-047c-4d9d-8d35-eefae7408531" />

<img width="798" height="662" alt="image" src="https://github.com/user-attachments/assets/50602ae5-8c81-4bec-9c29-a4c1865a3a55" />















