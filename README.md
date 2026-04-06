<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>多益單字大挑戰</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
            max-width: 500px;
            width: 90%;
        }
        h1 { color: #1a73e8; }
        .score { font-size: 1.2rem; margin-bottom: 1rem; color: #555; }
        .word-box {
            font-size: 2.5rem;
            font-weight: bold;
            margin: 20px 0;
            color: #333;
        }
        .options {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }
        button {
            padding: 15px;
            font-size: 1rem;
            border: 2px solid #ddd;
            border-radius: 8px;
            background: white;
            cursor: pointer;
            transition: all 0.2s;
        }
        button:hover { background-color: #f8f9fa; border-color: #1a73e8; }
        button.correct { background-color: #4caf50; color: white; border-color: #4caf50; }
        button.wrong { background-color: #f44336; color: white; border-color: #f44336; }
        .next-btn {
            margin-top: 20px;
            background-color: #1a73e8;
            color: white;
            border: none;
            padding: 10px 25px;
            display: none;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>TOEIC 挑戰賽</h1>
    <div class="score">得分: <span id="score">0</span></div>
    <div class="word-box" id="english-word">Loading...</div>
    <div class="options" id="options-container">
        </div>
    <button class="next-btn" id="next-btn" onclick="nextQuestion()">下一題</button>
</div>

<script>
    // 單字資料庫 (你可以自由增加更多)
    const vocabulary = [
        { en: "Mandatory", zh: "強制性的" },
        { en: "Procrastinate", zh: "拖延" },
        { en: "Incentive", zh: "誘因/獎勵" },
        { en: "Substantial", zh: "大量的/實質的" },
        { en: "Implementation", zh: "實施/執行" },
        { en: "Versatile", zh: "多才多藝的/多功能的" },
        { en: "Agile", zh: "敏捷的" },
        { en: "Compliance", zh: "合規/遵守" },
        { en: "Preliminary", zh: "初步的" },
        { en: "Negotiation", zh: "談判" }
    ];

    let currentWord = {};
    let score = 0;

    function nextQuestion() {
        // 重置 UI
        document.getElementById('next-btn').style.display = 'none';
        const container = document.getElementById('options-container');
        container.innerHTML = '';
        
        // 隨機選一個單字
        currentWord = vocabulary[Math.floor(Math.random() * vocabulary.length)];
        document.getElementById('english-word').innerText = currentWord.en;

        // 產生干擾選項
        let options = [currentWord.zh];
        while(options.length < 4) {
            let randomZh = vocabulary[Math.floor(Math.random() * vocabulary.length)].zh;
            if(!options.includes(randomZh)) options.push(randomZh);
        }
        
        // 打亂選項順序
        options.sort(() => Math.random() - 0.5);

        // 渲染按鈕
        options.forEach(opt => {
            const btn = document.createElement('button');
            btn.innerText = opt;
            btn.onclick = () => checkAnswer(btn, opt);
            container.appendChild(btn);
        });
    }

    function checkAnswer(btn, choice) {
        const allBtns = document.querySelectorAll('.options button');
        allBtns.forEach(b => b.disabled = true); // 點選後禁用所有按鈕

        if (choice === currentWord.zh) {
            btn.classList.add('correct');
            score += 10;
            document.getElementById('score').innerText = score;
        } else {
            btn.classList.add('wrong');
            // 標示出正確答案
            allBtns.forEach(b => {
                if(b.innerText === currentWord.zh) b.classList.add('correct');
            });
        }
        document.getElementById('next-btn').style.display = 'inline-block';
    }

    // 初始化第一題
    nextQuestion();
</script>

</body>
</html>
