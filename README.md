import React, { useState } from 'react';
import { ChevronLeft, ChevronRight, RotateCcw } from 'lucide-react';

const App = () => {
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

  const [currentIndex, setCurrentIndex] = useState(0);
  const [isFlipped, setIsFlipped] = useState(false);

  const currentWord = vocabulary[currentIndex];

  const nextCard = () => {
    setIsFlipped(false);
    setTimeout(() => {
      setCurrentIndex((prev) => (prev + 1) % vocabulary.length);
    }, 150);
  };

  const prevCard = () => {
    setIsFlipped(false);
    setTimeout(() => {
      setCurrentIndex((prev) => (prev - 1 + vocabulary.length) % vocabulary.length);
    }, 150);
  };

  const shuffleCards = () => {
    setIsFlipped(false);
    setCurrentIndex(Math.floor(Math.random() * vocabulary.length));
  };

  return (
    <div className="min-h-screen bg-slate-50 flex flex-col items-center justify-center p-4 font-sans text-slate-800">
      <div className="w-full max-w-md space-y-6">
        
        {/* Header */}
        <div className="text-center space-y-2">
          <h1 className="text-3xl font-bold text-indigo-600">Китайские Карточки</h1>
          <p className="text-slate-500 italic">Учитесь эффективно без лишних деталей</p>
        </div>

        {/* Flashcard */}
        <div 
          className="group perspective h-72 cursor-pointer"
          onClick={() => setIsFlipped(!isFlipped)}
        >
          <div className={`relative w-full h-full transition-all duration-500 preserve-3d shadow-xl rounded-3xl ${isFlipped ? 'rotate-y-180' : ''}`}>
            
            {/* Front (Russian) */}
            <div className="absolute inset-0 backface-hidden bg-white rounded-3xl flex flex-col items-center justify-center p-8 text-center border border-slate-100">
              <span className="text-xs text-indigo-400 font-bold mb-4 uppercase tracking-[0.2em]">Перевод</span>
              <p className="text-4xl font-semibold text-slate-700 leading-tight">{currentWord.ru}</p>
              <p className="mt-8 text-slate-300 text-sm">Нажмите, чтобы перевернуть</p>
            </div>

            {/* Back (Chinese) */}
            <div className="absolute inset-0 backface-hidden bg-indigo-600 rounded-3xl flex flex-col items-center justify-center p-8 text-center rotate-y-180 shadow-indigo-200 shadow-2xl">
              <span className="text-xs text-indigo-200 font-bold mb-4 uppercase tracking-[0.2em]">Иероглиф</span>
              <p className="text-7xl mb-6 text-white font-bold tracking-normal">{currentWord.zh}</p>
              <div className="bg-indigo-500/30 px-4 py-2 rounded-lg backdrop-blur-sm">
                <p className="text-2xl text-white font-medium">{currentWord.py}</p>
              </div>
            </div>

          </div>
        </div>

        {/* Controls */}
        <div className="flex flex-col gap-6">
          <div className="flex items-center justify-between px-2">
            <button 
              onClick={prevCard}
              className="p-4 bg-white hover:bg-indigo-50 text-indigo-600 rounded-2xl shadow-sm border border-slate-200 transition-all active:scale-95"
              title="Предыдущая"
            >
              <ChevronLeft className="w-6 h-6" />
            </button>

            <div className="flex flex-col items-center">
              <div className="text-slate-400 text-xs mb-1 uppercase tracking-tighter font-bold">Прогресс</div>
              <div className="text-indigo-600 font-mono text-lg font-bold bg-indigo-50 px-4 py-1 rounded-full">
                {currentIndex + 1} / {vocabulary.length}
              </div>
            </div>

            <button 
              onClick={nextCard}
              className="p-4 bg-white hover:bg-indigo-50 text-indigo-600 rounded-2xl shadow-sm border border-slate-200 transition-all active:scale-95"
              title="Следующая"
            >
              <ChevronRight className="w-6 h-6" />
            </button>
          </div>

          <button 
            onClick={shuffleCards}
            className="w-full flex items-center justify-center gap-2 py-3 bg-white hover:bg-slate-100 text-slate-600 rounded-xl border border-slate-200 shadow-sm transition-all font-medium text-sm active:bg-slate-200"
          >
            <RotateCcw className="w-4 h-4" /> Случайное слово
          </button>
        </div>

        {/* Footer info */}
        <div className="pt-4 text-center">
            <p className="text-slate-400 text-xs">Всего слов в наборе: {vocabulary.length}</p>
        </div>
      </div>

      <style>{`
        .perspective {
          perspective: 1500px;
        }
        .preserve-3d {
          transform-style: preserve-3d;
        }
        .backface-hidden {
          backface-visibility: hidden;
        }
        .rotate-y-180 {
          transform: rotateY(180deg);
        }
      `}</style>
    </div>
  );
};

export default App;
