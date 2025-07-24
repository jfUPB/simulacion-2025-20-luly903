# Unidad 1

## 🛠 Fase: Apply

### Actividad 8

para esta actividad decidi crear un sketch interactivo donde se pone a prueba la creatividad del usuario para crear arte cuando no tiene control absoluto de sus herramientas a emplear. El sketch consiste en un fondo nublado en escala de grises (que sera diferente en cada iteracion del sketch), y en un trazo negro el cual siempre se encuentra desplazandose hacia la derecha y cada vez que llega al final de mapa regresa a su punto de inicio en el borde izquierdo del mismo.

Para interactuar con el sketch estan las siguientes opciones:

**flecha arriba:** hacer que el trazo se desplaze hacia arriba
**flecha abajo:** hacer que el trazo se desplaze hacia abajo

**flecha izquierda:** reduce la velocidad de desplazamiento del trazo en una cantidad aleatoria.
**flecha derecha:** aumenta la velocidad de desplazamiento del trazo en una cantidad aleatoria.

**tecla enter:** cambiar el color del trazo a un color aleatorio

**tecla +:** aumenta el tamaño del trazo
**tecla -:** disminuye el tamaño del trazo


coidgo del sketch:

``` js
let x = 0;
let y = 200;
let size = 10;
let speed = 0.2;
let previousSize = 0;


let value1 = 0;
let value2 = 0;
let value3 = 0;

function setup() {
  createCanvas(600, 400);
  background(242, 210, 67);
  
  let noiseLevel = 250;
  let noiseScale = 0.001;

  // Iterate from top to bottom.
  for (let y = 0; y < height; y += 1) {
    // Iterate from left to right.
    for (let x = 0; x < width; x += 1) {
      // Scale the input coordinates.
      let nx = noiseScale * x;
      let ny = noiseScale * y;

      // Compute the noise value.
      let c = noiseLevel * noise(nx, ny);

      // Draw the point.
      stroke(c);
      point(x, y);
    }
  } 
}

function draw() {
  background(255,255,255,0);
  
   let deltaX = speed * deltaTime;

  // Update the x variable.
  x += deltaX;

  // Reset x to 0 if it's
  // greater than 100.
  if (x > 600)  {
    x = 0;
  }
  noStroke();
  fill(value1, value2, value3);
  circle(x, y, size);
  
    if (keyIsDown(UP_ARROW) === true) {
    y -= 2;
  }

  if (keyIsDown(DOWN_ARROW) === true) {
    y += 2;
  }
  
  if (keyIsDown(107) === true) {
     size +++ 5;
  }

  if (keyIsDown(109) === true) {
     size --- 5;
  }
  
}

function keyReleased() {
  if (keyCode === ENTER) {
    value1 = random(255);
    value2 = random(255);
    value3 = random(255); 
  }
  
   if (keyCode === LEFT_ARROW) {
    previousSpeed = speed;
    speed =- randomGaussian(0.05,0.1); 
     if(speed <=0 || speed >= previousSpeed){
       
       speed = 0;
     }

     
     
  }
  
  if (keyCode === RIGHT_ARROW) {
    previousSpeed = speed;
    speed =+ randomGaussian(0.05,0.1); 
     if(speed <=0 || speed >= 0.25 || previousSpeed <= speed){
       
       speed = 0.25;
     }

  }    
  
}
```
[link al sketch](https://editor.p5js.org/luly903/sketches/gjrs6qWoL)

captura de prueba del sketch:

<img width="760" height="513" alt="image" src="https://github.com/user-attachments/assets/fd036198-5e9f-4f29-b449-5dcb0668de9b" />




