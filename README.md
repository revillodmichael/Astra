<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Galaxie Astra - Projet Complet avec Interface AI</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  /* Styles généraux */
  body, html {
    margin: 0; padding: 0; background: #000;
    font-family: Arial, sans-serif;
    color: #0ff;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  #header {
    margin: 15px;
    text-align: center;
    font-size: 1.5rem;
    text-shadow: 0 0 10px #0ff;
  }
  #container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    width: 95vw;
    max-width: 1200px;
  }
  canvas {
    background: radial-gradient(circle at center, #001f3f 0%, #000033 100%);
    border-radius: 20px;
    box-shadow: 0 0 30px rgba(80, 150, 255, 0.7);
    transition: box-shadow 0.5s ease, transform 0.3s ease;
  }
  /* Planète interactive */
  #planet {
    position: fixed;
    bottom: 20px;
    right: 20px;
    width: 60px;
    height: 60px;
    background: radial-gradient(circle at center, #0ea5e9, #0369a1);
    border-radius: 50%;
    box-shadow: 0 0 15px #0ea5e9;
    cursor: pointer;
    z-index: 10000;
    display: flex;
    align-items: center;
    justify-content: center;
    user-select: none;
    transition: transform 0.3s ease;
  }
  #planet:hover {
    transform: scale(1.1) rotate(15deg);
  }
  #planet svg {
    width: 30px;
    height: 30px;
    fill: #fff;
  }

  /* Menu radial */
  #radialMenu {
    position: fixed;
    bottom: 90px;
    right: 20px;
    display: none;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    z-index: 10001;
  }
  .radialButton {
    background: #0ea5e9;
    color: #000;
    border: none;
    border-radius: 50%;
    width: 48px;
    height: 48px;
    cursor: pointer;
    box-shadow: 0 0 10px #0ea5e9;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    font-size: 14px;
    transition: background 0.3s, transform 0.3s;
  }
  .radialButton:hover {
    background: #22c55e;
    transform: scale(1.2);
  }

  /* Champ Énergétique Planétaire */
  #energyFieldPanel {
    width: 90vw; /* Ajustement à 90% de la largeur de l'écran */
    max-width: 620px; /* Largeur maximale */
    height: 90vh; /* Ajustement à 90% de la hauteur de l'écran */
    max-height: 620px; /* Hauteur maximale */
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: rgba(17, 24, 39, 0.8);
    border-radius: 20px;
    box-shadow: 0 0 30px rgba(14, 165, 233, 0.7);
    margin: 20px; /* Marge pour espacer des autres éléments */
  }
  #energyFieldCanvas {
    width: 100%; /* Prend toute la largeur du panneau */
    height: 100%; /* Prend toute la hauteur du panneau */
    border-radius: 20px;
  }

  /* Sections cachées par défaut */
  #socialNetwork, #bankingPanel {
    display: none; /* Caché par défaut */
  }
</style>
</head>
<body>

<div id="header">Galaxie Astra - Projet Complet avec Interface AI</div>

<div id="container">
  <!-- Champ Énergétique Planétaire -->
  <div id="energyFieldPanel">
    <h3 style="color: #0ea5e9; text-align: center; margin-bottom: 10px; font-weight: 600;">
      Champ Énergétique Planétaire & Réseau
    </h3>
    <canvas id="energyFieldCanvas"></canvas>
  </div>

  <!-- Vos panneaux existants -->
  <div class="panel" id="panel1" data-module="performance">
    <div class="ia-mascot"></div>
    <h3>Performance</h3>
    <p>Optimisation en cours...</p>
    <div class="ia-message" id="msg1">Analyse des performances...</div>
  </div>
  <div class="panel" id="panel2" data-module="scalability">
    <div class="ia-mascot"></div>
    <h3>Scalabilité</h3>
    <p>Chargement des données...</p>
    <div class="ia-message" id="msg2">Réduction de la latence...</div>
  </div>
  <div class="panel" id="panel3" data-module="security">
    <div class="ia-mascot"></div>
    <h3>Sécurité</h3>
    <p>Vérification des protocoles...</p>
    <div class="ia-message" id="msg3">Application des mesures...</div>
  </div>
</div>

<!-- Chat -->
<div id="chat">
  <div id="messages"></div>
  <div id="inputArea">
    <input type="text" id="messageInput" placeholder="Écrivez un message..." autocomplete="off" />
    <button id="sendBtn">Envoyer</button>
  </div>
</div>

<!-- Banking panel -->
<div id="bankingPanel">
  <h3>Épargne & Prêt</h3>
  <label>Dépôt épargne :</label>
  <input type="number" id="depositAmount" placeholder="Montant à déposer" />
  <button id="depositBtn">Déposer</button>

  <label>Retrait épargne :</label>
  <input type="number" id="withdrawAmount" placeholder="Montant à retirer" />
  <button id="withdrawBtn">Retirer</button>

  <p>Solde épargne : <span id="savingsBalance">0</span></p>

  <hr />

  <label>Montant prêt :</label>
  <input type="number" id="loanAmount" placeholder="Montant à emprunter" />
  <button id="takeLoanBtn">Emprunter</button>

  <label>Remboursement prêt :</label>
  <input type="number" id="repayAmount" placeholder="Montant à rembourser" />
  <button id="repayLoanBtn">Rembourser</button>

  <p>Solde prêt : <span id="loanBalance">0</span></p>
</div>

<!-- Social Music Network -->
<div id="socialNetwork">
  <h3>Réseau Social Musical</h3>
  <input type="text" id="username" placeholder="Pseudo" />
  <input type="text" id="musicTitle" placeholder="Titre" />
  <textarea id="musicDescription" placeholder="Description" rows="2"></textarea>
  <input type="url" id="musicURL" placeholder="URL audio" />
  <button id="publishBtn">Publier et Gagner des Tokens</button>
  <div id="rewardMessage" class="mt-2 text-green-400 text-center text-sm"></div>
  <div id="postsContainer" class="space-y-2"></div>
</div>

<!-- Dashboard AI flottant -->
<div id="aiDashboard">
  <h3>Dashboard Maintenance AI</h3>
  <canvas id="aiChart" width="320" height="150" style="background:#111827; border-radius:8px; margin-bottom:10px;"></canvas>
  <div style="display:flex; justify-content: space-between; margin-bottom: 10px;">
    <button id="btnAnalyze">Analyser</button>
    <button id="btnOptimize">Optimiser</button>
    <button id="btnSecurity">Sécurité</button>
  </div>
  <div id="aiLogs"></div>
</div>

<!-- Planète interactive -->
<div id="planet" title="Ouvrir le menu AI">
  <svg viewBox="0 0 24 24" aria-hidden="true">
    <circle cx="12" cy="12" r="10" stroke="white" stroke-width="2" fill="none"/>
    <path d="M12 2a10 10 0 0 1 0 20" stroke="white" stroke-width="2" fill="none"/>
  </svg>
</div>

<!-- Menu radial -->
<div id="radialMenu">
  <button class="radialButton" id="toggleChat" title="Chat">C</button>
  <button class="radialButton" id="toggleBanking" title="Épargne">E</button>
  <button class="radialButton" id="toggleSocial" title="Réseau Social">S</button>
  <button class="radialButton" id="toggleAiDashboard" title="Dashboard AI">AI</button>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.9-1/crypto-js.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  // Instance blockchain simulée
  const blockchain = {
    energyModulation: 0.5,
    nodes: [
      {id: 'node1', power: 800},
      {id: 'node2', power: 900},
      {id: 'node3', power: 750}
    ],
    calculateNodeEfficiency(node) {
      return Math.min(1, node.power / 1000 * this.energyModulation);
    },
    calculateAvgEfficiency() {
      if (this.nodes.length === 0) return 0;
      let total = 0;
      for (const node of this.nodes) {
        total += this.calculateNodeEfficiency(node);
      }
      return total / this.nodes.length;
    }
  };

  const galaxieAstra = { blockchain };

  // Dashboard AI
  const aiLogsDiv = document.getElementById('aiLogs');
  const ctx = document.getElementById('aiChart').getContext('2d');

  let efficiencyData = [];
  let timeLabels = [];

  function logAI(message) {
    const time = new Date().toLocaleTimeString();
    const line = document.createElement('div');
    line.textContent = `[${time}] ${message}`;
    aiLogsDiv.appendChild(line);
    aiLogsDiv.scrollTop = aiLogsDiv.scrollHeight;
  }

  const aiChart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: timeLabels,
      datasets: [{
        label: 'Efficacité Réseau (%)',
        data: efficiencyData,
        borderColor: '#0ea5e9',
        backgroundColor: 'rgba(14, 165, 233, 0.3)',
        fill: true,
        tension: 0.3,
        pointRadius: 3,
        pointHoverRadius: 6,
      }]
    },
    options: {
      responsive: false,
      scales: {
        y: {
          min: 0,
          max: 100,
          ticks: { color: '#0ff' },
          grid: { color: '#0ff33', borderDash: [5,5] }
        },
        x: {
          ticks: { color: '#0ff' },
          grid: { color: '#0ff33', borderDash: [5,5] }
        }
      },
      plugins: {
        legend: { labels: { color: '#0ff' } }
      }
    }
  });

  // Planète interactive
  const planet = document.getElementById('planet');
  const radialMenu = document.getElementById('radialMenu');

  // Toggle menu radial
  planet.addEventListener('click', () => {
    if (radialMenu.style.display === 'flex') {
      radialMenu.style.display = 'none';
      planet.style.transform = 'scale(1)';
    } else {
      radialMenu.style.display = 'flex';
      planet.style.transform = 'scale(1.1) rotate(15deg)';
    }
  });

  // Fermer menu si clic en dehors
  document.addEventListener('click', (e) => {
    if (!planet.contains(e.target) && !radialMenu.contains(e.target)) {
      radialMenu.style.display = 'none';
      planet.style.transform = 'scale(1)';
    }
  });

  // Fonctions pour toggle sections
  function toggleDisplay(id) {
    const el = document.getElementById(id);
    if (!el) return;
    if (el.style.display === 'none' || el.style.display === '') {
      el.style.display = (id === 'chat' || id === 'socialNetwork') ? 'flex' : 'block';
    } else {
      el.style.display = 'none';
    }
  }

  document.getElementById('toggleChat').addEventListener('click', () => {
    toggleDisplay('chat');
    radialMenu.style.display = 'none';
    planet.style.transform = 'scale(1)';
  });
  document.getElementById('toggleBanking').addEventListener('click', () => {
    toggleDisplay('bankingPanel');
    radialMenu.style.display = 'none';
    planet.style.transform = 'scale(1)';
  });
  document.getElementById('toggleSocial').addEventListener('click', () => {
    toggleDisplay('socialNetwork');
    radialMenu.style.display = 'none';
    planet.style.transform = 'scale(1)';
  });
  document.getElementById('toggleAiDashboard').addEventListener('click', () => {
    toggleDisplay('aiDashboard');
    radialMenu.style.display = 'none';
    planet.style.transform = 'scale(1)';
  });

  // Champ Énergétique Planétaire
  (function(){
    const energyCanvas = document.getElementById('energyFieldCanvas');
    const energyCtx = energyCanvas.getContext('2d');
    const energyCenterX = energyCanvas.width / 2;
    const energyCenterY = energyCanvas.height / 2;

    function drawPlanet(x, y, radius, color, glowColor) {
      energyCtx.beginPath();
      energyCtx.arc(x, y, radius, 0, 2 * Math.PI);
      energyCtx.fillStyle = color;
      energyCtx.shadowColor = glowColor;
      energyCtx.shadowBlur = 20;
      energyCtx.fill();
      energyCtx.shadowBlur = 0;
    }

    function drawEnergyField(time) {
      energyCtx.clearRect(0, 0, energyCanvas.width, energyCanvas.height);

      // Planète centrale
      drawPlanet(energyCenterX, energyCenterY, 80, '#0ea5e9', '#22d3ee');

      // Orbites et planètes satellites animées
      const orbitCount = 4;
      for (let i = 1; i <= orbitCount; i++) {
        const orbitRadius = 120 + i * 50;
        energyCtx.beginPath();
        energyCtx.strokeStyle = `rgba(14, 165, 233, 0.2)`;
        energyCtx.lineWidth = 1;
        energyCtx.setLineDash([5, 10]);
        energyCtx.arc(energyCenterX, energyCenterY, orbitRadius, 0, 2 * Math.PI);
        energyCtx.stroke();
        energyCtx.setLineDash([]);

        // Planète en orbite animée
        const angle = (time * 0.001 * (0.5 + i * 0.2)) % (2 * Math.PI);
        const planetX = energyCenterX + orbitRadius * Math.cos(angle);
        const planetY = energyCenterY + orbitRadius * Math.sin(angle);
        drawPlanet(planetX, planetY, 15, '#22d3ee', '#3b82f6');
      }

      // Pulsation d'énergie autour de la planète centrale
      const pulseRadius = 90 + 10 * Math.sin(time * 0.005);
      energyCtx.beginPath();
      energyCtx.arc(energyCenterX, energyCenterY, pulseRadius, 0, 2 * Math.PI);
      energyCtx.strokeStyle = 'rgba(14, 165, 233, 0.4)';
      energyCtx.lineWidth = 4;
      energyCtx.shadowColor = '#0ea5e9';
      energyCtx.shadowBlur = 15;
      energyCtx.stroke();
      energyCtx.shadowBlur = 0;
    }

    function animateEnergyField() {
      const time = Date.now();
      drawEnergyField(time);
      requestAnimationFrame(animateEnergyField);
    }

    animateEnergyField();
  })();
</script>

</body>
</html>
# Astra