# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 01

**¿Cómo funciona la suma dos vectores en p5.js?**
- En p5.js, la suma de vectores se realiza sumando las variables que contienen dichos vectores, y la suma es una suma convencional de vectores, es decir, de componente a componenete.

**¿Por qué esta línea position = position + velocity; no funciona?**
- porque p5.js puede sumar vecotres, no escalares.

### Actividad 02

**¿Qué tuviste que hacer para hacer la conversión propuesta?**
- cambiar las variables que contenian la posicion (x,y) del walker por un vector que realizara lo mismo.

**Muestra el código que utilizaste para resolver el ejercicio.**
- codigo para el walker vectorizado:
```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;


function setup() {
  createCanvas(640, 240);
  background(255);
  walker = new Walker(width/2,height/2);
  
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor(_x,_y) {
    
    this.position = createVector(_x,_y);  
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
}
```











```
