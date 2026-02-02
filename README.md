<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Китайские Карточки - Тренажер</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .perspective {
            perspective: 1500px;
        }
        .preserve-3d {
            transform-style: preserve-3d;
            transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .backface-hidden {
            backface-visibility: hidden;
        }
        .rotate-y-180 {
            transform: rotateY(180deg);
        }
    </style>
</head>
<body class="bg-slate-50 min-h-screen flex flex-col items-center justify-center p-4 font-sans text-slate-800">

    <div class="w-full max-w-md space-y-6">
        
        <!-- Заголовок -->
        <div class="text-center space-y-2">
            <h1 class="text-3xl font-bold text-indigo-600">Китайские Карточки</h1>
            <p class="text-slate-500 italic">Нажимайте на карту, чтобы увидеть перевод</p>
        </div>

        <!-- Контейнер для карточки -->
        <div id="card-trigger" class="group perspective h-72 cursor-pointer">
            <div id="card-inner" class="relative w-full h-full preserve-3d shadow-xl rounded-3xl">
                
                <!-- Лицевая сторона (Русский) -->
                <div class="absolute inset-0 backface-hidden bg-white rounded-3xl flex flex-col items-center justify-center p-8 text-center border border-slate-100">
                    <span class="text-xs text-indigo-400 font-bold mb-4 uppercase tracking-[0.2em]">Перевод</span>
                    <p id="text-ru" class="text-4xl font-semibold text-slate-700 leading-tight">Загрузка...</p>
                    <p class="mt-8 text-slate-300 text-sm">Нажмите, чтобы перевернуть</p>
                </div>

                <!-- Обратная сторона (Китайский) -->
                <div class="absolute inset-0 backface-hidden bg-indigo-600 rounded-3xl flex flex-col items-center justify-center p-8 text-center rotate-y-180 shadow-indigo-200 shadow-2xl">
                    <span class="text-xs text-indigo-200 font-bold mb-4 uppercase tracking-[0.2em]">Иероглиф</span>
                    <p id="text-zh" class="text-7xl mb-6 text-white font-bold tracking-normal"></p>
                    <div class="bg-indigo-500/30 px-4 py-2 rounded-lg backdrop-blur-sm">
                        <p id="text-py" class="text-2xl text-white font-medium"></p>
                    </div>
                </div>

            </div>
        </div>

        <!-- Управление -->
        <div class="flex flex-col gap-6">
            <div class="flex items-center justify-between px-2">
                <button id="prev-btn" class="p-4 bg-white hover:bg-indigo-50 text-indigo-600 rounded-2xl shadow-sm border border-slate-200 transition-all active:scale-95">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
                </button>

                <div class="flex flex-col items-center">
                    <div class="text-slate-400 text-xs mb-1 uppercase tracking-tighter font-bold">Прогресс</div>
                    <div id="progress" class="text-indigo-600 font-mono text-lg font-bold bg-indigo-50 px-4 py-1 rounded-full">
                        0 / 0
                    </div>
                </div>

                <button id="next-btn" class="p-4 bg-white hover:bg-indigo-50 text-indigo-600 rounded-2xl shadow-sm border border-slate-200 transition-all active:scale-95">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
                </button>
            </div>

            <button id="shuffle-btn" class="w-full flex items-center justify-center gap-2 py-3 bg-white hover:bg-slate-100 text-slate-600 rounded-xl border border-slate-200 shadow-sm transition-all font-medium text-sm active:bg-slate-200">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 9-9 9.75 9.75 0 0 1 6.74 2.74L21 8"/><path d="M21 3v5h-5"/><path d="M21 12a9 9 0 0 1-9 9 9.75 9.75 0 0 1-6.74-2.74L3 16"/><path d="M3 21v-5h5"/></svg>
                Случайное слово
            </button>
        </div>

        <!-- Подвал -->
        <div class="pt-4 text-center">
            <p id="total-count" class="text-slate-400 text-xs"></p>
        </div>
    </div>

    <script>
        const vocabulary = [
            { zh: "她", py: "tā", ru: "она" },
            { zh: "他", py: "tā", ru: "он" },
            { zh: "我", py: "wǒ", ru: "я" },
            { zh: "你", py: "nǐ", ru: "ты" },
            { zh: "不", py: "bù", ru: "не / нет" },
            { zh: "的", py: "de", ru: "частица (принадлежность)" },
            { zh: "这", py: "zhè", ru: "это / этот" },
            { zh: "那", py: "nà", ru: "то / тот" },
            { zh: "书", py: "shū", ru: "книга" },
            { zh: "车", py: "chē", ru: "машина" },
            { zh: "笔", py: "bǐ", ru: "ручка / карандаш" },
            { zh: "地图", py: "dìtú", ru: "карта" },
            { zh: "世界", py: "shìjiè", ru: "мир" },
            { zh: "北京", py: "Běijīng", ru: "Пекин" },
            { zh: "上海", py: "Shànghǎi", ru: "Шанхай" },
            { zh: "什么", py: "shénme", ru: "что / какой" },
            { zh: "谁", py: "shéi", ru: "кто" },
            { zh: "谁的", py: "shéi de", ru: "чей" },
            { zh: "看", py: "kàn", ru: "смотреть" },
            { zh: "请", py: "qǐng", ru: "пожалуйста (просьба)" },
            { zh: "哪", py: "nǎ", ru: "какой / где" },
            { zh: "欧洲", py: "Ōuzhōu", ru: "Европа" },
            { zh: "亚洲", py: "Yàzhōu", ru: "Азия" },
            { zh: "美洲", py: "Měizhōu", ru: "Америка (континент)" },
            { zh: "非洲", py: "Fēizhōu", ru: "Африка" },
            { zh: "大洋洲", py: "Dàyángzhōu", ru: "Океания" },
            { zh: "长江", py: "Chángjiāng", ru: "Янцзы" },
            { zh: "黄河", py: "Huánghé", ru: "Хуанхэ" },
            { zh: "长城", py: "Chángchéng", ru: "Великая стена" },
            { zh: "茶", py: "chá", ru: "чай" },
            { zh: "牛奶", py: "niúnǎi", ru: "молоко" },
            { zh: "喝", py: "hē", ru: "пить" },
            { zh: "吃", py: "chī", ru: "есть (принимать пищу)" },
            { zh: "谢谢", py: "xièxie", ru: "спасибо" },
            { zh: "不客气", py: "búkèqi", ru: "не за что" },
            { zh: "您", py: "nín", ru: "Вы (вежливо)" },
            { zh: "进", py: "jìn", ru: "входить" },
            { zh: "欢迎", py: "huānyíng", ru: "добро пожаловать" },
            { zh: "咖啡", py: "kāfēi", ru: "кофе" },
            { zh: "水", py: "shuǐ", ru: "вода" },
            { zh: "啤酒", py: "píjiǔ", ru: "пиво" },
            { zh: "中国", py: "Zhōngguó", ru: "Китай" },
            { zh: "美国", py: "Měiguó", ru: "США" },
            { zh: "法国", py: "Fǎguó", ru: "Франция" },
            { zh: "德国", py: "Déguó", ru: "Германия" },
            { zh: "泰国", py: "Tàiguó", ru: "Таиланд" },
            { zh: "汉语", py: "Hànyǔ", ru: "Китайский язык" },
            { zh: "日本", py: "Rìběn", ru: "Япония" },
            { zh: "俄罗斯", py: "Éluósī", ru: "Россия" },
            { zh: "法语", py: "Fǎyǔ", ru: "Французский язык" },
            { zh: "德语", py: "Déyǔ", ru: "Немецкий язык" },
            { zh: "朝鲜", py: "Cháoxiǎn", ru: "Корея (КНДР)" },
            { zh: "老师", py: "lǎoshī", ru: "учитель" },
            { zh: "太太", py: "tàitai", ru: "госпожа / жена" },
            { zh: "大夫", py: "dàifu", ru: "врач" },
            { zh: "先生", py: "xiānsheng", ru: "господин / муж" },
            { zh: "吸烟", py: "xīyān", ru: "курить" }
        ];

        let currentIndex = 0;
        let isFlipped = false;

        const cardInner = document.getElementById('card-inner');
        const cardTrigger = document.getElementById('card-trigger');
        const ruText = document.getElementById('text-ru');
        const zhText = document.getElementById('text-zh');
        const pyText = document.getElementById('text-py');
        const progressText = document.getElementById('progress');
        const totalText = document.getElementById('total-count');

        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const shuffleBtn = document.getElementById('shuffle-btn');

        function updateCard() {
            const word = vocabulary[currentIndex];
            
            // Сбрасываем переворот перед сменой текста
            if (isFlipped) {
                isFlipped = false;
                cardInner.classList.remove('rotate-y-180');
                setTimeout(() => {
                    renderContent(word);
                }, 300);
            } else {
                renderContent(word);
            }
        }

        function renderContent(word) {
            ruText.textContent = word.ru;
            zhText.textContent = word.zh;
            pyText.textContent = word.py;
            progressText.textContent = `${currentIndex + 1} / ${vocabulary.length}`;
            totalText.textContent = `Всего слов в наборе: ${vocabulary.length}`;
        }

        cardTrigger.addEventListener('click', () => {
            isFlipped = !isFlipped;
            cardInner.classList.toggle('rotate-y-180', isFlipped);
        });

        nextBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            currentIndex = (currentIndex + 1) % vocabulary.length;
            updateCard();
        });

        prevBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            currentIndex = (currentIndex - 1 + vocabulary.length) % vocabulary.length;
            updateCard();
        });

        shuffleBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            currentIndex = Math.floor(Math.random() * vocabulary.length);
            updateCard();
        });

        // Инициализация
        updateCard();
    </script>
</body>
</html>
