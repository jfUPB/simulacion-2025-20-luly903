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

 ### Actividad 03

```
let position;

function setup() {
    createCanvas(400, 400);
    posicion = createVector(6,9);
    console.log(posicion.toString());
    playingVector(posicion);
    console.log(posicion.toString());
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}
```

**¿Qué resultado esperas obtener en el programa anterior?**
- Esperaba que apareciera un punto en las coordenadas del vector posicion.

**¿Qué resultado obtuviste?**
- No aparecio nada

**Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.**
- paso por valor:
```
function cambiarNumero(num) {
    num = num + 10;
    console.log("Dentro de la función:", num);
}

let x = 5;
cambiarNumero(x);

console.log("Fuera de la función:", x);

```
- paso por referencia:
```
function cambiarObjeto(obj) {
    obj.nombre = "Carlos";  
    console.log("Dentro de la función:", obj);
}

let persona = { nombre: "Ana" };
cambiarObjeto(persona);

console.log("Fuera de la función:", persona);

```

**¿Qué tipo de paso se está realizando en el código?**
- en el codigo se esta realizando un paso por valor.

**¿Qué aprendiste?**
- la los conceptos de paso por valor y paso por referencia y que los diferencia.


### Actividad 04

**¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?**
- el metodo mag() sirve para calcular la longitud o magnitud de un vector 2D, la diferencia entre mag() y magsq() es que mags() calcula la magnitud al cuadrado.

**¿Para qué sirve el método normalize()?**
- escala los componentes de un objeto vector para q su magnitud sea 1.

**Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?**
- para encontrar la forma en la que 2 vectores se encuentran cruzados.

**El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?**
- la estatica devuelve un v

**Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.**
- el resultado de un producto cruz es un vector que se encuentra perpendicular al plano que forman los vectores que se usaron para la multiplicacion, su magnitud es igual al area del paralelogramo que formarian los dos vectores de los que resulta.

**¿Para que te puede servir el método dist()?**
- el metodo dist sirve para calcular la distancia entre dos puntos, esto serviria para saber que tan lejos se encuentran dos vecotres a partir del caculo de la distancia entre 2 puntos que se encuentren en dichos vectores.

**¿Para qué sirven los métodos normalize() y limit()?**
- sirven para regular y limitar la magnitud de un vector.



### Actividad 05

codigo modificado:
```

```

¿Cómo funciona lerp() y lerpColor()?
- 

¿Cómo se dibuja una flecha usando drawArrow()?
- 




