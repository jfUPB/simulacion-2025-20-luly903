# Unidad 3

## 🔎 Fase: Set + Seek


### Actividad 09

Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:

**obra 1: Friccion**
- para esta obra, se aplico la fuerza de friccion en el movimiento delcuadrado cada vez que el circulo (que controla el usuario) lo empuja, para este se aplico la formula 'Fr = -c*v', donde c es el coeficiente de friccion y v es el vector de velocidad del cuadrado, esto se hizo para hacer el movimiento del cuadrado mas realista y entretenido, ya que no se queda automaticamente quieto o sigue moviendose a la misma velocidad, sino que este va parando a medida que se desplaza. 

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

<img width="918" height="687" alt="image" src="https://github.com/user-attachments/assets/8905cf78-095c-421e-acf7-45eeaebad812" />




**obra 2: Resistencia del aire y fluidos**
- para esta obra se aplico tanto la resistencia del aire como del agua en cuanto al movimiento de las particulas o "gotas" que caen en la obra, para ello se aplico la formula 'Fd = -c * |v|^2 * v^', donde c es el coeficiente de drag o arrastre del aire o fluido, |v| es la magnitud del vector de velocidad de las gotas, y v^ es la direccion de del vector de velocidad de las gotas. Gracias a esto el usuario puede cambiar la velocidad de movimiento de las gotas dependiendo de la resistencia que se aplique al hacer click izquierdo.

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

[Enlace a la obra](https://editor.p5js.org/luly903/sketches/U3MyJcUWq)

Imagen:

<img width="702" height="501" alt="image" src="https://github.com/user-attachments/assets/54fd1b4d-9eae-470f-9ae6-2fecd371d529" />



**obra 3: Atraccion Gravitacional**
- para esta obra se aplico la fuerza de atraccion gravitacional para el movimiento de la "luna" alrededor de la "tierra", para ello se aplico la formula <img width="258" height="89" alt="image" src="https://github.com/user-attachments/assets/84c66784-697b-4f7c-973a-05ff9370d28c" /> donde G es la constante gravitacional, m1 y m2 son las masas de los objetos, r es la distancia entre la "luna" y la "tierra", y <img width="25" height="28" alt="image" src="https://github.com/user-attachments/assets/f2d6ba12-a2fa-47cc-8f88-258520d3f99b" /> es el vector unitario hacia la "tierra". esto para simular el movimiento de la "luna" con respecto a la fuerza de atraccion gravitacional de la "tierra", aun asi, el usuario puede interactuar con la "luna" y agarrarla y arrojarla por la pantalla, aun asi, esta eventualmente vuelve a su trayectoria original que pareciera orbitando la "tierra".


codigo:
```js
let ball;
let G = 20; 
let bounce = -0.7;
let dragging = false;
let prevMouse;
let planet; 

function setup() {
  createCanvas(600, 400);
  
 
  ball = {
    pos: createVector(width/2, 100),
    vel: createVector(2, 0),
    acc: createVector(0, 0),
    r: 15,
    mass: 1
  };


  planet = {
    pos: createVector(width/2, height/2),
    mass: 20,
    r: 30
  };

  prevMouse = createVector(mouseX, mouseY);
}

function draw() {
  background(0,0,0,50);

  if (!dragging) {
    // calcular fuerza de atracción gravitacional
    let force = p5.Vector.sub(planet.pos, ball.pos); // vector hacia el planeta
    let distanceSq = constrain(force.magSq(), 100, 10000); // evitar explosión numérica
    let strength = (G * ball.mass * planet.mass) / distanceSq;
    force.setMag(strength);

    // aplicar fuerza
    ball.acc.add(force);

    // integrar movimiento
    ball.vel.add(ball.acc);
    ball.pos.add(ball.vel);
    ball.acc.mult(0);

    
    if (ball.pos.x - ball.r < 0 || ball.pos.x + ball.r > width) {
      ball.vel.x *= bounce;
      ball.pos.x = constrain(ball.pos.x, ball.r, width - ball.r);
    }
    if (ball.pos.y - ball.r < 0 || ball.pos.y + ball.r > height) {
      ball.vel.y *= bounce;
      ball.pos.y = constrain(ball.pos.y, ball.r, height - ball.r);
    }
  } else {
    // seguir el mouse cuando se arrastra
    ball.pos.set(mouseX, mouseY);

    // calcular velocidad según arrastre
    let mouseNow = createVector(mouseX, mouseY);
    ball.vel = p5.Vector.sub(mouseNow, prevMouse);
  }


  fill(154, 255, 0);
  ellipse(planet.pos.x, planet.pos.y, planet.r*2);

 
  noStroke();
  fill(255, 255, 255);
  ellipse(ball.pos.x, ball.pos.y, ball.r*2);

  // actualizar referencia del mouse
  prevMouse.set(mouseX, mouseY);


  fill(255);
  textSize(16);
  text("Arrastra la pelota y suéltala: gravedad hacia el planeta", 20, 30);
}

function mousePressed() {
  // verificar si el mouse está dentro de la pelota
  let d = dist(mouseX, mouseY, ball.pos.x, ball.pos.y);
  if (d < ball.r) {
    dragging = true;
    ball.vel.set(0, 0);
  }
}

function mouseReleased() {
  dragging = false;
}
```

[Enlace a la obra](https://editor.p5js.org/luly903/sketches/7cIdJH-uh)

Imagen:

<img width="692" height="476" alt="image" src="https://github.com/user-attachments/assets/00312ab3-b297-4728-8df4-088cf09ca8d4" />






