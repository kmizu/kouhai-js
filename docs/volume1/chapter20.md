# 第20話 ループで紡ぐ物語

## 10月29日（日）午前10時

昨日のデート。

初めてのキス。

恋人になった私たち。

でも、今日はゲーム制作に集中しなきゃ。

「おはよう、美久」

隆弘くんの部屋に入ると、もうパソコンの前に座ってた。

「おはよう」

昨日と違って、今日は作業着。

でも、恋人として過ごす初めての作業日。

ドキドキする。

◇◇◇◇

## 午前10時30分　新しい機能の実装

「今日は、ループ処理について教えるよ」

隆弘くんがホワイトボードに図を描く。

「ループ？」

「同じ処理を繰り返す仕組み。for文って言うんだ」

```javascript
// 基本的なfor文
for (let i = 0; i < 5; i++) {
    console.log(`${i + 1}回目のループ`);
}

// 1回目のループ
// 2回目のループ
// 3回目のループ
// 4回目のループ
// 5回目のループ
```

「なるほど！同じことを何度も書かなくていいんだ」

「そう。ゲームでも便利だよ」

隆弘くんが新しいコードを書き始める。

◇◇◇◇

## 午前11時15分　セーブスロットの実装

「セーブデータを複数作れるようにしよう」

```javascript
// セーブスロットの管理
const saveSlots = [];
const maxSlots = 5;

// 全スロットの初期化
for (let i = 0; i < maxSlots; i++) {
    saveSlots[i] = {
        slotNumber: i + 1,
        isEmpty: true,
        data: null,
        saveDate: null
    };
}

// セーブ機能
function saveToSlot(slotNumber) {
    const gameData = {
        scene: currentScene,
        choices: playerChoices,
        affection: characterAffection,
        timestamp: new Date()
    };
    
    saveSlots[slotNumber - 1] = {
        slotNumber: slotNumber,
        isEmpty: false,
        data: gameData,
        saveDate: new Date().toLocaleString()
    };
    
    console.log(`スロット${slotNumber}にセーブしました`);
}
```

「5つもセーブできるの？」

「うん。ループを使えば、100個でも1000個でも作れる」

隆弘くんの説明を聞きながら、コードを理解していく。

プログラミングって、本当に便利。

◇◇◇◇

## 午後0時　CG回想モードの作成

「美久の描いたCGを見返せる機能も作ろう」

隆弘くんが提案する。

「わあ、嬉しい！」

```javascript
// CG回想システム
const cgGallery = [
    { id: "cg001", title: "初めての出会い", unlocked: false, file: "opening.jpg" },
    { id: "cg002", title: "プログラミングレッスン", unlocked: false, file: "lesson.jpg" },
    { id: "cg003", title: "深夜のデバッグ", unlocked: false, file: "debug.jpg" },
    { id: "cg004", title: "告白", unlocked: false, file: "confession.jpg" },
    { id: "cg005", title: "初めてのキス", unlocked: false, file: "first_kiss.jpg" }
];

// CGを解放する関数
function unlockCG(cgId) {
    for (let i = 0; i < cgGallery.length; i++) {
        if (cgGallery[i].id === cgId) {
            cgGallery[i].unlocked = true;
            console.log(`CG「${cgGallery[i].title}」が解放されました！`);
            break;
        }
    }
}

// 解放済みCGを表示
function displayUnlockedCGs() {
    const unlockedCGs = [];
    
    for (let cg of cgGallery) {
        if (cg.unlocked) {
            unlockedCGs.push(cg);
        }
    }
    
    return unlockedCGs;
}
```

「初めてのキスって……」

顔が赤くなる。

「昨日のこと、CGにする？」

隆弘くんがニヤリと笑う。

「も、もう！」

でも、嬉しい。

◇◇◇◇

## 午後1時30分　ランチ休憩

「お腹すいた」

「じゃあ、休憩にしよう」

隆弘くんがカップラーメンを作ってくれる。

二人で向かい合って食べる。

「ねえ、隆弘くん」

「ん？」

「昨日の続き、聞いてもいい？」

「何の？」

「プログラミング始めた理由」

隆弘くんが箸を止める。

「実はね、美久が描いた絵を見て思ったんだ」

「私の絵？」

「小学校の時、美久が描いてくれたゲームのキャラクター、覚えてる？」

思い出す。

確かに、ゲームのキャラクターを描いて、隆弘くんに見せたことがある。

「あの時、『このキャラクターを動かせたらいいのに』って言ったよね」

「そんなこと言ったかな……」

「言った。だから、動かせるようになりたくて」

隆弘くんの優しい眼差し。

ずっと、私のことを考えてくれてたんだ。

◇◇◇◇

## 午後2時30分　複雑なループ処理

「もっと複雑なループも見てみよう」

```javascript
// 二重ループでマップを生成
const mapWidth = 5;
const mapHeight = 5;
const gameMap = [];

for (let y = 0; y < mapHeight; y++) {
    gameMap[y] = [];
    for (let x = 0; x < mapWidth; x++) {
        gameMap[y][x] = {
            x: x,
            y: y,
            type: "grass",
            event: null
        };
    }
}

// 特定の場所にイベントを配置
gameMap[2][2].event = {
    type: "treasure",
    item: "幸せの鍵"
};

// マップ全体をチェック
for (let row of gameMap) {
    for (let cell of row) {
        if (cell.event) {
            console.log(`(${cell.x}, ${cell.y})に${cell.event.item}があります`);
        }
    }
}
```

「二重ループ？」

「ループの中にループがあるんだ」

難しそうだけど、隆弘くんの説明は分かりやすい。

「幸せの鍵って、ロマンチック」

「美久と見つけた鍵だから」

また照れちゃう。

◇◇◇◇

## 午後4時　エンディングリストの実装

「全てのエンディングを記録する機能も作ろう」

```javascript
// エンディングリスト
const endings = [
    { id: 1, name: "True End", achieved: false, condition: "美久との愛を貫く" },
    { id: 2, name: "Good End", achieved: false, condition: "みんなと仲良く" },
    { id: 3, name: "Normal End", achieved: false, condition: "普通の日常" },
    { id: 4, name: "Bad End", achieved: false, condition: "すれ違いの果てに" }
];

// エンディング到達時の処理
function achieveEnding(endingId) {
    for (let ending of endings) {
        if (ending.id === endingId) {
            ending.achieved = true;
            displayEndingAchievement(ending);
            break;
        }
    }
    
    // 全エンディング達成チェック
    checkAllEndingsAchieved();
}

// 全エンディング達成チェック
function checkAllEndingsAchieved() {
    let achievedCount = 0;
    
    for (let ending of endings) {
        if (ending.achieved) {
            achievedCount++;
        }
    }
    
    if (achievedCount === endings.length) {
        unlockSpecialContent();
    }
}

// 特別なコンテンツを解放
function unlockSpecialContent() {
    console.log("全てのエンディングを見ました！");
    console.log("特別なエピローグが解放されました");
    
    // 開発者からのメッセージ
    const message = `
        このゲームをプレイしてくれてありがとう。
        プログラミングと恋愛、どちらも大切。
        あなたにも素敵な出会いがありますように。
        
        - 隆弘 & 美久
    `;
}
```

「Bad Endなんて作りたくない」

私がつぶやくと、隆弘くんが優しく笑った。

「現実では、ずっとTrue Endだから」

「うん」

◇◇◇◇

## 午後5時30分　最後の仕上げ

「ループを使って、スタッフロールも作ろう」

```javascript
// スタッフロール
const staffCredits = [
    { role: "プログラマー", name: "嵐山隆弘" },
    { role: "イラストレーター", name: "河内美久" },
    { role: "シナリオライター", name: "河内美久" },
    { role: "BGM", name: "フリー素材より" },
    { role: "スペシャルサンクス", name: "プログラミング部のみんな" },
    { role: "そして", name: "プレイしてくれたあなたへ" }
];

// スタッフロールを流す
function playStaffRoll() {
    let delay = 0;
    
    for (let credit of staffCredits) {
        setTimeout(() => {
            displayCredit(credit.role, credit.name);
        }, delay);
        
        delay += 2000; // 2秒ずつ遅らせる
    }
    
    // 最後にメッセージ
    setTimeout(() => {
        displayFinalMessage();
    }, delay + 3000);
}

function displayFinalMessage() {
    const message = "Thank you for playing!";
    const subMessage = "~ Fin ~";
    
    console.log(message);
    console.log(subMessage);
}
```

「私の名前が2回も」

「美久がいなければ、このゲームは作れなかった」

隆弘くんの言葉が、心に染みる。

◇◇◇◇

## 午後6時45分　完成間近

「ループ処理、理解できた？」

「うん。繰り返しって、人生みたい」

私の言葉に、隆弘くんが首を傾げる。

「どういうこと？」

「毎日会って、毎日好きになって、その繰り返し」

恥ずかしいけど、本心。

隆弘くんが立ち上がって、私の隣に来た。

「美久」

「なに？」

「ありがとう」

突然、抱きしめられた。

「きゃっ」

「美久と一緒にゲームを作れて、本当に良かった」

温かい腕の中で、幸せを感じる。

「私も。隆弘くんと出会えて良かった」

◇◇◇◇

## 午後7時30分　約束

「文化祭まであと5日」

「間に合いそう？」

「うん。二人なら大丈夫」

隆弘くんの自信に満ちた言葉。

「ねえ、約束して」

「なに？」

「文化祭が終わっても、一緒にゲーム作ろう」

私の提案に、隆弘くんが微笑む。

「もちろん。次はもっとすごいの作ろう」

「うん！」

手を繋いで、誓い合う。

これからも、ずっと一緒に。

プログラミングで繋がった私たち。

ループのように、何度でも愛を確かめ合いながら。

「じゃあ、今日はここまで」

「お疲れ様」

部屋を出る前に、もう一度キス。

昨日より、自然に。

「明日も頑張ろうね」

「うん」

恋人として、パートナーとして。

私たちの物語は、これからも続いていく。

無限ループのように、ずっと、ずっと。