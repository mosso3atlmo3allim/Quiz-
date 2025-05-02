<!DOCTYPE html><html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz ديني</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: url('https://example.com/quiz-background.jpg') no-repeat center center/cover;
            font-family: Arial, sans-serif;
            color: #fff;
        }
        .quiz-container {
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            width: 90%;
            max-width: 600px;
        }
        .countdown {
            font-size: 2rem;
            margin-bottom: 20px;
            color: #ffd700;
        }
        .question-number {
            font-size: 1.2rem;
            margin-bottom: 10px;
            color: #ffd700;
        }
        .question {
            font-size: 1.5rem;
            margin-bottom: 20px;
        }
        .options button {
            display: block;
            margin: 10px auto;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background: #007bff;
            color: #fff;
            font-size: 1rem;
            cursor: pointer;
        }
        .options button:hover {
            background: #0056b3;
        }
        .result {
            font-size: 1.2rem;
            margin-top: 20px;
        }
        .score {
            font-size: 1.2rem;
            margin-top: 20px;
            color: #00ff00;
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div id="countdown" class="countdown">5</div>
        <div id="quiz" style="display: none;">
            <div class="question-number" id="question-number">السؤال رقم: 1</div>
            <div class="question" id="question">Loading...</div>
            <div class="options" id="options"></div>
            <div class="result" id="result"></div>
            <div class="score" id="score">النقاط: 0</div>
        </div>
    </div><audio id="correct-sound" src="https://example.com/correct.mp3"></audio>
<audio id="wrong-sound" src="https://example.com/wrong.mp3"></audio>
<audio id="countdown-sound" src="https://example.com/countdown.mp3"></audio>

<script>
    const quizData = [
        { question: "ما هو عدد السور التي تبدأ بالحروف المقطعة؟", options: ["29", "30", "28"], answer: 0 },
        { question: "في أي سورة وردت آية الكرسي؟", options: ["سورة البقرة", "سورة آل عمران", "سورة النساء"], answer: 0 },
        { question: "كم عدد آيات سورة الكهف؟", options: ["110", "109", "111"], answer: 0 },
        { question: "ما هو الاسم الآخر ليوم القيامة المذكور في القرآن؟", options: ["الحاقة", "الغاشية", "الزلزلة"], answer: 0 },
        { question: "ما هي أول غزوة في الإسلام؟", options: ["غزوة بدر", "غزوة أحد", "غزوة الخندق"], answer: 0 },
        { question: "كم كان عمر النبي محمد صلى الله عليه وسلم عندما توفي؟", options: ["63", "60", "65"], answer: 0 },
        { question: "في أي سنة هجرية كانت غزوة بدر؟", options: ["2 هجرية", "3 هجرية", "1 هجرية"], answer: 0 },
        { question: "من هو الصحابي الملقب بسيف الله المسلول؟", options: ["خالد بن الوليد", "عمر بن الخطاب", "علي بن أبي طالب"], answer: 0 },
        { question: "ما هي السورة التي تنتهي بكل آياتها بحرف الدال؟", options: ["سورة الإخلاص", "سورة المسد", "سورة الفلق"], answer: 1 },
        { question: "كم مرة ورد ذكر اسم \"محمد\" في القرآن الكريم؟", options: ["4 مرات", "5 مرات", "6 مرات"], answer: 0 },
        // Add remaining questions here...
    ];

    let currentQuestion = 0;
    let score = 0;
    const countdownElement = document.getElementById('countdown');
    const quizElement = document.getElementById('quiz');
    const questionNumberElement = document.getElementById('question-number');
    const questionElement = document.getElementById('question');
    const optionsElement = document.getElementById('options');
    const resultElement = document.getElementById('result');
    const scoreElement = document.getElementById('score');
    const correctSound = document.getElementById('correct-sound');
    const wrongSound = document.getElementById('wrong-sound');
    const countdownSound = document.getElementById('countdown-sound');

    function startCountdown() {
        let countdown = 5;
        const interval = setInterval(() => {
            countdownSound.play();
            countdown -= 1;
            if (countdown >= 0) {
                countdownElement.textContent = countdown;
            } else {
                clearInterval(interval);
                countdownElement.textContent = 'انطلق!';
                setTimeout(() => {
                    countdownElement.style.display = 'none';
                    quizElement.style.display = 'block';
                    loadQuestion();
                }, 1000);
            }
        }, 1000);
    }

    function loadQuestion() {
        const currentQuiz = quizData[currentQuestion];
        questionNumberElement.textContent = `السؤال رقم: ${currentQuestion + 1}`;
        questionElement.textContent = currentQuiz.question;
        optionsElement.innerHTML = '';
        resultElement.textContent = '';

        currentQuiz.options.forEach((option, index) => {
            const button = document.createElement('button');
            button.textContent = option;
            button.addEventListener('click', () => checkAnswer(index));
            optionsElement.appendChild(button);
        });
    }

    function checkAnswer(selectedOption) {
        const currentQuiz = quizData[currentQuestion];
        const correctOption = currentQuiz.answer;
        if (selectedOption === correctOption) {
            resultElement.textContent = 'إجابة صحيحة!';
            correctSound.play();
            score += 1;
            scoreElement.textContent = `النقاط: ${score}`;
        } else {
            resultElement.textContent = `إجابة خاطئة! الإجابة الصحيحة هي: ${currentQuiz.options[correctOption]}`;
            wrongSound.play();
        }

        currentQuestion += 1;
        if (currentQuestion < quizData.length) {
            setTimeout(loadQuestion, 2000);
        } else {
            setTimeout(() => {
                questionElement.textContent = 'انتهى الاختبار!';
                optionsElement.innerHTML = '';
                resultElement.textContent = `لقد حصلت على ${score} من ${quizData.length}`;
            }, 2000);
        }
    }

    startCountdown();
</script>

</body>
</html>
