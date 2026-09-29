<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3단계 중국어 마스터 게임</title>
    <style>
        * { box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
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
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }
        .header-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }
        .stage-badge {
            padding: 6px 14px;
            background-color: #2c3e50;
            color: #ffffff;
            border-radius: 20px;
            font-size: 13px;
            font-weight: bold;
        }
        .progress {
            font-size: 14px;
            color: #7f8c8d;
            font-weight: bold;
        }
        .question-box {
            font-size: 24px;
            font-weight: bold;
            color: #2c3e50;
            margin: 15px 0 25px 0;
            background-color: #f8f9fa;
            padding: 20px;
            border-radius: 12px;
            border-left: 6px solid #3498db;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
        }

        ruby {
            font-size: 26px;
            font-weight: bold;
            color: #2c3e50;
            margin: 0 2px;
        }
        rt {
            font-size: 13px;
            font-weight: normal;
            color: #e67e22;
        }

        .drop-zone {
            min-height: 85px;
            border: 2px dashed #bdc3c7;
            border-radius: 12px;
            padding: 10px;
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            align-items: center;
            justify-content: center;
            margin-bottom: 15px;
            background-color: #fafafa;
        }
        .word-pool {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin-bottom: 20px;
        }
        .word-block {
            background: #ffffff;
            border: 2px solid #3498db;
            border-radius: 10px;
            padding: 8px 12px;
            cursor: pointer;
            user-select: none;
            transition: all 0.2s;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            display: inline-flex;
            flex-direction: column;
            align-items: center;
        }
        .word-block:hover {
            transform: translateY(-2px);
            background-color: #ebf5fb;
        }
        .word-block ruby { font-size: 20px; }
        .word-block rt { font-size: 11px; }

        .options-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .opt-btn {
            padding: 14px;
            font-size: 16px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            background: white;
            cursor: pointer;
            text-align: left;
            padding-left: 20px;
            transition: all 0.2s;
        }
        .opt-btn:hover { background: #f7f9fa; border-color: #3498db; }

        .correct { background-color: #2ecc71 !important; color: white !important; border-color: #2ecc71 !important; }
        .wrong { background-color: #e74c3c !important; color: white !important; border-color: #e74c3c !important; }

        .btn-action {
            margin-top: 15px;
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
        .explanation {
            margin-top: 15px;
            padding: 12px;
            background-color: #fff9e6;
            border: 1px solid #ffeaa7;
            border-radius: 8px;
            font-size: 14px;
            color: #d35400;
            text-align: left;
            display: none;
        }
        .result-box {
            background-color: #f8f9fa;
            border-radius: 12px;
            padding: 20px;
            margin: 20px 0;
        }
        .time-badge {
            font-size: 18px;
            color: #e67e22;
            font-weight: bold;
            margin-top: 10px;
        }
    </style>
</head>
<body>

<div class="container" id="game-container">
    <div class="header-info">
        <span class="stage-badge" id="stage-badge">1단계: 어휘 복습</span>
        <span class="progress" id="progress">1 / 18</span>
    </div>

    <div class="question-box" id="question-box"></div>

    <div id="choice-section">
        <div class="options-list" id="options-list"></div>
    </div>

    <div id="arrange-section" style="display: none;">
        <p style="font-size: 13px; color: #888; margin-bottom: 8px;">단어 블록을 클릭하여 아래 박스로 이동시키세요.</p>
        <div class="drop-zone" id="drop-zone"></div>
        <div class="word-pool" id="word-pool"></div>
        <button class="btn-action" id="check-btn" onclick="checkArrangeAnswer()">정답 확인하기</button>
    </div>

    <div class="explanation" id="explanation"></div>
    <button class="btn-action" id="next-btn" style="display: none;" onclick="nextQuestion()">다음 문제</button>
</div>

<script>
const quizData = [
    // --- 1단계: 어휘 복습 (6문항) ---
    {
        stage: "1단계: 어휘 복습",
        type: "choice",
        questionHtml: "<ruby>感冒<rt>gǎnmào</rt></ruby>",
        options: [
            { text: "감기에 걸리다", correct: true },
            { text: "주사를 맞다", correct: false },
            { text: "기침하다", correct: false },
            { text: "단련하다", correct: false }
        ],
        exp: "感冒(gǎnmào)는 '감기에 걸리다'라는 뜻입니다."
    },
    {
        stage: "1단계: 어휘 복습",
        type: "choice",
        questionHtml: "<ruby>咳嗽<rt>késou</rt></ruby>",
        options: [
            { text: "안색", correct: false },
            { text: "기침하다", correct: true },
            { text: "콧물", correct: false },
            { text: "아프다", correct: false }
        ],
        exp: "咳嗽(késou)는 '기침하다'라는 뜻입니다."
    },
    {
        stage: "1단계: 어휘 복습",
        type: "choice",
        questionHtml: "<ruby>打针<rt>dǎzhēn</rt></ruby>",
        options: [
            { text: "약", correct: false },
            { text: "병원", correct: false },
            { text: "주사를 맞다", correct: true },
            { text: "동반하다", correct: false }
        ],
        exp: "打针(dǎzhēn)은 '주사를 맞다'라는 뜻입니다."
    },
    {
        stage: "1단계: 어휘 복습",
        type: "choice",
        questionHtml: "<ruby>大夫<rt>dàifu</rt></ruby>",
        options: [
            { text: "의사", correct: true },
            { text: "간호사", correct: false },
            { text: "환자", correct: false },
            { text: "친구", correct: false }
        ],
        exp: "大夫(dàifu)는 회화에서 '의사'를 가리키는 단어입니다."
    },
    {
        stage: "1단계: 어휘 복습",
        type: "choice",
        questionHtml: "<ruby>口罩<rt>kǒuzhào</rt></ruby>",
        options: [
            { text: "모자", correct: false },
            { text: "마스크", correct: true },
            { text: "안경", correct: false },
            { text: "약", correct: false }
        ],
        exp: "口罩(kǒuzhào)는 '마스크'를 의미합니다."
    },
    {
        stage: "1단계: 어휘 복습",
        type: "choice",
        questionHtml: "<ruby>后悔<rt>hòuhuǐ</rt></ruby>",
        options: [
            { text: "준비하다", correct: false },
            { text: "쉬다", correct: false },
            { text: "후회하다", correct: true },
            { text: "착용하다", correct: false }
        ],
        exp: "后悔(hòuhuǐ)는 '후회하다'라는 뜻입니다."
    },

    // --- 2단계: 중국어 문장 보고 한국어 의미 맞추기 (6문항) ---
    {
        stage: "2단계: 문장 해석",
        type: "choice",
        questionHtml: "<ruby>你<rt>nǐ</rt></ruby><ruby>哪儿<rt>nǎr</rt></ruby><ruby>不<rt>bù</rt></ruby><ruby>舒服<rt>shūfu</rt></ruby>？",
        options: [
            { text: "어디가 안 좋으신가요?", correct: true },
            { text: "어느 병원에 가시나요?", correct: false },
            { text: "약은 잘 드셨나요?", correct: false },
            { text: "몸이 괜찮아지셨나요?", correct: false }
        ],
        exp: "你哪儿不舒服？는 의사가 환자에게 증상을 물어볼 때 쓰는 표준 질문입니다."
    },
    {
        stage: "2단계: 문장 해석",
        type: "choice",
        questionHtml: "<ruby>你<rt>nǐ</rt></ruby><ruby>得<rt>děi</rt></ruby><ruby>去<rt>qù</rt></ruby><ruby>医院<rt>yīyuàn</rt></ruby><ruby>看看<rt>kànkan</rt></ruby>。",
        options: [
            { text: "너는 병원에 진찰받으러 가야 해.", correct: true },
            { text: "너는 병원에 갈 필요가 없어.", correct: false },
            { text: "너는 병원에서 약을 사야 해.", correct: false },
            { text: "너는 병원에 갈 수 있니?", correct: false }
        ],
        exp: "得(děi)는 강한 필요성(~해야 한다)을 나타내며, 看看은 동사를 중첩하여 '좀 보다/진찰받다'를 뜻합니다."
    },
    {
        stage: "2단계: 문장 해석",
        type: "choice",
        questionHtml: "<ruby>吃<rt>chī</rt></ruby><ruby>点儿<rt>diǎnr</rt></ruby><ruby>药<rt>yào</rt></ruby><ruby>就<rt>jiù</rt></ruby><ruby>行<rt>xíng</rt></ruby>。",
        options: [
            { text: "약 좀 먹으면 돼.", correct: true },
            { text: "주사를 맞아야만 해.", correct: false },
            { text: "약이 너무 매워.", correct: false },
            { text: "제때 약을 먹어야 해.", correct: false }
        ],
        exp: "(一)点儿은 양이 '약간'임을 뜻하며, 就行은 '~하면 된다'는 표현입니다."
    },
    {
        stage: "2단계: 문장 해석",
        type: "choice",
        questionHtml: "<ruby>不用<rt>búyòng</rt></ruby><ruby>去<rt>qù</rt></ruby><ruby>医院<rt>yīyuàn</rt></ruby>。",
        options: [
            { text: "병원에 갈 필요 없다.", correct: true },
            { text: "병원에 가야만 한다.", correct: false },
            { text: "병원에 가기 싫다.", correct: false },
            { text: "병원 위치를 모른다.", correct: false }
        ],
        exp: "不用(búyòng)은 '得(děi)'의 부정형으로 '~할 필요가 없다'라는 뜻입니다."
    },
    {
        stage: "2단계: 문장 해석",
        type: "choice",
        questionHtml: "<ruby>我<rt>wǒ</rt></ruby><ruby>肚子<rt>dùzi</rt></ruby><ruby>有点儿<rt>yǒudiǎnr</rt></ruby><ruby>不<rt>bù</rt></ruby><ruby>舒服<rt>shūfu</rt></ruby>。",
        options: [
            { text: "나는 배가 약간 불편하다.", correct: true },
            { text: "나는 머리가 많이 아프다.", correct: false },
            { text: "나는 목이 조금 쓰리다.", correct: false },
            { text: "나는 감기에 걸린 것 같다.", correct: false }
        ],
        exp: "형용사 앞에 위치하는 有点儿은 불만족스러운 느낌의 '약간/조금'을 나타냅니다."
    },
    {
        stage: "2단계: 문장 해석",
        type: "choice",
        questionHtml: "<ruby>我<rt>wǒ</rt></ruby><ruby>陪<rt>péi</rt></ruby><ruby>你<rt>nǐ</rt></ruby><ruby>一起<rt>yìqǐ</rt></ruby><ruby>去<rt>qù</rt></ruby><ruby>吧<rt>ba</rt></ruby>。",
        options: [
            { text: "내가 너랑 같이 가줄게.", correct: true },
            { text: "너 혼자 다녀올 수 있니?", correct: false },
            { text: "나 대신 가줄 수 있어?", correct: false },
            { text: "우리 내일 같이 가자.", correct: false }
        ],
        exp: "陪(péi)는 '동반하다/함께해주다'라는 뜻의 동사입니다."
    },

    // --- 3단계: 단어 드래그로 문장 만들기 (6문항) ---
    {
        stage: "3단계: 문장 완성 (배열)",
        type: "arrange",
        meaning: "너는 병원에 진찰을 받으러 가야 해.",
        words: [
            { hz: "你", py: "nǐ" },
            { hz: "得", py: "děi" },
            { hz: "去", py: "qù" },
            { hz: "医院", py: "yīyuàn" },
            { hz: "看看", py: "kànkan" }
        ],
        correctOrder: [0, 1, 2, 3, 4],
        exp: "주어(你) + 得(děi) + 동사구(去医院看看) 순서로 배열합니다."
    },
    {
        stage: "3단계: 문장 완성 (배열)",
        type: "arrange",
        meaning: "약 좀 먹으면 돼요.",
        words: [
            { hz: "吃", py: "chī" },
            { hz: "点儿", py: "diǎnr" },
            { hz: "药", py: "yào" },
            { hz: "就", py: "jiù" },
            { hz: "行", py: "xíng" }
        ],
        correctOrder: [0, 1, 2, 3, 4],
        exp: "吃点儿药(약 좀 먹다) + 就行(~하면 된다) 조합입니다."
    },
    {
        stage: "3단계: 문장 완성 (배열)",
        type: "arrange",
        meaning: "머리가 아프고, 목도 아픕니다.",
        words: [
            { hz: "头疼", py: "tóu téng" },
            { hz: "嗓子", py: "sǎngzi" },
            { hz: "也", py: "yě" },
            { hz: "疼", py: "téng" }
        ],
        correctOrder: [0, 1, 2, 3],
        exp: "也(yě)는 부사이므로 형용사 疼(téng) 바로 앞에 위치합니다."
    },
    {
        stage: "3단계: 문장 완성 (배열)",
        type: "arrange",
        meaning: "따뜻한 물을 조금 마시렴.",
        words: [
            { hz: "你", py: "nǐ" },
            { hz: "喝", py: "hē" },
            { hz: "点儿", py: "diǎnr" },
            { hz: "热水", py: "rè shuǐ" },
            { hz: "吧", py: "ba" }
        ],
        correctOrder: [0, 1, 2, 3, 4],
        exp: "동사(喝) + 点儿 + 명사(热水) + 어기조사(吧) 순서입니다."
    },
    {
        stage: "3단계: 문장 완성 (배열)",
        type: "arrange",
        meaning: "마스크를 쓰세요.",
        words: [
            { hz: "戴上", py: "dàishang" },
            { hz: "口罩", py: "kǒuzhào" },
            { hz: "吧", py: "ba" }
        ],
        correctOrder: [0, 1, 2],
        exp: "戴上(착용하다) + 口罩(마스크) + 吧(권유) 형태입니다."
    },
    {
        stage: "3단계: 문장 완성 (배열)",
        type: "arrange",
        meaning: "필요 없어요, 나 혼자 할 수 있어요.",
        words: [
            { hz: "不用", py: "búyòng" },
            { hz: "我", py: "wǒ" },
            { hz: "一个人", py: "yí ge rén" },
            { hz: "可以", py: "kěyǐ" }
        ],
        correctOrder: [0, 1, 2, 3],
        exp: "不用(괜찮아요/필요없어요), 我一个人可以(나 혼자 가능해요) 문장입니다."
    }
];

let currentIdx = 0;
let score = 0;
let userPlacedIndices = [];
let startTime = null;

function loadQuiz() {
    if (currentIdx === 0 && !startTime) {
        startTime = new Date();
    }

    const q = quizData[currentIdx];
    document.getElementById("progress").innerText = `${currentIdx + 1} / ${quizData.length}`;
    document.getElementById("stage-badge").innerText = q.stage;
    document.getElementById("explanation").style.display = "none";
    document.getElementById("next-btn").style.display = "none";

    const qBox = document.getElementById("question-box");

    if (q.type === "choice") {
        document.getElementById("choice-section").style.display = "block";
        document.getElementById("arrange-section").style.display = "none";
        qBox.innerHTML = q.questionHtml;
        setupChoiceQuestion(q);
    } else {
        document.getElementById("choice-section").style.display = "none";
        document.getElementById("arrange-section").style.display = "block";
        qBox.innerHTML = `<span>${q.meaning}</span>`;
        setupArrangeQuestion(q);
    }
}

function setupChoiceQuestion(q) {
    const list = document.getElementById("options-list");
    list.innerHTML = "";

    q.options.forEach((opt, idx) => {
        const btn = document.createElement("button");
        btn.className = "opt-btn";
        btn.innerHTML = `${idx + 1}. ${opt.text}`;
        btn.onclick = () => {
            const allBtns = document.querySelectorAll(".opt-btn");
            allBtns.forEach(b => b.disabled = true);
            if (opt.correct) {
                btn.classList.add("correct");
                score++;
            } else {
                btn.classList.add("wrong");
            }
            showExp(q.exp);
        };
        list.appendChild(btn);
    });
}

function setupArrangeQuestion(q) {
    const dropZone = document.getElementById("drop-zone");
    const wordPool = document.getElementById("word-pool");
    dropZone.innerHTML = "";
    wordPool.innerHTML = "";
    userPlacedIndices = [];

    let shuffled = q.words.map((w, idx) => ({ ...w, originalIdx: idx }));
    shuffled.sort(() => Math.random() - 0.5);

    shuffled.forEach(item => {
        const block = createWordBlock(item.hz, item.py, item.originalIdx);
        block.onclick = () => moveToDropZone(block, item.originalIdx);
        wordPool.appendChild(block);
    });

    document.getElementById("check-btn").style.display = "block";
}

function createWordBlock(hz, py, idx) {
    const div = document.createElement("div");
    div.className = "word-block";
    div.dataset.idx = idx;
    div.innerHTML = `<ruby>${hz}<rt>${py}</rt></ruby>`;
    return div;
}

function moveToDropZone(block, originalIdx) {
    const dropZone = document.getElementById("drop-zone");
    const wordPool = document.getElementById("word-pool");

    if (block.parentElement === wordPool) {
        dropZone.appendChild(block);
        userPlacedIndices.push(originalIdx);
    } else {
        wordPool.appendChild(block);
        userPlacedIndices = userPlacedIndices.filter(i => i !== originalIdx);
    }
}

function checkArrangeAnswer() {
    const q = quizData[currentIdx];
    const isCorrect = JSON.stringify(userPlacedIndices) === JSON.stringify(q.correctOrder);
    
    document.getElementById("check-btn").style.display = "none";

    const blocks = document.querySelectorAll("#drop-zone .word-block");
    if (isCorrect) {
        blocks.forEach(b => b.classList.add("correct"));
        score++;
    } else {
        blocks.forEach(b => b.classList.add("wrong"));
    }

    showExp(q.exp);
}

function showExp(expText) {
    const expDiv = document.getElementById("explanation");
    expDiv.innerHTML = "💡 <b>설명:</b> " + expText;
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
    const endTime = new Date();
    const totalSeconds = Math.floor((endTime - startTime) / 1000);
    const minutes = Math.floor(totalSeconds / 60);
    const seconds = totalSeconds % 60;

    let timeText = "";
    if (minutes > 0) {
        timeText = `${minutes}분 ${seconds}초`;
    } else {
        timeText = `${seconds}초`;
    }

    const container = document.getElementById("game-container");
    container.innerHTML = `
        <h2 style="color:#2c3e50;">🎉 모든 단계 완료!</h2>
        <div class="result-box">
            <p style="font-size: 20px; margin: 10px 0;">최종 점수: <b>${score}</b> / ${quizData.length}</p>
            <div class="time-badge">⏱️ 총 소요 시간: ${timeText}</div>
        </div>
        <p style="color: #7f8c8d; line-height: 1.6;">
            ${score === quizData.length ? '완벽합니다! 최고 기록에 도전해 보세요. 💯' : '수고하셨습니다! 다음번엔 더 빠른 시간 내에 도전해 보세요.'}
        </p>
        <button class="btn-action" style="margin-top:20px; background-color:#2ecc71;" onclick="location.reload()">다시 시작하기</button>
    `;
}

loadQuiz();
</script>

</body>
</html>
