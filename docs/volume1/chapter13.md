# 第13話 イベントリスナーと心の距離

## 10月17日（月）午前7時20分

昨日の夜のこと。

『隆弘くん、大好き』

私、本当にあんなこと言っちゃった。

布団の中で、顔を覆う。

（恥ずかしい……でも、後悔はしてない）

だって、本当の気持ちだから。

隆弘くんは、どう思ったかな。

困ったかな。それとも……。

スマホを確認するけど、特にメッセージはない。

いつも通り、ということかな。

◇◇◇◇

## 午前7時45分　マンション廊下

「おはよう、美久」

いつものように、隆弘くんが待っていた。

でも、なんだか照れたような表情。

「お、おはよう」

私も緊張してしまう。

昨日の告白のせいで、空気がぎこちない。

「今日も一緒に行こう」

「うん」

エレベーターの中、いつもより距離を感じる。

（やっぱり、困らせちゃったかな）

不安になる私。

でも、隆弘くんがそっと言った。

「昨日の……ありがとう」

え？

「俺も、美久のこと、大好きだよ」

小さな声だったけど、はっきりと聞こえた。

私の心臓が、跳ね上がる。

◇◇◇◇

## 午後4時30分　プログラミング部室

「今日は、イベントリスナーについて教えるね」

放課後、いつもの部室で。

隆弘くんとの距離は、なんだか昨日より近い気がする。

「イベントリスナー？」

「ユーザーの操作を検知して、それに反応する仕組みだよ」

隆弘くんが説明を始める。

```javascript
// ボタンをクリックしたときの処理
document.getElementById('startButton').addEventListener('click', function() {
    console.log('ゲームスタート！');
    startGame();
});

// マウスが要素の上に乗ったとき
document.getElementById('character').addEventListener('mouseover', function() {
    this.style.opacity = '0.8';
});
```

「なるほど！クリックとか、マウスの動きとかを感知するんですね」

「そう。これがあるから、インタラクティブなゲームが作れる」

隆弘くんの説明を聞きながら、ふと思う。

人の心にも、イベントリスナーがあればいいのに。

相手の気持ちの変化を、すぐに感知できたら。

◇◇◇◇

## 午後5時15分　実装練習

「じゃあ、実際に使ってみよう」

隆弘くんが新しいコードを書き始める。

```javascript
// キャラクターをクリックしたときの反応
const mikuCharacter = document.getElementById('miku');

mikuCharacter.addEventListener('click', function() {
    // クリックされた回数をカウント
    clickCount++;
    
    // クリック数に応じて反応を変える
    if (clickCount < 3) {
        showMessage('えっ、なに？');
    } else if (clickCount < 5) {
        showMessage('もう、しつこいよ〜');
    } else {
        showMessage('...実は嬉しい');
    }
});
```

「わあ、クリックするたびに反応が変わる！」

私が何度もクリックしていると、隆弘くんが笑った。

「美久みたいだね、この反応」

「え？」

「最初は驚いて、次に困って、でも本当は嬉しいって」

隆弘くんの言葉に、顔が熱くなる。

見透かされてる。

◇◇◇◇

## 午後6時　複雑なイベント処理

「もっと複雑なイベントも扱えるよ」

```javascript
// キーボード入力を検知
document.addEventListener('keydown', function(event) {
    if (event.key === 'Enter') {
        nextMessage();
    } else if (event.key === ' ') {
        skipAnimation();
    }
});

// ドラッグ＆ドロップ
let draggedItem = null;

document.addEventListener('dragstart', function(e) {
    draggedItem = e.target;
    e.target.style.opacity = '0.5';
});

document.addEventListener('dragend', function(e) {
    e.target.style.opacity = '';
});
```

「すごい！キーボードやドラッグも検知できるんだ」

「これで、より直感的な操作ができるゲームが作れる」

隆弘くんの横顔を見つめる。

真剣にコードを書く姿が、やっぱりかっこいい。

（好きだな）

改めて、そう思う。

◇◇◇◇

## 午後6時45分　特別な実装

「美久のために、特別な機能を作ってみた」

隆弘くんが、少し照れながら言う。

```javascript
// ハートのアニメーション
const heart = document.createElement('div');
heart.className = 'heart';
heart.innerHTML = '❤';

document.addEventListener('click', function(e) {
    const newHeart = heart.cloneNode(true);
    newHeart.style.left = e.clientX + 'px';
    newHeart.style.top = e.clientY + 'px';
    document.body.appendChild(newHeart);
    
    // アニメーション後に削除
    setTimeout(() => {
        newHeart.remove();
    }, 1000);
});

// CSSアニメーション
const style = document.createElement('style');
style.textContent = `
    .heart {
        position: absolute;
        color: #ff69b4;
        font-size: 20px;
        animation: float-up 1s ease-out;
        pointer-events: none;
    }
    
    @keyframes float-up {
        to {
            transform: translateY(-100px);
            opacity: 0;
        }
    }
`;
document.head.appendChild(style);
```

画面をクリックすると、ハートが浮かび上がる。

「わあ、可愛い！」

思わず、何度もクリックしてしまう。

画面中にピンクのハートが舞う。

「美久が喜ぶと思って」

隆弘くんの優しさに、胸が温かくなる。

◇◇◇◇

## 午後7時30分　今日の成果

「イベントリスナーを使えば、こんなこともできる」

隆弘くんが最後に見せてくれたのは、特別なプログラム。

```javascript
// 特別なメッセージシステム
let messageCount = 0;
const messages = [
    "プログラミングを教えられて嬉しい",
    "美久と一緒に作るゲームは特別",
    "毎日が楽しい",
    "これからもずっと一緒に",
    "大好き"
];

document.getElementById('special-button').addEventListener('click', function() {
    if (messageCount < messages.length) {
        showMessage(messages[messageCount]);
        messageCount++;
    } else {
        showMessage("♥");
    }
});
```

ボタンをクリックするたびに、隆弘くんからのメッセージが表示される。

最後まで読んで、涙が出そうになった。

「隆弘くん……」

「昨日の返事、ちゃんとしたくて」

真っ直ぐな瞳で見つめられる。

「俺も美久のこと、本当に大好きだ」

今度は、はっきりと、力強く。

私の目から、涙がこぼれた。

嬉しくて、嬉しくて。

◇◇◇◇

## 午後8時　帰り道

「泣かせるつもりじゃなかったんだけど」

隆弘くんが心配そうに言う。

「嬉し涙だから、大丈夫」

私は笑顔を見せる。

二人で並んで歩く帰り道。

手と手の距離が、今日は特に近い。

「ねえ、隆弘くん」

「ん？」

「イベントリスナーみたいに、お互いの気持ちの変化も感知できたらいいのにね」

私の言葉に、隆弘くんは優しく笑った。

「でも、言葉で伝え合えるから、人間なんじゃないかな」

「そうだね」

エレベーターで7階に着く。

今日は、名残惜しさがいつも以上に強い。

「明日も、よろしくね」

「うん、明日も」

それぞれの部屋に入る前、隆弘くんが振り返った。

「美久」

「なに？」

「今日のプログラム、本当の気持ちだから」

そう言って、照れたように部屋に入っていった。

私も自分の部屋に入り、ドアにもたれかかる。

胸がいっぱいで、言葉にならない。

イベントリスナーが、二人の心の距離を教えてくれた。

もう、隠す必要はない。

私たちは、お互いに「大好き」なんだから。

スマホが震えた。隆弘くんからのLINE。

『今日のコード、復習用に送るね。特別なやつも含めて』

『ありがとう。大切にする』

『おやすみ、美久』

『おやすみなさい、隆弘くん』

そして、最後にもう一言。

『大好き』

『俺も大好き』

画面を見つめながら、幸せな気持ちで眠りについた。

明日も、隆弘くんに会える。

それだけで、世界が輝いて見える。