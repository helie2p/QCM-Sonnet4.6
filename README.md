# QCM-Sonnet4.6
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>QCM – L'IA dans l'enseignement</title>
<style>
  :root {
    --neon: #00c3ff;
    --neon2: #0066ff;
    --bg: #07090f;
    --card: #0d1120;
    --card2: #111827;
    --text: #e8eaf6;
    --muted: #8899bb;
    --green: #00e676;
    --red: #ff4444;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Segoe UI', system-ui, sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ─── NEON BORDER ─── */
  .neon-frame {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 9999;
  }
  .neon-frame::before,
  .neon-frame::after {
    content: '';
    position: absolute;
    border-radius: 4px;
  }
  /* Top bar */
  .neon-frame::before {
    top: 0; left: 0; right: 0;
    height: 3px;
    background: linear-gradient(90deg, transparent, var(--neon), var(--neon2), var(--neon), transparent);
    background-size: 200% 100%;
    animation: neon-run-h 3s linear infinite;
    box-shadow: 0 0 12px var(--neon), 0 0 30px var(--neon2);
  }
  /* Bottom bar */
  .neon-frame::after {
    bottom: 0; left: 0; right: 0;
    height: 3px;
    background: linear-gradient(90deg, transparent, var(--neon2), var(--neon), var(--neon2), transparent);
    background-size: 200% 100%;
    animation: neon-run-h 3s linear infinite reverse;
    box-shadow: 0 0 12px var(--neon), 0 0 30px var(--neon2);
  }
  .neon-left, .neon-right {
    position: fixed;
    top: 0; bottom: 0;
    width: 3px;
    background: linear-gradient(180deg, transparent, var(--neon), var(--neon2), var(--neon), transparent);
    background-size: 100% 200%;
    z-index: 9999;
    pointer-events: none;
  }
  .neon-left {
    left: 0;
    animation: neon-run-v 3s linear infinite;
    box-shadow: 0 0 12px var(--neon), 0 0 30px var(--neon2);
  }
  .neon-right {
    right: 0;
    animation: neon-run-v 3s linear infinite reverse;
    box-shadow: 0 0 12px var(--neon), 0 0 30px var(--neon2);
  }

  @keyframes neon-run-h {
    0%   { background-position: -100% 0; }
    100% { background-position: 300% 0; }
  }
  @keyframes neon-run-v {
    0%   { background-position: 0 -100%; }
    100% { background-position: 0 300%; }
  }

  /* ─── CONFETTI CANVAS ─── */
  #confetti-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    pointer-events: none;
    z-index: 1000;
    display: none;
  }

  /* ─── WELCOME SCREEN ─── */
  #welcome {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 2rem;
    text-align: center;
    position: relative;
  }
  #welcome .logo {
    font-size: clamp(2.5rem, 6vw, 5rem);
    font-weight: 900;
    background: linear-gradient(135deg, var(--neon), var(--neon2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    letter-spacing: -0.02em;
    margin-bottom: 0.5rem;
    text-shadow: none;
    filter: drop-shadow(0 0 20px rgba(0,195,255,0.5));
  }
  #welcome .subtitle {
    font-size: clamp(0.9rem, 2vw, 1.3rem);
    color: var(--muted);
    margin-bottom: 0.8rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }
  #welcome .institution {
    font-size: clamp(0.75rem, 1.5vw, 1rem);
    color: rgba(136,153,187,0.6);
    margin-bottom: 3rem;
  }
  #welcome .tagline {
    font-size: clamp(1rem, 2.2vw, 1.5rem);
    color: var(--text);
    max-width: 600px;
    line-height: 1.6;
    margin-bottom: 3rem;
  }
  .btn-start {
    padding: 1.1rem 3.5rem;
    font-size: 1.2rem;
    font-weight: 700;
    color: var(--bg);
    background: linear-gradient(135deg, var(--neon), var(--neon2));
    border: none;
    border-radius: 50px;
    cursor: pointer;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    box-shadow: 0 0 25px rgba(0,195,255,0.5), 0 0 50px rgba(0,102,255,0.3);
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .btn-start:hover {
    transform: scale(1.06);
    box-shadow: 0 0 40px rgba(0,195,255,0.7), 0 0 70px rgba(0,102,255,0.5);
  }
  .deco-circle {
    position: absolute;
    border-radius: 50%;
    opacity: 0.06;
    animation: pulse-deco 6s ease-in-out infinite;
  }
  .deco-circle:nth-child(1) { width: 500px; height: 500px; background: var(--neon); top: -100px; right: -100px; animation-delay: 0s; }
  .deco-circle:nth-child(2) { width: 300px; height: 300px; background: var(--neon2); bottom: -50px; left: -50px; animation-delay: 2s; }
  @keyframes pulse-deco { 0%,100% { transform: scale(1); } 50% { transform: scale(1.15); } }

  /* ─── QUIZ SCREEN ─── */
  #quiz { display: none; flex-direction: column; min-height: 100vh; padding: 0; }

  /* TOP FIXED PANEL */
  #top-panel {
    position: sticky;
    top: 0;
    z-index: 100;
    background: linear-gradient(180deg, var(--bg) 80%, transparent);
    padding: 1.5rem 3rem 1rem;
    backdrop-filter: blur(10px);
  }
  .quiz-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 1rem;
    flex-wrap: wrap;
    gap: 1rem;
  }
  .quiz-title {
    font-size: clamp(1rem, 2.5vw, 1.6rem);
    font-weight: 800;
    background: linear-gradient(135deg, var(--neon), var(--neon2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .score-badge {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    background: var(--card);
    border: 1px solid rgba(0,195,255,0.2);
    border-radius: 50px;
    padding: 0.5rem 1.2rem;
  }
  .score-badge span:first-child { color: var(--muted); font-size: 0.85rem; }
  .score-badge .score-val {
    font-size: 1.4rem;
    font-weight: 900;
    color: var(--neon);
    filter: drop-shadow(0 0 8px var(--neon));
  }
  .progress-bar-wrapper {
    background: rgba(255,255,255,0.06);
    border-radius: 50px;
    height: 8px;
    overflow: hidden;
    position: relative;
  }
  .progress-bar-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--neon2), var(--neon));
    border-radius: 50px;
    transition: width 0.5s cubic-bezier(.4,0,.2,1);
    box-shadow: 0 0 10px var(--neon);
    width: 0%;
  }
  .progress-label {
    text-align: right;
    font-size: 0.75rem;
    color: var(--muted);
    margin-top: 0.3rem;
  }

  /* BOTTOM SCROLLABLE PANEL */
  #bottom-panel {
    flex: 1;
    padding: 1rem 3rem 3rem;
    display: flex;
    flex-direction: column;
  }

  .question-card {
    background: var(--card);
    border: 1px solid rgba(0,195,255,0.12);
    border-radius: 20px;
    padding: 2rem 2.5rem;
    margin-bottom: 1.5rem;
    box-shadow: 0 4px 30px rgba(0,0,0,0.4);
  }
  .question-num {
    font-size: 0.8rem;
    color: var(--neon);
    letter-spacing: 0.12em;
    text-transform: uppercase;
    margin-bottom: 0.6rem;
    font-weight: 600;
  }
  .question-text {
    font-size: clamp(1.1rem, 2.5vw, 1.6rem);
    font-weight: 700;
    line-height: 1.45;
    color: var(--text);
  }

  .options-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  @media (max-width: 700px) { .options-grid { grid-template-columns: 1fr; } }

  .option-btn {
    display: flex;
    align-items: flex-start;
    gap: 1rem;
    background: var(--card2);
    border: 2px solid rgba(255,255,255,0.08);
    border-radius: 14px;
    padding: 1rem 1.2rem;
    cursor: pointer;
    text-align: left;
    color: var(--text);
    font-size: clamp(0.9rem, 1.5vw, 1.05rem);
    line-height: 1.45;
    transition: border-color 0.15s, background 0.15s, transform 0.1s;
    position: relative;
    overflow: hidden;
  }
  .option-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,195,255,0.06), transparent);
    opacity: 0;
    transition: opacity 0.2s;
  }
  .option-btn:hover:not(:disabled)::before { opacity: 1; }
  .option-btn:hover:not(:disabled) {
    border-color: rgba(0,195,255,0.4);
    transform: translateY(-2px);
  }
  .option-btn:disabled { cursor: default; transform: none !important; }
  .option-btn.correct {
    border-color: var(--green) !important;
    background: rgba(0,230,118,0.1) !important;
    box-shadow: 0 0 20px rgba(0,230,118,0.2);
    animation: bounce-in 0.3s ease;
  }
  .option-btn.wrong {
    border-color: var(--red) !important;
    background: rgba(255,68,68,0.1) !important;
    box-shadow: 0 0 20px rgba(255,68,68,0.15);
    animation: shake 0.4s ease;
  }
  @keyframes bounce-in {
    0% { transform: scale(0.97); }
    60% { transform: scale(1.03); }
    100% { transform: scale(1); }
  }
  @keyframes shake {
    0%,100% { transform: translateX(0); }
    25% { transform: translateX(-5px); }
    75% { transform: translateX(5px); }
  }

  .letter-badge {
    min-width: 2rem;
    height: 2rem;
    border-radius: 50%;
    background: rgba(0,195,255,0.12);
    border: 1.5px solid rgba(0,195,255,0.3);
    display: flex; align-items: center; justify-content: center;
    font-weight: 800;
    font-size: 0.9rem;
    color: var(--neon);
    flex-shrink: 0;
    margin-top: 0.05rem;
    transition: background 0.2s;
  }
  .option-btn.correct .letter-badge { background: rgba(0,230,118,0.2); border-color: var(--green); color: var(--green); }
  .option-btn.wrong .letter-badge { background: rgba(255,68,68,0.2); border-color: var(--red); color: var(--red); }

  /* FEEDBACK */
  .feedback-box {
    border-radius: 14px;
    padding: 1.2rem 1.5rem;
    font-size: clamp(0.9rem, 1.5vw, 1rem);
    line-height: 1.6;
    display: none;
    margin-bottom: 1.5rem;
    animation: fade-in 0.3s ease;
  }
  .feedback-box.show { display: block; }
  .feedback-box.success {
    background: rgba(0,230,118,0.1);
    border: 1.5px solid rgba(0,230,118,0.4);
    color: #b9fbd6;
  }
  .feedback-box.error {
    background: rgba(255,68,68,0.08);
    border: 1.5px solid rgba(255,68,68,0.35);
    color: #ffbaba;
  }
  .feedback-icon { font-size: 1.4rem; margin-bottom: 0.3rem; }
  @keyframes fade-in { from { opacity: 0; transform: translateY(6px); } to { opacity:1; transform:none; } }

  /* NEXT BUTTON */
  .btn-next {
    display: none;
    padding: 0.9rem 2.5rem;
    font-size: 1rem;
    font-weight: 700;
    color: var(--bg);
    background: linear-gradient(135deg, var(--neon), var(--neon2));
    border: none;
    border-radius: 50px;
    cursor: pointer;
    letter-spacing: 0.04em;
    box-shadow: 0 0 20px rgba(0,195,255,0.4);
    transition: transform 0.2s, box-shadow 0.2s;
    align-self: flex-end;
    margin-top: 0.5rem;
  }
  .btn-next:hover { transform: scale(1.05); box-shadow: 0 0 30px rgba(0,195,255,0.6); }
  .btn-next.show { display: block; }

  /* ─── RESULT SCREEN ─── */
  #result { display: none; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; padding: 3rem 2rem; text-align: center; }
  .result-title { font-size: clamp(1.8rem, 5vw, 4rem); font-weight: 900; background: linear-gradient(135deg, var(--neon), var(--neon2)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; margin-bottom: 0.5rem; }
  .result-score-big { font-size: clamp(4rem, 12vw, 9rem); font-weight: 900; color: var(--neon); filter: drop-shadow(0 0 30px var(--neon)); line-height: 1; margin: 1rem 0; }
  .result-max { font-size: 1.2rem; color: var(--muted); margin-bottom: 2rem; }
  .result-msg { font-size: clamp(1rem, 2vw, 1.4rem); max-width: 600px; line-height: 1.7; color: var(--text); margin-bottom: 2.5rem; }
  .btn-restart {
    padding: 1rem 3rem;
    font-size: 1rem;
    font-weight: 700;
    color: var(--bg);
    background: linear-gradient(135deg, var(--neon), var(--neon2));
    border: none;
    border-radius: 50px;
    cursor: pointer;
    box-shadow: 0 0 20px rgba(0,195,255,0.4);
    transition: transform 0.2s;
  }
  .btn-restart:hover { transform: scale(1.05); }
</style>
</head>
<body>

<!-- NEON FRAME -->
<div class="neon-frame"></div>
<div class="neon-left"></div>
<div class="neon-right"></div>

<!-- CONFETTI -->
<canvas id="confetti-canvas"></canvas>

<!-- ══════════ WELCOME SCREEN ══════════ -->
<section id="welcome">
  <div class="deco-circle"></div>
  <div class="deco-circle"></div>
  <div class="logo">IA &amp; Enseignement</div>
  <div class="subtitle">Formation introductive — QCM de clôture</div>
  <div class="institution">IPC – Facultés Libres de Philosophie et Psychologie</div>
  <p class="tagline">7 questions pour valider vos acquis de la journée sur l'intelligence artificielle dans le contexte éducatif.</p>
  <button class="btn-start" onclick="startQuiz()">Commencer →</button>
</section>

<!-- ══════════ QUIZ SCREEN ══════════ -->
<section id="quiz">
  <!-- TOP FIXED -->
  <div id="top-panel">
    <div class="quiz-header">
      <div class="quiz-title">🧠 IA &amp; Enseignement — QCM</div>
      <div class="score-badge">
        <span>Score</span>
        <span class="score-val" id="score-display">0</span>
        <span style="color:var(--muted);font-size:0.85rem">/ 7</span>
      </div>
    </div>
    <div class="progress-bar-wrapper">
      <div class="progress-bar-fill" id="progress-fill"></div>
    </div>
    <div class="progress-label" id="progress-label">Question 0 / 7</div>
  </div>

  <!-- BOTTOM SCROLLABLE -->
  <div id="bottom-panel">
    <div class="question-card">
      <div class="question-num" id="q-num">Question 1 / 7</div>
      <div class="question-text" id="q-text"></div>
    </div>
    <div class="options-grid" id="options-grid"></div>
    <div class="feedback-box" id="feedback-box">
      <div class="feedback-icon" id="feedback-icon"></div>
      <div id="feedback-text"></div>
    </div>
    <button class="btn-next" id="btn-next" onclick="nextQuestion()">Question suivante →</button>
  </div>
</section>

<!-- ══════════ RESULT SCREEN ══════════ -->
<section id="result">
  <div class="result-title">Résultats</div>
  <div class="result-score-big" id="final-score">0</div>
  <div class="result-max">/ 7 bonnes réponses</div>
  <div class="result-msg" id="result-msg"></div>
  <button class="btn-restart" onclick="restart()">↩ Recommencer</button>
</section>

<script>
/* ══════════════════════════════
   QUESTIONS (issues du diaporama)
══════════════════════════════ */
const ALL_QUESTIONS = [
  {
    text: "En combien de jours ChatGPT a-t-il atteint 1 million d'utilisateurs lors de son lancement en novembre 2022 ?",
    answers: [
      { letter: "A", text: "En 3 jours" },
      { letter: "B", text: "En 5 jours" },
      { letter: "C", text: "En 10 jours" },
      { letter: "D", text: "En 30 jours" }
    ],
    correct: 1, // index 0-based → B
    explanation: "ChatGPT a atteint 1 million d'utilisateurs en seulement 5 jours après son lancement, un record de vitesse d'adoption sans précédent — à titre de comparaison, Netflix a mis 3,5 ans pour y parvenir !"
  },
  {
    text: "Quel était le nombre de visites mensuelles enregistrées par ChatGPT en mai 2025 ?",
    answers: [
      { letter: "A", text: "18 millions" },
      { letter: "B", text: "500 millions" },
      { letter: "C", text: "5 milliards" },
      { letter: "D", text: "1 milliard" }
    ],
    correct: 2, // C
    explanation: "En mai 2025, ChatGPT enregistrait 5 milliards de visites mensuelles, contre 18 millions lors de ses premiers mois de lancement en 2022. Une croissance phénoménale en moins de 3 ans."
  },
  {
    text: "Qu'est-ce qu'un LLM (Large Language Model) ?",
    answers: [
      { letter: "A", text: "Un logiciel de traduction automatique basé sur des règles grammaticales" },
      { letter: "B", text: "Un réseau de neurones profonds entraîné par apprentissage auto-supervisé sur de grandes quantités de textes" },
      { letter: "C", text: "Une base de données encyclopédique mise à jour en temps réel" },
      { letter: "D", text: "Un algorithme de reconnaissance vocale spécialisé pour l'éducation" }
    ],
    correct: 1, // B
    explanation: "Un LLM (Large Language Model) est un réseau de neurones profonds entraîné par apprentissage auto-supervisé sur de grandes quantités de textes. Il filtre automatiquement les données brutes en informations utiles. ChatGPT, Claude, Gemini en sont des exemples."
  },
  {
    text: "Qu'appelle-t-on une « hallucination » dans le contexte des IA comme ChatGPT ?",
    answers: [
      { letter: "A", text: "Une réponse que l'IA refuse de donner car elle juge le sujet sensible" },
      { letter: "B", text: "Un biais racial ou de genre présent dans les données d'entraînement" },
      { letter: "C", text: "Une information fausse ou inventée présentée comme vraie par l'IA" },
      { letter: "D", text: "Une confusion entre deux langues dans une même réponse" }
    ],
    correct: 2, // C
    explanation: "Une hallucination désigne une information fausse ou inventée que l'IA présente comme vraie. L'IA ne vérifie pas ses réponses comme un humain : elle génère du texte en se basant sur les patterns appris à l'entraînement, et peut produire quelque chose de plausible mais inexact."
  },
  {
    text: "Quelle est la proportion de professionnels déclarant utiliser l'IA au travail en 2026, selon les données présentées dans la formation ?",
    answers: [
      { letter: "A", text: "48 %" },
      { letter: "B", text: "62 %" },
      { letter: "C", text: "78 %" },
      { letter: "D", text: "91 %" }
    ],
    correct: 2, // C
    explanation: "En 2026, 78 % des professionnels déclarent utiliser l'IA au travail, contre seulement 48 % en 2024. L'adoption massive de l'IA dans le monde professionnel est donc bien une réalité que l'éducation doit intégrer (source : Intapp, Independent)."
  },
  {
    text: "Parmi ces techniques de rédaction de prompt, laquelle consiste à demander à l'IA de générer elle-même le prompt qui vous permettra d'obtenir un meilleur résultat ?",
    answers: [
      { letter: "A", text: "Le prompting contextuel" },
      { letter: "B", text: "Le méta-prompting" },
      { letter: "C", text: "Le chain-of-thought" },
      { letter: "D", text: "Le prompt négatif" }
    ],
    correct: 1, // B
    explanation: "Le méta-prompting consiste à demander à l'IA de générer votre prompt à votre place, puis à l'affiner si nécessaire. C'est une technique puissante pour obtenir des résultats ciblés, même quand on ne sait pas exactement comment formuler sa demande."
  },
  {
    text: "Combien de principes éthiques le règlement européen sur l'IA (AI Act) définit-il pour encadrer les systèmes d'IA dignes de confiance ?",
    answers: [
      { letter: "A", text: "3 principes" },
      { letter: "B", text: "5 principes" },
      { letter: "C", text: "7 principes" },
      { letter: "D", text: "10 principes" }
    ],
    correct: 2, // C
    explanation: "Le règlement européen sur l'IA définit 7 principes éthiques : action humaine et contrôle, robustesse technique et sécurité, respect de la vie privée, gouvernance des données, transparence, diversité et non-discrimination, et bien-être environnemental et social."
  }
];

/* Shuffle answers so correct answer isn't always in same spot */
function shuffleAnswers(q) {
  const correctText = q.answers[q.correct].text;
  const shuffled = [...q.answers].sort(() => Math.random() - 0.5);
  const newCorrect = shuffled.findIndex(a => a.text === correctText);
  const letters = ['A','B','C','D'];
  shuffled.forEach((a, i) => a.letter = letters[i]);
  return { ...q, answers: shuffled, correct: newCorrect };
}

let questions = [];
let current = 0;
let score = 0;
let answered = false;

/* ── CONFETTI ENGINE ── */
const canvas = document.getElementById('confetti-canvas');
const ctx = canvas.getContext('2d');
let confettiParticles = [];
let confettiLoop = null;
let confettiContinuous = false;

const COLORS = ['#00c3ff','#0066ff','#00e676','#ffea00','#ff6b6b','#c77dff','#ffffff'];

function spawnConfetti(n, y = 0) {
  for (let i = 0; i < n; i++) {
    confettiParticles.push({
      x: Math.random() * canvas.width,
      y: y - Math.random() * 20,
      r: Math.random() * 6 + 3,
      d: Math.random() * 20 + 5,
      color: COLORS[Math.floor(Math.random() * COLORS.length)],
      tilt: Math.random() * 10 - 5,
      tiltSpeed: Math.random() * 0.1 + 0.05,
      speed: Math.random() * 3 + 1.5,
      opacity: 1,
      shape: Math.random() > 0.5 ? 'rect' : 'circle'
    });
  }
}

function drawConfetti() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  confettiParticles.forEach(p => {
    ctx.save();
    ctx.globalAlpha = p.opacity;
    ctx.fillStyle = p.color;
    ctx.translate(p.x, p.y);
    ctx.rotate(p.tilt * Math.PI / 180);
    if (p.shape === 'rect') {
      ctx.fillRect(-p.r, -p.r / 2, p.r * 2, p.r);
    } else {
      ctx.beginPath();
      ctx.arc(0, 0, p.r, 0, Math.PI * 2);
      ctx.fill();
    }
    ctx.restore();
    p.y += p.speed;
    p.tilt += p.tiltSpeed;
    p.x += Math.sin(p.tilt) * 1.2;
    if (!confettiContinuous) p.opacity -= 0.008;
  });
  confettiParticles = confettiParticles.filter(p => p.y < canvas.height + 20 && p.opacity > 0);
  if (confettiContinuous) {
    if (Math.random() > 0.5) spawnConfetti(3);
  }
}

function startConfettiBurst() {
  canvas.style.display = 'block';
  confettiContinuous = false;
  spawnConfetti(120);
  if (confettiLoop) cancelAnimationFrame(confettiLoop);
  function loop() {
    drawConfetti();
    if (confettiParticles.length > 0) {
      confettiLoop = requestAnimationFrame(loop);
    } else {
      canvas.style.display = 'none';
    }
  }
  confettiLoop = requestAnimationFrame(loop);
}

function startConfettiRain() {
  canvas.style.display = 'block';
  confettiContinuous = true;
  confettiParticles = [];
  spawnConfetti(80);
  if (confettiLoop) cancelAnimationFrame(confettiLoop);
  function loop() {
    drawConfetti();
    confettiLoop = requestAnimationFrame(loop);
  }
  confettiLoop = requestAnimationFrame(loop);
}

function stopConfetti() {
  confettiContinuous = false;
  if (confettiLoop) cancelAnimationFrame(confettiLoop);
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  canvas.style.display = 'none';
  confettiParticles = [];
}

/* ── QUIZ LOGIC ── */
function startQuiz() {
  questions = ALL_QUESTIONS.map(shuffleAnswers);
  current = 0;
  score = 0;
  answered = false;
  document.getElementById('welcome').style.display = 'none';
  document.getElementById('quiz').style.display = 'flex';
  document.getElementById('score-display').textContent = '0';
  renderQuestion();
}

function renderQuestion() {
  const q = questions[current];
  answered = false;

  // Progress
  const pct = (current / 7) * 100;
  document.getElementById('progress-fill').style.width = pct + '%';
  document.getElementById('progress-label').textContent = `Question ${current + 1} / 7`;

  // Question
  document.getElementById('q-num').textContent = `Question ${current + 1} / 7`;
  document.getElementById('q-text').textContent = q.text;

  // Options
  const grid = document.getElementById('options-grid');
  grid.innerHTML = '';
  q.answers.forEach((ans, i) => {
    const btn = document.createElement('button');
    btn.className = 'option-btn';
    btn.innerHTML = `<span class="letter-badge">${ans.letter}</span><span>${ans.text}</span>`;
    btn.onclick = () => selectAnswer(i);
    grid.appendChild(btn);
  });

  // Reset feedback
  const fb = document.getElementById('feedback-box');
  fb.className = 'feedback-box';
  document.getElementById('btn-next').className = 'btn-next';

  // Scroll top of bottom panel
  document.getElementById('bottom-panel').scrollTop = 0;

  stopConfetti();
}

function selectAnswer(idx) {
  if (answered) return;
  answered = true;
  const q = questions[current];
  const buttons = document.querySelectorAll('.option-btn');
  buttons.forEach(b => b.disabled = true);

  const fb = document.getElementById('feedback-box');
  const fbIcon = document.getElementById('feedback-icon');
  const fbText = document.getElementById('feedback-text');

  if (idx === q.correct) {
    buttons[idx].classList.add('correct');
    fb.className = 'feedback-box success show';
    fbIcon.textContent = '🎉';
    fbText.innerHTML = `<strong>Bonne réponse !</strong><br>${q.explanation}`;
    score++;
    document.getElementById('score-display').textContent = score;
    startConfettiBurst();
  } else {
    buttons[idx].classList.add('wrong');
    buttons[q.correct].classList.add('correct');
    fb.className = 'feedback-box error show';
    fbIcon.textContent = '💡';
    fbText.innerHTML = `<strong>Pas tout à fait…</strong><br>${q.explanation}`;
  }

  if (current < 6) {
    document.getElementById('btn-next').className = 'btn-next show';
  } else {
    setTimeout(showResult, 1400);
  }
}

function nextQuestion() {
  stopConfetti();
  current++;
  renderQuestion();
}

function showResult() {
  stopConfetti();
  document.getElementById('quiz').style.display = 'none';
  const result = document.getElementById('result');
  result.style.display = 'flex';
  document.getElementById('final-score').textContent = score;

  const msgs = [
    { min: 0, max: 2, text: "La journée était dense ! 😊 L'essentiel est d'avoir découvert ces outils. Relire les grandes notions (LLM, hallucination, prompting) sera un bon point de départ pour approfondir." },
    { min: 3, max: 4, text: "Un bon début ! 👍 Vous avez saisi plusieurs concepts clés. Quelques révisions sur le règlement européen et les techniques de prompt vous permettront de consolider tout ça." },
    { min: 5, max: 6, text: "Très bien ! 🌟 Vous avez bien assimilé la formation. Vos élèves auront de la chance d'avoir un enseignant aussi attentif aux enjeux de l'IA." },
    { min: 7, max: 7, text: "Parfait ! 🏆 Score maximal ! Vous avez tout retenu. La formation n'a (presque) plus de secrets pour vous. Prêt·e à intégrer l'IA dans vos pratiques pédagogiques !" }
  ];
  const msg = msgs.find(m => score >= m.min && score <= m.max);
  document.getElementById('result-msg').textContent = msg.text;
  startConfettiRain();
}

function restart() {
  stopConfetti();
  document.getElementById('result').style.display = 'none';
  startQuiz();
}

window.addEventListener('resize', () => {
  if (canvas.style.display === 'block') {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
});
</script>
</body>
</html>
