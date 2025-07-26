let frases = [
  "Tribunal del Corazón - Caso #2025",
  "Acusado: Jesús Juárez",
  "Crímenes: Amar a Melissa con exceso de ternura.",
  "Evidencia presentada: Miradas, sonrisas, pensamientos constantes.",
  "Defensa: 'Culpable de sentir algo muy especial por ella'.",
  "Fiscal: 'Él no deja de pensar en Melissa desde el primer día'.",
  "Melissa preside como Jueza Suprema del Amor.",
  "Se delibera... 💭",
  "Veredicto: ¡CULPABLE DE AMAR SINCERAMENTE!",
  "Sentencia: Invitarla a salir y pasar momentos inolvidables juntos ❤️",
  "Con amor, Jesús Juárez"
];

let index = 0;
let currentText = "";
let charIndex = 0;
let lastChange = 0;
let delay = 45;
let animations = [];
let animationType = 0;

// Para el corazón pulsante fijo abajo
let pulseHeart;

function setup() {
  createCanvas(windowWidth, windowHeight);
  textFont("Georgia, serif");
  textAlign(CENTER, TOP);
  textSize(28);
  noStroke();

  pulseHeart = new PulsingHeart(width / 2, height * 0.9, 80);
}

function draw() {
  setGradientBackground();

  if (index < frases.length) {
    if (millis() - lastChange > delay && charIndex <= frases[index].length) {
      currentText = frases[index].substring(0, charIndex);
      charIndex++;
      lastChange = millis();
    }
  } else {
    currentText = "Gracias por ser tan bonita! ❤️";
  }

  fill(40, 15, 60);
  let marginTop = height * 0.1;
  let marginSide = width * 0.1;
  let maxTextWidth = width * 0.8;

  push();
  textSize(28);
  textLeading(36);
  textWrap(WORD);
  text(currentText, marginSide, marginTop, maxTextWidth);
  pop();

  // Animaciones de clic arriba del corazón pulsante
  for (let i = animations.length - 1; i >= 0; i--) {
    animations[i].update();
    animations[i].show();
    if (animations[i].isDone()) animations.splice(i, 1);
  }

  // Dibuja el corazón pulsante fijo abajo
  pulseHeart.update();
  pulseHeart.show();

  textSize(36);
  fill(90, 40, 110);
  drawingContext.shadowBlur = 12;
  drawingContext.shadowColor = 'rgba(150, 50, 150, 0.7)';
  textAlign(CENTER, TOP);
  text("⚖️ JUICIO DEL CORAZÓN ⚖️", width / 2, 10);
  drawingContext.shadowBlur = 0;
}

function mousePressed() {
  if (index < frases.length - 1) {
    index++;
    currentText = "";
    charIndex = 0;
    animationType = (animationType + 1) % 4;
    launchAnimation(animationType);
  } else if (index === frases.length - 1) {
    index++;
    currentText = "";
    charIndex = 0;
    launchAnimation(4);
  }

  // Activa latido corazón fijo abajo
  pulseHeart.pulse();
}

function launchAnimation(type) {
  switch(type) {
    case 0:
      for (let i = 0; i < 40; i++) animations.push(new ElegantHeart());
      break;
    case 1:
      for (let i = 0; i < 50; i++) animations.push(new TwinkleStar());
      break;
    case 2:
      for (let i = 0; i < 60; i++) animations.push(new SoftParticle());
      break;
    case 3:
      for (let i = 0; i < 30; i++) animations.push(new BloomFlower());
      break;
    case 4:
      for (let i = 0; i < 25; i++) animations.push(new RadiantHeart());
      break;
  }
}

// Animación corazón pulsante fijo abajo
class PulsingHeart {
  constructor(x, y, baseSize) {
    this.x = x;
    this.y = y;
    this.baseSize = baseSize;
    this.size = baseSize;
    this.pulseAmount = 0;
    this.pulsing = false;
  }
  pulse() {
    this.pulsing = true;
    this.pulseAmount = 0;
  }
  update() {
    if(this.pulsing) {
      this.pulseAmount += 0.1;
      this.size = this.baseSize * (1 + 0.3 * sin(TWO_PI * this.pulseAmount));
      if(this.pulseAmount > 1) {
        this.pulsing = false;
        this.size = this.baseSize;
      }
    }
  }
  show() {
    push();
    translate(this.x, this.y);
    noStroke();
    fill(255, 90, 140, 200);
    scale(this.size / this.baseSize);
    beginShape();
    vertex(0, -this.baseSize / 2);
    bezierVertex(-this.baseSize / 2, -this.baseSize, -this.baseSize, this.baseSize / 3, 0, this.baseSize);
    bezierVertex(this.baseSize, this.baseSize / 3, this.baseSize / 2, -this.baseSize, 0, -this.baseSize / 2);
    endShape(CLOSE);
    pop();
  }
}

// === Otras animaciones ===

class ElegantHeart {
  constructor() {
    this.x = random(width * 0.2, width * 0.8);
    this.y = height + random(20, 100);
    this.size = random(15, 30);
    this.speed = random(1, 2.2);
    this.alpha = 255;
    this.oscillation = random(TAU);
    this.colorBase = color(255, 100, 180);
  }
  update() {
    this.y -= this.speed;
    this.x += sin(this.oscillation) * 0.8;
    this.oscillation += 0.05;
    this.alpha -= 1.8;
  }
  show() {
    push();
    translate(this.x, this.y);
    noStroke();
    let alphaColor = color(
      red(this.colorBase), green(this.colorBase), blue(this.colorBase), this.alpha
    );
    fill(alphaColor);
    scale(1.1 + 0.2 * sin(frameCount * 0.1 + this.x));
    beginShape();
    vertex(0, -this.size / 2);
    bezierVertex(-this.size / 2, -this.size, -this.size, this.size / 3, 0, this.size);
    bezierVertex(this.size, this.size / 3, this.size / 2, -this.size, 0, -this.size / 2);
    endShape(CLOSE);
    pop();
  }
  isDone() {
    return this.alpha <= 0;
  }
}

class TwinkleStar {
  constructor() {
    this.x = random(width * 0.1, width * 0.9);
    this.y = random(height * 0.75, height * 0.95);
    this.size = random(8, 15);
    this.alpha = 0;
    this.alphaDir = 3;
    this.rotation = random(TAU);
    this.rotationSpeed = random(-0.03, 0.03);
  }
  update() {
    this.alpha += this.alphaDir;
    if(this.alpha >= 255 || this.alpha <= 0) this.alphaDir *= -1;
    this.rotation += this.rotationSpeed;
  }
  show() {
    push();
    translate(this.x, this.y);
    rotate(this.rotation);
    noStroke();
    fill(255, 230, 200, this.alpha);
    starShape(0, 0, this.size / 2, this.size, 5);
    pop();
  }
  isDone() {
    return false;
  }
}

class SoftParticle {
  constructor() {
    this.pos = createVector(width/2 + random(-50, 50), height * 0.85 + random(-20, 20));
    this.vel = p5.Vector.random2D().mult(random(0.8, 2.5));
    this.size = random(4, 9);
    this.alpha = 255;
    this.colorBase = color(180, 150, 255);
  }
  update() {
    this.pos.add(this.vel);
    this.alpha -= 3;
  }
  show() {
    noStroke();
    fill(red(this.colorBase), green(this.colorBase), blue(this.colorBase), this.alpha);
    ellipse(this.pos.x, this.pos.y, this.size);
  }
  isDone() {
    return this.alpha <= 0;
  }
}

class BloomFlower {
  constructor() {
    this.x = random(width * 0.3, width * 0.7);
    this.y = height + random(20, 80);
    this.size = random(20, 40);
    this.speed = random(1, 2);
    this.alpha = 255;
    this.petalCount = 6;
    this.angle = 0;
  }
  update() {
    this.y -= this.speed;
    this.alpha -= 1.5;
    this.angle += 0.02;
  }
  show() {
    push();
    translate(this.x, this.y);
    rotate(this.angle);
    noStroke();
    fill(255, 170, 210, this.alpha);
    for(let i=0; i<this.petalCount; i++) {
      ellipse(
        cos(TAU*i/this.petalCount) * this.size / 3,
        sin(TAU*i/this.petalCount) * this.size / 3,
        this.size / 2,
        this.size / 1.5
      );
    }
    pop();
  }
  isDone() {
    return this.alpha <= 0;
  }
}

class RadiantHeart {
  constructor() {
    this.x = width / 2 + random(-60, 60);
    this.y = height * 0.85 + random(-30, 30);
    this.baseSize = random(50, 80);
    this.alpha = 255;
    this.pulse = random(TAU);
  }
  update() {
    this.pulse += 0.06;
    this.alpha -= 1.5;
  }
  show() {
    push();
    translate(this.x, this.y);
    let scalePulse = 1 + 0.15 * sin(this.pulse);
    scale(scalePulse);
    noStroke();
    fill(255, 90, 140, this.alpha);
    beginShape();
    vertex(0, -this.baseSize / 2);
    bezierVertex(-this.baseSize / 2, -this.baseSize, -this.baseSize, this.baseSize / 3, 0, this.baseSize);
    bezierVertex(this.baseSize, this.baseSize / 3, this.baseSize / 2, -this.baseSize, 0, -this.baseSize / 2);
    endShape(CLOSE);
    pop();
  }
  isDone() {
    return this.alpha <= 0;
  }
}

function starShape(x, y, radius1, radius2, npoints) {
  let angle = TWO_PI / npoints;
  let halfAngle = angle / 2.0;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    let sx = x + cos(a) * radius2;
    let sy = y + sin(a) * radius2;
    vertex(sx, sy);
    sx = x + cos(a + halfAngle) * radius1;
    sy = y + sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}

function setGradientBackground() {
  let c1 = color(255, 240, 250);
  let c2 = color(230, 200, 255);
  let inter = map(sin(frameCount * 0.01), -1, 1, 0, 1);
  let c = lerpColor(c1, c2, inter);

  for(let y=0; y < height; y++) {
    let interY = map(y, 0, height, 0, 1);
    let col = lerpColor(c, color(255, 230, 245), interY);
    stroke(col);
    line(0, y, width, y);
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
