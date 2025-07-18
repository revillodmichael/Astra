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

  /* Styles pour NeuralLink */
  #info {
    margin: 10px;
    font-size: 1.2rem;
    text-shadow: 0 0 10px #0ff;
    text-align: center;
  }
  #energyCanvas {
    background: radial-gradient(circle at center, #001f3f 0%, #000 80%);
    border-radius: 20px;
    box-shadow: 0 0 30px #0ff;
    display: block;
    width: 600px;
    height: 600px;
  }
  #chat {
    margin-top: 10px;
    width: 600px;
    max-width: 90vw;
    background: #111827;
    border-radius: 10px;
    padding: 10px;
    box-shadow: 0 0 15px #0ff;
    display: flex;
    flex-direction: column;
  }
  #messages {
    height: 200px;
    overflow-y: auto;
    padding: 10px;
    font-size: 14px;
    color: #eee;
  }
  #inputArea {
    display: flex;
    border-top: 1px solid #334155;
  }
  #inputArea input {
    flex: 1;
    padding: 10px;
    border: none;
    font-size: 16px;
    border-radius: 0 0 0 10px;
    background: #1f2937;
    color: white;
  }
  #inputArea button {
    padding: 10px 20px;
    background: #6366f1;
    border: none;
    color: white;
    font-weight: bold;
    cursor: pointer;
    border-radius: 0 0 10px 0;
    transition: background-color 0.3s;
  }
  #inputArea button:hover {
    background-color: #4f46e5;
  }
  .message {
    margin: 5px 0;
    max-width: 80%;
    word-wrap: break-word;
  }
  .message.self {
    align-self: flex-end;
    background: #6366f1;
    padding: 6px 10px;
    border-radius: 10px 10px 0 10px;
  }
  .message.other {
    align-self: flex-start;
    background: #0f172a;
    padding: 6px 10px;
    border-radius: 10px 10px 10px 0;
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
  // --- Simulation Champ Énergétique & Réseau ---
  const canvas = document.getElementById('energyCanvas');
  const ctx = canvas.getContext('2d');
  const width = canvas.width;
  const height = canvas.height;
  const centerX = width / 2;
  const centerY = height / 2;

  const maxRadius = width * 0.45;
  const waveCount = 6;
  const waveSpeed = 0.015;

  const nodeCount = 30;
  let nodes = [];
  for(let i=0; i<nodeCount; i++) {
    const angle = Math.random() * 2 * Math.PI;
    const radius = 80 + Math.random() * (maxRadius - 120);
    nodes.push({
      x: centerX + radius * Math.cos(angle),
      y: centerY + radius * Math.sin(angle),
      baseRadius: 4 + Math.random() * 2,
      pulse: Math.random() * Math.PI * 2,
      color: '#0ff',
      reactiveRadius: 0
    });
  }

  let time = 0;

  function drawBackground() {
    const gradient = ctx.createRadialGradient(centerX, centerY, 0, centerX, centerY, maxRadius);
    gradient.addColorStop(0, 'rgba(0,255,255,0.05)');
    gradient.addColorStop(1, 'rgba(0,0,0,0.9)');
    ctx.fillStyle = gradient;
    ctx.fillRect(0, 0, width, height);
  }

  function drawPlanetCore() {
    const grad = ctx.createRadialGradient(centerX, centerY, 20, centerX, centerY, 100);
    grad.addColorStop(0, '#0ff');
    grad.addColorStop(1, 'transparent');
    ctx.fillStyle = grad;
    ctx.beginPath();
    ctx.arc(centerX, centerY, 60, 0, Math.PI * 2);
    ctx.fill();

    ctx.beginPath();
    ctx.arc(centerX, centerY, 20, 0, Math.PI * 2);
    ctx.fillStyle = '#0ff';
    ctx.shadowColor = '#0ff';
    ctx.shadowBlur = 20;
    ctx.fill();
    ctx.shadowBlur = 0;
  }

  function drawWaves() {
    for(let i=0; i<waveCount; i++) {
      let progress = (time + i / waveCount) % 1;
      let radius = progress * maxRadius;
      let alpha = 1 - progress;
      ctx.beginPath();
      ctx.arc(centerX, centerY, radius, 0, Math.PI * 2);
      ctx.strokeStyle = `rgba(0, 255, 255, ${alpha * 0.3})`;
      ctx.lineWidth = 2;
      ctx.shadowColor = 'cyan';
      ctx.shadowBlur = 15;
      ctx.stroke();
      ctx.shadowBlur = 0;

      // Perturbation detection
      nodes.forEach(node => {
        const dist = Math.hypot(node.x - centerX, node.y - centerY);
        const waveThickness = 10;
        if (Math.abs(dist - radius) < waveThickness) {
          node.reactiveRadius = 10;
          node.color = '#ff3300';
        }
      });
    }
  }

  function drawNodes() {
    nodes.forEach(node => {
      if (node.reactiveRadius > 0) {
        node.reactiveRadius -= 0.3;
        if (node.reactiveRadius < 0) node.reactiveRadius = 0;
      } else {
        node.color = '#0ff';
      }

      const pulse = (Math.sin(time * 5 + node.pulse) + 1) / 2;
      const radius = node.baseRadius + pulse * 2 + node.reactiveRadius;

      ctx.beginPath();
      ctx.arc(node.x, node.y, radius, 0, Math.PI * 2);
      ctx.fillStyle = node.color;
      ctx.shadowColor = node.color;
      ctx.shadowBlur = 10;
      ctx.fill();
      ctx.shadowBlur = 0;
    });
  }

  // --- IA simulée pour ajuster les nœuds ---
  function simulateAIAdjustments() {
    nodes.forEach(node => {
      node.newRadius = node.baseRadius * (1 + 0.3 * Math.sin(time * 2 + node.pulse));
    });
  }

  function animate() {
    ctx.clearRect(0, 0, width, height);
    drawBackground();
    drawPlanetCore();
    drawWaves();
    drawNodes();
    simulateAIAdjustments();
    time += waveSpeed;
    requestAnimationFrame(animate);
  }

  animate();

  // --- Chat simple avec chiffrement simulé ---
  const messagesDiv = document.getElementById('messages');
  const input = document.getElementById('messageInput');
  const sendBtn = document.getElementById('sendBtn');

  function addMessage(text, self = false) {
    const div = document.createElement('div');
    div.textContent = text;
    div.className = 'message ' + (self ? 'self' : 'other');
    messagesDiv.appendChild(div);
    messagesDiv.scrollTop = messagesDiv.scrollHeight;
  }

  sendBtn.addEventListener('click', () => {
    const text = input.value.trim();
    if (!text) return;
    addMessage("Vous : " + text, true);
    input.value = '';

    // Simulation chiffrement (simple inversion)
    const encrypted = text.split('').reverse().join('');
    setTimeout(() => {
      const decrypted = encrypted.split('').reverse().join('');
      addMessage("NeuralLink IA : " + decrypted, false);
    }, 500);
  });

  input.addEventListener('keydown', e => {
    if (e.key === 'Enter') sendBtn.click();
  });

  // Instance blockchain simulée
  const blockchain = {
    energyMod