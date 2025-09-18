# Evidencias de la unidad 5

## Actividad 02

**¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**
- En todos los ejemplos, la creacion y desparicion de las particulas se gestiona en la clase emitter que se encarga de crear las particulas, guardarlas y eliminarlas despues de una cantidad de tiempo determinada, de esta forma, el sistema no se satura ya que cada vez que una particula realiza su recorrido y vive el tiempo debido, este no solo desaparece sino que tambien es eliminado de la memoria, optimizando asi la gestion de la misma.


**modificacion codigos:**

*ejemplo 4.2: An array of particles:* agregue 3 variables random() R, G Y B en la clase particle para randomizar los colores de las particulas (Unidad 1).

codigo fuente:

particle
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.R = random(255);
    this.G = random(255);
    this.B = random(255);
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(this.R, this.G, this.B, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return (this.lifespan < 0.0);
  }
}
```
sketch
```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let particles = [];

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  particles.push(new Particle(width / 2, 20));

  // Looping through backwards to delete
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      //remove the particle
      particles.splice(i, 1);
    }
  }
}
```
[link al sketch](https://editor.p5js.org/luly903/full/j3Ap0QYie)

imagen:

<img width="693" height="258" alt="image" src="https://github.com/user-attachments/assets/a8ab7e63-5966-4b43-8146-7ebf8ce1c50c" />


*ejemplo 4.4: A system of systems:* Agregue un vector de velocidad a la clase emitter una funcion update para aplicar el motion 101 en el origen del emitter para que este se mueva de arriba a abajo mientras genera las particulas (Unidad 2).

codigo fuente:

emitter
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.velocity = createVector(0, 0.5);

  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }
  
  update() {
    this.origin.add(this.velocity);
    
     if (this.origin.y > height || this.origin.y < 0) {
      this.velocity.y *= -1;
    }
  
  }
  
  run() {
    // Looping through backwards to delete
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        // Remove the particle
        this.particles.splice(i, 1);
      }
      
      this.update();
      
    }

    // Run every particle
    // ES6 for..of loop
    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
    // https://www.youtube.com/watch?v=Y8sMnRQYr3c
    // for (let particle of this.particles) {
    //   particle.run();
    // }

    // Filter removes any elements of the array that do not pass the test
    // I am also using ES6 arrow snytax
    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions
    // https://www.youtube.com/watch?v=mrYMzpbFz18
    // this.particles = this.particles.filter(particle => !particle.isDead());

    // Without ES6 arrow code would look like:
    // this.particles = this.particles.filter(function(particle) {
    //   return !particle.isDead();
    // });
  }
  
   
  
  
  
}
```

particles
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}
```

sketch
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Particles are generated each cycle through draw(),
// fall with gravity and fade out over time
// A ParticleSystem object manages a variable size
// list of particles.

// an array of ParticleSystems
let emitters = [];

function setup() {
  createCanvas(640, 240);
  let text = createP("click to add particle systems");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}
```

[Link al sketch](https://editor.p5js.org/luly903/full/O5NLjdHy2)

imagen: 

<img width="700" height="273" alt="image" src="https://github.com/user-attachments/assets/a40f627f-1475-4721-afce-6fd729ede826" />



*ejemplo 4.5: a Particle System with Inheritance and Polymorphism:* modifique la clase emetitter para que este colgara de un pendulo que se mueve de forma oscilatoria mientras genera las particulas.

codigo fuente:
emitter
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Emitter {
  constructor(x, y) {
  // Punto de anclaje (arriba del péndulo)
    this.anchor = createVector(x, y);

    // Parámetros del péndulo
    this.r = 150;            // longitud del péndulo
    this.angle = PI / 4;     // ángulo inicial (45°)
    this.aVelocity = 0.0;    // velocidad angular
    this.aAcceleration = 0.0;// aceleración angular
    this.damping = 0.9999;    // amortiguación para que no oscile infinito
    this.gravity = 0.4;      // "fuerza" de la gravedad

    // Posición del emitter calculada desde el ángulo
    this.position = createVector(
      this.anchor.x + this.r * sin(this.angle),
      this.anchor.y + this.r * cos(this.angle)
    );

    // Partículas
    this.particles = [];
  }
  
   applyForce(force) {
    this.acceleration.add(force);
  }
  
   update() {
     // Fórmula física del péndulo
    // a = (-g / r) * sin(theta)
    this.aAcceleration = (-1 * this.gravity / this.r) * sin(this.angle);

    // Motion angular: a → v → θ
    this.aVelocity += this.aAcceleration;
    this.aVelocity *= this.damping;
    this.angle += this.aVelocity;

    // Actualizar posición según el ángulo
    this.position.x = this.anchor.x + this.r * sin(this.angle);
    this.position.y = this.anchor.y + this.r * cos(this.angle);

    // Crear nueva partícula en la posición actual del emitter
    this.particles.push(new Particle(this.position.x, this.position.y));

    // Actualizar partículas
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

   showAnchor() {
    fill(200, 100, 100);
    ellipse(this.anchor.x, this.anchor.y, 12, 12);
    stroke(200);
    line(this.anchor.x, this.anchor.y, this.position.x, this.position.y);
  }

  addParticle() {
    let r = random(1);
    if (r < 0.5) {
      this.particles.push(new Particle(this.position.x, this.position.y));
    } else {
      this.particles.push(new Confetti(this.position.x, this.position.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

```

particle
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

```

confetti
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Child class constructor
class Confetti extends Particle {
  // Override the show method
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);

    rectMode(CENTER);
    fill(127, this.lifespan);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}

```

sketch
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Particles are generated each cycle through draw(),
// fall with gravity and fade out over time
// A ParticleSystem object manages a variable size
// list of particles.


let emitter;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 20);
}

function draw() {
  background(255);
  emitter.update();
  emitter.showAnchor();
  emitter.addParticle();
  emitter.run();
}
```


[Link al sketch](https://editor.p5js.org/luly903/full/TAlPIYPTJ)

imagen: 

<img width="701" height="263" alt="image" src="https://github.com/user-attachments/assets/2506b55f-29a1-421b-bbbd-757374b6c0d7" />


ejemplo 4.6: a Particle System with Forces.








## Actividad 03

**concepto:** una varita magica que tenga destellos saliendo de este y se pueda usar para dibujar en el canvas usando el mouse, con la posibilidad de cmabiar de color cuando se oprima la tecla espacio y el canvas se limpie cuando se oprima la tecla SHIFT. El proposito es que el mismo usuario pueda crear su propia obra de arte de una forma divertida y dinamica.

**Codigo fuente**
```js
let particles = [];
let strokes = [];
let currentStroke = null;
let currentColor;

const MAX_STROKES = 100;   // máximo de trazos en memoria
const MAX_PARTICLES = 200; // máximo de partículas en memoria

function setup() {
  createCanvas(800, 600);
  background(20);
  currentColor = color(random(255), random(255), random(255));
}

function draw() {
  background(20, 40);

  // Posición de la varita
  let wandX = mouseX;
  let wandY = mouseY;
  let wandLength = 50;
  let wandThickness = 8;

  let angle = PI / 4; // fijo de 45°

  // Dibuja la varita
  push();
  translate(wandX, wandY);
  rotate(angle);
  fill(255);
  noStroke();
  rectMode(CENTER);
  rect(0, 0, wandLength, wandThickness, 4);
  pop();

  // Punta de la varita
  let tipX = wandX + cos(angle) * -(wandLength / 2);
  let tipY = wandY + sin(angle) * -(wandLength / 2);

  // Crear un nuevo trazo si el mouse está presionado
  if (mouseIsPressed) {
    if (!currentStroke) {
      currentStroke = new Stroke(currentColor);
      strokes.push(currentStroke);

      // Limitar cantidad de trazos
      if (strokes.length > MAX_STROKES) {
        strokes.shift();
      }
    }
    currentStroke.addPoint(tipX, tipY);
  } else {
    currentStroke = null;
  }

  // Dibujar todos los trazos
  for (let s of strokes) {
    s.show();
  }

  // Emitir nueva partícula
  let type = random() < 0.5 ? "circle" : "triangle";
  particles.push(new Particle(tipX, tipY, currentColor, type));

  // Limitar cantidad de partículas
  if (particles.length > MAX_PARTICLES) {
    particles.splice(0, particles.length - MAX_PARTICLES);
  }

  // Actualizar y mostrar partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].applyForce(createVector(0, 0.05));
    particles[i].update();
    particles[i].show();
    if (particles[i].finished()) {
      particles.splice(i, 1);
    }
  }
}

// ==== CLASES BASE ====

class Drawable {
  show() {}
}

// Stroke hereda de Drawable
class Stroke extends Drawable {
  constructor(c) {
    super();
    this.points = [];
    this.c = c;
  }

  addPoint(x, y) {
    this.points.push(createVector(x, y));
  }

  show() {
    noFill();

    strokeWeight(12);
    stroke(red(this.c), green(this.c), blue(this.c), 40);
    beginShape();
    for (let pt of this.points) curveVertex(pt.x, pt.y);
    endShape();

    strokeWeight(8);
    stroke(red(this.c), green(this.c), blue(this.c), 80);
    beginShape();
    for (let pt of this.points) curveVertex(pt.x, pt.y);
    endShape();

    strokeWeight(6);
    stroke(red(this.c), green(this.c), blue(this.c), 200);
    beginShape();
    for (let pt of this.points) curveVertex(pt.x, pt.y);
    endShape();
  }
}

// Particle hereda de Drawable
class Particle extends Drawable {
  constructor(x, y, c, type) {
    super();
    this.pos = createVector(x, y);
    this.vel = createVector(random(-1, 1), random(-2, -0.5));
    this.acc = createVector(0, 0);
    this.lifetime = 255;
    this.size = random(4, 8);
    this.c = c;
    this.type = type; // "circle" o "triangle"
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifetime -= 3;
  }

  finished() {
    return this.lifetime < 0;
  }

  show() {
    noStroke();
    for (let r = 20; r > 0; r -= 5) {
      let alpha = map(r, 20, 0, 0, this.lifetime);
      fill(red(this.c), green(this.c), blue(this.c), alpha / 3);
      if (this.type === "circle") {
        ellipse(this.pos.x, this.pos.y, this.size + r);
      } else {
        push();
        translate(this.pos.x, this.pos.y);
        rotate(frameCount * 0.05);
        triangle(
          0, -this.size - r,
          -this.size - r, this.size + r,
          this.size + r, this.size + r
        );
        pop();
      }
    }

    fill(red(this.c), green(this.c), blue(this.c), this.lifetime);
    if (this.type === "circle") {
      ellipse(this.pos.x, this.pos.y, this.size);
    } else {
      push();
      translate(this.pos.x, this.pos.y);
      rotate(frameCount * 0.05);
      triangle(
        0, -this.size,
        -this.size, this.size,
        this.size, this.size
      );
      pop();
    }
  }
}

// ==== INPUTS ====

function keyPressed() {
  if (key === ' ') {
    currentColor = color(random(255), random(255), random(255));
  }
  if (keyCode === SHIFT) {
    strokes.length = 0;
    particles.length = 0;
  }
}
```

imagenes de la obra:

<img width="895" height="667" alt="image" src="https://github.com/user-attachments/assets/c3e6ca92-2d56-4f77-b8fc-a1ba586f9589" />

<img width="900" height="676" alt="image" src="https://github.com/user-attachments/assets/752e6fdc-7db8-48c4-ad06-8c4c3f6bc16f" />


<img width="896" height="680" alt="image" src="https://github.com/user-attachments/assets/c1827b26-da2e-4e88-bf1a-12c353797bd4" />



