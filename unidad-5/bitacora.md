# Evidencias de la unidad 5

## Actividad 02

**¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?**
- En todos los ejemplos, la creacion y desparicion de las particulas se gestiona en la clase emitter que se encarga de crear las particulas, guardarlas y eliminarlas despues de una cantidad de tiempo determinada, de esta forma, el sistema no se satura ya que cada vez que una particula realiza su recorrido y vive el tiempo debido, este no solo desaparece sino que tambien es eliminado de la memoria, optimizando asi la gestion de la misma.






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

