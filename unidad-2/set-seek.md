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
let t = 0;  // parámetro de interpolación
let increasing = true;

function setup() {
  createCanvas(200, 200);
}

function draw() {
  background(200);
  
  let color = lerpColor('red','blue',t);
  

  let v0 = createVector(100, 100);  // origen
  let v1 = createVector(60, 0);     // vector rojo
  let v2 = createVector(0, 60);     // vector azul
  
  let startGreen = p5.Vector.add(v0, v1);       // inicio en punta de rojo
  let vecGreen = p5.Vector.sub(v2, v1);        // apunta hacia la punta de   azul

  let v3 = p5.Vector.lerp(v1, v2, t); //cambio de color de la flecha

  // Vectores principales
  drawArrow(startGreen, vecGreen, 'green');
  drawArrow(v0, v1, 'red');
  drawArrow(v0, v2, 'blue');
  drawArrow(v0, v3, color);

  // Flecha verde: desde la punta de v1 hasta la punta de v2
  
  

  // Animación de t (va de 0 → 1 → 0)
  if (increasing) {
    t += 0.01;
    if (t >= 1) increasing = false;
  } else {
    t -= 0.01;
    if (t <= 0) increasing = true;
  }
}

function drawArrow(base, vec, myColor) {
  push();
  stroke(myColor);
  strokeWeight(3);
  fill(myColor);
  translate(base.x, base.y);
  line(0, 0, vec.x, vec.y);
  rotate(vec.heading());
  let arrowSize = 7;
  translate(vec.mag() - arrowSize, 0);
  triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
  pop();
}
```

¿Cómo funciona lerp() y lerpColor()?
- lerp() realiza una interpolacion linear entre 2 valores especificos que se den, esto se puede usar para crear un movimiento entre 2 puntos.
- lerpColor() realiza una interpolacion linear entre 2 colores especificos, esto se puede usar para crear un degradado entre 2 colores.

¿Cómo se dibuja una flecha usando drawArrow()?
- primero se le da uno punto de origen o base de donde partir, luego le agrega el color que se le asigne, por ultimo traza un vector en la direccion del punto final que se le de y dibuja la cabeza de la flecha.


### Actividad 06

**Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.**
- El concepto de motion 101 es que el movimiento de un objeto consista en 2 pasos que se repiten una y otra vez:
  1. a la posicion del objeto se le añade velocidad
  2. se dibuja la nueva posicion del objeto

geometricamente hablando, el motion 101 consiste en 2 vectores, uno que determina la posicion del objeto, y otro que determina la velocidad de desplazamiento de este, para realizar el movimiento del objeto lo que se hace es sumar los valores del vector de velocidad al vector de posicion y actualizar la ubicacion donde se dibuja el objeto acorde al vector posicion.

**¿Cómo se aplica motion 101 en el ejemplo?**
- en el ejemplo se aplico el motion 101 en la clase mover, donde creo un vector position y un vector velocity, para despues en la funcion update hacer que el vector velocity fuera sumado al vector position para hacer que el  objeto se mueva a lo largo del canvas.

### Actividad 07

*Para investigador el significado de esta frase te propone que construyas un experimento donde analices cómo se comporta un objeto en movimiento con:*

*Aceleración constante.
Aceleración aleatoria.
Aceleración hacia el mouse.*

**¿Qué observaste cuando usas cada una de las aceleraciones propuestas?**
- cuando la aceleracion es aleatoria, la velocidad de desplazamiento del objeto variaba bastante, parecia erratico y lo hacia impredecible no solo en cuanto a velocidad sino que tambien direccion.
- cuando la aceleracion es constante, la velocidad de desplazamiento del objeto aumenta gradualmente pero de forma constante, lo que hacia que cada vez fuera mas rapido.
- cuando la acelracion es hacia el mouse, este aceleraba y se hacia mas rapido hasta que llegara a la posicion del mouse, de ahi este desaceleraba, aunque para este punto ya la velocidad era tanta que se pasaba de la posicion y volvia a aclerar hasta que llegara al mouse y asi seguia el ciclo de desacelerar/frenar y acelerar.

