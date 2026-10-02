<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta
    name="description"
    content="Jogo educativo sobre netiqueta e combate ao cyberbullying."
  >
  <title>Guardiões da Netiqueta</title>

  <style>
    /* =========================
       Configurações gerais
    ========================== */
    :root {
      --primary: #2563eb;
      --primary-dark: #1d4ed8;
      --secondary: #06b6d4;
      --background: #eff6ff;
      --surface: #ffffff;
      --text: #172554;
      --text-light: #475569;
      --success: #15803d;
      --success-bg: #dcfce7;
      --danger: #b91c1c;
      --danger-bg: #fee2e2;
      --border: #bfdbfe;
      --shadow: 0 20px 50px rgba(30, 64, 175, 0.15);
      --radius: 22px;
    }

    * {
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      margin: 0;
      display: grid;
      place-items: center;
      padding: 24px;
      color: var(--text);
      font-family:
        Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont,
        "Segoe UI", sans-serif;
      background:
        radial-gradient(circle at top left, #cffafe 0, transparent 35%),
        radial-gradient(circle at bottom right, #dbeafe 0, transparent 40%),
        var(--background);
    }

    button {
      font: inherit;
    }

    /* =========================
       Estrutura principal
    ========================== */
    .game {
      width: min(100%, 760px);
    }

    .screen {
      display: none;
      padding: clamp(24px, 5vw, 48px);
      overflow: hidden;
      background: rgba(255, 255, 255, 0.96);
      border: 1px solid rgba(191, 219, 254, 0.8);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      animation: enter 0.35s ease;
    }

    .screen.active {
      display: block;
    }

    @keyframes enter {
      from {
        opacity: 0;
        transform: translateY(12px);
      }

      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      margin-bottom: 16px;
      padding: 8px 13px;
      color: var(--primary-dark);
      font-size: 0.86rem;
      font-weight: 800;
      letter-spacing: 0.04em;
      text-transform: uppercase;
      background: #dbeafe;
      border-radius: 999px;
    }

    h1,
    h2 {
      margin: 0;
      line-height: 1.15;
    }

    h1 {
      max-width: 650px;
      font-size: clamp(2.1rem, 7vw, 4.2rem);
      letter-spacing: -0.055em;
    }

    h2 {
      font-size: clamp(1.35rem, 4vw, 2rem);
      letter-spacing: -0.025em;
    }

    .lead {
      max-width: 620px;
      margin: 20px 0 28px;
      color: var(--text-light);
      font-size: clamp(1rem, 2.5vw, 1.15rem);
      line-height: 1.7;
    }

    /* =========================
       Botões
    ========================== */
    .primary-button {
      min-height: 52px;
      padding: 14px 24px;
      color: #ffffff;
      font-weight: 800;
      cursor: pointer;
      background: linear-gradient(135deg, var(--primary), var(--secondary));
      border: 0;
      border-radius: 14px;
      box-shadow: 0 10px 24px rgba(37, 99, 235, 0.24);
      transition:
        transform 0.2s ease,
        box-shadow 0.2s ease;
    }

    .primary-button:hover {
      transform: translateY(-2px);
      box-shadow: 0 14px 28px rgba(37, 99, 235, 0.3);
    }

    .primary-button:active {
      transform: translateY(0);
    }

    button:focus-visible {
      outline: 4px solid rgba(6, 182, 212, 0.28);
      outline-offset: 3px;
    }

    /* =========================
       Cabeçalho e progresso
    ========================== */
    .status {
      display: flex;
      justify-content: space-between;
      gap: 16px;
      margin-bottom: 14px;
      color: var(--text-light);
      font-size: 0.95rem;
      font-weight: 750;
    }

    .progress-track {
      height: 10px;
      margin-bottom: 30px;
      overflow: hidden;
      background: #e2e8f0;
      border-radius: 999px;
    }

    .progress-bar {
      width: 0;
      height: 100%;
      background: linear-gradient(90deg, var(--primary), var(--secondary));
      border-radius: inherit;
      transition: width 0.35s ease;
    }

    /* =========================
       Perguntas e alternativas
    ========================== */
    .scenario-label {
      margin: 0 0 8px;
      color: var(--primary);
      font-size: 0.82rem;
      font-weight: 850;
      letter-spacing: 0.08em;
      text-transform: uppercase;
    }

    .question {
      margin-bottom: 24px;
      line-height: 1.4;
    }

    .answers {
      display: grid;
      gap: 12px;
    }

    .answer-button {
      width: 100%;
      min-height: 58px;
      padding: 15px 18px;
      color: var(--text);
      text-align: left;
      line-height: 1.45;
      cursor: pointer;
      background: #f8fafc;
      border: 2px solid #dbeafe;
      border-radius: 14px;
      transition:
        border-color 0.2s ease,
        background 0.2s ease,
        transform 0.2s ease;
    }

    .answer-button:hover:not(:disabled) {
      background: #eff6ff;
      border-color: var(--primary);
      transform: translateX(3px);
    }

    .answer-button:disabled {
      cursor: default;
    }

    .answer-button.correct {
      color: #14532d;
      font-weight: 700;
      background: var(--success-bg);
      border-color: #22c55e;
    }

    .answer-button.incorrect {
      color: #7f1d1d;
      font-weight: 700;
      background: var(--danger-bg);
      border-color: #ef4444;
    }

    /* =========================
       Feedback
    ========================== */
    .feedback {
      display: none;
      margin-top: 22px;
      padding: 18px;
      border-radius: 14px;
    }

    .feedback.visible {
      display: block;
      animation: enter 0.25s ease;
    }

    .feedback.success {
      color: #14532d;
      background: var(--success-bg);
      border-left: 5px solid #22c55e;
    }

    .feedback.error {
      color: #7f1d1d;
      background: var(--danger-bg);
      border-left: 5px solid #ef4444;
    }

    .feedback-title {
      display: block;
      margin-bottom: 5px;
      font-size: 1.05rem;
    }

    .feedback p {
      margin: 0;
      line-height: 1.55;
    }

    .next-button {
      display: none;
      margin-top: 18px;
    }

    .next-button.visible {
      display: inline-block;
    }

    /* =========================
       Tela de resultado
    ========================== */
    .result-content {
      text-align: center;
    }

    .result-icon {
      display: grid;
      width: 92px;
      height: 92px;
      margin: 0 auto 22px;
      place-items: center;
      font-size: 3rem;
      background: linear-gradient(135deg, #dbeafe, #cffafe);
      border-radius: 50%;
    }

    .score-card {
      margin: 26px 0;
      padding: 24px;
      background: #eff6ff;
      border: 1px solid var(--border);
      border-radius: 18px;
    }

    .score-number {
      display: block;
      margin-bottom: 5px;
      color: var(--primary-dark);
      font-size: clamp(2.7rem, 9vw, 4.6rem);
      font-weight: 900;
      line-height: 1;
    }

    .result-message {
      margin: 0 0 26px;
      color: var(--text-light);
      font-size: 1.05rem;
      line-height: 1.6;
    }

    @media (max-width: 520px) {
      body {
        padding: 12px;
      }

      .screen {
        padding: 24px 18px;
        border-radius: 18px;
      }

      .status {
        font-size: 0.86rem;
      }

      .primary-button {
        width: 100%;
      }
    }

    @media (prefers-reduced-motion: reduce) {
      *,
      *::before,
      *::after {
        scroll-behavior: auto !important;
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
      }
    }
  </style>
</head>

<body>
  <main class="game">
    <!-- Tela inicial -->
    <section id="start-screen" class="screen active" aria-labelledby="game-title">
      <span class="badge">🛡️ Missão digital</span>

      <h1 id="game-title">Guardiões da Netiqueta</h1>

      <p class="lead">
        Enfrente cinco situações comuns da internet e escolha a atitude mais
        respeitosa. Cada decisão correta ajuda a tornar o ambiente digital
        mais seguro e acolhedor.
      </p>

      <button id="start-button" class="primary-button" type="button">
        Iniciar missão
      </button>
    </section>

    <!-- Tela das fases -->
    <section id="game-screen" class="screen" aria-labelledby="question-title">
      <div class="status">
        <span id="question-counter">Cenário 1 de 5</span>
        <span id="score-display">Pontuação: 0</span>
      </div>

      <div
        class="progress-track"
        role="progressbar"
        aria-label="Progresso do jogo"
        aria-valuemin="0"
        aria-valuemax="5"
        aria-valuenow="1"
      >
        <div id="progress-bar" class="progress-bar"></div>
      </div>

      <p class="scenario-label">O que você faria?</p>
      <h2 id="question-title" class="question"></h2>

      <div
        id="answers"
        class="answers"
        role="group"
        aria-label="Alternativas"
      ></div>

      <div
        id="feedback"
        class="feedback"
        role="status"
        aria-live="polite"
      >
        <strong id="feedback-title" class="feedback-title"></strong>
        <p id="feedback-text"></p>
      </div>

      <button id="next-button" class="primary-button next-button" type="button">
        Próximo cenário
      </button>
    </section>

    <!-- Tela de resultado -->
    <section
      id="result-screen"
      class="screen result-content"
      aria-labelledby="result-title"
    >
      <div class="result-icon" aria-hidden="true">🏆</div>
      <span class="badge">Missão concluída</span>

      <h2 id="result-title">Resultado final</h2>

      <div class="score-card">
        <span id="final-score" class="score-number">0/5</span>
        <span>respostas corretas</span>
      </div>

      <p id="result-message" class="result-message"></p>

      <button id="restart-button" class="primary-button" type="button">
        Reiniciar missão
      </button>
    </section>
  </main>

  <script>
    "use strict";

    /*
     * Cada objeto representa um cenário.
     * A propriedade "correct" indica o índice da alternativa correta.
     */
    const scenarios = [
      {
        question:
          "Um colega publicou uma opinião da qual você discorda. Algumas pessoas começaram a insultá-lo nos comentários. Como agir?",
        answers: [
          "Entrar na discussão e insultar o colega também.",
          "Discordar com respeito, sem atacar a pessoa, e não incentivar as ofensas.",
          "Compartilhar a publicação para que mais pessoas façam piadas.",
          "Criar um perfil falso para continuar a discussão."
        ],
        correct: 1,
        explanation:
          "É possível discordar de uma ideia sem atacar quem a publicou. Uma resposta respeitosa reduz o conflito e contribui para um diálogo saudável."
      },
      {
        question:
          "Você recebeu uma imagem constrangedora de um estudante da escola em um grupo. Qual é a melhor atitude?",
        answers: [
          "Repassar somente para seus amigos mais próximos.",
          "Guardar a imagem para usar em uma brincadeira depois.",
          "Não compartilhar, pedir que parem e procurar ajuda de um adulto responsável.",
          "Publicar a imagem sem identificar a pessoa."
        ],
        correct: 2,
        explanation:
          "Não compartilhar interrompe a exposição da vítima. Pedir que parem e buscar ajuda são atitudes importantes no combate ao cyberbullying."
      },
      {
        question:
          "Durante um jogo online, outro jogador comete um erro e seu time perde. Como você deve responder?",
        answers: [
          "Explicar o erro com calma e incentivar o time a tentar novamente.",
          "Enviar mensagens ofensivas até que o jogador saia.",
          "Expor o perfil do jogador em outras redes.",
          "Incentivar todos a denunciá-lo sem motivo."
        ],
        correct: 0,
        explanation:
          "Erros fazem parte dos jogos. Orientar com calma e manter o respeito fortalece a equipe e evita comportamentos tóxicos."
      },
      {
        question:
          "Uma pessoa começa a enviar mensagens ofensivas repetidamente para você. O que é mais seguro fazer?",
        answers: [
          "Responder com ameaças ainda mais fortes.",
          "Apagar tudo imediatamente e não contar a ninguém.",
          "Divulgar os dados pessoais da pessoa como vingança.",
          "Não revidar, guardar evidências, bloquear e denunciar o perfil."
        ],
        correct: 3,
        explanation:
          "Guardar evidências, bloquear e denunciar ajuda a interromper a agressão. Um adulto de confiança também pode ajudar a lidar com a situação."
      },
      {
        question:
          "Você percebe que um amigo está sendo excluído e ridicularizado em um grupo online. Como pode ajudá-lo?",
        answers: [
          "Ficar em silêncio para não se envolver.",
          "Apoiar o amigo, não participar das ofensas e comunicar a situação a alguém responsável.",
          "Reagir publicando ofensas contra os agressores.",
          "Sair do grupo sem falar com ninguém."
        ],
        correct: 1,
        explanation:
          "Apoiar a vítima mostra que ela não está sozinha. Não alimentar as ofensas e procurar ajuda são formas responsáveis de intervenção."
      }
    ];

    // Referências aos elementos da interface.
    const screens = {
      start: document.getElementById("start-screen"),
      game: document.getElementById("game-screen"),
      result: document.getElementById("result-screen")
    };

    const startButton = document.getElementById("start-button");
    const nextButton = document.getElementById("next-button");
    const restartButton = document.getElementById("restart-button");
    const questionCounter = document.getElementById("question-counter");
    const scoreDisplay = document.getElementById("score-display");
    const progressTrack = document.querySelector(".progress-track");
    const progressBar = document.getElementById("progress-bar");
    const questionTitle = document.getElementById("question-title");
    const answersContainer = document.getElementById("answers");
    const feedback = document.getElementById("feedback");
    const feedbackTitle = document.getElementById("feedback-title");
    const feedbackText = document.getElementById("feedback-text");
    const finalScore = document.getElementById("final-score");
    const resultMessage = document.getElementById("result-message");

    let currentScenario = 0;
    let score = 0;
    let answered = false;

    /**
     * Exibe somente a tela informada.
     * @param {"start" | "game" | "result"} screenName
     */
    function showScreen(screenName) {
      Object.entries(screens).forEach(([name, element]) => {
        element.classList.toggle("active", name === screenName);
      });
    }

    // Reinicia os dados e abre o primeiro cenário.
    function startGame() {
      currentScenario = 0;
      score = 0;
      answered = false;

      showScreen("game");
      renderScenario();
    }

    // Monta o cenário atual e suas alternativas.
    function renderScenario() {
      const scenario = scenarios[currentScenario];
      answered = false;

      questionCounter.textContent =
        `Cenário ${currentScenario + 1} de ${scenarios.length}`;
      scoreDisplay.textContent = `Pontuação: ${score}`;
      questionTitle.textContent = scenario.question;

      const progress = ((currentScenario + 1) / scenarios.length) * 100;
      progressBar.style.width = `${progress}%`;
      progressTrack.setAttribute("aria-valuenow", currentScenario + 1);

      // Limpa o conteúdo do cenário anterior.
      answersContainer.innerHTML = "";
      feedback.className = "feedback";
      feedbackTitle.textContent = "";
      feedbackText.textContent = "";
      nextButton.classList.remove("visible");

      scenario.answers.forEach((answer, index) => {
        const button = document.createElement("button");

        button.type = "button";
        button.className = "answer-button";
        button.textContent = answer;
        button.addEventListener("click", () => selectAnswer(index));

        answersContainer.appendChild(button);
      });

      // Move o foco para a pergunta após a troca de cenário.
      questionTitle.setAttribute("tabindex", "-1");
      questionTitle.focus();
    }

    // Avalia a resposta e apresenta o feedback imediato.
    function selectAnswer(selectedIndex) {
      if (answered) {
        return;
      }

      answered = true;

      const scenario = scenarios[currentScenario];
      const buttons = [...answersContainer.querySelectorAll(".answer-button")];
      const isCorrect = selectedIndex === scenario.correct;

      buttons.forEach((button, index) => {
        button.disabled = true;

        if (index === scenario.correct) {
          button.classList.add("correct");
        }

        if (index === selectedIndex && !isCorrect) {
          button.classList.add("incorrect");
        }
      });

      if (isCorrect) {
        score += 10;
        feedback.classList.add("visible", "success");
        feedbackTitle.textContent = "Resposta correta! +10 pontos";
      } else {
        feedback.classList.add("visible", "error");
        feedbackTitle.textContent = "Essa não é a atitude mais segura.";
      }

      scoreDisplay.textContent = `Pontuação: ${score}`;
      feedbackText.textContent = scenario.explanation;
      nextButton.textContent =
        currentScenario === scenarios.length - 1
          ? "Ver resultado"
          : "Próximo cenário";
      nextButton.classList.add("visible");
      nextButton.focus();
    }

    // Avança para a próxima fase ou encerra o jogo.
    function goToNextScenario() {
      if (!answered) {
        return;
      }

      currentScenario += 1;

      if (currentScenario < scenarios.length) {
        renderScenario();
      } else {
        showResult();
      }
    }

    // Calcula e exibe a mensagem da tela final.
    function showResult() {
      const correctAnswers = score / 10;
      const percentage = (correctAnswers / scenarios.length) * 100;

      finalScore.textContent = `${correctAnswers}/${scenarios.length}`;

      if (percentage === 100) {
        resultMessage.textContent =
          "Excelente! Você demonstrou domínio da netiqueta e está pronto para proteger comunidades digitais.";
      } else if (percentage >= 60) {
        resultMessage.textContent =
          "Muito bem! Você tomou boas decisões. Continue praticando respeito, empatia e segurança na internet.";
      } else {
        resultMessage.textContent =
          "Cada escolha é uma oportunidade de aprender. Lembre-se: não revide, não compartilhe agressões e procure ajuda.";
      }

      showScreen("result");
      restartButton.focus();
    }

    // Eventos principais.
    startButton.addEventListener("click", startGame);
    nextButton.addEventListener("click", goToNextScenario);
    restartButton.addEventListener("click", startGame);
  </script>
</body>
</html>
