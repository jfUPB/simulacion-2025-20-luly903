# Evidencias de la unidad 7

## SET

### Actividad 01

**1.**

<img width="477" height="480" alt="image" src="https://github.com/user-attachments/assets/404b0c2e-7711-482e-9f3e-7b7dc4d32bf4" />

- en esta imagen las letras que conforman la palabra "gravity" caen al piso como si esta estuvieran siendo afectadas por la gravedad misma en lugar de estar simplemente estaticas en la pantalla.

<img width="479" height="478" alt="image" src="https://github.com/user-attachments/assets/4ee7f4e7-f67d-46fc-a898-c9424b0d119d" />

- en esta imagen la I esta angulada de tal forma que parece una salida, y la parte baja de la X estan anguladas de tal forma que parecen piernas, juntos hacen parecer como si la X estuviera yendo hacia una salida en la palabra "exit".

<img width="479" height="483" alt="image" src="https://github.com/user-attachments/assets/0d409ad1-48fb-4958-8f7a-2906e604cc67" />

- En esta imagen la segunda o de "moon" no solo es mas pequeña que la primera, sino que tambien esta ubicada un poco mas arriba de la primera "o", esto acompañadado con el fondo negro y las letras blancas, hacen parecer que la segunda "o" es una luna orbitando la primera "o" como si fuera un planeta.

**2.**

- para la palabra "movie" giraria la "v" para que parezca que esta proyectando algo y voltearia la "para que parezca una siila y poner la "i" ahi, de esta forma dar la ilusion de que alguien esta viendo una pelicula en un proyector.

- para la palabra "kite" haria que la "t" estuviera mas arriba y hacia la derecha y la giraria de tal moodo que pueda parecerse a una cometa, angularia la "k" para que parezca una silla y curvearia la "i" para que parezca una persona, de esta forma haciendo parecer que alguen esta volando una cometa.

- en la palabra "Alligator" ajustaria la "A" y la "r" para qu parezcan la boca y cola de un caiman.


## SEEK

### Actividad 02

**experimento 1:**

**codigo fuente:**
circle
```js
class Circle {
  constructor(x, y, r){
    this.r = r;
    this.body = Bodies.circle(x, y, this.r, {
      restitution: 0.8,
      friction: 0.3,
      density: 0.01,
      isStatic: false
    });
    Composite.add(engine.world, this.body);
  }
  
  followMouse(x, y) {
    Body.setPosition(this.body, { x: x, y: y });
  }

  display() {
    let pos = this.body.position;
    push();
    translate(pos.x, pos.y);
    fill(100, 180, 255);
    noStroke();
    circle(0, 0, this.r * 2);
    pop();
  }
}

```

trapezoid
```js
class Trap {
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    this.color = color(random(50, 255), random(50, 255), random(50, 255)); // color aleatorio

    this.body = Bodies.trapezoid(x, y, this.w, this.h, 0.4, {
      restitution: 0.2,
      friction: 0.8,
      density: 0.002
    });
    Composite.add(engine.world, this.body);
  }

  display() {
    let pos = this.body.position;
    let angle = this.body.angle;

    push();
    translate(pos.x, pos.y);
    rotate(angle);
    stroke(0);
    fill(this.color); // aplicar color aleatorio
    beginShape();
    for (let v of this.body.vertices) {
      vertex(v.x - pos.x, v.y - pos.y);
    }
    endShape(CLOSE);
    pop();
  }
}
```

ground
```js
class Ground {
    constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    
    this.body = Bodies.rectangle(x, y, this.w, this.h, {isStatic: true});
    Composite.add(engine.world, this.body);
    }
  
   display() {
    push();
    rectMode(CENTER);
    let x = this.body.position.x;
    let y = this.body.position.y; 
    translate(x, y);
    rect(0, 0, this.w, this.h);
    pop();
  }
  
}
```

sketch
```js
const { Engine, Body, Bodies, Composite } = Matter;

let engine;
let boxes = []; 
let ground;
let circulo;
let lastSpawn = 0;

function setup() {
  createCanvas(400, 400);
  engine = Engine.create();

  ground = new Ground(200, 380, 400, 20);
  circulo = new Circle(200, 200, 15);
}

function draw() {
  background(220);
  Engine.update(engine);

  circulo.followMouse(mouseX, mouseY);
  circulo.display();

  for (let b of boxes) {
    b.display();
  }

  ground.display();

  // crea un trapezoide cada 200ms mientras se presione la tecla A
  if (keyIsDown(65) && millis() - lastSpawn > 200) {
    boxes.push(new Trap(mouseX, mouseY, 60, 40));
    lastSpawn = millis();
  }
}
```

[link](https://editor.p5js.org/luly903/sketches/bhwcGfr7R)

**captura de pantalla:**

<img width="469" height="438" alt="image" src="https://github.com/user-attachments/assets/94cb33ff-7d91-49bb-8f71-4d6047bdea3d" />



**Experimento 2:**

**codigo fuente:**

sketch
```js
// p5.js + Matter.js: ejemplo estable con MouseConstraint correcto

const { Engine, World, Bodies, Mouse, MouseConstraint, Constraint } = Matter;

let engine, world;
let ground;
let boxes = [];
let circles = [];
let mConstraint;
let constraint;
let canvas;

function setup() {
  // crear canvas y guardar referencia
  canvas = createCanvas(800, 600);
  // crear engine
  engine = Engine.create();
  world = engine.world;

  // suelo estático
  ground = Bodies.rectangle(width / 2, height - 20, width, 40, { isStatic: true, friction: 1 });
  World.add(world, ground);

  // cuerpos de prueba
  for (let i = 0; i < 6; i++) {
    const b = Bodies.rectangle(random(100, 700), random(-100, 0), 50, 40, {
      restitution: 0.2,
      friction: 0.4,
      density: 0.002
    });
    boxes.push(b);
    World.add(world, b);
  }

  for (let i = 0; i < 4; i++) {
    const c = Bodies.circle(random(100, 700), random(-200, -20), 22, {
      restitution: 0.6,
      friction: 0.2,
      density: 0.001
    });
    circles.push(c);
    World.add(world, c);
  }

  // ejemplo de Constraint entre dos cuerpos (opcional)
  constraint = Constraint.create({
    bodyA: boxes[0],
    bodyB: circles[0],
    length: 140,
    stiffness: 0.04,
    render: { visible: false }
  });
  World.add(world, constraint);

  // --- MouseConstraint correcto ---
  // create() con canvas.elt y ajustar pixelRatio para que coordinates coincidan
  const canvasMouse = Mouse.create(canvas.elt);
  // ajustar pixel ratio según p5 (importante en displays retina / DPR != 1)
  canvasMouse.pixelRatio = pixelDensity();

  const mcOptions = {
    mouse: canvasMouse,
    constraint: {
      stiffness: 0.18,
      angularStiffness: 0.9,
      render: { visible: false }
    }
  };
  mConstraint = MouseConstraint.create(engine, mcOptions);
  World.add(world, mConstraint);

  // NO usar Runner.run ni Engine.run aquí. Usaremos Engine.update() dentro de draw()
}

function draw() {
  background(240);

  // actualizar el engine con deltaTime de p5 (ms)
  // deltaTime es el tiempo en ms desde el último frame en p5
  // si deltaTime es 0 (al inicio), usar 1000/60 por defecto
  const dt = deltaTime > 0 ? deltaTime : 1000 / 60;
  Engine.update(engine, dt);

  // dibujar suelo
  noStroke();
  fill(120);
  drawVertices(ground.vertices);

  // dibujar cajas
  fill(20, 120, 220);
  stroke(0);
  for (let b of boxes) {
    drawVertices(b.vertices);
  }

  // dibujar círculos
  fill(255, 140, 40);
  stroke(0);
  for (let c of circles) {
    drawVertices(c.vertices);
  }

  // dibujar constraint entre bodies (si existe)
  if (constraint && constraint.bodyA && constraint.bodyB) {
    stroke(180, 20, 20);
    strokeWeight(2);
    line(
      constraint.bodyA.position.x,
      constraint.bodyA.position.y,
      constraint.bodyB.position.x,
      constraint.bodyB.position.y
    );
    strokeWeight(1);
  }

  // dibujar la conexión del MouseConstraint cuando está agarrando algo
  if (mConstraint && mConstraint.body) {
    const bodyPos = mConstraint.body.position;
    const mousePos = mConstraint.mouse.position;
    stroke(0);
    strokeWeight(2);
    line(bodyPos.x, bodyPos.y, mousePos.x, mousePos.y);

    // opcional: colorear el cuerpo mientras lo arrastras
    push();
    fill(255, 255, 100, 150);
    noStroke();
    // intentar detectar si es circular (aprox. con radius) o poligonal:
    if (mConstraint.body.circleRadius) {
      const r = mConstraint.body.circleRadius;
      ellipse(mConstraint.body.position.x, mConstraint.body.position.y, r * 2);
    } else {
      drawVertices(mConstraint.body.vertices);
    }
    pop();
  }
}

// helper para dibujar un cuerpo a partir de sus vértices
function drawVertices(vertices) {
  beginShape();
  for (let v of vertices) vertex(v.x, v.y);
  endShape(CLOSE);
}
```
[link](https://editor.p5js.org/luly903/sketches/oF23uUggx)

**imagen: **

<img width="908" height="669" alt="image" src="https://github.com/user-attachments/assets/1ad1d7b8-5bc4-4d36-ab8c-64c2f8e9608c" />

**Conceptos segun yo:**

**Engine:** motor del sistema que se encarga de correr la simulacion.

**world:** El mundo/espacio que contiene todos los elementos fisicos de la simulacion.

**bodies:** conjunto de cuerpos con caracterisiticas y propiedades establecidas.

**constrain:** "cadena" invisible que conecta dos objetos juntos.

**MouseaConstrain:** constrain temporal entre un objeto y el mouse.

**dificultades:**
entender la documentacion escrita de matter.js, lo que mas me sirvio fue ir al canal de patt vira y ver sus otros ejercicios con matter.js, ya que la bilbioteca de ejemplos estan desactualizados o no funcionan, y la documentacion fue un poco confusa de entender. No pude crear el mouse constrain por mi cuenta por lo que chatgpt me tuvo que ayudar en cuanto la interaccion mouse-world en el experimento 2.

## APPLY

### Actividad 03





### Autoevaluacion

**nota propuesta:** 4

**justificacion:** realice 2 de las 3 actividades propuestas lo mejor que pude.

