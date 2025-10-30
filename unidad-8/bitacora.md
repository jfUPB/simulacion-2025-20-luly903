# Evidencias de la unidad 8


## SET

### Actividad 01
**Describe tus observaciones sobre la conexión sonido-imagen en al menos dos de las performances vistas.**

*En dimension N:*
- el tamaño e intensidad del arte cambiaba con dependiendo del tono e intensidad de la melodia, ademas de que la apariencia de este cambiaba tambien, pasaba de orbes/circulos a lineas y destellos.

*En become ocean:*
- el comportamiento de la obra recordo mucho al de las olas del mar, como los "trazos" se movian de forma lenta y fluida, ondulandose y dando una sensacion de que envuelve todo lo que toca, sobreponiendose encima del arte en el fondo.


**Explica qué elementos te parecieron generativos y por qué crees que cada visualización sería única.**
*En dimension N:*
- el comportamiento de los "trazos" de la obra me parecieron generativos, cada visualizacion es unica ya que el comportamiento de los trazos cambia con cada iteracion, ya que siguen un comportamiento relativamente aleatorio el cual reacciona a la melodia y al mismo artista.

*En become ocean:*
- principalmente los trazos azules y como se movian, pues estos seguian el la melodia de una forma "natural" similar a una corriente de agua, siento que esto es exactamente lo que hace que cada visualizacion sea unica.

**Comparte tu reflexión sobre la sensación de “liveness”.**
- estas obras se sienten vivas gracias a esos comportamientos impredecibles y fluidos que reaccionan conb la melodia, ya que dan la sensacion que estan bailando al sonido de la musica. Incluso si los movimientos no tienen mucha logica en algunos casos, esa misma espontaniedad solo incrementa la sensacion de que la obra esta viva.


## SEEK

### Actividad 02

**pieza musical elegida**
Shatter me - lindsey Stirling

**descripción del concepto visual.**
el concepto visual consisitira en un conjunto de lineas horizontales que simularan las cuerdas de un violin, estas se curvearan y generaran particulas a partir de la intensidad de la frecuencia de las ondas, algunas cuerdas se veran afectadas por las frecuencias altas y otros por frecuencias bajas, las cuerdas van a dejar un "rastro" o eco cuando se muevan, el fondo sera completamente negro las cuerdas seran blancas, y las partiuclas seran de un mismo color que ira cambiando con el tiempo.

**inputs seleccionados y la justificación de por qué los elegiste.**

**¿Qué algoritmos o técnicas planeas usar (ej: flow fields, flocking, física, partículas, etc.) y por qué?**
- planeo usar particulas y fisica para realizar la actividad, esto ya que estoy familiarizada con con estas tecnicas lo cual hara mas facil realizar la obra.

**Tus bocetos y una explicación de cómo los inputs influirán en los visuales.**
![Imagen de WhatsApp 2025-10-29 a las 22 38 27_1492aaea](https://github.com/user-attachments/assets/5a241c67-c41f-47df-a532-14be4531283e)

como se describe en la imagen, las cuerdas se presentaran en un fondo oscuro, ellas seran influenciadas por los inputs de sonido de la cancion, las cuerdas superiores recibiran las frecuencias agudas mientras que las inferiores las frecuencias mas graves. Inicialmente, solo seran las cuerdas y el fondo negro, pero una vez las cuerdas se ven afectadas por el sonido, esta se moveran como si estuvieran "vibrando" y hubieran sido afectadas por una fuerza superior, al mismo tiempo, particulas saldran de las cuerdas como si fueran producto de las vibraciones, entre mas fuerte o mas potencia tenga el sonido y la frecuencia, mas se agitaran las cuerdas y mas particulas saldran de ellas, las particulas seran de colores aleatorios de tonos claros y/o neon.


## APPLY

### Actividad 03

**Codigo fuente**
```js
let song;
let fft;
let numStrings = 6;
let strings = [];
let particles = [];
let colorShift = 0;
let smoothing = 0.6;
let amplitude;
let vibrationStrength = 2.5;

function preload() {
  song = loadSound("Lindsey Stirling - Shatter Me - TokyoOtakuJapan.mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  fft = new p5.FFT(smoothing, 1024);
  amplitude = new p5.Amplitude();
  noFill();
  colorMode(HSB, 360, 100, 100, 255);

  let spacing = height / (numStrings + 1);
  for (let i = 0; i < numStrings; i++) {
    strings.push(new StringLine(spacing * (i + 1), i));
  }

  textAlign(CENTER);
  textSize(18);
  fill(200);
}

function draw() {
  background(0);

  if (!song.isPlaying()) {
    text("Click para reproducir la canción", width / 2, height / 2);
    return;
  }

  let spectrum = fft.analyze();
  let bass = fft.getEnergy("bass");
  let mid = fft.getEnergy("mid");
  let treble = fft.getEnergy("treble");

  // ⚙️ Reducir sensibilidad del bass
  bass *= 0.7; // Puedes ajustar a 0.6 o 0.4 según la canción
  mid *= 0.8;
  

  colorShift += 0.3;

  let energies = [];
  for (let s of strings) {
    let e = s.computeEnergy(bass, mid, treble);
    energies.push(e);
  }

  // Detectar las 3 cuerdas más activas
  let topIndexes = energies
    .map((val, i) => ({ val, i }))
    .sort((a, b) => b.val - a.val)
    .slice(0, 3)
    .map((obj) => obj.i);

  for (let i = 0; i < strings.length; i++) {
    let active = topIndexes.includes(i);
    strings[i].update(spectrum, bass, mid, treble, active);
    strings[i].display();
  }

  // Actualizar partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.update();
    p.display();
    if (p.isDead()) particles.splice(i, 1);
  }
}

function mousePressed() {
  if (!song.isPlaying()) song.play();
  else song.pause();
}

// ==========================================================
// Clase StringLine
// ==========================================================
class StringLine {
  constructor(y, index) {
    this.y = y;
    this.index = index;
    this.points = [];
    this.detail = 120;
    this.offset = random(1000);
    this.thickness = 2;
    this.targetThickness = 2;
    this.glowIntensity = 0;
    for (let i = 0; i < this.detail; i++) this.points.push(0);
  }

  computeEnergy(bass, mid, treble) {
    if (this.index <= 1) return treble;
    else if (this.index <= 3) return mid;
    else return bass;
  }

  update(spectrum, bass, mid, treble, active) {
    let freqEnergy = this.computeEnergy(bass, mid, treble);
    let amplitudeFactor = map(freqEnergy, 0, 255, 0, 40 * vibrationStrength);
    this.targetThickness = map(freqEnergy, 0, 255, 2, 10);
    this.thickness = lerp(this.thickness, this.targetThickness, 0.15);
    this.glowIntensity = lerp(this.glowIntensity, map(freqEnergy, 0, 255, 0.1, 1), 0.1);

    for (let i = 0; i < this.points.length; i++) {
      let n = noise(i * 0.1, frameCount * 0.02 + this.offset);
      let wave = sin(i * 0.3 + frameCount * 0.05);
      this.points[i] = wave * amplitudeFactor * n;

      // Emitir partículas
      if (active && freqEnergy > 100 && random() < 0.02) {
        let x = map(i, 0, this.points.length - 1, 0, width);
        let y = this.y + this.points[i];
        let hueColor = (colorShift + random(60)) % 360;
        let c = color(hueColor, 100, 100, 255);

        // Calcular la dirección normal
        let prevY = this.points[max(0, i - 1)];
        let nextY = this.points[min(this.points.length - 1, i + 1)];
        let slope = nextY - prevY;
        let normal = createVector(-slope, 10).normalize();

        // Repulsión
        let repelDir = normal.mult(-1);
        let velocity = repelDir.mult(random(1.5, 3.5));
        velocity.add(p5.Vector.random2D().mult(0.5));

        // Crear partícula con referencia a la cuerda
        particles.push(new Particle(x, y, velocity, c, this.y));
      }
    }
  }

  display() {
    // halo luminoso
    let glowLayers = 5;
    for (let g = glowLayers; g > 0; g--) {
      let alpha = map(g, glowLayers, 0, 20, 0, 255 * this.glowIntensity);
      stroke(255, alpha);
      strokeWeight(this.thickness + g * 5);
      noFill();
      beginShape();
      for (let i = 0; i < this.points.length; i++) {
        let x = map(i, 0, this.points.length - 1, 0, width);
        vertex(x, this.y + this.points[i]);
      }
      endShape();
    }

    // cuerda principal
    stroke(255);
    strokeWeight(this.thickness);
    beginShape();
    for (let i = 0; i < this.points.length; i++) {
      let x = map(i, 0, this.points.length - 1, 0, width);
      vertex(x, this.y + this.points[i]);
    }
    endShape();
  }
}

// ==========================================================
// Clase Particle
// ==========================================================
class Particle {
  constructor(x, y, velocity, c, originY) {
    this.pos = createVector(x, y);
    this.vel = velocity.copy();
    this.originY = originY;
    this.life = 100;
    this.totalLife = 100;
    this.baseHue = hue(c);
    this.baseColor = c;
    this.size = random(1, 4);
    this.angle = random(TWO_PI);
    this.waveSpeed = random(0.01, 0.02);
    this.spread = random(0.8, 1.8);
  }

  update() {
    this.angle += this.waveSpeed;
    this.pos.x += sin(this.angle) * this.spread;
    this.pos.y += cos(this.angle * 0.8) * 0.4;
    this.pos.add(this.vel);
    this.vel.mult(0.985);
    this.life -= 1.0;
  }

  display() {
    noStroke();

    // Distancia desde la cuerda de origen
    let distY = abs(this.pos.y - this.originY);
    let distFactor = constrain(map(distY, 0, height / 3, 0, 1), 0, 1);

    // Al principio son blancas, luego adoptan color neón
    let hueVal = lerp(0, this.baseHue, distFactor);
    let satVal = lerp(0, 100, distFactor);
    let brightVal = 100;

    let alphaVal = map(this.life, 0, this.totalLife, 0, 255);
    let c = color(hueVal, satVal, brightVal, alphaVal);

    for (let i = 4; i > 0; i--) {
      let alpha = alphaVal * (i / 4);
      let r = this.size * i * 1.3;
      fill(hue(c), saturation(c), brightness(c), alpha);
      ellipse(this.pos.x, this.pos.y, r);
    }
  }

  isDead() {
    return this.life < 0;
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
```

[link al sketch](https://editor.p5js.org/luly903/sketches/Gbn0-Uk-l)

**Capturas de pantalla**


<img width="833" height="770" alt="image" src="https://github.com/user-attachments/assets/c47b2f1d-985d-4a06-9dfe-ea8afd26820e" />

<img width="830" height="769" alt="image" src="https://github.com/user-attachments/assets/aa8bc033-a497-4c56-ae92-afe0b19ddd53" />

<img width="830" height="766" alt="image" src="https://github.com/user-attachments/assets/9f10c57a-934e-4108-8ed0-4c84447a9def" />

<img width="837" height="769" alt="image" src="https://github.com/user-attachments/assets/103c311b-f515-4262-82cc-6739955995fe" />




## RUBRICA

NOTA: 5

justificacion: realice las 3 actividades y la autoevaluacion.
