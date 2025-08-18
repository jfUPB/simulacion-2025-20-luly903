# Unidad 3

## 🔎 Fase: Set + Seek


### Actividad 09

Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:

**obra 1: Friccion**
- para esta obra, se aplico la fuerza de friccion en el movimiento delcuadrado cada vez que el circulo (que controla el usuario) lo empuja, para este se aplico la formula de Fr = -c*v, donde c es el coeficiente de friccion y v es el vector de velocidad del cuadrado, esto se hizo para hacer el movimiento del cuadrado mas realista y entretenido, ya que no se queda automaticamente quieto o sigue moviendose a la misma velocidad, sino que este va parando a medida que se desplaza.

codigo:
```js
let circle, box;
let keys = {};

function setup() {
  createCanvas(800, 600);
  
  circle = {
    pos: createVector(100, height/2),
    r: 25,
    speed: 3
  };
  
  box = {
    pos: createVector(400, height/2 - 25),
    vel: createVector(0, 0),
    size: 50,
    mass: 2,
    friction: 0.95
  };
}



function draw() {
  background(220);

  // Reiniciar vector de movimiento del círculo
  let vel = createVector(0, 0);

  if (keyIsDown(UP_ARROW) === true) {
  vel.y = -circle.speed;
  }
  
    if (keyIsDown(DOWN_ARROW) === true) {
    vel.y =  circle.speed;
  }
  
    if (keyIsDown(LEFT_ARROW) === true) {
    vel.x = -circle.speed;
  }

  if (keyIsDown(RIGHT_ARROW) === true) {
    vel.x =  circle.speed;
  }


  circle.pos.add(vel);

  
  circle.pos.x = constrain(circle.pos.x, circle.r, width - circle.r);
  circle.pos.y = constrain(circle.pos.y, circle.r, height - circle.r);

  // Colisión círculo-cuadrado
  if (circleBoxCollision(circle, box)) {
    let dir = p5.Vector.sub(
      createVector(box.pos.x + box.size/2, box.pos.y + box.size/2), 
      circle.pos
    ).normalize();
    box.vel.add(dir.mult(0.5 / box.mass));
  }

  box.pos.add(box.vel);
  box.vel.mult(box.friction);


  box.pos.x = constrain(box.pos.x, 0, width - box.size);
  box.pos.y = constrain(box.pos.y, 0, height - box.size);

 
  fill(50, 100, 200);
  ellipse(circle.pos.x, circle.pos.y, circle.r*2);

  
  fill(200, 100, 50);
  rect(box.pos.x, box.pos.y, box.size, box.size);
}

// Colisión círculo-cuadrado
function circleBoxCollision(c, b) {
  let x = constrain(c.pos.x, b.pos.x, b.pos.x + b.size);
  let y = constrain(c.pos.y, b.pos.y, b.pos.y + b.size);
  let dx = c.pos.x - x, dy = c.pos.y - y;
  return dx*dx + dy*dy < c.r*c.r;
}


```

[Enlace a la obra](https://editor.p5js.org/luly903/sketches/ekBhiNLJ-)

Imagen:

**obra 2: Resistencia del aire y fluidos**
-

codigo:
```js
let particles = [];
let medium = "aire"; // aire o agua

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < 40; i++) {
    particles.push({
      pos: createVector(random(width), random(-600, -30)),
      vel: createVector(0,0),
      acc: createVector(0,0),
      r: 10
    });
  }
}

function draw() {
  background(0);

  for (let p of particles) {
    // Gravedad
    let gravity = createVector(0, 0.98);
    p.acc.add(gravity);

    // Resistencia del medio
    let c = medium === "aire" ? 0.01 : 0.1;
    let drag = p.vel.copy();
    drag.normalize();
    let speedSq = p.vel.magSq();
    drag.mult(-c * speedSq);
    p.acc.add(drag);

    // Integración
    p.vel.add(p.acc);
    p.pos.add(p.vel);
    p.acc.mult(0);

    // Dibujar
    fill(100, 200, 250);
    ellipse(p.pos.x, p.pos.y, p.r*2);
    
    // Reset cuando salen
    if (p.pos.y > height + p.r) {
      p.pos.y = random(-600, -30);
      p.vel.set(0,0);
    }
  }

  // Texto
  fill(255);
  textSize(16);
  text("Medio: " + medium + " (clic para cambiar)", 20, 20);
}

function mousePressed() {
  medium = (medium === "aire") ? "agua" : "aire";
}

```

[Enlace a la obra]()

Imagen:


**obra 3: Atraccion Gravitacional**
-

codigo:
```js
let ball;
let gravity;  
let bounce = -0.7;   
let dragging = false;
let prevMouse;       

function setup() {
  createCanvas(600, 400);
  
 
  ball = {
    pos: createVector(width/2, 100),
    vel: createVector(0, 0),
    r: 25
  };

  gravity = createVector(0, 0.5); 
  prevMouse = createVector(mouseX, mouseY);
}

function draw() {
  background(0,0,0,50);

  if (!dragging) {
    // aplicar gravedad
    ball.vel.add(gravity);

    // actualizar posición
    ball.pos.add(ball.vel);

    // rebote en el piso
    if (ball.pos.y + ball.r > height) {
      ball.pos.y = height - ball.r;
      ball.vel.y *= bounce;
    }

    // rebote en paredes
    if (ball.pos.x - ball.r < 0 || ball.pos.x + ball.r > width || ball.pos.y - ball.r < 0 || ball.pos.y + ball.r > height) {
      ball.vel.x *= bounce;
      ball.vel.y *= bounce;
      ball.pos.x = constrain(ball.pos.x, ball.r, width - ball.r);
      ball.pos.y = constrain(ball.pos.y, ball.r, height - ball.r);
    }
  } else {
    // seguir el mouse cuando se arrastra
    ball.pos.set(mouseX, mouseY);

    // calcular velocidad según movimiento del mouse
    let mouseNow = createVector(mouseX, mouseY);
    ball.vel = p5.Vector.sub(mouseNow, prevMouse);
  }

  fill(100, 200, 255);
  ellipse(ball.pos.x, ball.pos.y, ball.r*2);

  // actualizar referencia del mouse
  prevMouse.set(mouseX, mouseY);

  // texto
  fill(255);
  textSize(16);
  text("Arrastra la pelota y suéltala para lanzarla", 20, 30);
}

function mousePressed() {
  // verificar si el mouse está dentro de la pelota
  let d = dist(mouseX, mouseY, ball.pos.x, ball.pos.y);
  if (d < ball.r) {
    dragging = true;
    ball.vel.set(0, 0); // resetea velocidad al agarrar
  }
}

function mouseReleased() {
  dragging = false; // al soltar, se lanza con la velocidad acumulada
}

```

[Enlace a la obra]()

Imagen:



