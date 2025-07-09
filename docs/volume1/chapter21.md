# 第21話 関数で整理する想い

## 10月30日（月）午前7時30分

月曜日の朝。

週末は隆弘くんと過ごして、幸せだった。

でも、学校では気をつけなきゃ。

付き合ってることは、まだ内緒にしてる。

「おはよう」

教室に入ると、友達が話しかけてきた。

「なんか最近、幸せそうだね」

「え、そ、そうかな？」

バレてる？

◇◇◇◇

## 午後4時　プログラミング部室

「今日は関数について、もっと詳しく教えるよ」

放課後、いつもの部室。

隆弘くんが新しい概念を説明し始める。

「関数って、前にも習ったよね？」

「うん。でも今日は、もっと実践的な使い方」

```javascript
// 基本的な関数の定義
function greet(name) {
    return `こんにちは、${name}さん！`;
}

// アロー関数（新しい書き方）
const greetArrow = (name) => {
    return `こんにちは、${name}さん！`;
};

// もっと短く書ける
const greetShort = name => `こんにちは、${name}さん！`;

console.log(greet("美久")); // こんにちは、美久さん！
```

「アロー関数？」

「新しい関数の書き方。短くて便利なんだ」

隆弘くんの説明を聞きながら、ノートを取る。

恋人になっても、真面目に勉強。

でも、時々目が合うと、ドキドキしちゃう。

◇◇◇◇

## 午後4時45分　ゲームシステムの整理

「関数を使って、ゲームのコードを整理しよう」

```javascript
// ゲームの各機能を関数で分割
const GameSystem = {
    // 初期化
    init: function() {
        this.loadSettings();
        this.createUI();
        this.startBGM();
        console.log("ゲームを初期化しました");
    },
    
    // 設定の読み込み
    loadSettings: function() {
        const saved = localStorage.getItem('gameSettings');
        if (saved) {
            this.settings = JSON.parse(saved);
        } else {
            this.settings = this.defaultSettings();
        }
    },
    
    // デフォルト設定
    defaultSettings: function() {
        return {
            volume: 0.7,
            textSpeed: "normal",
            autoMode: false,
            skipRead: true
        };
    },
    
    // UI作成
    createUI: function() {
        this.createTextBox();
        this.createChoiceButtons();
        this.createMenuButton();
    },
    
    // テキストボックス作成
    createTextBox: () => {
        console.log("テキストボックスを作成");
    },
    
    // 選択肢ボタン作成
    createChoiceButtons: () => {
        console.log("選択肢ボタンを作成");
    },
    
    // メニューボタン作成
    createMenuButton: () => {
        console.log("メニューボタンを作成");
    },
    
    // BGM開始
    startBGM: function() {
        const bgm = new Audio('bgm/title.mp3');
        bgm.volume = this.settings.volume;
        bgm.play();
    }
};
```

「すごい！機能ごとに分かれてて、見やすい」

「これが関数の力。複雑なプログラムも整理できる」

確かに、前はごちゃごちゃしてたコードが、すっきりしてる。

◇◇◇◇

## 午後5時30分　恋愛シミュレーション部分

「好感度システムも関数で整理しよう」

```javascript
// キャラクター管理システム
const CharacterManager = {
    characters: {
        miku: { name: "美久", affection: 0, route: false },
        yui: { name: "ゆい", affection: 0, route: false },
        sakura: { name: "さくら", affection: 0, route: false }
    },
    
    // 好感度を上げる
    increaseAffection: function(character, amount) {
        if (this.characters[character]) {
            this.characters[character].affection += amount;
            this.checkRouteCondition(character);
            this.displayAffectionChange(character, amount);
        }
    },
    
    // ルート条件チェック
    checkRouteCondition: function(character) {
        const char = this.characters[character];
        if (char.affection >= 80 && !char.route) {
            char.route = true;
            this.unlockRoute(character);
        }
    },
    
    // ルート解放
    unlockRoute: function(character) {
        console.log(`${this.characters[character].name}ルートが解放されました！`);
        // 特別なイベントを発生させる
        EventManager.trigger(`${character}_route_unlocked`);
    },
    
    // 好感度変化の表示
    displayAffectionChange: function(character, amount) {
        const message = amount > 0 
            ? `${this.characters[character].name}の好感度が${amount}上がった！`
            : `${this.characters[character].name}の好感度が${Math.abs(amount)}下がった...`;
        
        this.showFloatingText(message);
    },
    
    // フローティングテキスト表示
    showFloatingText: function(text) {
        console.log(`♥ ${text} ♥`);
    },
    
    // 現在の最高好感度キャラを取得
    getHighestAffection: function() {
        let highest = null;
        let maxAffection = -1;
        
        for (let char in this.characters) {
            if (this.characters[char].affection > maxAffection) {
                maxAffection = this.characters[char].affection;
                highest = char;
            }
        }
        
        return highest;
    }
};
```

「私の好感度、どれくらい？」

美久がいたずらっぽく聞いてくる。

「計測不能」

「もう、真面目に答えて」

「真面目だよ。Infinityだから」

顔を赤くする美久が可愛い。

◇◇◇◇

## 午後6時15分　イベントシステム

「イベント管理も関数で作ろう」

```javascript
// イベント管理システム
const EventManager = {
    listeners: {},
    
    // イベントリスナー登録
    on: function(eventName, callback) {
        if (!this.listeners[eventName]) {
            this.listeners[eventName] = [];
        }
        this.listeners[eventName].push(callback);
    },
    
    // イベント発火
    trigger: function(eventName, data) {
        if (this.listeners[eventName]) {
            this.listeners[eventName].forEach(callback => {
                callback(data);
            });
        }
    },
    
    // 一度だけ実行されるイベント
    once: function(eventName, callback) {
        const wrapper = (data) => {
            callback(data);
            this.off(eventName, wrapper);
        };
        this.on(eventName, wrapper);
    },
    
    // イベントリスナー削除
    off: function(eventName, callback) {
        if (this.listeners[eventName]) {
            this.listeners[eventName] = this.listeners[eventName]
                .filter(cb => cb !== callback);
        }
    }
};

// 使用例
EventManager.on('confession_scene', () => {
    console.log("告白シーンが始まりました");
    CharacterManager.showFloatingText("ドキドキ...");
});

EventManager.on('first_kiss', () => {
    console.log("初めてのキス...");
    Achievement.unlock('first_kiss');
});
```

「これで、特定のシーンで特別な演出ができるんだ」

隆弘くんの説明を聞きながら、思い出す。

土曜日の夕暮れ、川辺でのキス。

「first_kissイベント、もう発火しちゃったね」

隆弘くんが小声で言う。

「もう！」

でも、嬉しい。

◇◇◇◇

## 午後7時　ユーティリティ関数

「便利な関数もまとめて作っておこう」

```javascript
// 便利な関数たち
const Utils = {
    // ランダムな整数を生成
    random: (min, max) => {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    
    // 配列をシャッフル
    shuffle: (array) => {
        const newArray = [...array];
        for (let i = newArray.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
        }
        return newArray;
    },
    
    // 文字列を1文字ずつ表示
    typeWriter: async (text, element, speed = 50) => {
        element.textContent = '';
        for (let char of text) {
            element.textContent += char;
            await Utils.sleep(speed);
        }
    },
    
    // 指定時間待機
    sleep: (ms) => {
        return new Promise(resolve => setTimeout(resolve, ms));
    },
    
    // 日付をフォーマット
    formatDate: (date) => {
        const d = new Date(date);
        return `${d.getFullYear()}年${d.getMonth() + 1}月${d.getDate()}日`;
    },
    
    // 愛のメッセージ生成（おまけ）
    generateLoveMessage: (name) => {
        const messages = [
            `${name}、大好きだよ`,
            `${name}といると幸せ`,
            `${name}、愛してる`,
            `ずっと${name}と一緒にいたい`
        ];
        return messages[Utils.random(0, messages.length - 1)];
    }
};

// テスト
console.log(Utils.generateLoveMessage("美久"));
// "美久、愛してる"
```

「愛のメッセージ生成関数！？」

「実装してみた」

隆弘くんが照れくさそうに笑う。

「もう一回実行して」

```javascript
console.log(Utils.generateLoveMessage("美久"));
// "ずっと美久と一緒にいたい"
```

胸がきゅんとする。

プログラムが言ってるんじゃない。

隆弘くんの気持ちが、コードに込められてる。

◇◇◇◇

## 午後7時45分　今日のまとめ

「関数を使えば、複雑なプログラムも管理しやすくなる」

部室には、また二人きり。

「人の気持ちも、関数で整理できたらいいのに」

私がつぶやく。

「どういうこと？」

「好きって気持ちを、きちんと整理して、伝えたい時に呼び出せたら」

隆弘くんが優しく微笑む。

「でも、整理できないから素敵なんじゃない？」

「そうかな」

「予測できない関数があってもいい」

そう言って、隆弘くんが私の手を取る。

「美久のこと考えると、いつも違う気持ちになる」

「どんな？」

「幸せで、切なくて、愛おしくて」

その言葉に、涙が出そうになる。

◇◇◇◇

## 午後8時15分　帰り道

「文化祭まで、あと4日」

マンションへの帰り道。

手を繋いで歩く。

「緊張する？」

「うん。でも、楽しみ」

「みんなに遊んでもらえるといいね」

「隆弘くんと作ったゲームだから、きっと大丈夫」

信じてる。

二人で作ったものは、きっと誰かの心に届く。

「美久」

「なに？」

「文化祭が終わったら、デートしよう」

「うん」

「今度は、遊園地とか」

「楽しみ」

エレベーターで7階へ。

今日も名残惜しい。

「おやすみ」

「おやすみなさい」

部屋に入る前に、隆弘くんが言った。

「美久、今日も可愛かった」

「急に何？」

「generateLoveMessage関数の実行結果」

プログラマーらしい愛の伝え方。

でも、それがまた愛おしい。

私たちの恋は、関数では整理できない。

でも、それでいい。

予測不能で、バグだらけで、でも最高に幸せ。

それが、私たちの恋愛プログラム。