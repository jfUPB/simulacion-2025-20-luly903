# Unidad 2


## 🛠 Fase: Apply

### Activida 08

**descripción**
- El concepto de mi siguiente obra es de simetria y reflejos, en este caso cuando la persona interactue con la obra usando el mouse, se generaran dos traos: uno que siga la posicion del mouse (en una velocidad que va creciendo y decreciendo linealmente)  y otro que vaya en direccion opuesta a este, de esta forma, el usuario puede dejar su imaginacion fluir y crear obras de arte simetricas, pues lo que este pasando en un lado del canvas se vera reflejado en el otro lado de este.

**Como aplico el motion 101**
- para esta obra aplico el motion 101 en la formq que dirijo el movimiento de los trazos con el mouse, en este caso, creo unos vectores posicion, velocidad, aceleracion y direccion. Aqui no uso solo el vector de velocidad para determinar la direccion del objeto ya que planeo agregar aceleracion, ademas de que la direccion es una determinada por el mouse.

**Que algoritmo de aceleracion planeo usar**
- En este caso decidi aprovechar de lerp() para hacer que la aceleracion oscile entre 2 valores diferentes, de esta forma la velocidad aumenta y disminuye constantemente. Este algoritmo lo decidi aplicar para añadir un toque de vida propia al trazo, pues al moverse pareciera que primero observa para donde va el mouse para despues seguirlo.

[link al codigo del sketch](https://editor.p5js.org/luly903/sketches/liaOuCkCY)

**codigo**
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
  circle(position1.x, position1.y, 30); 
  
  noStroke();
  fill(colorOp);
  circle(position2.x, position2.y, 30); 
}
```

**foto del arte generativo:**
<img width="696" height="461" alt="image" src="https://github.com/user-attachments/assets/5995b578-4a39-4c6d-a33d-ebc55f35c94b" />
