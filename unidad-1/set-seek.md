# Unidad 1

## 🔎 Fase: Set + Seek


### Actividad 01

- la aleatoriedad permite que el arte generativo sea espontaneo, dinamico y único en cada experiencia.

### Actividad 02

- la aleatoriedad en la obra de sofia permite que la experiencia sea única e irrepetible, pues cada persona experimentara un resultado diferente al ejecutar el programa, lo que lo vuelve único.

- En mi perfil profesional como artista y animadora, la aleatoriedad muchas veces ayuda a que un producto se sienta mas orgánico y espontaneo, por ejemplo, si fuera a crear un cuarto desordenado, ubicar los elementos que lo conforman de forma aleatoria permite resaltar el toque "desordenado" de la habitación, pues puede que hayan cosas que no esten en el lugar que deberían.

### Actividad 3

-   En el codigo cambie el color del punto a puntar para qu e en lugar de que fuera solo negro, en su lugar fuera un color aleatorio, lo que espero es que cada vez que se pinte un punto nuevo este sea de un color aleatorio en lugar de negro siempre.

```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 5;
    this.y = height / 2;
  }

  show() {
    stroke(random(255));
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```

- despues de ejecutar el codigo, se cumplio lo que supuse hasta cierto punto, pues aunque el color SI cambiaba en cada paso, el rango de colores fue en una escala de grises en lugar de una variedad de colores, probablemente debido al valor que puse dentro del random, ya que puse '255', en este caso no investigue bien como funciona el rango de colores y como se relacionan con el valor numerico que se ingresa, y solo sabia que '0' = negro y '255' = blanco.

- despues de realizar una breve investigacion, me di cuenta de que lo que pasaba es que hay diferentes maneras de invocar `stroke()`, y la manera en la que yo la invoque fue con `stroke(value)` por lo que de esta forma el stroke solo capta colores en escala de grises, hice una modificacion, cambiandolo a `stroke(values)`, mas especificamente escribi: `stroke(random(255),random(255),random(255));` para garantizar que fueran diferentes colores en la escala de rgb.

### Actividad 4

- en una distribucion uniforme todos los numeros posibles tienen la misma probabilidad de ocurrir, mientras que en una distribucion no uniforme habran ciertos numeros que tendran mas o menos posibilidad de ocurrir.}

```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(200);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(randomGaussian(1,4));
    if (choice == 0) {
      this.y--;
    } else if (choice == 1) {
      this.y++;
    } else if (choice == 2) {
      this.x--;
    } else {
      this.x++;
    }
  }
}
```
### Actividad 5
```
function setup() {
  createCanvas(500, 250);
   background(230);
}

function draw() {
   let x = randomGaussian(240, 10);
  let y = randomGaussian(120, 30);
  noStroke();
  fill(0, 20);
  square(x, y, 10);
}
```
[link al codigo del sketch](https://editor.p5js.org/luly903/sketches/i5mQtEm6e)

captura del sketch:

<img width="657" height="336" alt="image" src="https://github.com/user-attachments/assets/62c76e2d-0b02-4487-884a-2c5525b4b1e0" />


### Actividad 6

Para esta actividad hice 2 codigos

codigo 1:
```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com
// codigo modififcado por Juliana Monroy / luly903

function setup() {
  createCanvas(640, 400);
  background(230);
}

function draw() {
  let x = 0;
  let y = 120;
  let r = random(1);
  if (r < 0.01) {
   x= 400;
  } else {
   x = random(300);
  }
  noStroke();
  fill(0, 10);
  circle(x, y, 16);
}
```
[link al codigo del sketch](https://editor.p5js.org/natureofcode/sketches/Yk_eSiNOR)

captura del sketch:

<img width="818" height="523" alt="image" src="https://github.com/user-attachments/assets/a1ae6c37-bef3-43a3-a46c-85f3ea3b66e4" />

codigo 2:
```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com
//codigo modificado por Juliana Monroy / luly903

function setup() {
  createCanvas(640, 400);
  background(230);
}

function draw() {
  let x = random(640);
  let y = 120;
  let r = random(1);
  if (r < 0.01) {
   fill(random(255),random(255),random(255))
  } else {
   fill(0,10);
  }
  noStroke();
  
  circle(x, y, 30);
}
```
[link al codigo del sketch](https://editor.p5js.org/luly903/sketches/KMchQgZH-)

captura del sketch:

<img width="831" height="521" alt="image" src="https://github.com/user-attachments/assets/f75098e9-3d64-40d8-bc70-ebc185ac2f46" />

