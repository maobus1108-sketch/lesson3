# lesson3
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>중국어 3단원 종합 복습 게임</title>
    <style>
        * { box-sizing: border-box; }
        body {
            font-family: 'Noto Sans KR', sans-serif;
            background-color: #f0f4f8;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        .container {
            background: #ffffff;
            padding: 30px;
            border-radius: 18px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 520px;
            text-align: center;
        }
        .header {
            font-size: 14px;
            color: #7f8c8d;
            font-weight: bold;
            margin-bottom: 10px;
        }
        .category {
            display: inline-block;
            padding: 4px 12px;
            background-color: #e8f4fd;
            color: #2980b9;
            border-radius: 20px;
            font-size: 13px;
            font-weight: bold;
            margin-bottom: 15px;
        }
        .question {
            font-size: 24px;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
            line-height: 1.4;
        }
        .sub-text {
            font-size: 16px;
            color: #7f8c8d;
            margin-bottom: 20px;
        }
        .options {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        button.opt-btn {
            padding: 14px;
            font-size: 16px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            background: white;
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
            padding-left: 20px;
        }
        button.opt-btn:hover {
            background: #f7f9fa;
            border-color: #3498db;
        }
        .correct { background-color: #2ecc71 !important; color: white; border-color: #2ecc71 !important; }
        .wrong { background-color: #e74c3c !important; color: white; border-color: #e74c3c !important; }
        .explanation {
            margin-top: 15px;
            padding: 12px;
            background-color: #f8f9fa;
            border-radius: 8px;
            font-size: 14px;
            color: #555;
            text-align: left;
            display: none;
        }
        .next-btn, .restart-btn {
            margin-top: 20px;
            width: 100%;
            padding: 14px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
        }
        .restart-btn { background-color: #e67e22; }
    </style>
</head>
<body>

<div class="container" id="quiz-box">
    <div class="header" id="progress">문제 1 / 10</div>
    <div class="category" id="category">단어 암기</div>
    <div class="question" id="question">어디</div>
    <div class="sub-text" id="sub-text"></div>
    <div class="options" id="options"></div>
    <div class="explanation" id="explanation"></div>
    <button class="next-btn" id="next-btn" style="display: none;" onclick="nextQuestion()">다음 문제</button>
</div>

<script>
const quizData = [
    {
        category: "단어 퀴즈",
        question: "哪儿 (nǎr)",
        sub: "이 단어의 알맞은 뜻은?",
        options: ["어디", "병원", "목", "의사"],
        answer: 0,
        exp: "哪儿(nǎr)은 '어디'라는 뜻입니다."
    },
    {
        category: "단어 퀴즈",
        question: "'감기에 걸리다'의 중국어 표현은?",
        sub: "알맞은 한자와 병음을 골라보세요.",
        options: ["打针 (dǎzhēn)", "感冒 (gǎnmào)", "咳嗽 (késou)", "发烧 (fāshāo)"],
        answer: 1,
        exp: "感冒(gǎnmào)는 '감기에 걸리다'라는 뜻입니다."
    },
    {
        category: "문법 포인트",
        question: "你得去医院看看。",
        sub: "여기서 '得(děi)'의 역할과 의미는?",
        options: [
            "~하고 싶다 (희망)",
            "~해야 한다 (강한 필요성)",
            "~할 수 있다 (가능)",
            "~하지 마라 (금지)"
        ],
        answer: 1,
        exp: "'得(děi)'는 동사 앞에서 강한 필요성('~해야 한다')을 나타냅니다."
    },
    {
        category: "문법 포인트",
        question: "你得去医院。(병원에 가야 해)",
        sub: "이 문장을 부정문('병원에 갈 필요가 없다')으로 바꿀 때 빈칸은?",
        sub2: "你 (   ) 去医院。",
        options: ["不", "没", "不用", "不要"],
        answer: 2,
        exp: "'得(děi)'의 부정을 나타낼 때는 '不用(búyòng, ~할 필요가 없다)'을 사용합니다."
    },
    {
        category: "문법 포인트",
        question: "你喝 (   ) 热水吧。",
        sub: "'따뜻한 물을 [조금] 마시렴' 빈칸에 들어갈 말은?",
        options: ["有点儿", "点儿", "不太", "不用"],
        answer: 1,
        exp: "동사/형용사 뒤에서 양이 '조금, 약간'임을 나타낼 때는 (一)点儿을 씁니다. (喝点儿热水)"
    },
    {
        category: "문법 포인트",
        question: "我肚子 (   ) 不舒服。",
        sub: "'나는 배가 [조금] 불편하다' 빈칸에 들어갈 말은?",
        options: ["点儿", "有点儿", "不用", "得"],
        answer: 1,
        exp: "형용사 심리동사 앞에서 다소 불만족스러운 어감의 '조금, 약간'은 有点儿을 씁니다."
    },
    {
        category: "회화 응용",
        question: "你哪儿不舒服？",
        sub: "의사의 이 질문에 대한 알맞은 한국어 해석은?",
        options: [
            "어디가 안 좋으신가요?",
            "어느 병원에 가시나요?",
            "약은 드셨나요?",
            "주사를 맞으시겠어요?"
        ],
        answer: 0,
        exp: "你哪儿不舒服？는 '어디가 불편하세요 / 어디가 안 좋으신가요?'라는 뜻입니다."
    },
    {
        category: "회화 응용",
        question: "我头疼，嗓子也疼。",
        sub: "이 문장의 뜻으로 올바른 것은?",
        options: [
            "열이 나고 콧물이 흘러요.",
            "머리가 아프고, 목도 아파요.",
            "배가 아프고 감기에 걸렸어요.",
            "안색이 안 좋아 보여요."
        ],
        answer: 1,
        exp: "头疼(머리가 아프다) + 嗓子也疼(목도 아프다)입니다."
    },
    {
        category: "본문 완성",
        question: "不严重，不用打针，吃点儿药 (   )。",
        sub: "'심각하지 않으니 주사는 안 맞아도 되고, 약 좀 먹으면 돼요'",
        options: ["就得", "就行", "不用", "也很"],
        answer: 1,
        exp: "'就行(jiù xíng)'은 '~하면 된다 / 괜찮다'라는 의미를 완성해 줍니다."
    },
    {
        category: "단어 응용",
        question: "戴 (dài) 와 가장 잘 어울리는 단어는?",
        sub: "이미지 속 표현을 올려 보세요.",
        options: ["身体 (신체)", "口罩 (마스크)", "鼻涕 (콧물)", "医院 (병원)"],
        answer: 1,
        exp: "戴口罩(dàishang kǒuzhào)는 '마스크를 쓰다(착용하다)'입니다."
    }
];

let currentIdx = 0;
let score = 0;

function loadQuiz() {
    const q = quizData[currentIdx];
    document.getElementById("progress").innerText = `문제 ${currentIdx + 1} / ${quizData.length}`;
    document.getElementById("category").innerText = q.category;
    document.getElementById("question").innerText = q.question;
    document.getElementById("sub-text").innerText = q.sub2 ? `${q.sub}\n${q.sub2}` : q.sub;
    
    const optionsDiv = document.getElementById("options");
    const expDiv = document.getElementById("explanation");
    const nextBtn = document.getElementById("next-btn");
    
    optionsDiv.innerHTML = "";
    expDiv.style.display = "none";
    nextBtn.style.display = "none";

    q.options.forEach((opt, index) => {
        const btn = document.createElement("button");
        btn.className = "opt-btn";
        btn.innerText = `${index + 1}. ${opt}`;
        btn.onclick = () => selectAnswer(index, q.answer, q.exp);
        optionsDiv.appendChild(btn);
    });
}

function selectAnswer(selected, correct, exp) {
    const btns = document.querySelectorAll(".opt-btn");
    btns.forEach(btn => btn.disabled = true);

    if (selected === correct) {
        btns[selected].classList.add("correct");
        score++;
    } else {
        btns[selected].classList.add("wrong");
        btns[correct].classList.add("correct");
    }

    const expDiv = document.getElementById("explanation");
    expDiv.innerText = "💡 해설: " + exp;
    expDiv.style.display = "block";

    document.getElementById("next-btn").style.display = "block";
}

function nextQuestion() {
    currentIdx++;
    if (currentIdx < quizData.length) {
        loadQuiz();
    } else {
        showFinalResult();
    }
}

function showFinalResult() {
    const box = document.getElementById("quiz-box");
    box.innerHTML = `
        <h2>🎉 복습 완료!</h2>
        <p style="font-size: 20px; margin: 20px 0;">총 <b>${quizData.length}</b>문제 중 <b>${score}</b>문제를 맞추셨습니다!</p>
        <p style="color: #7f8c8d;">${score === quizData.length ? '완벽합니다! 주요 단어와 문법을 모두 마스터하셨어요. 👏' : '틀린 문제를 다시 한번 확인해보세요!'}</p>
        <button class="restart-btn" onclick="location.reload()">다시 풀기</button>
    `;
}

loadQuiz();
</script>

</body>
</html>
