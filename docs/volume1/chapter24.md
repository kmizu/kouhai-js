# 第24話 完成前夜の輝き

## 11月2日（木）午前7時

文化祭前日。

今日でゲームを完成させなきゃ。

でも、不思議と焦りはない。

隆弘くんと一緒に作ってきたから。

きっと大丈夫。

「今日は遅くなるかも」

朝食を食べながら、お母さんに伝える。

「文化祭の準備？」

「うん。大事なゲームがあるの」

お母さんは優しく微笑んだ。

◇◇◇◇

## 午後4時　プログラミング部室

「いよいよ最終チェックだね」

部室に入ると、隆弘くんがもう準備してた。

机の上には、チェックリストが広げられてる。

「全部で30項目」

「多いね」

「でも、全部大事なこと」

```javascript
// 最終チェックリスト
const finalChecklist = [
    { id: 1, task: "全ルートのプレイテスト", completed: false },
    { id: 2, task: "セーブ/ロード機能の確認", completed: false },
    { id: 3, task: "BGM/SE の音量調整", completed: false },
    { id: 4, task: "画像の表示確認", completed: false },
    { id: 5, task: "エラーハンドリング", completed: false },
    { id: 6, task: "パフォーマンス最適化", completed: false },
    { id: 7, task: "隠し要素の実装", completed: false },
    // ... 他23項目
];

// チェック状況を表示
function displayChecklist() {
    const completed = finalChecklist.filter(item => item.completed).length;
    const total = finalChecklist.length;
    const percentage = Math.floor((completed / total) * 100);
    
    console.log(`完成度: ${percentage}% (${completed}/${total})`);
    
    // プログレスバー表示
    const progressBar = '█'.repeat(percentage / 5) + '░'.repeat(20 - percentage / 5);
    console.log(`[${progressBar}]`);
}
```

◇◇◇◇

## 午後4時30分　最終テストプレイ

「まず、全ルートを通しでプレイしてみよう」

隆弘くんがゲームを起動する。

タイトル画面が表示される。

『あざとい後輩女子にJavaScriptを教えて欲しいと言われたんだけど』

私たちが考えたタイトル。

「スタート！」

```javascript
// ゲームの統計情報
class GameStatistics {
    constructor() {
        this.startTime = new Date();
        this.choices = [];
        this.routesCompleted = new Set();
        this.cgUnlocked = new Set();
        this.totalPlayTime = 0;
    }
    
    // プレイ統計を記録
    recordChoice(sceneId, choiceIndex) {
        this.choices.push({
            scene: sceneId,
            choice: choiceIndex,
            timestamp: new Date()
        });
    }
    
    // ルート完了を記録
    completeRoute(routeName) {
        this.routesCompleted.add(routeName);
        
        // 全ルート制覇チェック
        if (this.routesCompleted.size === 4) { // True, Good, Normal, Bad
            this.unlockSpecialContent();
        }
    }
    
    // 特別なコンテンツを解放
    unlockSpecialContent() {
        console.log("全ルート制覇！特別なエピローグが解放されました");
        
        // 隠しCGを解放
        this.cgUnlocked.add('secret_epilogue');
        
        // 開発者コメンタリーを有効化
        gameSettings.developerCommentary = true;
    }
    
    // プレイレポートを生成
    generateReport() {
        const endTime = new Date();
        this.totalPlayTime = Math.floor((endTime - this.startTime) / 1000 / 60);
        
        return {
            playTime: `${this.totalPlayTime}分`,
            choicesCount: this.choices.length,
            routesCompleted: Array.from(this.routesCompleted),
            cgUnlocked: this.cgUnlocked.size,
            favoriteCharacter: this.analyzeFavorite()
        };
    }
    
    // お気に入りキャラクター分析
    analyzeFavorite() {
        const characterChoices = {};
        
        this.choices.forEach(choice => {
            // 選択肢からキャラクター関連の選択を抽出
            // ... 分析ロジック
        });
        
        return "美久"; // もちろん美久が一番！
    }
}
```

プレイしながら、細かい部分をチェックしていく。

◇◇◇◇

## 午後5時30分　バグ修正

「あ、ここおかしい」

美久ルートで、テキストが一瞬消える現象を発見。

「すぐ直すよ」

```javascript
// バグ修正前
function displayText(text) {
    messageBox.innerHTML = ''; // ここで一瞬空になる
    typeWriter(text, messageBox);
}

// バグ修正後
async function displayText(text) {
    // フェードアウト
    messageBox.style.opacity = '0';
    await wait(200);
    
    // テキストをクリアして新しいテキストを設定
    messageBox.innerHTML = '';
    
    // フェードイン
    messageBox.style.opacity = '1';
    await typeWriter(text, messageBox);
}

// より滑らかなトランジション
function smoothTextTransition(oldText, newText) {
    return new Promise(resolve => {
        // 古いテキストをフェードアウト
        const fadeOut = messageBox.animate([
            { opacity: 1 },
            { opacity: 0 }
        ], {
            duration: 300,
            fill: 'forwards'
        });
        
        fadeOut.onfinish = () => {
            messageBox.textContent = '';
            typeWriter(newText, messageBox).then(resolve);
        };
    });
}
```

「これで滑らかになった」

隆弘くんの手際の良さに、改めて感心する。

◇◇◇◇

## 午後6時15分　隠し要素の実装

「さて、約束の隠し要素を作ろう」

二人で顔を見合わせる。

少し照れくさい。

「私から作っていい？」

「どうぞ」

```javascript
// 美久の隠し要素
const secretRouteConditions = {
    // 特定の選択肢の組み合わせ
    requiredChoices: [
        { scene: 3, choice: 0 }, // "プログラミングは楽しい"を選択
        { scene: 7, choice: 1 }, // "美久と一緒なら"を選択
        { scene: 12, choice: 0 }, // "ずっと一緒に作りたい"を選択
    ],
    
    // 特定の行動
    requiredActions: [
        'save_at_confession_scene', // 告白シーンでセーブ
        'view_all_cg', // 全CGを閲覧
        'complete_game_once' // 一度クリア
    ],
    
    // 隠しコマンド
    secretCommand: 'mikuLove' // タイトル画面で入力
};

// 隠しルートのシナリオ
const secretScenario = {
    id: 'secret_miku_route',
    title: '永遠のパートナー',
    scenes: [
        {
            background: 'special/starry_night.jpg',
            bgm: 'secret/eternal_love.mp3',
            dialogue: [
                {
                    speaker: '美久',
                    text: 'ねえ、隆弘くん。このメッセージを見つけてくれてありがとう',
                    expression: 'special_smile'
                },
                {
                    speaker: '美久',
                    text: 'プログラミングを教えてくれて、本当にありがとう',
                    expression: 'grateful'
                },
                {
                    speaker: '美久',
                    text: 'でも、一番嬉しかったのは……',
                    expression: 'blush'
                },
                {
                    speaker: '美久',
                    text: '隆弘くんと一緒に過ごせた時間',
                    expression: 'love'
                },
                {
                    speaker: '美久',
                    text: 'これからも、ずっと一緒にいてください',
                    expression: 'tears_of_joy'
                },
                {
                    speaker: 'システム',
                    text: '～ True End : 永遠のパートナー ～'
                }
            ]
        }
    ]
};
```

「美久……」

隆弘くんが画面を見つめる。

その目が、少し潤んでるように見える。

◇◇◇◇

## 午後7時　隆弘の番

「じゃあ、僕も」

隆弘くんが自分の隠し要素を作り始める。

```javascript
// 隆弘の隠し要素
const takahiroSecretRoute = {
    // 発動条件：コナミコマンド風
    inputSequence: ['↑', '↑', '↓', '↓', '←', '→', '←', '→', 'B', 'A'],
    
    // または特別な日付
    specialDate: {
        month: 10,
        day: 28 // 初めてのデートの日
    },
    
    // 特別なメッセージ
    message: `
        美久へ
        
        このメッセージを見つけてくれてありがとう。
        
        君と出会えて、本当に良かった。
        プログラミングを通じて、君と繋がれたこと。
        一緒にゲームを作れたこと。
        全てが、僕の宝物だ。
        
        これは、ゲームの中の告白じゃない。
        現実の僕から、現実の君への想い。
        
        愛してる、美久。
        
        これからも、ずっと一緒に。
        新しいゲームを、新しい未来を、作っていこう。
        
        隆弘より
    `,
    
    // 特別な演出
    specialEffect: async function() {
        // 画面全体に星を降らせる
        for (let i = 0; i < 100; i++) {
            createStar();
            await wait(50);
        }
        
        // ハートのパーティクル
        createHeartParticles(50);
        
        // 特別なBGM
        playBGM('secret/our_melody.mp3');
        
        // エンドロール
        await showSpecialCredits();
    }
};

// 特別なエンドロール
async function showSpecialCredits() {
    const credits = [
        'プログラミング: 嵐山隆弘',
        'イラスト・シナリオ: 河内美久',
        '',
        'Special Thanks:',
        'このゲームをプレイしてくれた全ての人',
        'プログラミング部のみんな',
        '',
        'そして...',
        '',
        '世界で一番大切な人',
        '河内美久へ',
        '',
        'このゲームを、君に捧げます'
    ];
    
    for (let credit of credits) {
        await displayCreditLine(credit);
        await wait(2000);
    }
}
```

読んでいて、涙が止まらない。

「隆弘くん……」

「恥ずかしいから、あんまり見ないで」

隆弘くんが照れくさそうに言う。

でも、その頬も赤い。

◇◇◇◇

## 午後8時　最後の仕上げ

「あと少しで完成だ」

チェックリストを確認する。

```javascript
displayChecklist();
// 完成度: 93% (28/30)
// [██████████████████░░]
```

残りは、最終的な動作確認と、展示用の準備だけ。

「ねえ、記念写真撮ろう」

「え？」

「完成記念」

スマホを取り出して、二人で画面の前に並ぶ。

ゲームのタイトル画面をバックに。

「はい、チーズ！」

カシャッ。

二人の笑顔が、写真に収まった。

◇◇◇◇

## 午後9時　完成

「完成！」

最後のチェックが終わった。

```javascript
// 完成度: 100% (30/30)
// [████████████████████]

console.log("🎉 ゲーム完成おめでとう！ 🎉");

// ゲームの統計
const gameStats = {
    totalLines: 12847,
    jsFiles: 23,
    cssFiles: 8,
    htmlFiles: 5,
    images: 156,
    sounds: 42,
    developmentDays: 30,
    developers: 2,
    love: Infinity
};

console.log("開発統計:", gameStats);
```

「やった……本当に完成した」

二人で顔を見合わせる。

達成感と、少しの寂しさ。

「美久」

「なに？」

「ありがとう」

「私の方こそ」

自然に、抱き合っていた。

温かい。

この温もりも、ゲームに込められている。

◇◇◇◇

## 午後9時30分　明日への準備

「明日の展示準備もしておこう」

文化祭当日のことを考える。

```javascript
// 展示用設定
const exhibitionConfig = {
    // デモモード（自動プレイ）
    demoMode: true,
    demoInterval: 30000, // 30秒ごとに自動進行
    
    // キオスクモード（全画面固定）
    kioskMode: true,
    
    // プレイ統計を記録
    analytics: true,
    
    // 感想入力フォーム
    feedbackEnabled: true,
    
    // QRコード表示（持ち帰り用）
    qrCode: 'https://example.com/our-game'
};

// 来場者へのメッセージ
const welcomeMessage = `
    ようこそ！
    
    このゲームは、プログラミング部の
    2年生と1年生が協力して作りました。
    
    プログラミング初心者の方でも
    楽しく遊べる内容になっています。
    
    ぜひ、最後までプレイしてみてください！
    
    制作：嵐山隆弘 & 河内美久
`;
```

「これで準備万端」

「うん」

◇◇◇◇

## 午後10時　帰り道

「遅くなっちゃった」

マンションへの帰り道。

手を繋いで歩く。

「でも、間に合って良かった」

「うん。隆弘くんのおかげ」

「美久がいたからだよ」

明日は文化祭。

みんなに遊んでもらえるかな。

不安もあるけど、楽しみの方が大きい。

「ねえ、隆弘くん」

「ん？」

「明日、一緒に展示番しよう」

「もちろん」

「みんなの反応、一緒に見たい」

「僕も」

エレベーターで7階へ。

今日は特別に、もう少し一緒にいたい。

「お茶でも飲んでく？」

隆弘くんが提案する。

「うん」

◇◇◇◇

## 午後11時　約束

隆弘くんの部屋で、ゆっくりお茶を飲む。

完成したゲームの話。

明日の文化祭の話。

そして、これからの話。

「文化祭が終わっても、また一緒に作ろうね」

「うん。今度はもっとすごいの」

「楽しみ」

時計を見ると、もう11時過ぎ。

「そろそろ帰らなきゃ」

「送るよ」

「隣の部屋なのに」

「それでも」

ドアの前で、もう一度ハグ。

「明日、頑張ろうね」

「うん」

「おやすみ、美久」

「おやすみなさい」

部屋に戻って、ベッドに倒れ込む。

明日は、私たちのゲームデビューの日。

ドキドキして眠れそうにない。

でも、幸せ。

隆弘くんと作ったゲーム。

きっと、みんなに愛されるといいな。

そう願いながら、目を閉じた。