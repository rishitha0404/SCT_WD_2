# SCT_WD_2
Calculater web application
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Interactive Quiz Game</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #222831;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .quiz-container {
      background: #393e46;
      padding: 25px;
      border-radius: 10px;
      width: 400px;
      box-shadow: 0 0 10px rgba(0,0,0,0.4);
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
    }

    .question {
      margin-bottom: 15px;
      font-size: 18px;
    }

    .options label {
      display: block;
      margin-bottom: 8px;
      cursor: pointer;
    }

    input[type="text"] {
      width: 100%;
      padding: 8px;
      border-radius: 5px;
      border: none;
      margin-top: 5px;
    }

    button {
      width: 100%;
      padding: 10px;
      margin-top: 15px;
      background: #00adb5;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
    }

    button:hover {
      background: #02cfd8;
    }

    .score {
      text-align: center;
      font-size: 20px;
    }
  </style>
</head>

<body>

<div class="quiz-container">
  <h2>Quiz Game</h2>
  <div id="quiz"></div>
  <button id="nextBtn">Next</button>
</div>

<script>
  const quizData = [
    {
      type: "single",
      question: "Which language is used for web development?",
      options: ["Python", "HTML", "C", "Java"],
      answer: "HTML"
    },
    {
      type: "multiple",
      question: "Select programming languages:",
      options: ["HTML", "Python", "CSS", "JavaScript"],
      answer: ["Python", "JavaScript"]
    },
    {
      type: "fill",
      question: "HTML stands for ________ Markup Language.",
      answer: "Hypertext"
    }
  ];

  let currentQuestion = 0;
  let score = 0;

  const quiz = document.getElementById("quiz");
  const nextBtn = document.getElementById("nextBtn");

  loadQuestion();

  function loadQuestion() {
    quiz.innerHTML = "";
    const q = quizData[currentQuestion];

    const questionEl = document.createElement("div");
    questionEl.className = "question";
    questionEl.textContent = q.question;
    quiz.appendChild(questionEl);

    const optionsEl = document.createElement("div");
    optionsEl.className = "options";

    if (q.type === "single") {
      q.options.forEach(option => {
        optionsEl.innerHTML += `
          <label>
            <input type="radio" name="answer" value="${option}"> ${option}
          </label>
        `;
      });
    }

    if (q.type === "multiple") {
      q.options.forEach(option => {
        optionsEl.innerHTML += `
          <label>
            <input type="checkbox" value="${option}"> ${option}
          </label>
        `;
      });
    }

    if (q.type === "fill") {
      optionsEl.innerHTML = `<input type="text" id="fillAnswer" placeholder="Type your answer">`;
    }

    quiz.appendChild(optionsEl);
  }

  nextBtn.addEventListener("click", () => {
    checkAnswer();
    currentQuestion++;

    if (currentQuestion < quizData.length) {
      loadQuestion();
    } else {
      showScore();
    }
  });

  function checkAnswer() {
    const q = quizData[currentQuestion];

    if (q.type === "single") {
      const selected = document.querySelector("input[name='answer']:checked");
      if (selected && selected.value === q.answer) score++;
    }

    if (q.type === "multiple") {
      const selected = Array.from(document.querySelectorAll("input[type='checkbox']:checked"))
                            .map(cb => cb.value);
      if (JSON.stringify(selected.sort()) === JSON.stringify(q.answer.sort())) {
        score++;
      }
    }

    if (q.type === "fill") {
      const userAnswer = document.getElementById("fillAnswer").value.trim();
      if (userAnswer.toLowerCase() === q.answer.toLowerCase()) score++;
    }
  }

  function showScore() {
    quiz.innerHTML = `
      <div class="score">
        🎉 Quiz Completed! <br><br>
        Your Score: ${score} / ${quizData.length}
      </div>
    `;
    nextBtn.style.display = "none";
  }
</script>

</body>
</html>
