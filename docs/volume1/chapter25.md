# 第25話 文化祭当日

## 11月3日（金）午前7時

ついに文化祭当日。

昨夜はドキドキして、あまり眠れなかった。

でも、不思議と元気。

今日は、私たちのゲームが世界に羽ばたく日。

「頑張るぞ！」

鏡の前で気合を入れる。

制服をきちんと着て、髪も丁寧にセット。

今日は特別な日だから。

◇◇◇◇

## 午前8時　学校到着

「おはよう、美久」

校門で隆弘くんが待っててくれた。

「おはよう」

「準備はバッチリ？」

「うん！」

二人で教室に向かう。

廊下は文化祭の準備で賑わってる。

「緊張する？」

「ちょっとだけ」

「大丈夫。僕たちのゲームは最高だから」

隆弘くんの自信に満ちた言葉に、勇気をもらう。

◇◇◇◇

## 午前9時　プログラミング部展示準備

部室に到着すると、部長たちがもう準備を始めてた。

「おお、主役が来た！」

部長が笑顔で迎えてくれる。

「今日はよろしくお願いします」

展示用のパソコンをセッティング。

```javascript
// 文化祭展示用の初期設定
function initializeExhibition() {
    // フルスクリーンモード
    document.documentElement.requestFullscreen();
    
    // 展示用メッセージを表示
    showWelcomeMessage();
    
    // デモモードを有効化
    if (!hasActivePlayer()) {
        startDemoMode();
    }
    
    // アナリティクス開始
    startAnalytics();
    
    // BGMボリュームを調整（騒がしい環境用）
    adjustVolumeForExhibition();
    
    console.log("文化祭展示モード起動完了！");
}

// 来場者検知
function hasActivePlayer() {
    let lastActivity = Date.now();
    
    document.addEventListener('click', () => {
        lastActivity = Date.now();
        if (isDemoMode) {
            stopDemoMode();
            console.log("プレイヤー検知！デモモード終了");
        }
    });
    
    // 5分間操作がなければデモモードに戻る
    setInterval(() => {
        if (Date.now() - lastActivity > 300000) {
            startDemoMode();
        }
    }, 10000);
}
```

「完璧だね」

隆弘くんが最終チェックをする。

◇◇◇◇

## 午前10時　文化祭開始

「いよいよだね」

開始のアナウンスが流れる。

最初のお客さんがやってきた。

「プログラミング部の展示ですか？」

「はい！私たちが作ったノベルゲームです」

緊張しながら説明する。

「へぇ、高校生が作ったの？すごいね」

お客さんが興味深そうにプレイし始める。

◇◇◇◇

## 午前11時　順調な滑り出し

```javascript
// リアルタイムアナリティクス
const exhibitionAnalytics = {
    visitors: 0,
    totalPlays: 0,
    completedRoutes: 0,
    feedback: [],
    popularChoices: {},
    
    // 訪問者をカウント
    countVisitor() {
        this.visitors++;
        updateDisplay();
    },
    
    // プレイ統計を更新
    updatePlayStats(stats) {
        this.totalPlays++;
        
        if (stats.completed) {
            this.completedRoutes++;
        }
        
        // 人気の選択肢を集計
        stats.choices.forEach(choice => {
            const key = `${choice.scene}_${choice.option}`;
            this.popularChoices[key] = (this.popularChoices[key] || 0) + 1;
        });
    },
    
    // フィードバックを記録
    addFeedback(message) {
        this.feedback.push({
            time: new Date(),
            message: message
        });
        
        console.log(`新しいフィードバック: ${message}`);
    },
    
    // 統計を表示
    displayStats() {
        console.log(`
            来場者数: ${this.visitors}人
            プレイ回数: ${this.totalPlays}回
            完走率: ${Math.round(this.completedRoutes / this.totalPlays * 100)}%
            フィードバック数: ${this.feedback.length}件
        `);
    }
};
```

「すごい！もう50人も来てる」

画面に表示される統計を見て驚く。

「評判いいみたいだよ」

隆弘くんが嬉しそうに言う。

◇◇◇◇

## 午後0時30分　お昼休憩

「ちょっと休憩しようか」

交代で昼食を取ることに。

屋上でお弁当を食べる。

「思ったより反響あってよかった」

「うん。みんな楽しんでくれてる」

「特に告白シーンの反応がいいね」

隆弘くんの言葉に照れる。

「あのシーン、力入れたから」

「分かるよ。美久の気持ちが込められてる」

◇◇◇◇

## 午後2時　特別な来場者

「あら、これが噂のゲーム？」

見覚えのある声。

振り返ると、生徒会長が立ってた。

「会長！」

「文化祭実行委員会でも話題になってるわよ」

会長がゲームをプレイし始める。

緊張しながら見守る。

「素敵ね。プログラミングって、こんなこともできるのね」

会長の言葉に、胸が熱くなる。

◇◇◇◇

## 午後3時30分　友達の反応

「美久ちゃん！これ作ったの！？」

クラスメイトたちが集まってきた。

「すごーい！」

「美久ちゃん、実はプログラマー？」

みんなの反応が嬉しい。

```javascript
// 友達からのコメントを表示
const friendsComments = [
    { name: "さくら", comment: "美久ちゃんの隠れた才能発見！" },
    { name: "ゆい", comment: "イラストも可愛いし、ストーリーも素敵" },
    { name: "あかり", comment: "私もプログラミング始めようかな" },
    { name: "みお", comment: "先輩との共同作業、羨ましい〜" }
];

friendsComments.forEach(friend => {
    console.log(`${friend.name}: ${friend.comment}`);
    exhibitionAnalytics.addFeedback(`${friend.name}: ${friend.comment}`);
});
```

「みんな、ありがとう」

友達に囲まれて、幸せを感じる。

◇◇◇◇

## 午後4時45分　終了間近

「そろそろ終了時間だね」

一日があっという間だった。

最終的な統計を確認する。

```javascript
// 最終統計
const finalStats = {
    visitors: 342,
    totalPlays: 127,
    completedRoutes: 89,
    feedback: 76,
    rating: 4.8 // 5点満点
};

console.log("=== 文化祭最終統計 ===");
console.log(`来場者数: ${finalStats.visitors}人`);
console.log(`総プレイ数: ${finalStats.totalPlays}回`);
console.log(`完走数: ${finalStats.completedRoutes}回`);
console.log(`完走率: ${Math.round(finalStats.completedRoutes / finalStats.totalPlays * 100)}%`);
console.log(`フィードバック: ${finalStats.feedback}件`);
console.log(`平均評価: ⭐${finalStats.rating}/5.0`);
console.log("==================");

// 特別なメッセージ
if (finalStats.rating >= 4.5) {
    console.log("🎉 大成功！素晴らしい作品でした！ 🎉");
}
```

「すごい数字……」

予想以上の反響に驚く。

◇◇◇◇

## 午後5時30分　片付け

展示を片付けながら、今日一日を振り返る。

「楽しかったね」

「うん。あっという間だった」

「みんなの笑顔が見れて良かった」

隆弘くんと一緒に機材を片付ける。

この達成感は、二人で分かち合うもの。

◇◇◇◇

## 午後6時　打ち上げ

プログラミング部のみんなで打ち上げ。

「今年の文化祭MVP は、間違いなく君たちだ！」

部長が乾杯の音頭を取る。

「ありがとうございます！」

みんなでジュースで乾杯。

「来年はもっとすごいの作ろうね」

「うん！」

◇◇◇◇

## 午後7時　二人きり

打ち上げが終わって、二人で帰路につく。

「今日は本当にお疲れ様」

「隆弘くんも」

夕暮れの校庭を歩く。

「ねえ、覚えてる？」

「何を？」

「最初にプログラミング教えてって言った日」

「もちろん」

「あの時は、こんな風になるなんて思ってなかった」

隆弘くんが優しく微笑む。

「僕も。でも、最高の結果になった」

「うん」

手を繋いで歩く。

◇◇◇◇

## 午後8時　約束の場所

「ちょっと寄り道しない？」

隆弘くんが提案する。

向かった先は、学校の屋上。

夜景がきれいに見える。

「美久」

「なに？」

「今日で一つの区切りだけど」

「うん」

「これからも、ずっと一緒にいてくれる？」

真剣な眼差し。

「もちろん」

即答だった。

「次は何を作る？」

「そうだな……もっと大きなゲーム？」

「Webアプリとか？」

「スマホアプリも面白そう」

二人で夢を語り合う。

プログラミングが繋いだ絆は、これからも続いていく。

「好きだよ、美久」

「私も好き」

星空の下、もう一度キスをした。

文化祭は終わったけど、私たちの物語は続く。

JavaScriptが紡ぐ、永遠のラブストーリー。