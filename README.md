<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Desafio The Dark Cavern - Quiz</title>
    <style>
        * { box-sizing: border-box; }
        body {
            background-color: #08080a;
            color: #d1d5db;
            font-family: 'Courier New', Courier, monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 15px;
        }
        .container {
            width: 100%;
            max-width: 500px;
            background: #111827;
            border: 2px solid #374151;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.8);
        }
        h1 {
            color: #a855f7;
            text-align: center;
            margin-top: 0;
            text-transform: uppercase;
            letter-spacing: 2px;
        }
        .progress {
            font-size: 0.9em;
            color: #9ca3af;
            margin-bottom: 15px;
            text-align: right;
        }
        .question {
            font-size: 1.1em;
            margin-bottom: 20px;
            font-weight: bold;
            color: #f3f4f6;
        }
        .options {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .opt-btn {
            background: #1f2937;
            color: #f3f4f6;
            border: 1px solid #4b5563;
            padding: 12px 15px;
            border-radius: 6px;
            text-align: left;
            cursor: pointer;
            font-size: 1em;
            transition: background 0.2s;
        }
        .opt-btn:hover { background: #374151; border-color: #a855f7; }
        .result-box { text-align: center; }
        .btn-reset {
            background: #2563eb;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            margin-top: 15px;
        }
        .btn-reset:hover { background: #1d4ed8; }
        footer {
            margin-top: 20px;
            text-align: center;
            font-size: 0.85em;
        }
        footer a { color: #a855f7; text-decoration: none; font-weight: bold; }
    </style>
</head>
<body>

<div class="container">
    <h1>Desafio da Caverna</h1>
    <div class="progress" id="progress">Pergunta 1 de 3</div>
    
    <div id="quiz-content">
        <div class="question" id="question-text">A carregar pergunta...</div>
        <div class="options" id="options-container"></div>
    </div>

    <div class="result-box" id="result-content" style="display: none;">
        <h3>Fim do Desafio!</h3>
        <p id="score-text" style="font-size: 1.1em; margin: 15px 0;"></p>
        <button class="btn-reset" onclick="restartQuiz()">Tentar Novamente</button>
    </div>

    <footer>
        Instagram do Vilão: <a href="https://instagram.com/_maiko_lone_" target="_blank">@_maiko_lone_</a>
    </footer>
</div>

<script>
    const questions = [
        {
            question: "Quem é o temido vilão principal em The Dark Cavern?",
            options: ["Esqueleto Sombrio", "WARKELL", "Necromante das Trevas", "Guardião Perdido"],
            answer: 1
        },
        {
            question: "Qual é o principal objetivo dos aventureiros ao entrar na caverna?",
            options: ["Encontrar tesouros escondidos", "Resgatar o refém e enfrentar Warkell", "Construir uma casa", "Explorar sem rumo"],
            answer: 1
        },
        {
            question: "Onde podes encontrar mais informações sobre o projeto?",
            options: ["No perfil do Instagram @_maiko_lone_", "Num livro impresso", "Na televisão", "Num jornal local"],
            answer: 0
        }
    ];

    let currentQuestion = 0;
    let score = 0;

    const questionText = document.getElementById('question-text');
    const optionsContainer = document.getElementById('options-container');
    const progressEl = document.getElementById('progress');
    const quizContent = document.getElementById('quiz-content');
    const resultContent = document.getElementById('result-content');
    const scoreText = document.getElementById('score-text');

    function loadQuestion() {
        const q = questions[currentQuestion];
        progressEl.innerText = `Pergunta ${currentQuestion + 1} de ${questions.length}`;
        questionText.innerText = q.question;
        optionsContainer.innerHTML = '';

        q.options.forEach((opt, idx) => {
            const btn = document.createElement('button');
            btn.className = 'opt-btn';
            btn.innerText = opt;
            btn.onclick = () => selectOption(idx);
            optionsContainer.appendChild(btn);
        });
    }

    function selectOption(idx) {
        if (idx === questions[currentQuestion].answer) {
            score++;
        }
        currentQuestion++;

        if (currentQuestion < questions.length) {
            loadQuestion();
        } else {
            showResult();
        }
    }

    function showResult() {
        quizContent.style.display = 'none';
        progressEl.style.display = 'none';
        resultContent.style.display = 'block';
        scoreText.innerHTML = `Acertaste <b>${score}</b> de <b>${questions.length}</b> perguntas com sucesso!`;
    }

    function restartQuiz() {
        currentQuestion = 0;
        score = 0;
        quizContent.style.display = 'block';
        progressEl.style.display = 'block';
        resultContent.style.display = 'none';
        loadQuestion();
    }

    loadQuestion();
</script>
</body>
</html>
