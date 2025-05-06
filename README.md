<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Ordem Paranormal: Ecos do Abismo</title>
  <style>
    body {
      background: #111;
      color: white;
      font-family: sans-serif;
      padding: 20px;
    }
    button {
      background: #333;
      color: white;
      padding: 10px;
      margin: 5px;
      border: none;
      cursor: pointer;
      border-radius: 4px;
    }
    .log {
      background: #222;
      padding: 10px;
      border-radius: 5px;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <h1>Ordem Paranormal: Ecos do Abismo</h1>
  <div id="game"></div>

  <script>
    let gameState = {
      started: false,
      playerClass: null,
      stats: { HP: 100, Sanidade: 100, Exposicao: 0 },
      log: ["Você sente a presença de algo além do véu..."]
    };

    function startGame(cls) {
      gameState.started = true;
      gameState.playerClass = cls;
      gameState.stats = { HP: 100, Sanidade: 100, Exposicao: 0 };
      gameState.log = [`Você iniciou como um ${cls}. Prepare-se para o desconhecido.`];
      render();
    }

    function explore() {
      const roll = Math.random();
      let log = "";

      if (roll < 0.3) {
        log = "Você encontrou uma relíquia misteriosa. Sua sanidade vacila.";
        gameState.stats.Exposicao += 10;
        gameState.stats.Sanidade -= 5;
      } else if (roll < 0.6) {
        log = "Um culto oculto te emboscou! Você foi ferido.";
        gameState.stats.HP -= 20;
        gameState.stats.Sanidade -= 10;
      } else {
        log = "Você meditou em um local sagrado. Sua mente clareia.";
        gameState.stats.Sanidade += 10;
      }

      if (gameState.stats.HP <= 0 || gameState.stats.Sanidade <= 0) {
        gameState.log = ["Você foi consumido pelo paranormal..."];
        gameState.started = false;
      } else {
        gameState.log.unshift(log);
        gameState.log = gameState.log.slice(0, 5);
      }

      render();
    }

    function render() {
      const g = gameState;
      let html = "";

      if (!g.started) {
        html += "<p>Escolha sua classe:</p>";
        ["Ocultista", "Combatente", "Intuitivo", "Técnico"].forEach(cls => {
          html += `<button onclick="startGame('${cls}')">${cls}</button>`;
        });
      } else {
        html += `<p><strong>Classe:</strong> ${g.playerClass}</p>`;
        html += `<p><strong>HP:</strong> ${g.stats.HP}</p>`;
        html += `<p><strong>Sanidade:</strong> ${g.stats.Sanidade}</p>`;
        html += `<p><strong>Exposição:</strong> ${g.stats.Exposicao}</p>`;
        html += `<button onclick="explore()">Explorar</button>`;
        html += "<div class='log'>";
        g.log.forEach(entry => html += `<p>- ${entry}</p>`);
        html += "</div>";
      }

      document.getElementById("game").innerHTML = html;
    }

    render();
  </script>
</body>
</html>
