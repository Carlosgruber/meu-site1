<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cartinha Virtual do Amor</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.2/p5.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Parisienne&display=swap" rel="stylesheet">
  <style>
    body { margin: 0; overflow: hidden; font-family: 'Parisienne', cursive; }
    canvas { position: absolute; top: 0; left: 0; z-index: -1; }
    .message {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      opacity: 0;
      animation: fadeIn 2s ease-in forwards;
    }
    @keyframes fadeIn {
      0% { opacity: 0; transform: translate(-50%, -40%); }
      100% { opacity: 1; transform: translate(-50%, -50%); }
    }
    .bg-gradient {
      background: linear-gradient(45deg, #ff6f91, #ffb6c1, #ffdde1, #ffb6c1);
      background-size: 400%;
      animation: gradient 15s ease infinite;
      height: 100vh;
    }
    @keyframes gradient {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    .carousel-container {
      display: none;
      position: absolute;
      top: 60%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 90%;
      max-width: 600px;
      background: rgba(255, 255, 255, 0.8);
      border-radius: 1rem;
      padding: 1rem;
      text-align: center;
      box-shadow: 0 0 20px rgba(0,0,0,0.2);
    }
    .carousel-container img {
      width: 100%;
      border-radius: 1rem;
    }
    .carousel-container p {
      margin-top: 0.5rem;
      font-size: 1.2rem;
      color: #e91e63;
    }
    .arrow {
      cursor: pointer;
      font-size: 2rem;
      margin: 0 1rem;
      color: #e91e63;
      user-select: none;
    }
    .typewriter {
      overflow: hidden;
      white-space: nowrap;
      border-right: 0.15em solid white;
      animation: typing 4s steps(40, end), blink-caret 0.75s step-end infinite;
    }
    @keyframes typing {
      from { width: 0 }
      to { width: 100% }
    }
    @keyframes blink-caret {
      from, to { border-color: transparent }
      50% { border-color: white }
    }
  </style>
</head>
<body class="bg-gradient">
  <audio autoplay loop>
    <source src="musica.mp3" type="audio/mpeg">
    Seu navegador não suporta áudio.
  </audio>

  <div class="message text-white text-3xl md:text-5xl font-parisienne drop-shadow-lg">
    <h1>Para <span id="nome">meu amor</span>,</h1>
    <p class="mt-4 text-xl md:text-2xl typewriter" id="frase">Você é a luz dos meus dias. Te amo eternamente! 💖</p>
    <button onclick="mostrarGaleria()" class="mt-6 px-6 py-2 bg-white text-pink-500 font-bold rounded-lg shadow hover:bg-pink-100 transition">Clique para ver nossos momentos 💞</button>
    <button onclick="mostrarCarta()" class="mt-4 px-6 py-2 bg-pink-100 text-pink-800 font-semibold rounded-lg shadow hover:bg-pink-200 transition">Abrir cartinha 💌</button>
  </div>

  <div id="galeria" class="carousel-container">
    <div class="flex justify-center items-center">
      <span class="arrow" onclick="anteriorFoto()">⟨</span>
      <div class="flex flex-col items-center">
        <img id="fotoCasal" src="" alt="Momentos do casal">
        <p id="mensagemFoto"></p>
      </div>
      <span class="arrow" onclick="proximaFoto()">⟩</span>
    </div>
  </div>

  <div id="carta" style="display:none; position: absolute; top: 20%; left: 50%; transform: translateX(-50%); background: rgba(255,255,255,0.95); padding: 2rem; border-radius: 1rem; box-shadow: 0 0 10px rgba(0,0,0,0.2); width: 80%; max-width: 500px; text-align: center; font-size: 1.2rem; color: #e91e63;">
    <p>Desde que você entrou na minha vida, tudo ficou mais bonito. Obrigado por ser minha inspiração diária. Te amo com todo meu coração! ❤️</p>
  </div>

  <script>
    const urlParams = new URLSearchParams(window.location.search);
    const nome = urlParams.get('nome') || 'meu amor';
    const frase = urlParams.get('frase') || 'Você é a luz dos meus dias. Te amo eternamente! 💖';
    document.getElementById('nome').textContent = nome;
    document.getElementById('frase').textContent = frase;

    let hearts = [];

    function setup() {
      createCanvas(windowWidth, windowHeight);
      for (let i = 0; i < 30; i++) {
        hearts.push(new Heart());
      }
    }

    function draw() {
      clear();
      for (let heart of hearts) {
        heart.update();
        heart.show();
      }
    }

    class Heart {
      constructor() {
        this.x = random(width);
        this.y = random(height);
        this.size = random(20, 60);
        this.speed = random(1, 3);
        this.angle = random(TWO_PI);
        this.pulse = random(0.5, 1);
      }
      update() {
        this.y -= this.speed;
        this.x += sin(this.angle) * 0.5;
        this.angle += 0.05;
        this.pulse = 0.9 + 0.1 * sin(frameCount * 0.05);
        if (this.y < -this.size) {
          this.y = height + this.size;
          this.x = random(width);
        }
      }
      show() {
        noStroke();
        fill(255, 105, 180, 200);
        push();
        translate(this.x, this.y);
        scale(this.pulse);
        beginShape();
        vertex(0, 0);
        bezierVertex(-this.size / 2, -this.size / 2, -this.size, this.size / 2, 0, this.size);
        bezierVertex(this.size, this.size / 2, this.size / 2, -this.size / 2, 0, 0);
        endShape(CLOSE);
        pop();
      }
    }

    const fotos = [
      { src: 'foto1.jpg', mensagem: 'Nosso primeiro encontro ❤️' },
      { src: 'foto2.jpg', mensagem: 'Aquela viagem inesquecível 💑' },
      { src: 'foto3.jpg', mensagem: 'Sempre sorrindo juntos 💕' },
      { src: 'foto4.jpg', mensagem: 'Nosso aniversário de namoro 🎉' },
      { src: 'foto5.jpg', mensagem: 'Um passeio especial 🌅' }
    ];

    let indexAtual = 0;

    function mostrarGaleria() {
      document.getElementById('galeria').style.display = 'block';
      atualizarFoto();
    }

    function atualizarFoto() {
      document.getElementById('fotoCasal').src = fotos[indexAtual].src;
      document.getElementById('mensagemFoto').textContent = fotos[indexAtual].mensagem;
    }

    function proximaFoto() {
      indexAtual = (indexAtual + 1) % fotos.length;
      atualizarFoto();
    }

    function anteriorFoto() {
      indexAtual = (indexAtual - 1 + fotos.length) % fotos.length;
      atualizarFoto();
    }

    function mostrarCarta() {
      document.getElementById('carta').style.display = 'block';
    }
  </script>
</body>
</html>
