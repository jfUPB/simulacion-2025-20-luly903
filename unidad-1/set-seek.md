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


