# Unidad 2


## 🛠 Fase: Apply

### Activida 08

**descripción**
El concepto

codigo
```js
let position1;
let position2;
let velocity1;
let velocity2;

let color1;
let color2;
let color3;
let color4;

let t = 0;
let increasing = true;

function setup() {
  createCanvas(600, 400);
  background(0, 0, 0);

  position1 = createVector(width / 2, height / 2); 
  position2 = createVector(width / 2, height / 2); 
  
  velocity1 = createVector(0, 0);
  velocity2 = createVector(0, 0);
  

  color1 = color('red');
  color2 = color('yellow');
  color3 = color('blue');
  color4 = color('cyan');
}

function draw() {
  background(0, 0, 0, 0.1);
  

  let colorOg = lerpColor(color1, color2, t);
  let colorOp = lerpColor(color3, color4, t);
  

  if (increasing) {
    t += 0.03;
    if (t >= 1) increasing = false;
  } else {
    t -= 0.03;
    if (t <= 0) increasing = true;
  }
  

  let minSpeedObject = 1;
  let maxSpeedObject = 8;
  let speedObject = lerp(minSpeedObject, maxSpeedObject, t);
  
 
  let mouse = createVector(mouseX, mouseY);
  let direction = p5.Vector.sub(mouse, position1);
  direction.normalize();
  

  velocity1 = direction.copy().mult(speedObject);
  position1.add(velocity1);
  
 
  velocity2 = direction.copy().mult(-speedObject);
  position2.add(velocity2);

 
  noStroke();
  fill(colorOg);
  circle(position1.x, position1.y, 48); 
  
  noStroke();
  fill(colorOp);
  circle(position2.x, position2.y, 48); 
}

```
