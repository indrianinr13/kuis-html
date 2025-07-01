# kuis-html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Kuis Responsif dengan Timer</title>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f1f1f1;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    .container {
      background: #fff;
      border-radius: 10px;
      padding: 25px;
      max-width: 600px;
      width: 90%;
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
    }
    h2 {
      margin-bottom: 15px;
      font-size: 24px;
    }
    .question {
      font-size: 18px;
      margin-bottom: 10px;
    }
    .answers label {
      display: block;
      margin-bottom: 10px;
      font-size: 16px;
    }
    input[type="radio"] {
      margin-right: 10px;
    }
    .button-timer {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 20px;
    }
    #submit {
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
    }
    #submit:hover {
      background-color: #0056b3;
    }
    #timer {
      font-weight: bold;
      color: #d9534f;
      font-size: 16px;
    }
    .score-section {
      font-size: 20px;
      font-weight: bold;
      text-align: center;
      margin-top: 20px;
    }
    @media (max-width: 500px) {
      h2 {
        font-size: 20px;
      }
      .question {
        font-size: 16px;
      }
      .answers label {
        font-size: 14px;
      }
      #submit {
        font-size: 14px;
        padding: 8px 16px;
      }
    }
  </style>
</head>
<body>

<div class="container">
  <h2>Kuis Pengetahuan Umum</h2>
  <div id="quiz">
    <div class="question" id="question">Memuat soal...</div>
    <div class="answers" id="answers"></div>
    <div class="button-timer">
      <button id="submit">Jawab</button>
      <div id="timer">⏱ 15 detik</div>
    </div>
    <div class="score-section" id="score"></div>
  </div>
</div>

<script>
  const quizData = [
    {
      question: "Apa ibu kota Indonesia?",
      a: "Bandung",
      b: "Medan",
      c: "Jakarta",
      d: "Surabaya",
      correct: "c"
    },
    {
      question: "Siapa presiden pertama Indonesia?",
      a: "Joko Widodo",
      b: "Soekarno",
      c: "Habibie",
      d: "Megawati",
      correct: "b"
    },
    {
      question: "Berapa jumlah pulau di Indonesia (sekitar)?",
      a: "17.000",
      b: "7.000",
      c: "25.000",
      d: "10.000",
      correct: "a"
    }
  ];

  let currentQuiz = 0;
  let score = 0;
  let timer;
  let timeLeft = 15;

  const questionEl = document.getElementById("question");
  const answersEl = document.getElementById("answers");
  const submitBtn = document.getElementById("submit");
  const scoreEl = document.getElementById("score");
  const timerEl = document.getElementById("timer");

  function loadQuiz() {
    resetTimer();
    deselectAnswers();
    const current = quizData[currentQuiz];
    questionEl.textContent = current.question;
    answersEl.innerHTML = `
      <label><input type="radio" name="answer" value="a"> ${current.a}</label>
      <label><input type="radio" name="answer" value="b"> ${current.b}</label>
      <label><input type="radio" name="answer" value="c"> ${current.c}</label>
      <label><input type="radio" name="answer" value="d"> ${current.d}</label>
    `;
    startTimer();
  }

  function getSelected() {
    const radios = document.getElementsByName("answer");
    for (let radio of radios) {
      if (radio.checked) return radio.value;
    }
    return null;
  }

  function deselectAnswers() {
    document.getElementsByName("answer").forEach(el => el.checked = false);
  }

  function startTimer() {
    timeLeft = 15;
    timerEl.textContent = `⏱ ${timeLeft} detik`;
    timer = setInterval(() => {
      timeLeft--;
      timerEl.textContent = `⏱ ${timeLeft} detik`;
      if (timeLeft <= 0) {
        clearInterval(timer);
        autoSubmit();
      }
    }, 1000);
  }

  function resetTimer() {
    clearInterval(timer);
    timerEl.textContent = "";
  }

  function autoSubmit() {
    checkAnswer(getSelected());
  }

  function checkAnswer(selected) {
    resetTimer();

    if (selected === quizData[currentQuiz].correct) {
      score++;
    }

    currentQuiz++;
    if (currentQuiz < quizData.length) {
      loadQuiz();
    } else {
      showResult();
    }
  }

  submitBtn.addEventListener("click", () => {
    const selected = getSelected();
    if (!selected) {
      alert("Pilih jawaban dulu!");
      return;
    }
    checkAnswer(selected);
  });

  function showResult() {
    document.getElementById("quiz").innerHTML = `
      <h2>🎉 Selesai!</h2>
      <div class="score-section">Skor kamu: ${score} dari ${quizData.length}</div>
      <button onclick="location.reload()">Main Lagi</button>
    `;
  }

  // Start
  loadQuiz();
</script>

</body>
</html>
