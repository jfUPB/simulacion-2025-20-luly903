# Unidad 3


## 🛠 Fase: Apply


### Actividad 10

**Explica cómo modelaste el problema de los n-cuerpos en tu obra.**

[link a la obra]()

**código**
```js
let movers = [];
let mode = "nbody"; // "nbody" o "fall"
let t = 0; // tiempo global para ruido

function setup() {
  createCanvas(800, 600);
  for (let i = 0; i < 8; i++) {
    movers.push(new Mover(random(15, 30), random(width), random(height / 2)));
  }
}

function draw() {
  background(240);

  if (mode === "nbody") {
    // interacción gravitacional entre los cuerpos
    for (let i = 0; i < movers.length; i++) {
      for (let j = 0; j < movers.length; j++) {
        if (i !== j) {
          let force = movers[j].attract(movers[i]);
          movers[i].applyForce(force);
        }
      }
    }
  } else if (mode === "fall") {
    // gravedad hacia abajo (con probabilidad de Levy flight hacia arriba)
    for (let mover of movers) {
      let gravity;
      if (random(1) < 0.15) { // 15% de probabilidad de salto Levy
        let step = levyFlight(); 
        gravity = createVector(0, -abs(step)); // hacia arriba
      } else {
        gravity = createVector(0, 0.3 * mover.mass); // caída normal
      }
      mover.applyForce(gravity);
    }
  }

  // Dibujar conexiones (varillas estilo Calder)
  stroke(0);
  strokeWeight(2);
  for (let i = 0; i < movers.length - 1; i++) {
    line(movers[i].pos.x, movers[i].pos.y, movers[i + 1].pos.x, movers[i + 1].pos.y);
  }

  // Actualizar y mostrar
  for (let mover of movers) {
    mover.update();
    mover.display();
    mover.checkEdges();
  }

  t += 0.01; // avanza el tiempo del ruido
}

class Mover {
  constructor(m, x, y) {
    this.mass = m;
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);

    // offsets de ruido únicos para cada objeto
    this.noiseX = random(1000);
    this.noiseY = random(1000);

    this.color = random([
      color(255, 0, 0),   // rojo
      color(0, 0, 255),   // azul
      color(255, 255, 0), // amarillo
      color(0),           // negro
      color(255)          // blanco
    ]);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    // Fuerza basada en ruido Perlin, centrada en 0
    let windX = (noise(this.noiseX + t) - 0.5) * 0.8;
    let windY = (noise(this.noiseY + t) - 0.5) * 0.8;
    let wind = createVector(windX, windY);
    this.applyForce(wind);

    // Fuerza suave hacia el centro
    let center = createVector(width / 2, height / 2);
    let dirToCenter = p5.Vector.sub(center, this.pos);
    dirToCenter.setMag(0.02);
    this.applyForce(dirToCenter);

    // Movimiento normal
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  attract(other) {
    let force = p5.Vector.sub(this.pos, other.pos);
    let distance = constrain(force.mag(), 20, 120);
    let strength = (0.4 * this.mass * other.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  display() {
    noStroke();
    fill(this.color);
    ellipse(this.pos.x, this.pos.y, this.mass * 2);
  }

  checkEdges() {
    // Limitar dentro del canvas
    if (this.pos.x < this.mass) {
      this.pos.x = this.mass;
      this.vel.x *= -0.5;
    } else if (this.pos.x > width - this.mass) {
      this.pos.x = width - this.mass;
      this.vel.x *= -0.5;
    }

    if (this.pos.y < this.mass) {
      this.pos.y = this.mass;
      this.vel.y *= -0.5;
    } else if (this.pos.y > height - this.mass) {
      this.pos.y = height - this.mass;
      this.vel.y *= -0.5;
    }
  }
}

function mousePressed() {
  if (mouseButton === LEFT) {
    mode = "fall";
  } else if (mouseButton === RIGHT) {
    mode = "nbody";
  }
  return false; // evita menú contextual en clic derecho
}

// -------------------------
// Levy flight generator
// -------------------------
function levyFlight() {
  // Usamos distribución de potencias
  let beta = 1.5; // parámetro típico Levy
  let u = randomGaussian(0, 1);
  let v = randomGaussian(0, 1);
  let step = u / pow(abs(v), 1 / beta);
  return constrain(step, -5, 5); // limitar salto extremo
}

```

**Imagen de la obra:**

<img width="884" height="667" alt="image" src="https://github.com/user-attachments/assets/dbed5523-feee-4f53-a93c-fc697f3f1000" />

