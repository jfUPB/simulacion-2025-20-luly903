# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Aplique los concetos del movimiento angular y oscilatorio en el pendulo principal que uno puede mover y arrastrar a otros lados

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
>

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
> Para la interacción hice que el pendulo pueda ser maniulado por el ususario a traves del mouse, al hacer click izquierdo sobre el cuadrado blanco del pendulo, este segura la trayectoria del mousemientras este seleccionado, una vez se suelte el click izquierdo, este se comportara como una mezcla entre pendulo y resorte estirandose y contrayendose al mismo tiempo que oscila.

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](https://editor.p5js.org/luly903/full/D16CPLkm5)

## Código de la obra 

``` js
class Pendulum {
  constructor(x, y, length) {
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.length = length;
    this.restLength = length;
    this.angle = PI / 4;

    this.angularVelocity = 0;
    this.angularAcceleration = 0;
    this.angularDamping = 0.995;

    this.radialVelocity = 0;
    this.radialAcceleration = 0;
    this.springConstant = 0.005;
    this.radialDamping = 0.06;

    this.bobSize = 24;
    this.isDragging = false;
    this.previousAngle = this.angle;

    this.orbiters = [];
    for (let i = 0; i < 7; i++) {
      this.orbiters.push({
        size: random(5, 15),
        restDistance: random(40, 70),
        currentDistance: 0,
        distanceVelocity: 0,
        springConstant: 0.08,
        damping: 0.12,
        angle: random(TWO_PI),
        speed: random(0.01, 0.03),
        hueOffset: random(360)
      });
    }
    for (let orb of this.orbiters) orb.currentDistance = orb.restDistance;

    this.couplingStrength = 0.35;
  }

  update() {
    if (!this.isDragging) {
      let gravity = 0.3;
      this.angularAcceleration = (-gravity / max(this.length, 0.0001)) * sin(this.angle);
      this.angularVelocity += this.angularAcceleration;
      this.angularVelocity *= this.angularDamping;
      this.angle += this.angularVelocity;

      let stretch = this.length - this.restLength;
      this.radialAcceleration = -this.springConstant * stretch - this.radialDamping * this.radialVelocity;
      this.radialVelocity += this.radialAcceleration;
      this.length += this.radialVelocity;
      this.length = constrain(this.length, 40, 0.8 * min(width, height));
    } else {
      let mouseVector = createVector(mouseX, mouseY);
      this.length = p5.Vector.dist(this.pivot, mouseVector);
      this.previousAngle = this.angle;
    }

    for (let orb of this.orbiters) {
      orb.angle += orb.speed;
    }

    let stretch = this.length - this.restLength;
    for (let orb of this.orbiters) {
      let target = orb.restDistance + this.couplingStrength * stretch;
      let delta = orb.currentDistance - target;
      let acc = -orb.springConstant * delta - orb.damping * orb.distanceVelocity;
      orb.distanceVelocity += acc;
      orb.currentDistance += orb.distanceVelocity;
      orb.currentDistance = constrain(orb.currentDistance, 10, 200);
    }
  }

  show() {
    this.bob.set(this.length * sin(this.angle), this.length * cos(this.angle), 0);
    this.bob.add(this.pivot);

    stroke(250);
    strokeWeight(2);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);

    fill(250);
    noStroke();
    square(this.bob.x, this.bob.y, this.bobSize);

    colorMode(HSB);
    noStroke();
    let t = millis() * 0.05;
    for (let orb of this.orbiters) {
      let ox = this.bob.x + orb.currentDistance * cos(orb.angle);
      let oy = this.bob.y + orb.currentDistance * sin(orb.angle);
      let hue = (t + orb.hueOffset) % 360;
      fill(hue, 80, 100);
      square(ox, oy, orb.size * 2);
    }
    colorMode(RGB);
  }

  clicked(mx, my) {
    if (dist(mx, my, this.bob.x, this.bob.y) < this.bobSize) {
      this.isDragging = true;
      this.radialVelocity = 0;
    }
  }

  stopDragging() {
    this.isDragging = false;
    let deltaAngle = this.angle - this.previousAngle;
    this.angularVelocity = constrain(deltaAngle, -0.2, 0.2);
  }

  drag() {
    if (this.isDragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(-diff.y, diff.x) - radians(90);
    }
  }
}

let pendulum;

function setup() {
  createCanvas(700, 600);
  background(0);
  pendulum = new Pendulum(width / 2, height / 2, 175);
}

function draw() {
  background(0, 0, 0, 0.01);
  pendulum.update();
  pendulum.show();
  pendulum.drag();
}

function mousePressed() {
  pendulum.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum.stopDragging();
}
```

## Captura de pantalla representativa

<img width="817" height="711" alt="image" src="https://github.com/user-attachments/assets/45614ce5-53d2-4a34-a055-355d64f6422f" />







