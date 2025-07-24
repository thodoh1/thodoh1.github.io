<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Игра "Угадай перевод"</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            touch-action: manipulation;
        }
        .chinese-word {
            font-size: 4rem;
            line-height: 1.2;
        }
        .btn {
            transition: all 0.2s ease-in-out;
        }
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        .btn.correct {
            background-color: #22c55e !important; /* green-500 */
            color: white;
            border-color: #16a34a; /* green-600 */
        }
        .btn.incorrect {
            background-color: #ef4444 !important; /* red-500 */
            color: white;
            border-color: #dc2626; /* red-600 */
        }
        .feedback {
            min-height: 100px;
        }
        #word-list-container {
            max-height: 400px;
            overflow-y: auto;
        }
    </style>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-200 flex flex-col items-center justify-center min-h-screen p-4">

    <div class="w-full max-w-2xl mx-auto bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6 md:p-8 text-center">
        <h1 class="text-2xl md:text-3xl font-bold text-gray-900 dark:text-white mb-2">Угадай перевод</h1>
        <p class="text-gray-600 dark:text-gray-400 mb-6">Выбери правильный перевод для китайского слова.</p>

        <div id="game-container" class="mb-6">
            <div class="bg-gray-100 dark:bg-gray-700 rounded-lg p-8 mb-6 flex items-center justify-center">
                <p id="chinese-word" class="chinese-word font-bold text-gray-900 dark:text-white"></p>
            </div>
            <div id="options-container" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Кнопки с вариантами ответов будут здесь -->
            </div>
        </div>

        <div id="feedback-container" class="feedback flex flex-col items-center justify-center mb-6 text-center">
            <p id="pinyin" class="text-2xl md:text-3xl text-blue-500 dark:text-blue-400 font-semibold mb-2"></p>
            <p id="result" class="text-lg md:text-xl font-medium"></p>
        </div>

        <button id="next-word" class="w-full bg-blue-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-4 focus:ring-blue-300 dark:focus:ring-blue-800 disabled:bg-gray-400 dark:disabled:bg-gray-600 disabled:cursor-not-allowed">Следующее слово</button>
        
        <div class="mt-6 text-left">
            <button id="toggle-word-list" class="w-full text-center text-blue-600 dark:text-blue-400 hover:underline">Показать/скрыть весь список слов</button>
            <div id="word-list-container" class="hidden mt-4 p-4 bg-gray-50 dark:bg-gray-700 rounded-lg border border-gray-200 dark:border-gray-600">
                <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
                    <thead class="text-xs text-gray-700 uppercase bg-gray-100 dark:bg-gray-800 dark:text-gray-400">
                        <tr>
                            <th scope="col" class="px-6 py-3">Слово</th>
                            <th scope="col" class="px-6 py-3">Пиньинь</th>
                            <th scope="col" class="px-6 py-3">Перевод</th>
                        </tr>
                    </thead>
                    <tbody id="word-list-body">
                        <!-- Список слов будет здесь -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <script>
        const wordsData = [
            { chinese: "阿姨", pinyin: "āyí", translation: "тётя, тётка; (при обращении) тётя; няня (в детском саду)" },
            { chinese: "啊", pinyin: "a", translation: "частица восклицательных и побудительных предложений; не переводится" },
            { chinese: "唉", pinyin: "āi", translation: "да; ох; эх; ах" },
            { chinese: "唉哟", pinyin: "āiyō", translation: "ой" },
            { chinese: "癌症", pinyin: "áizhèng", translation: "рак (мед.)" },
            { chinese: "矮", pinyin: "ǎi", translation: "низкорослый, низкий" },
            { chinese: "爱", pinyin: "ài", translation: "любовь; любить" },
            { chinese: "爱不释手", pinyin: "àibùshìshǒu", translation: "понравилось так, что из рук не выпустишь" },
            { chinese: "爱戴", pinyin: "àidài", translation: "любить; почитать; любовь" },
            { chinese: "爱好", pinyin: "àihào", translation: "любить, иметь склонность к чему-либо, хобби" },
            { chinese: "爱护", pinyin: "àihù", translation: "беречь; любить; проявлять заботу" },
            { chinese: "爱情", pinyin: "àiqíng", translation: "любовь; чувство любви" },
            { chinese: "爱惜", pinyin: "àixī", translation: "дорожить чем-либо; беречь что-либо" },
            { chinese: "爱心", pinyin: "àixīn", translation: "влюблённая душа; любовь, страсть" },
            { chinese: "暧昧", pinyin: "àimèi", translation: "туманный; неопределённый; двусмысленный" },
            { chinese: "挨", pinyin: "āi", translation: "рядом, около" },
            { chinese: "安静", pinyin: "ānjìng", translation: "покойный; тихий; тишина; покой" },
            { chinese: "安居乐业", pinyin: "ānjū lèyè", translation: "жить в мире и благоденствии" },
            { chinese: "安宁", pinyin: "ānníng", translation: "спокойствие; безмятежность; спокойный" },
            { chinese: "安排", pinyin: "ānpái", translation: "налаживать; устраивать; планировать" },
            { chinese: "安全", pinyin: "ānquán", translation: "безопасность; безопасный" },
            { chinese: "安慰", pinyin: "ānwèi", translation: "утешать; успокаивать; утешение" },
            { chinese: "安详", pinyin: "ānxiáng", translation: "спокойный; невозмутимый" },
            { chinese: "安置", pinyin: "ānzhì", translation: "устроить; разместить" },
            { chinese: "安装", pinyin: "ānzhuāng", translation: "монтировать; устанавливать" },
            { chinese: "岸", pinyin: "àn", translation: "берег" },
            { chinese: "暗", pinyin: "àn", translation: "тёмный; мрачный; тусклый" },
            { chinese: "暗示", pinyin: "ànshì", translation: "намекать; делать намёк; намёк" },
            { chinese: "案件", pinyin: "ànjiàn", translation: "дело (юр.)" },
            { chinese: "案例", pinyin: "ànlì", translation: "судебное дело, разбирательство" },
            { chinese: "按摩", pinyin: "ànmó", translation: "делать массаж; массировать" },
            { chinese: "按时", pinyin: "ànshí", translation: "вовремя; своевременно" },
            { chinese: "按照", pinyin: "ànzhào", translation: "согласно; по; в соответствии с" },
            { chinese: "昂贵", pinyin: "ángguì", translation: "дорогой; втридорога" },
            { chinese: "凹凸", pinyin: "āotū", translation: "вогнуто-выпуклый" },
            { chinese: "熬", pinyin: "áo", translation: "долго варить; вываривать" },
            { chinese: "奥秘", pinyin: "àomì", translation: "тайна; секрет; сокровенный" },
            { chinese: "八", pinyin: "bā", translation: "восемь; восьмой" },
            { chinese: "扒", pinyin: "bā", translation: "копать; рыть; сносить; сдирать" },
            { chinese: "疤", pinyin: "bā", translation: "шрам, рубец" },
            { chinese: "巴不得", pinyin: "bābude", translation: "как хорошо бы...; так хочется..." },
            { chinese: "巴结", pinyin: "bājie", translation: "заискивать, угодничать" },
            { chinese: "拔苗助长", pinyin: "bámiáo zhùzhǎng", translation: "тянуть ростки, чтобы помочь их росту (медвежья услуга)" },
            { chinese: "把", pinyin: "bǎ", translation: "держать в руках; взять(ся)" },
            { chinese: "把关", pinyin: "bǎguān", translation: "охранять заставу; держать ключевые позиции" },
            { chinese: "把手", pinyin: "bǎshou", translation: "ручка; рукоятка" },
            { chinese: "把握", pinyin: "bǎwò", translation: "уверенность; гарантия" },
            { chinese: "把戏", pinyin: "bǎxì", translation: "фокус; трюк, уловка" },
            { chinese: "爸爸", pinyin: "bàba", translation: "папа, тятя" },
            { chinese: "霸道", pinyin: "bàdào", translation: "деспотичность; жестокость" },
            { chinese: "罢工", pinyin: "bàgōng", translation: "забастовка; бастовать" },
            { chinese: "吧", pinyin: "ba", translation: "выражает побуждение или предположение" },
            { chinese: "掰", pinyin: "bāi", translation: "разломить" },
            { chinese: "白", pinyin: "bái", translation: "белый; седой" },
            { chinese: "百", pinyin: "bǎi", translation: "сто; сотня" },
            { chinese: "百分点", pinyin: "bǎifēndiǎn", translation: "процент (чего-либо)" },
            { chinese: "摆", pinyin: "bǎi", translation: "ставить; класть" },
            { chinese: "摆脱", pinyin: "bǎituō", translation: "отделаться от...; освободиться" },
            { chinese: "拜访", pinyin: "bàifǎng", translation: "наносить визит; навещать" },
            { chinese: "拜年", pinyin: "bàinián", translation: "поздравлять с Новым годом" },
            { chinese: "拜托", pinyin: "bàituō", translation: "будьте добры!, будьте любезны" },
            { chinese: "败坏", pinyin: "bàihuài", translation: "портить, разлагать; порочить" },
            { chinese: "搬", pinyin: "bān", translation: "передвигать; переносить" },
            { chinese: "班", pinyin: "bān", translation: "группа; класс" },
            { chinese: "班主任", pinyin: "bānzhǔrén", translation: "классный руководитель" },
            { chinese: "颁布", pinyin: "bānbù", translation: "опубликовать; обнародовать" },
            { chinese: "颁发", pinyin: "bānfā", translation: "издавать; рассылать" },
            { chinese: "斑纹", pinyin: "bānwén", translation: "полосы, цветные полоски" },
            { chinese: "版本", pinyin: "bǎnběn", translation: "издание" },
            { chinese: "半", pinyin: "bàn", translation: "половина; полу-; пол-" },
            { chinese: "半途而废", pinyin: "bàntú ér fèi", translation: "бросить на полпути" },
            { chinese: "办法", pinyin: "bànfǎ", translation: "способ, метод" },
            { chinese: "办公室", pinyin: "bàngōngshì", translation: "кабинет; офис" },
            { chinese: "办理", pinyin: "bànlǐ", translation: "заниматься (делами); вести (дела)" },
            { chinese: "伴侣", pinyin: "bànlǚ", translation: "спутник; партнёр" },
            { chinese: "伴随", pinyin: "bànsuí", translation: "сопровождать; сопутствовать" },
            { chinese: "扮演", pinyin: "bànyǎn", translation: "играть роль; выступать в роли" },
            { chinese: "帮忙", pinyin: "bāngmáng", translation: "помогать; оказывать услугу" },
            { chinese: "帮助", pinyin: "bāngzhù", translation: "помогать; помощь" },
            { chinese: "绑架", pinyin: "bǎngjià", translation: "похитить; насильственно увести" },
            { chinese: "榜样", pinyin: "bǎngyàng", translation: "образец, пример" },
            { chinese: "磅", pinyin: "bàng", translation: "фунт (мера веса); весы" },
            { chinese: "棒", pinyin: "bàng", translation: "палка; здорово, замечательно" },
            { chinese: "傍晚", pinyin: "bàngwǎn", translation: "под вечер, к вечеру" },
            { chinese: "包", pinyin: "bāo", translation: "завёртывать, обёртывать; упаковывать" },
            { chinese: "包庇", pinyin: "bāobì", translation: "брать под защиту; покрывать" },
            { chinese: "包袱", pinyin: "bāofu", translation: "узел (с вещами)" },
            { chinese: "包裹", pinyin: "bāoguǒ", translation: "узел; пакет; посылка" },
            { chinese: "包含", pinyin: "bāohán", translation: "содержать в себе; заключать в себе" },
            { chinese: "包括", pinyin: "bāokuò", translation: "охватывать, включать; в том числе" },
            { chinese: "包围", pinyin: "bāowéi", translation: "окружать, брать в кольцо" },
            { chinese: "包装", pinyin: "bāozhuāng", translation: "упаковывать; упаковка" },
            { chinese: "包子", pinyin: "bāozi", translation: "паровые пирожки" },
            { chinese: "饱", pinyin: "bǎo", translation: "насытиться; досыта" },
            { chinese: "饱和", pinyin: "bǎohé", translation: "насыщение; насыщенный" },
            { chinese: "饱经沧桑", pinyin: "bǎojīngcāngsāng", translation: "претерпеть превратности судьбы" },
            { chinese: "宝贝", pinyin: "bǎobèi", translation: "драгоценность; сокровище; ребеночек" },
            { chinese: "薄", pinyin: "báo", translation: "тонкий" },
            { chinese: "宝贵", pinyin: "bǎoguì", translation: "драгоценный; ценный" },
            { chinese: "保持", pinyin: "bǎochí", translation: "сохранять, поддерживать; соблюдать" },
            { chinese: "保存", pinyin: "bǎocún", translation: "хранить; сохранять" },
            { chinese: "保管", pinyin: "bǎoguǎn", translation: "хранить; содержать; управлять" },
            { chinese: "保护", pinyin: "bǎohù", translation: "защищать; охранять; охрана" },
            { chinese: "保留", pinyin: "bǎoliú", translation: "сохранять" },
            { chinese: "保密", pinyin: "bǎomì", translation: "сохранять тайну; держать в секрете" },
            { chinese: "保姆", pinyin: "bǎomǔ", translation: "воспитательница, няня" },
            { chinese: "保守", pinyin: "bǎoshǒu", translation: "сохранять; соблюдать" },
            { chinese: "保卫", pinyin: "bǎowèi", translation: "защищать; охранять; оборонять" },
            { chinese: "保险", pinyin: "bǎoxiǎn", translation: "страховать; страхование; безопасный" },
            { chinese: "保养", pinyin: "bǎoyǎng", translation: "беречь" },
            { chinese: "保障", pinyin: "bǎozhàng", translation: "гарантировать; обеспечивать; гарантия" },
            { chinese: "保证", pinyin: "bǎozhèng", translation: "гарантировать; ручаться; гарантия" },
            { chinese: "保重", pinyin: "bǎozhòng", translation: "беречь себя; заботиться о своём здоровье" },
            { chinese: "抱负", pinyin: "bàofù", translation: "нести в охапке и за плечами; ноша" },
            { chinese: "抱", pinyin: "bào", translation: "обнимать; охватывать руками" },
            { chinese: "抱歉", pinyin: "bàoqiàn", translation: "сожалеть, выражать сожаление" },
            { chinese: "抱怨", pinyin: "bàoyuàn", translation: "роптать; жаловаться; сетовать" },
            { chinese: "报仇", pinyin: "bàochóu", translation: "отомстить; месть; возмездие" },
            { chinese: "报酬", pinyin: "bàochóu", translation: "вознаграждение; плата, оплата" },
            { chinese: "报答", pinyin: "bàodá", translation: "отблагодарить; воздать; отплатить" },
            { chinese: "报到", pinyin: "bàodào", translation: "явиться и доложить о прибытии" },
            { chinese: "报道", pinyin: "bàodào", translation: "сообщать, информировать" },
            { chinese: "报复", pinyin: "bàofù", translation: "отплатить, отомстить; реванш" },
            { chinese: "报告", pinyin: "bàogào", translation: "докладывать; доносить; рапортовать" },
            { chinese: "报名", pinyin: "bàomíng", translation: "записаться, зарегистрироваться" },
            { chinese: "报社", pinyin: "bàoshè", translation: "редакция газеты" },
            { chinese: "报销", pinyin: "bàoxiāo", translation: "отчитаться (в деньгах); баланс" },
            { chinese: "报纸", pinyin: "bàozhǐ", translation: "газета; газеты" },
            { chinese: "爆发", pinyin: "bàofā", translation: "извергаться; извержение" },
            { chinese: "爆炸", pinyin: "bàozhà", translation: "взрываться; разрываться" },
            { chinese: "曝光", pinyin: "bàoguāng", translation: "экспозиция; выдержка" },
            { chinese: "暴力", pinyin: "bàolì", translation: "насилие; грубая сила" },
            { chinese: "暴露", pinyin: "bàolù", translation: "вскрыть, обнажить, раскрыть" },
            { chinese: "悲哀", pinyin: "bēi'āi", translation: "печальный, скорбный; печаль; горе" },
            { chinese: "悲惨", pinyin: "bēicǎn", translation: "трагический; трагичный" },
            { chinese: "悲观", pinyin: "bēiguān", translation: "пессимистический" },
            { chinese: "卑鄙", pinyin: "bēibǐ", translation: "подлый, низкий, мерзкий" },
            { chinese: "杯子", pinyin: "bēizi", translation: "стакан; кружка; бокал; рюмка" },
            { chinese: "北方", pinyin: "běifāng", translation: "север; северный" },
            { chinese: "北极", pinyin: "běijí", translation: "Северный полюс" },
            { chinese: "北京", pinyin: "běijīng", translation: "Пекин" },
            { chinese: "倍", pinyin: "bèi", translation: "раз; крат" },
            { chinese: "被", pinyin: "bèi", translation: "сл. слово для выражения пассива" },
            { chinese: "被动", pinyin: "bèidòng", translation: "быть пассивным; пассивный" },
            { chinese: "被告", pinyin: "bèigào", translation: "обвиняемый; подсудимый; ответчик" },
            { chinese: "被子", pinyin: "bèizi", translation: "одеяло" },
            { chinese: "背", pinyin: "bèi", translation: "спина; спинка" },
            { chinese: "背景", pinyin: "bèijǐng", translation: "задняя декорация; задний план, фон" },
            { chinese: "背叛", pinyin: "bèipàn", translation: "изменить; предать; измена" },
            { chinese: "背诵", pinyin: "bèisòng", translation: "читать наизусть, декламировать" },
            { chinese: "备份", pinyin: "bèifèn", translation: "резервная копия, бэкап" },
            { chinese: "备忘录", pinyin: "bèiwànglù", translation: "меморандум" },
            { chinese: "贝壳", pinyin: "bèiké", translation: "раковина; ракушка" },
            { chinese: "奔波", pinyin: "bēnbō", translation: "бегать, хлопотать; гонка, беготня" },
            { chinese: "奔驰", pinyin: "bēnchí", translation: "мчаться, нестись" },
            { chinese: "本", pinyin: "běn", translation: "счётное слово для растений, цветов" },
            { chinese: "本科", pinyin: "běnkē", translation: "основные предметы преподавания" },
            { chinese: "本来", pinyin: "běnlái", translation: "первоначальный; истинный; изначальный" },
            { chinese: "本领", pinyin: "běnlǐng", translation: "умение; навыки; мастерство" },
            { chinese: "本能", pinyin: "běnnéng", translation: "инстинкт; инстинктивно" },
            { chinese: "本钱", pinyin: "běnqián", translation: "капитал; деньги" },
            { chinese: "本人", pinyin: "běnrén", translation: "я; сам; лично" },
            { chinese: "本身", pinyin: "běnshēn", translation: "сам; сам по себе; свой; собственный" },
            { chinese: "本事", pinyin: "běnshi", translation: "способности; умение" },
            { chinese: "本着", pinyin: "běnzhe", translation: "на основании; в соответствии с…" },
            { chinese: "本质", pinyin: "běnzhì", translation: "сущность, суть, существо" },
            { chinese: "笨", pinyin: "bèn", translation: "глупый; тупой" },
            { chinese: "笨拙", pinyin: "bènzhuō", translation: "неуклюжий; неловкий; неловкость" },
            { chinese: "崩溃", pinyin: "bēngkuì", translation: "потерпеть крах; крах, крушение" },
            { chinese: "甭", pinyin: "béng", translation: "не надо, незачем; не стоит" },
            { chinese: "蹦", pinyin: "bèng", translation: "прыгать, скакать; подпрыгивать" },
            { chinese: "迸发", pinyin: "bèngfā", translation: "брызнуть; посыпаться (об искрах)" },
            { chinese: "逼迫", pinyin: "bīpò", translation: "вынуждать, заставлять" },
            { chinese: "鼻涕", pinyin: "bítì", translation: "сопли" },
            { chinese: "鼻子", pinyin: "bízi", translation: "нос" },
            { chinese: "比", pinyin: "bǐ", translation: "сравнивать; в сравнении" },
            { chinese: "比方", pinyin: "bǐfang", translation: "пример; например" },
            { chinese: "比较", pinyin: "bǐjiào", translation: "сравнительно" },
            { chinese: "比例", pinyin: "bǐlì", translation: "пропорция; соотношение" },
            { chinese: "比如", pinyin: "bǐrú", translation: "например; к примеру" },
            { chinese: "比赛", pinyin: "bǐsài", translation: "соревнования, состязания; турнир" },
            { chinese: "比喻", pinyin: "bǐyù", translation: "образное выражение; метафора" },
            { chinese: "比重", pinyin: "bǐzhòng", translation: "удельный вес" },
            { chinese: "彼此", pinyin: "bǐcǐ", translation: "и тот и другой; обе стороны; взаимно" },
            { chinese: "笔记本", pinyin: "bǐjìběn", translation: "ноутбук" },
            { chinese: "臂", pinyin: "bì", translation: "рука (выше кисти); предплечье" },
            { chinese: "弊病", pinyin: "bìbìng", translation: "недостаток; порок" },
            { chinese: "弊端", pinyin: "bìduān", translation: "злоупотребление" },
            { chinese: "必定", pinyin: "bìdìng", translation: "обязательно, непременно; неизбежно" },
            { chinese: "必然", pinyin: "bìrán", translation: "неизбежно; непременно; неизбежный" },
            { chinese: "必须", pinyin: "bìxū", translation: "необходимо, должно" },
            { chinese: "必需", pinyin: "bìxū", translation: "необходимый, нужный, требуемый" },
            { chinese: "必要", pinyin: "bìyào", translation: "необходимый, нужный; необходимость" },
            { chinese: "毕竟", pinyin: "bìjìng", translation: "в конце концов; всё-таки, всё же" }
        ];

        const chineseWordEl = document.getElementById('chinese-word');
        const optionsContainerEl = document.getElementById('options-container');
        const pinyinEl = document.getElementById('pinyin');
        const resultEl = document.getElementById('result');
        const nextWordBtn = document.getElementById('next-word');
        const toggleWordListBtn = document.getElementById('toggle-word-list');
        const wordListContainer = document.getElementById('word-list-container');
        const wordListBody = document.getElementById('word-list-body');

        let currentWord = {};
        let words = [...wordsData]; // Создаем копию массива для работы

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }
        
        function populateWordList() {
            wordListBody.innerHTML = '';
            wordsData.forEach(word => {
                const row = document.createElement('tr');
                row.className = 'bg-white border-b dark:bg-gray-800 dark:border-gray-700';
                row.innerHTML = `
                    <td class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">${word.chinese}</td>
                    <td class="px-6 py-4">${word.pinyin}</td>
                    <td class="px-6 py-4">${word.translation}</td>
                `;
                wordListBody.appendChild(row);
            });
        }

        function getNewWord() {
            // Сбрасываем состояние
            pinyinEl.textContent = '';
            resultEl.textContent = '';
            nextWordBtn.disabled = true;
            optionsContainerEl.innerHTML = '';

            // Перемешиваем массив, если он закончился
            if (words.length === 0) {
                 words = [...wordsData];
            }
            
            shuffleArray(words);
            currentWord = words.pop(); // Берем последнее слово и удаляем его из массива

            chineseWordEl.textContent = currentWord.chinese;

            const options = [currentWord];
            
            // Получаем 3 неправильных варианта
            while (options.length < 4) {
                const randomWord = wordsData[Math.floor(Math.random() * wordsData.length)];
                // Убеждаемся, что не добавляем дубликаты
                if (!options.some(opt => opt.chinese === randomWord.chinese)) {
                    options.push(randomWord);
                }
            }

            shuffleArray(options);

            options.forEach(option => {
                const button = document.createElement('button');
                button.textContent = option.translation.split(';')[0]; // Показываем только первую часть перевода для краткости
                button.classList.add('btn', 'w-full', 'p-4', 'rounded-lg', 'border-2', 'border-gray-300', 'dark:border-gray-600', 'bg-white', 'dark:bg-gray-800', 'hover:bg-gray-100', 'dark:hover:bg-gray-700', 'focus:outline-none', 'focus:ring-2', 'focus:ring-offset-2', 'focus:ring-blue-500', 'dark:focus:ring-offset-gray-900');
                button.onclick = () => checkAnswer(option.chinese, button);
                optionsContainerEl.appendChild(button);
            });
        }

        function checkAnswer(selectedChinese, button) {
            // Отключаем все кнопки после выбора
            const buttons = optionsContainerEl.querySelectorAll('button');
            buttons.forEach(btn => btn.disabled = true);

            pinyinEl.textContent = currentWord.pinyin;

            if (selectedChinese === currentWord.chinese) {
                resultEl.textContent = 'Правильно!';
                resultEl.classList.remove('text-red-500', 'dark:text-red-400');
                resultEl.classList.add('text-green-500', 'dark:text-green-400');
                button.classList.add('correct');
            } else {
                resultEl.textContent = `Неправильно. Правильный ответ: ${currentWord.translation.split(';')[0]}`;
                resultEl.classList.remove('text-green-500', 'dark:text-green-400');
                resultEl.classList.add('text-red-500', 'dark:text-red-400');
                button.classList.add('incorrect');
                // Показываем правильный ответ
                buttons.forEach(btn => {
                    const optionText = currentWord.translation.split(';')[0];
                    if (btn.textContent === optionText) {
                        btn.classList.add('correct');
                    }
                });
            }
            nextWordBtn.disabled = false;
            nextWordBtn.focus();
        }

        nextWordBtn.addEventListener('click', getNewWord);
        toggleWordListBtn.addEventListener('click', () => {
             wordListContainer.classList.toggle('hidden');
        });

        // Запуск игры
        populateWordList();
        getNewWord();
    </script>

</body>
</html>
