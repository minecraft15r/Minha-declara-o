<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Minha Declaração</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }

    body {
      background: linear-gradient(135deg, #ff9a9e, #fad0c4);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      color: #333;
      position: relative;
      overflow-x: hidden;
      padding-top: 60px;
    }

    body.dark-mode {
      background: #1e1e1e;
      color: #eee;
    }

    body.dark-mode .container {
      background: #333;
      color: #eee;
    }

    .container {
      background-color: white;
      padding: 40px;
      border-radius: 20px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
      max-width: 700px;
      width: 90%;
      text-align: center;
      position: relative;
      z-index: 1;
      margin-top: 20px;
      animation: fadeIn 1s ease;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(-20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    input[type="text"] {
      padding: 10px;
      border: 2px solid #ff9a9e;
      border-radius: 10px;
      width: 80%;
      margin-bottom: 20px;
      font-size: 16px;
    }

    .file-label, button {
      background-color: #ff9a9e;
      color: white;
      padding: 10px 20px;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      transition: background 0.3s;
      margin: 10px;
      border: none;
    }

    button:hover, .file-label:hover {
      background-color: #ff7b8c;
    }

    input[type="file"] { display: none; }

    .conteudo {
      display: flex;
      align-items: center;
      justify-content: center;
      flex-wrap: wrap;
    }

    .mensagem {
      flex: 1 1 300px;
      padding: 20px;
    }

    .imagem {
      flex: 0 1 200px;
      margin-left: 20px;
      position: relative;
    }

    .imagem img {
      max-width: 200px;
      border-radius: 50%;
      border: 5px solid #ff9a9e;
    }

    .imagem::before {
      content: '❤️';
      font-size: 40px;
      position: absolute;
      top: -20px;
      right: -20px;
      animation: pulse 1.5s infinite;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.2); }
      100% { transform: scale(1); }
    }

    .categoria-btns button {
      display: block;
      width: 80%;
      margin: 10px auto;
    }

    .tema-toggle {
      position: absolute;
      top: 20px;
      right: 20px;
    }

    .musica-toggle {
      position: absolute;
      top: 20px;
      left: 20px;
    }

    .spotify-embed {
      position: absolute;
      top: 100px;
      left: 20px;
      display: none;
      width: 300px;
      height: 352px;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 4px 20px rgba(0,0,0,0.2);
      z-index: 2;
    }
  </style>
</head>
<body>
  <div class="tema-toggle">
    <button onclick="alternarTema()">🌓 Alternar Tema</button>
  </div>

  <div class="musica-toggle">
    <button onclick="mostrarSpotify()">🎶 Música</button>
  </div>

  <div id="spotifyContainer" class="spotify-embed">
    <iframe id="spotifyEmbed" style="border:none;width:100%;height:100%;" src="" frameborder="0" allowfullscreen allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
  </div>

  <div class="container" id="entrada">
    <h2>Olá, insira seu nome para continuar</h2>
    <input type="text" id="nome" placeholder="Digite seu nome aqui...">
    <br>
    <label for="foto" class="file-label">Escolher uma foto ❤️</label>
    <input type="file" id="foto" accept="image/*">
    <br>
    <button onclick="irParaSelecao()">Continuar</button>
  </div>

  <div class="container" id="selecao" style="display: none;">
    <h2>Escolha uma categoria:</h2>
    <div class="categoria-btns">
      <button onclick="mostrarDeclaracao('fofo')">🐻 Fofo</button>
      <button onclick="mostrarDeclaracao('engracado')">😄 Engraçado</button>
      <button onclick="mostrarDeclaracao('poetico')">🎭 Poético</button>
      <button onclick="mostrarDeclaracao('profundo')">🌌 Profundo</button>
      <button onclick="mostrarDeclaracao('aceita')">❤️ Aceita?</button>
      <button onclick="surpreenda()">🎁 Surpreenda-me</button>
    </div>
  </div>

  <div class="container" id="declaracao" style="display: none;">
    <h2 id="titulo"></h2>
    <div class="conteudo">
      <div class="mensagem"><p id="mensagem"></p></div>
      <div class="imagem" id="fotoContainer" style="display: none;">
        <img id="imagemPreview" src="" alt="Foto da pessoa">
      </div>
    </div>
    <button onclick="window.print()">📄 Imprimir declaração</button>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  <script>
    const mensagensPorCategoria = {
      fofo: nome => `
        Se eu pudesse te dar uma coisa, seria o espelho da forma como te vejo:
incrível, forte, doce e absolutamente único(a).<br><br>
        Você é meu pensamento bom de todo dia.<br><br>
        Com carinho,
Alguém que gosta muito de você 💞
      `,
      engracado: nome => `
        ${nome},Se eu ganhasse uma moeda a cada vez que você me faz rir, já teria comprado um lanche... e um carro... e talvez uma ilha 🏝️
<br><br>
        Você tem o dom de transformar drama em comédia e preguiça em arte.<br><br>
        Cientificamente comprovado: 100% das vezes que você aparece, meu tédio vai embora e leva a dignidade junto 😅<br><br>
        Continue sendo essa mistura de meme ambulante e abraço sincero — o mundo agradece!
      `,
      poetico: nome => `
        Em meio a tantos caminhos, encontrei você.
Como se o universo soubesse exatamente o que me faltava.<br><br>
        Você é poesia em forma de gente,
um abraço que mora no pensamento.<br><br>
Que sorte a minha te ter por perto,
mesmo quando a distância insiste em aparecer.<br><br>
        Com afeto infinito,
Alguém que gosta muito de ter você ❤️   
      `,
      profundo: nome => `
        Dentro de você mora uma força que o mundo ainda não conseguiu medir.<br><br>
        Você é feito(a) de silêncios que gritam, de gestos que curam, e de uma luz que nem sempre percebe — mas todos ao seu redor sentem.<br><br>
        Mesmo nos dias nublados, sua alma é sol.<br><br>
        Nunca subestime o impacto de ser exatamente quem você é.<br><br>
        Obrigado(a) por existir, ${nome}. Você é perfeito(a).
      `,
      aceita: nome => `
        ${nome},<br><br>
       Opa... desculpa atrapalhar,
é que eu estava ensaiando isso faz um tempinho… e agora eu criei coragem. 💌<br><br>
        Desde que você apareceu, tudo ficou mais bonito.
Seu jeitinho, seu sorriso, sua voz… tudo em você parece encaixar direitinho no que eu nem sabia que faltava em mim.<br><br>
        Você me faz querer ser melhor, me faz sorrir do nada, me faz sentir aquele friozinho na barriga gostoso só de imaginar você por perto.<br><br>
        Com todo o amor do mundo,<br>
        Estar com você é como um abraço quentinho num dia frio, como aquele cheirinho de bolo saindo do forno — aconchegante, leve e cheio de carinho.<br><br>
       E por mais que eu tente escrever tudo o que sinto, parece que nenhuma palavra dá conta do tanto de carinho, cuidado e amor que eu guardo por você aqui dentro.<br><br>
       Então... pra tornar tudo isso ainda mais especial, só falta uma coisinha:<br><br>
        🌸 Você quer namorar comigo? 💕
      `
    };

    const playlists = [
      "https://open.spotify.com/embed/playlist/37i9dQZF1DXcBWIGoYBM5M?utm_source=generator"
    ];

    let spotifyVisible = false;

    function mostrarSpotify() {
      const embed = document.getElementById("spotifyEmbed");
      const container = document.getElementById("spotifyContainer");

      if (!spotifyVisible) {
        const aleatorio = playlists[Math.floor(Math.random() * playlists.length)];
        embed.src = aleatorio;
        container.style.display = "block";
      } else {
        container.style.display = "none";
        embed.src = "";
      }

      spotifyVisible = !spotifyVisible;
    }

    function alternarTema() {
      document.body.classList.toggle("dark-mode");
    }

    function irParaSelecao() {
      const nome = document.getElementById("nome").value.trim();
      if (nome === "") {
        alert("Por favor, insira seu nome.");
        return;
      }
      document.getElementById("entrada").style.display = "none";
      document.getElementById("selecao").style.display = "block";
    }

    function mostrarDeclaracao(categoria) {
      const nome = document.getElementById("nome").value.trim();
      const foto = document.getElementById("foto").files[0];

      const mensagemFunc = mensagensPorCategoria[categoria];
      if (!mensagemFunc) return;

      document.getElementById("selecao").style.display = "none";
      document.getElementById("declaracao").style.display = "block";
      document.getElementById("titulo").innerText = `Para você, ${nome}`;
      document.getElementById("mensagem").innerHTML = mensagemFunc(nome);

      if (foto) {
        const reader = new FileReader();
        reader.onload = function(e) {
          document.getElementById("imagemPreview").src = e.target.result;
          document.getElementById("fotoContainer").style.display = "block";
        };
        reader.readAsDataURL(foto);
      }

      setTimeout(() => {
        confetti({
          particleCount: 150,
          spread: 100,
          origin: { y: 0.6 }
        });
      }, 500);
    }

    function surpreenda() {
      const categorias = ['fofo', 'engracado', 'poetico', 'profundo'];
      const aleatoria = categorias[Math.floor(Math.random() * categorias.length)];
      mostrarDeclaracao(aleatoria);
    }
  </script>
</body>
</html>
