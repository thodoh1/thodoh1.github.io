<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Китайская Идиома Игра</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .game-container {
            background-color: #ffffff;
            border-radius: 1.5rem; /* rounded-xl */
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05); /* shadow-lg */
            padding: 2.5rem; /* p-10 */
            width: 100%;
            max-width: 768px; /* md:max-w-2xl */
            text-align: center;
            display: flex;
            flex-direction: column;
            gap: 1.5rem; /* gap-6 */
        }
        .idiom-display {
            font-size: 3rem; /* text-5xl */
            font-weight: 700; /* font-bold */
            color: #1a202c; /* text-gray-900 */
            margin-bottom: 1.5rem; /* mb-6 */
        }
        .options-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1rem; /* gap-4 */
            margin-bottom: 1.5rem; /* mb-6 */
        }
        @media (min-width: 640px) { /* sm */
            .options-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        .option-button {
            background-color: #e2e8f0; /* bg-gray-200 */
            color: #2d3748; /* text-gray-800 */
            padding: 1rem 1.5rem; /* py-4 px-6 */
            border-radius: 0.75rem; /* rounded-xl */
            font-size: 1.125rem; /* text-lg */
            font-weight: 600; /* font-semibold */
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.1s ease;
            border: 2px solid transparent;
        }
        .option-button:hover {
            background-color: #cbd5e0; /* bg-gray-300 */
            transform: translateY(-2px);
        }
        .option-button.correct {
            background-color: #48bb78; /* bg-green-500 */
            color: #ffffff;
            border-color: #38a169; /* border-green-600 */
        }
        .option-button.wrong {
            background-color: #ef4444; /* bg-red-500 */
            color: #ffffff;
            border-color: #dc2626; /* border-red-600 */
        }
        .option-button:disabled {
            opacity: 0.7;
            cursor: not-allowed;
        }
        .feedback {
            font-size: 1.25rem; /* text-xl */
            font-weight: 600; /* font-semibold */
            margin-top: 1rem; /* mt-4 */
            min-height: 2rem;
        }
        .example-sentence-container {
            background-color: #f7fafc; /* bg-gray-100 */
            border-radius: 0.75rem; /* rounded-xl */
            padding: 1.5rem; /* p-6 */
            margin-top: 1.5rem; /* mt-6 */
            text-align: left;
            border: 1px solid #e2e8f0; /* border-gray-200 */
        }
        .example-explanation-text {
            font-size: 1.125rem; /* text-lg */
            font-weight: 600; /* font-semibold */
            color: #2d3748; /* text-gray-800 */
            margin-bottom: 0.75rem;
        }
        .example-history-text {
            font-size: 1rem; /* text-base */
            color: #4a5568; /* text-gray-700 */
            line-height: 1.5;
            margin-bottom: 1rem;
        }
        .example-usage-text {
            font-size: 1rem; /* text-base */
            color: #4a5568; /* text-gray-700 */
            line-height: 1.5;
            margin-bottom: 1rem;
        }
        .example-grammatical-text {
            font-size: 1rem; /* text-base */
            color: #4a5568; /* text-gray-700 */
            line-height: 1.5;
            margin-bottom: 1rem;
        }
        .example-chinese-sentence {
            font-size: 1.5rem; /* text-2xl */
            font-weight: 700; /* font-bold */
            color: #1a202c; /* text-gray-900 */
            margin-bottom: 0.5rem;
        }
        .example-pinyin-sentence {
            font-size: 1.125rem; /* text-lg */
            color: #4a5568; /* text-gray-700 */
            margin-bottom: 0.5rem;
        }
        .example-russian-translation {
            font-size: 1rem; /* text-base */
            color: #4a5568; /* text-gray-700 */
            line-height: 1.5;
        }
        .control-buttons {
            display: flex;
            justify-content: center;
            gap: 1rem; /* gap-4 */
            margin-top: 2rem; /* mt-8 */
        }
        .control-button {
            background-color: #4299e1; /* bg-blue-500 */
            color: #ffffff;
            padding: 0.75rem 1.5rem; /* py-3 px-6 */
            border-radius: 0.75rem; /* rounded-xl */
            font-size: 1rem; /* text-base */
            font-weight: 600; /* font-semibold */
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.1s ease;
            border: none;
        }
        .control-button:hover {
            background-color: #3182ce; /* bg-blue-600 */
            transform: translateY(-2px);
        }
        .control-button:disabled {
            opacity: 0.7;
            cursor: not-allowed;
        }
        .score-display {
            font-size: 1.25rem; /* text-xl */
            font-weight: 600; /* font-semibold */
            color: #2d3748; /* text-gray-800 */
            margin-top: 1rem; /* mt-4 */
        }
        .loading-spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: #4299e1;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
            display: inline-block;
            vertical-align: middle;
            margin-left: 10px;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1 class="text-3xl font-bold text-gray-800 mb-4">Изучение Китайских Идиом</h1>

        <div class="idiom-display" id="idiomDisplay"></div>

        <div class="score-display">
            Счет: <span id="score">0</span> / <span id="totalQuestions">0</span>
        </div>

        <h2 class="text-2xl font-semibold text-gray-700 mt-6 mb-4">Выберите правильный пиньинь:</h2>
        <div class="options-grid" id="pinyinOptions"></div>

        <h2 class="text-2xl font-semibold text-gray-700 mt-6 mb-4">Выберите правильный перевод:</h2>
        <div class="options-grid" id="translationOptions"></div>

        <div class="feedback" id="feedback"></div>

        <div class="control-buttons">
            <button id="nextButton" class="control-button" disabled>Следующая Идиома</button>
            <button id="exampleButton" class="control-button">Пояснение и Пример</button>
        </div>

        <div id="exampleSentenceContainer" class="example-sentence-container hidden">
            <p class="example-explanation-text" id="exampleExplanationText"></p>
            <p class="example-history-text" id="exampleHistoryText"></p>
            <p class="example-usage-text" id="exampleUsageText"></p>
            <p class="example-grammatical-text" id="exampleGrammaticalText"></p>
            <p class="example-chinese-sentence" id="exampleChineseSentence"></p>
            <p class="example-pinyin-sentence" id="examplePinyinSentence"></p>
            <p class="example-russian-translation" id="exampleRussianTranslation"></p>
            <div id="loadingSpinner" class="loading-spinner hidden"></div>
        </div>
    </div>

    <script>
        // Идиомы из предоставленного списка
        const idioms = [
            { chinese: "东张西望", pinyin: "dōngzhāngxīwàng", translation: "смотреть по сторонам" },
            { chinese: "一举两得", pinyin: "yījǔliǎngdé", translation: "одним выстрелом убить двух зайцев" },
            { chinese: "南辕北辙", pinyin: "nányuánběizhé", translation: "действовать так, что это противоречит цели" },
            { chinese: "丢三落四", pinyin: "diūsānlàsì", translation: "забывчивый" },
            { chinese: "千方百计", pinyin: "qiānfāngbǎijì", translation: "всеми возможными средствами" },
            { chinese: "吞吞吐吐", pinyin: "tūntūntǔtǔ", translation: "мямлить; бормотать, как будто что-то скрывая" },
            { chinese: "家喻户晓", pinyin: "jiāyùhùxiǎo", translation: "общеизвестный" },
            { chinese: "新陈代谢", pinyin: "xīnchéndàxiè", translation: "метаболизм; новое заменяет старое" },
            { chinese: "一丝不苟", pinyin: "yīsībùgǒu", translation: "строго по правилам" },
            { chinese: "川流不息", pinyin: "chuānliúbùxī", translation: "непрерывный поток" },
            { chinese: "一帆风顺", pinyin: "yīfānfēngshùn", translation: "попутный ветер на протяжении всего пути" },
            { chinese: "悬崖峭壁", pinyin: "xuányáqiàobì", translation: "отвесные скалы и обрывистые каменные лица" },
            { chinese: "拔苗助长", pinyin: "bámiáo-zhùzhǎng", translation: "портить дело из-за чрезмерного энтузиазма" },
            { chinese: "斩钉截铁", pinyin: "zhǎndīngjiétiě", translation: "рубить гвоздь и резать железо (решительно)" },
            { chinese: "根深蒂固", pinyin: "gēnshēndìgù", translation: "глубоко укоренившийся" },
            { chinese: "滔滔不绝", pinyin: "tāotāobùjué", translation: "непрекращающийся поток" },
            { chinese: "狼吞虎咽", pinyin: "lángtūnhǔyàn", translation: "жадно есть; пожирать с жадностью" },
            { chinese: "画蛇添足", pinyin: "huàshétiānzú", translation: "нарисовать змее ноги (испортить, добавив лишнее)" },
            { chinese: "锦上添花", pinyin: "jǐnshàngtiānhuā", translation: "на парчу добавить цветы (украшать что-то уже идеальное)" },
            { chinese: "雪上加霜", pinyin: "xuěshàngjiāshuāng", translation: "добавлять град к снегу (одно бедствие за другим)" },
            { chinese: "鸦雀无声", pinyin: "yāquèwúshēng", translation: "полная тишина" },
            { chinese: "不可思议", pinyin: "bùkěsīyì", translation: "непостижимый; невообразимый" },
            { chinese: "一目了然", pinyin: "yīmùliǎorán", translation: "очевидно с первого взгляда" },
            { chinese: "不屑一顾", pinyin: "bùxièyīgù", translation: "пренебрегать как недостойным презрения" },
            { chinese: "不择手段", pinyin: "bùzéshǒuduàn", translation: "любыми средствами" },
            { chinese: "不相上下", pinyin: "bùxiāngshàngxià", translation: "равноценный" },
            { chinese: "不言而喻", pinyin: "bùyánéryù", translation: "само собой разумеется" },
            { chinese: "与日俱增", pinyin: "yǔrìjùzēng", translation: "неуклонно возрастать" },
            { chinese: "举世瞩目", pinyin: "jǔshìzhǔmù", translation: "привлекать всемирное внимание" },
            { chinese: "举足轻重", pinyin: "jǔzúqīngzhòng", translation: "играть решающую роль" },
            { chinese: "争先恐后", pinyin: "zhēngxiānkǒnghòu", translation: "стремясь быть первым и боясь быть последним" },
            { chinese: "任重道远", pinyin: "rènzhòngdàoyuǎn", translation: "тяжелая ноша и долгий путь" },
            { chinese: "众所周知", pinyin: "zhòngsuǒzhīzhī", translation: "как всем известно" },
            { chinese: "优胜劣汰", pinyin: "yōushèngliètài", translation: "выживание сильнейших" },
            { chinese: "侃侃而谈", pinyin: "kǎnkǎnértán", translation: "говорить откровенно и уверенно" },
            { chinese: "供不应求", pinyin: "gōngbùyìngqiú", translation: "спрос превышает предложение" },
            { chinese: "兢兢业业", pinyin: "jīngjīngyèyè", translation: "осторожный и добросовестный" },
            { chinese: "全力以赴", pinyin: "quánlìyǐfù", translation: "сделать любой ценой" },
            { chinese: "兴致勃勃", pinyin: "xìngzhìbóbó", translation: "в приподнятом настроении" },
            { chinese: "兴高采烈", pinyin: "xìnggāocǎiliè", translation: "счастливый и взволнованный" },
            { chinese: "再接再厉", pinyin: "zàijiēzàilì", translation: "продолжать борьбу; упорствовать" },
            { chinese: "刻不容缓", pinyin: "kèbùrónghuǎn", translation: "не терпеть отлагательств" },
            { chinese: "力所能及", pinyin: "lìsuǒnéngjí", translation: "по мере своих сил" },
            { chinese: "半途而废", pinyin: "bàntú'érfèi", translation: "бросить на полпути; оставить что-то незавершенным" },
            { chinese: "一如既往", pinyin: "yīrújìwǎng", translation: "как прежде" },
            { chinese: "博大精深", pinyin: "bódàjīngshēn", translation: "широкий и глубокий" },
            { chinese: "各抒己见", pinyin: "gèshūjǐjiàn", translation: "каждый высказывает свое мнение" },
            { chinese: "名副其实", pinyin: "míngfùqíshí", translation: "не только по названию, но и на самом деле" },
            { chinese: "后顾之忧", pinyin: "hòugùzhīyōu", translation: "семейные заботы (препятствующие свободе действий)" },
            { chinese: "喜闻乐见", pinyin: "xǐwénlèjiàn", translation: "любить слышать и видеть" },
            { chinese: "天伦之乐", pinyin: "tiānlúnzhīlè", translation: "семейная любовь и радость" },
            { chinese: "实事求是", pinyin: "shíshìqiúshì", translation: "искать истину в фактах" },
            { chinese: "小心翼翼", pinyin: "xiǎoxīnyìyì", translation: "очень осторожно" },
            { chinese: "岂有此理", pinyin: "qǐyǒucǐlǐ", translation: "нелепо" },
            { chinese: "废寝忘食", pinyin: "fèiqǐnwàngshí", translation: "пренебрегать сном и забывать о еде" },
            { chinese: "归根到底", pinyin: "guīgēndàodǐ", translation: "в конце концов" },
            { chinese: "当务之急", pinyin: "dāngwùzhījí", translation: "первоочередная задача" },
            { chinese: "得不偿失", pinyin: "débùchángshī", translation: "выгода не окупает потери" },
            { chinese: "得天独厚", pinyin: "détiāndúhòu", translation: "благословенный небесами" },
            { chinese: "循序渐进", pinyin: "xúnxùjiànjìn", translation: "постепенно достигать прогресса" },
            { chinese: "心甘情愿", pinyin: "xīngānqíngyuàn", translation: "с удовольствием" },
            { chinese: "急于求成", pinyin: "jíyúqiúchéng", translation: "стремиться к быстрым результатам" },
            { chinese: "急功近利", pinyin: "jígōngjìnlì", translation: "искать мгновенную выгоду; недальновидное видение" },
            { chinese: "总而言之", pinyin: "zǒng'éryánzhī", translation: "вкратце" },
            { chinese: "恍然大悟", pinyin: "huǎngrándàwù", translation: "внезапно осознать" },
            { chinese: "恰到好处", pinyin: "qiàdàohǎochù", translation: "это просто идеально" },
            { chinese: "想方设法", pinyin: "xiǎngfāngshèfǎ", translation: "придумывать все возможные методы" },
            { chinese: "无动于衷", pinyin: "wúdòngyúzhōng", translation: "равнодушный" },
            { chinese: "无微不至", pinyin: "wúwēibùzhì", translation: "всеми возможными способами; дотошный" },
            { chinese: "无忧无虑", pinyin: "wúyōuwúlǜ", translation: "беззаботный и безмятежный" },
            { chinese: "无理取闹", pinyin: "wúlǐqǔnào", translation: "устраивать проблемы без причины; быть намеренно провокационным" },
            { chinese: "无精打采", pinyin: "wújīngdǎcǎi", translation: "подавленный и унылый; вялый" },
            { chinese: "无能为力", pinyin: "wúnéngwéilì", translation: "бессильный" },
            { chinese: "日新月异", pinyin: "rìxīnyuèyì", translation: "ежедневное обновление, ежемесячное изменение" },
            { chinese: "有条不紊", pinyin: "yǒutiáobùwěn", translation: "методично расположенный" },
            { chinese: "朝气蓬勃", pinyin: "zhāoqìpéngbó", translation: "полный юношеской энергии; энергичный" },
            { chinese: "欣欣向荣", pinyin: "xīnxīnxiàngróng", translation: "процветающий" },
            { chinese: "津津有味", pinyin: "jīnjīnyǒuwèi", translation: "с большим интересом и удовольствием; с аппетитом" },
            { chinese: "深情厚谊", pinyin: "shēnqínghòuyì", translation: "глубокая дружба" },
            { chinese: "潜移默化", pinyin: "qiányímòhuà", translation: "незаметное влияние" },
            { chinese: "热泪盈眶", pinyin: "rèlèiyíngkuàng", translation: "глаза, полные слез волнения" },
            { chinese: "爱不释手", pinyin: "àibùshìshǒu", translation: "любить что-то слишком сильно, чтобы расстаться с этим" },
            { chinese: "物美价廉", pinyin: "wùměijiàlián", translation: "хорошее качество и низкая цена" },
            { chinese: "理所当然", pinyin: "lǐsuǒdāngrán", translation: "как и должно быть по праву" },
            { chinese: "理直气壮", pinyin: "lǐzhíqìzhuàng", translation: "правый и уверенный в себе" },
            { chinese: "相辅相成", pinyin: "xiāngfǔxiāngchéng", translation: "дополнять друг друга" },
            { chinese: "知足常乐", pinyin: "zhīzúchánglè", translation: "довольный тем, что имеешь" },
            { chinese: "礼尚往来", pinyin: "lǐshàngwǎnglái", translation: "правильное поведение основано на взаимности" },
            { chinese: "称心如意", pinyin: "chènxīnrúyì", translation: "по душе; приятный и удовлетворительный" },
            { chinese: "竭尽全力", pinyin: "jiéjìnquánlì", translation: "не щадить усилий; делать все возможное" },
            { chinese: "精打细算", pinyin: "jīngdǎxìsuàn", translation: "тщательное планирование и бережный учет" },
            { chinese: "精益求精", pinyin: "jīngyìqiújīng", translation: "совершенствовать то, что уже выдающееся; постоянно улучшать" },
            { chinese: "统筹兼顾", pinyin: "tǒngchóujiāngù", translation: "общий план, учитывающий все факторы" },
            { chinese: "聚精会神", pinyin: "jùjīnghuìshén", translation: "сосредоточить свое внимание" },
            { chinese: "肆无忌惮", pinyin: "sìwújìdàn", translation: "абсолютно несдержанный" },
            { chinese: "自力更生", pinyin: "zìlìgēngshēng", translation: "возрождение собственными усилиями" },
            { chinese: "苦尽甘来", pinyin: "kǔjìngānlái", translation: "трудные времена прошли, хорошие времена только начинаются" },
            { chinese: "莫名其妙", pinyin: "mòmíngqímiào", translation: "сбивающий с толку" },
            { chinese: "见义勇为", pinyin: "jiànyìyǒngwéi", translation: "видеть, что правильно, и действовать мужественно" },
            { chinese: "见多识广", pinyin: "jiànduōshìguǎng", translation: "опытный и знающий" },
            { chinese: "迄今为止", pinyin: "qìjīnwéizhǐ", translation: "до сих пор" },
            { chinese: "迫不及待", pinyin: "pòbùjídài", translation: "нетерпеливый; в спешке" },
            { chinese: "锲而不舍", pinyin: "qiè'érbùshě", translation: "оттачивать задачу и не бросать ее" },
            { chinese: "难能可贵", pinyin: "nánnéngkěguì", translation: "редкий и драгоценный" },
            { chinese: "风土人情", pinyin: "fēngtǔrénqíng", translation: "местные условия и обычаи" },
            { chinese: "饱经沧桑", pinyin: "bǎojīngcāngsāng", translation: "переживший много перемен" },
            { chinese: "齐心协力", pinyin: "qíxīnxiélì", translation: "работать с общей целью; прилагать согласованные усилия" },
            { chinese: "层出不穷", pinyin: "céngchūbùqióng", translation: "появляется все больше и больше" },
            { chinese: "无穷无尽", pinyin: "wúqióngwújìn", translation: "бесконечный" },
            { chinese: "络绎不绝", pinyin: "luòyìbùjué", translation: "непрерывно; бесконечным потоком" }
        ];

        let currentIdiom = {};
        let score = 0;
        let totalQuestions = 0;
        let pinyinCorrect = false;
        let translationCorrect = false;
        let exampleSentenceCache = {}; 

        const idiomDisplay = document.getElementById('idiomDisplay');
        const pinyinOptionsDiv = document.getElementById('pinyinOptions');
        const translationOptionsDiv = document.getElementById('translationOptions');
        const feedbackDiv = document.getElementById('feedback');
        const nextButton = document.getElementById('nextButton');
        const exampleButton = document.getElementById('exampleButton');
        const exampleSentenceContainer = document.getElementById('exampleSentenceContainer');
        const exampleExplanationText = document.getElementById('exampleExplanationText');
        const exampleHistoryText = document.getElementById('exampleHistoryText');
        const exampleUsageText = document.getElementById('exampleUsageText');
        const exampleGrammaticalText = document.getElementById('exampleGrammaticalText');
        const exampleChineseSentence = document.getElementById('exampleChineseSentence');
        const examplePinyinSentence = document.getElementById('examplePinyinSentence');
        const exampleRussianTranslation = document.getElementById('exampleRussianTranslation');
        const scoreDisplay = document.getElementById('score');
        const totalQuestionsDisplay = document.getElementById('totalQuestions');
        const loadingSpinner = document.getElementById('loadingSpinner');

        // Загрузка счета пользователя из localStorage
        function loadScore() {
            const savedScore = localStorage.getItem('idiomGameScore');
            const savedTotal = localStorage.getItem('idiomGameTotal');
            if (savedScore !== null && savedTotal !== null) {
                score = parseInt(savedScore, 10);
                totalQuestions = parseInt(savedTotal, 10);
            } else {
                score = 0;
                totalQuestions = 0;
            }
            updateScoreDisplay();
            console.log("Score loaded from localStorage:", score, totalQuestions);
        }

        // Сохранение счета пользователя в localStorage
        function saveScore() {
            localStorage.setItem('idiomGameScore', score.toString());
            localStorage.setItem('idiomGameTotal', totalQuestions.toString());
            console.log("Score saved to localStorage:", score, totalQuestions);
        }

        // Обновление отображения счета
        function updateScoreDisplay() {
            scoreDisplay.textContent = score;
            totalQuestionsDisplay.textContent = totalQuestions;
        }

        // Перемешивание массива
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Получение случайных неправильных вариантов
        function getRandomIncorrectOptions(correctOption, allOptions, count) {
            const incorrectOptions = allOptions.filter(opt => opt !== correctOption);
            shuffleArray(incorrectOptions);
            return incorrectOptions.slice(0, count);
        }

        // Отображение новой идиомы
        function displayNewIdiom() {
            pinyinCorrect = false;
            translationCorrect = false;
            feedbackDiv.textContent = '';
            nextButton.disabled = true;
            exampleSentenceContainer.classList.add('hidden');
            exampleExplanationText.textContent = '';
            exampleHistoryText.textContent = '';
            exampleUsageText.textContent = '';
            exampleGrammaticalText.textContent = '';
            exampleChineseSentence.textContent = '';
            examplePinyinSentence.textContent = '';
            exampleRussianTranslation.textContent = '';
            loadingSpinner.classList.add('hidden');

            pinyinOptionsDiv.innerHTML = '';
            translationOptionsDiv.innerHTML = '';

            const randomIndex = Math.floor(Math.random() * idioms.length);
            currentIdiom = idioms[randomIndex];

            idiomDisplay.textContent = currentIdiom.chinese;

            const allPinyins = idioms.map(idiom => idiom.pinyin);
            const incorrectPinyins = getRandomIncorrectOptions(currentIdiom.pinyin, allPinyins, 3);
            const pinyinChoices = shuffleArray([...incorrectPinyins, currentIdiom.pinyin]);

            pinyinChoices.forEach(pinyin => {
                const button = document.createElement('button');
                button.textContent = pinyin;
                button.classList.add('option-button');
                button.dataset.type = 'pinyin';
                button.dataset.value = pinyin;
                button.addEventListener('click', handleOptionClick);
                pinyinOptionsDiv.appendChild(button);
            });

            const allTranslations = idioms.map(idiom => idiom.translation);
            const incorrectTranslations = getRandomIncorrectOptions(currentIdiom.translation, allTranslations, 3);
            const translationChoices = shuffleArray([...incorrectTranslations, currentIdiom.translation]);

            translationChoices.forEach(translation => {
                const button = document.createElement('button');
                button.textContent = translation;
                button.classList.add('option-button');
                button.dataset.type = 'translation';
                button.dataset.value = translation;
                button.addEventListener('click', handleOptionClick);
                translationOptionsDiv.appendChild(button);
            });
        }

        // Обработчик клика по варианту
        function handleOptionClick(event) {
            const clickedButton = event.target;
            const type = clickedButton.dataset.type;
            const value = clickedButton.dataset.value;

            document.querySelectorAll(`.option-button[data-type="${type}"]`).forEach(btn => {
                btn.disabled = true;
                if (btn.dataset.value === currentIdiom[type]) {
                    btn.classList.add('correct');
                } else {
                    btn.classList.add('wrong');
                }
            });

            if (type === 'pinyin') {
                pinyinCorrect = (value === currentIdiom.pinyin);
            } else if (type === 'translation') {
                translationCorrect = (value === currentIdiom.translation);
            }

            const pinyinAnswered = document.querySelector('.option-button[data-type="pinyin"]:disabled') !== null;
            const translationAnswered = document.querySelector('.option-button[data-type="translation"]:disabled') !== null;

            if (pinyinAnswered && translationAnswered) {
                if (pinyinCorrect && translationCorrect) {
                    feedbackDiv.textContent = 'Правильно! 🎉';
                    feedbackDiv.style.color = '#48bb78'; // green-500
                    score++;
                } else {
                    feedbackDiv.textContent = 'Неправильно. Попробуйте в следующий раз!';
                    feedbackDiv.style.color = '#ef4444'; // red-500
                }
                totalQuestions++;
                nextButton.disabled = false;
                saveScore();
                updateScoreDisplay();
            }
        }

        // Получение примера предложения и пояснения с помощью Gemini API
        async function getExampleAndExplanation(idiom) {
            exampleExplanationText.textContent = '';
            exampleHistoryText.textContent = '';
            exampleUsageText.textContent = ''; 
            exampleGrammaticalText.textContent = '';
            exampleChineseSentence.textContent = '';
            examplePinyinSentence.textContent = '';
            exampleRussianTranslation.textContent = '';
            exampleSentenceContainer.classList.remove('hidden');
            loadingSpinner.classList.remove('hidden');
            exampleButton.disabled = true;

            if (exampleSentenceCache[idiom]) {
                const cached = exampleSentenceCache[idiom];
                exampleExplanationText.textContent = cached.explanation;
                exampleHistoryText.textContent = cached.history;
                exampleUsageText.textContent = cached.usageContext; 
                exampleGrammaticalText.textContent = cached.grammaticalPlacement;
                exampleChineseSentence.textContent = cached.chineseExample;
                examplePinyinSentence.textContent = cached.pinyinExample;
                exampleRussianTranslation.textContent = cached.russianExampleTranslation;
                loadingSpinner.classList.add('hidden');
                exampleButton.disabled = false;
                return;
            }

            try {
                const prompt = `Предоставь краткое пояснение на русском языке, краткую историю происхождения идиомы на русском языке, информацию о том, с чем обычно употребляется идиома (на русском языке), информацию о грамматической роли и месте идиомы в предложении (на русском языке), пример предложения на китайском языке, пиньинь для этого китайского предложения, и русский перевод этого китайского предложения для китайской идиомы "${idiom}". Пример предложения должен использовать идиому естественно. Ответ должен быть в формате JSON с полями "explanation", "history", "usageContext", "grammaticalPlacement", "chineseExample", "pinyinExample", "russianExampleTranslation".`;
                let chatHistory = [{ role: "user", parts: [{ text: prompt }] }];
                const payload = {
                    contents: chatHistory,
                    generationConfig: {
                        responseMimeType: "application/json",
                        responseSchema: {
                            type: "OBJECT",
                            properties: {
                                "explanation": { "type": "STRING" },
                                "history": { "type": "STRING" },
                                "usageContext": { "type": "STRING" }, 
                                "grammaticalPlacement": { "type": "STRING" },
                                "chineseExample": { "type": "STRING" },
                                "pinyinExample": { "type": "STRING" },
                                "russianExampleTranslation": { "type": "STRING" }
                            },
                            "propertyOrdering": ["explanation", "history", "usageContext", "grammaticalPlacement", "chineseExample", "pinyinExample", "russianExampleTranslation"]
                        }
                    }
                };
                const apiKey = ""; 
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();

                if (result.candidates && result.candidates[0].content && result.candidates[0].content.parts[0]) {
                    const jsonResponse = JSON.parse(result.candidates[0].content.parts[0].text);
                    exampleExplanationText.textContent = jsonResponse.explanation;
                    exampleHistoryText.textContent = jsonResponse.history;
                    exampleUsageText.textContent = jsonResponse.usageContext; 
                    exampleGrammaticalText.textContent = jsonResponse.grammaticalPlacement;
                    exampleChineseSentence.textContent = jsonResponse.chineseExample;
                    examplePinyinSentence.textContent = jsonResponse.pinyinExample;
                    exampleRussianTranslation.textContent = jsonResponse.russianExampleTranslation;
                    exampleSentenceCache[idiom] = jsonResponse;
                } else {
                    exampleExplanationText.textContent = 'Не удалось получить пояснение и пример.';
                    console.error("Unexpected API response structure:", result);
                }
            } catch (error) {
                exampleExplanationText.textContent = 'Ошибка при получении пояснения и примера.';
                console.error("Error fetching example sentence and explanation:", error);
            } finally {
                loadingSpinner.classList.add('hidden');
                exampleButton.disabled = false;
            }
        }

        // Обработчики событий кнопок
        nextButton.addEventListener('click', displayNewIdiom);
        exampleButton.addEventListener('click', () => getExampleAndExplanation(currentIdiom.chinese));

        // Запуск игры после загрузки страницы
        window.onload = function() {
            loadScore();
            displayNewIdiom();
        };

    </script>
</body>
</html>
