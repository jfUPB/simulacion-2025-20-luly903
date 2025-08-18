# Unidad 3

## 🔎 Fase: Set + Seek


### Actividad 09

Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:

**obra 1: Friccion**
-

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

[Enlace a la obra]()

Imagen:

**obra 2: Resistencia del aire y fluidos**
-

codigo:
```js

```

[Enlace a la obra]()

Imagen:


**obra 3: Atraccion Gravitacional**
-

codigo:
```js

```

[Enlace a la obra]()

Imagen:


