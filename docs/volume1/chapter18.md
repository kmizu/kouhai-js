# 第18話 オブジェクトで繋がる想い

## 10月26日（木）午前7時

昨夜のキス。

頬に触れた美久の柔らかい唇の感触が、まだ残っている。

（嬉しかった）

素直にそう思う。

美久の気持ちが、確実に僕に向いている。

それが分かって、世界が違って見える。

今日も美久に会える。

それだけで、朝から幸せな気分だ。

◇◇◇◇

## 午前7時45分　いつもの廊下

「おはよう、美久」

いつもより少し早く出て、美久を待っていた。

「お、おはよう」

美久の顔が真っ赤だ。

昨夜のことを思い出しているのかな。

「よく眠れた？」

「う、うん。隆弘くんは？」

「最高の夢を見た」

僕の言葉に、美久の顔がさらに赤くなる。

可愛い。

「今日も一緒に頑張ろう」

「うん！」

エレベーターで降りながら、美久がそっと言った。

「昨日は、ありがとう」

「こちらこそ」

二人で顔を見合わせて、照れ笑いを浮かべる。

新しい朝が、始まった。

◇◇◇◇

## 午後4時30分　プログラミング部室

「今日は、オブジェクトについて教えるよ」

放課後の部室。

他の部員たちは、それぞれの作業に没頭している。

「オブジェクト？」

「複数のデータをまとめて管理する仕組みだよ」

僕はホワイトボードに図を描きながら説明する。

```javascript
// キャラクターのデータをオブジェクトで表現
const miku = {
    name: "河内美久",
    age: 16,
    grade: 1,
    affection: 0,
    route: "childhood_friend",
    personality: ["明るい", "素直", "一途"],
    likes: ["プログラミング", "イラスト", "隆弘くん"]
};

console.log(miku.name); // "河内美久"
console.log(miku.likes[2]); // "隆弘くん"
```

「わあ、私のデータだ！」

美久が嬉しそうに画面を見つめる。

「でも、好きなものに隆弘くんって……」

「事実でしょ？」

僕がニヤリと笑うと、美久は真っ赤になった。

「もう、隆弘くんのいじわる」

でも、否定はしない。

それが嬉しい。

◇◇◇◇

## 午後5時15分　ゲームへの実装

「オブジェクトを使えば、ゲームのデータ管理が楽になる」

```javascript
// ゲーム全体の状態管理
const gameData = {
    player: {
        name: "主人公",
        choices: []
    },
    characters: {
        miku: {
            name: "美久",
            affection: 0,
            flags: {}
        },
        yui: {
            name: "ゆい",
            affection: 0,
            flags: {}
        },
        sakura: {
            name: "さくら",
            affection: 0,
            flags: {}
        }
    },
    scene: {
        current: 1,
        background: "classroom.jpg",
        bgm: "daily.mp3"
    }
};

// 好感度を上げる関数
function increaseAffection(character, amount) {
    gameData.characters[character].affection += amount;
    console.log(`${gameData.characters[character].name}の好感度が${amount}上がった！`);
}
```

「なるほど！全部のデータが整理されてる」

美久が理解した様子で頷く。

「これなら、どのキャラクターがどんな状態か、すぐ分かるね」

「そう。プログラミングは、整理整頓が大事なんだ」

僕の言葉に、美久が微笑む。

「隆弘くんの部屋みたいに？」

「あ、あれは……」

確かに、僕の部屋は整理整頓が行き届いている。

美久に褒められて、ちょっと照れる。

◇◇◇◇

## 午後6時　メソッドの追加

「オブジェクトには、関数も入れられるんだ」

```javascript
// キャラクターオブジェクトに機能を追加
const mikuCharacter = {
    name: "河内美久",
    affection: 50,
    mood: "happy",
    
    // メソッド（オブジェクトの関数）
    speak: function(text) {
        console.log(`${this.name}: ${text}`);
    },
    
    changeMode: function(newMood) {
        this.mood = newMood;
        console.log(`${this.name}の気分が${newMood}になった`);
    },
    
    confess: function() {
        if (this.affection >= 80) {
            this.speak("あの……実は、ずっと好きでした！");
            return true;
        } else {
            this.speak("え、えっと……");
            return false;
        }
    }
};

// 使用例
mikuCharacter.affection = 85;
mikuCharacter.confess(); // "河内美久: あの……実は、ずっと好きでした！"
```

「告白機能まで！？」

美久が顔を真っ赤にする。

「ゲームには必要でしょ？」

「そ、そうだけど……」

画面を見つめる美久の横顔。

昨夜のキスを思い出してしまう。

「実際の告白は、もっとドキドキするけどね」

つい、そんなことを言ってしまった。

美久が僕を見つめる。

その瞳に、期待のような何かが宿っている。

◇◇◇◇

## 午後6時45分　複雑なオブジェクト

「もっと複雑な構造も作れるよ」

```javascript
// シーンデータの管理
const sceneDatabase = {
    opening: {
        id: 1,
        title: "運命の出会い",
        location: "教室",
        time: "放課後",
        characters: ["miku"],
        dialogue: [
            {
                speaker: "miku",
                text: "先輩、プログラミング教えてください！",
                expression: "smile"
            },
            {
                speaker: "player",
                text: "え？どうして急に？",
                choices: [
                    {
                        text: "もちろん教えるよ",
                        effect: () => increaseAffection("miku", 10)
                    },
                    {
                        text: "ちょっと忙しいかな",
                        effect: () => increaseAffection("miku", -5)
                    }
                ]
            }
        ]
    },
    
    confession: {
        id: 99,
        title: "想いを伝える時",
        location: "屋上",
        time: "夕暮れ",
        required: {
            affection: { miku: 80 },
            flags: { "completed_game": true }
        },
        dialogue: [
            {
                speaker: "miku",
                text: "ゲーム、完成したね",
                expression: "happy"
            },
            {
                speaker: "player",
                text: "美久のおかげだよ"
            },
            {
                speaker: "miku",
                text: "違う。二人で作ったから",
                expression: "blush"
            },
            {
                speaker: "player",
                text: "美久、伝えたいことがあるんだ"
            }
        ]
    }
};
```

「すごい……物語の全てがデータになってる」

美久が感心したように呟く。

「告白シーンまで用意されてる」

「実装するかは別として、ね」

僕が言うと、美久が小さく呟いた。

「実装して欲しいな」

聞こえたような、聞こえなかったような。

でも、美久の頬が赤いのは確かだ。

◇◇◇◇

## 午後7時30分　オブジェクトの応用

「オブジェクトを使えば、セーブデータも簡単に作れる」

```javascript
// セーブデータの構造
const saveData = {
    version: "1.0.0",
    timestamp: new Date().toISOString(),
    playerName: "嵐山隆弘",
    currentScene: 42,
    gameProgress: {
        completedScenes: [1, 2, 3, 5, 7, 10, 15, 20, 25, 30, 35, 40, 42],
        unlockedCGs: ["opening", "first_lesson", "debugging_night"],
        unlockedBGMs: ["main_theme", "love_theme"]
    },
    characterStatus: {
        miku: {
            affection: 95,
            route: "true_end",
            memories: [
                "初めてのプログラミングレッスン",
                "一緒に作ったゲーム",
                "深夜のデバッグ",
                "頬へのキス"
            ]
        }
    }
};

// セーブ機能
function saveGame() {
    localStorage.setItem('gameData', JSON.stringify(saveData));
    console.log("ゲームを保存しました");
}

// ロード機能
function loadGame() {
    const saved = localStorage.getItem('gameData');
    if (saved) {
        return JSON.parse(saved);
    }
    return null;
}
```

「思い出まで保存されてる……」

美久が画面を見つめる。

「頬へのキスって」

「昨日の夜のことも、大切な思い出だから」

僕の言葉に、美久の目が潤む。

「隆弘くん……」

「美久との思い出は、全部大切に保存したい」

今度は僕が照れる番だった。

でも、本心だ。

◇◇◇◇

## 午後8時15分　今日のまとめ

「オブジェクトを使えば、複雑なデータも整理できる」

部室には、僕と美久だけになっていた。

他の部員は、みんな帰ってしまった。

「人の気持ちも、オブジェクトみたいに整理できたらいいのに」

美久がぽつりと呟く。

「どういうこと？」

「だって、好きとか、嬉しいとか、数値化できないじゃない」

美久の言葉に、僕は少し考えた。

「でも、それがいいんじゃないかな」

「え？」

「数値化できないから、言葉で伝える必要がある」

僕は美久を見つめる。

「昨日の夜みたいに、行動で示すこともできる」

美久の顔が、また赤くなった。

「そ、そうだね」

◇◇◇◇

## 午後8時45分　帰り道

「今日もありがとう」

マンションへの帰り道。

夜風が心地いい。

「オブジェクト、難しかった？」

「ううん。隆弘くんの説明、分かりやすかった」

「良かった」

しばらく無言で歩く。

でも、昨日とは違う。

もっと近い距離で、もっと自然に。

「ねえ、隆弘くん」

「ん？」

「私たちの関係も、オブジェクトで表現できる？」

突然の質問に、僕は考える。

そして、答えた。

```javascript
const ourRelationship = {
    type: "幼馴染から恋人へ",
    startDate: "unknown",
    currentStatus: "相思相愛",
    sharedMemories: ["countless"],
    futurePromise: "ずっと一緒",
    love: Infinity
};
```

頭の中で組み立てたコードを、そのまま言葉にする。

美久が立ち止まった。

「Infinityって……」

「無限大。測定不能なくらい大きい」

僕も立ち止まり、美久を見つめる。

「僕の美久への想いは、数値化できないから」

美久の瞳から、涙がこぼれた。

「ずるい……そんな言い方」

でも、笑っている。

「私も、Infinityだよ」

エレベーターで7階に着く。

今日は、部屋の前で少し話をした。

明日のこと、ゲームのこと、これからのこと。

「じゃあ、おやすみ」

「おやすみなさい」

それぞれの部屋に入る直前、美久が振り返った。

「隆弘くん」

「なに？」

「大好き」

昨日と同じ言葉。

でも、重みが違う。

「僕も大好きだよ、美久」

今度は、はっきりと伝えた。

オブジェクトで整理された想いは、確実に、正確に、相手に届く。

プログラミングが教えてくれた、大切なこと。

それは、想いを形にする方法。

そして、それを伝える勇気。