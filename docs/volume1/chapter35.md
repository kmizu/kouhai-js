# 第35話 受験準備の春

## 3月10日（日）午後2時

春の訪れを感じる季節。

でも、私たちには大きな山が待ってる。

大学受験。

「美久、この問題解ける？」

隆弘くんの部屋で、一緒に勉強中。

数学の問題集を見つめる。

◇◇◇◇

## 午後2時30分　勉強計画

「計画的にやろう」

隆弘くんがホワイトボードに書く。

```javascript
// 受験勉強計画
const examPreparation = {
    target: "東京工業大学",
    subjects: {
        math: {
            priority: "高",
            dailyHours: 3,
            topics: ["微積分", "線形代数", "確率統計"]
        },
        physics: {
            priority: "高",
            dailyHours: 2,
            topics: ["力学", "電磁気", "波動"]
        },
        english: {
            priority: "中",
            dailyHours: 1.5,
            topics: ["長文読解", "文法", "技術英語"]
        },
        programming: {
            priority: "並行して継続",
            dailyHours: 1,
            benefit: "論理的思考力の維持"
        }
    },
    
    schedule: {
        weekday: "学校 + 4時間",
        weekend: "8時間",
        breaks: "1時間ごとに10分"
    }
};

console.log("一緒に頑張ろう！");
```

◇◇◇◇

## 午後3時　数学の美しさ

「数学って、プログラミングに似てるね」

問題を解きながら気づく。

「そうだね。論理の積み重ね」

```javascript
// 数学とプログラミングの類似性
const mathAndProgramming = {
    similarities: [
        "論理的思考",
        "段階的な問題解決",
        "抽象化の概念",
        "パターン認識"
    ],
    
    example: {
        math: "f(x) = x² + 2x + 1",
        programming: "const f = (x) => x**2 + 2*x + 1"
    },
    
    recursion: {
        math: "フィボナッチ数列",
        programming: `
            const fib = (n) => 
                n <= 1 ? n : fib(n-1) + fib(n-2)
        `
    }
};
```

「プログラミング学んでてよかった」

「数学の理解も深まる」

◇◇◇◇

## 午後4時　休憩タイム

「ちょっと休憩しよう」

隆弘くんが紅茶を入れてくれる。

「疲れた？」

「ちょっとね。でも楽しい」

「隆弘くんと一緒だから」

照れくさそうに笑う隆弘くん。

◇◇◇◇

## 午後4時30分　物理の実験

「物理の実験、プログラムでシミュレーションしてみる？」

隆弘くんの提案。

```javascript
// 物理シミュレーション：自由落下
class FreeFallSimulation {
    constructor() {
        this.g = 9.8; // 重力加速度 (m/s²)
        this.position = 100; // 初期高さ (m)
        this.velocity = 0; // 初速度 (m/s)
        this.time = 0; // 経過時間 (s)
        this.dt = 0.01; // 時間刻み (s)
    }
    
    update() {
        this.velocity += this.g * this.dt;
        this.position -= this.velocity * this.dt;
        this.time += this.dt;
        
        if (this.position <= 0) {
            console.log(`着地！ 時間: ${this.time.toFixed(2)}秒`);
            return false;
        }
        return true;
    }
    
    run() {
        console.log("自由落下シミュレーション開始");
        while (this.update()) {
            if (this.time % 0.5 < this.dt) {
                console.log(`時間: ${this.time.toFixed(1)}s, 高さ: ${this.position.toFixed(1)}m`);
            }
        }
    }
}

const sim = new FreeFallSimulation();
sim.run();
```

「理論が目で見えるって面白い！」

「勉強も楽しくなるね」

◇◇◇◇

## 午後5時30分　英語の勉強

「技術文書読むの、いい練習になる」

GitHubのドキュメントを一緒に読む。

```javascript
// 技術英語の勉強法
const technicalEnglish = {
    resources: [
        "MDN Web Docs",
        "Stack Overflow",
        "技術ブログ",
        "APIドキュメント"
    ],
    
    vocabulary: {
        implementation: "実装",
        algorithm: "アルゴリズム",
        optimization: "最適化",
        abstraction: "抽象化",
        inheritance: "継承"
    },
    
    practice: {
        reading: "毎日1記事",
        writing: "コミットメッセージは英語で",
        speaking: "技術用語の発音練習"
    }
};
```

「実用的な英語が身につく」

「受験にも役立つし、将来も使える」

◇◇◇◇

## 午後6時　夕食休憩

「お腹すいた」

一緒にコンビニへ。

「おにぎりとサンドイッチ」

「栄養ドリンクも」

公園のベンチで食べる。

「受験、不安？」

隆弘くんが聞いてくる。

「正直、ある」

「でも、隆弘くんと同じ大学目指せるなら」

「頑張れる」

◇◇◇◇

## 午後7時　モチベーション管理

「モチベーション保つの大事だよね」

```javascript
// モチベーション管理システム
class MotivationManager {
    constructor(couple) {
        this.couple = couple;
        this.goals = [];
        this.achievements = [];
        this.supportMessages = [];
    }
    
    setGoal(goal) {
        this.goals.push({
            description: goal,
            deadline: new Date(),
            reward: "達成したらデート"
        });
    }
    
    achieve(achievement) {
        this.achievements.push({
            date: new Date(),
            description: achievement,
            celebratedWith: "ハイタッチ"
        });
        this.motivationBoost();
    }
    
    motivationBoost() {
        console.log("やったね！次も頑張ろう！");
        this.couple.forEach(person => {
            person.motivation += 10;
            person.confidence += 5;
        });
    }
    
    support(from, to, message) {
        this.supportMessages.push({
            from: from,
            to: to,
            message: message,
            effect: "元気100倍"
        });
    }
}

const ourMotivation = new MotivationManager(["美久", "隆弘"]);
ourMotivation.support("隆弘", "美久", "君ならできる！応援してる！");
ourMotivation.support("美久", "隆弘", "一緒に頑張ろう！大好き！");
```

◇◇◇◇

## 午後8時　将来の夢

勉強の合間に、将来の話。

「大学入ったら、何したい？」

「もっと高度なプログラミング学びたい」

「AI研究とか」

「私も！」

「あと、二人で起業もいいな」

夢が膨らむ。

```javascript
// 大学での目標
const universityGoals = {
    academic: [
        "コンピュータサイエンスの基礎固め",
        "AI・機械学習の研究",
        "OSやコンパイラの仕組み理解",
        "論文執筆"
    ],
    
    projects: [
        "オープンソース貢献",
        "新しいプログラミング言語開発",
        "社会問題解決アプリ",
        "起業準備"
    ],
    
    together: [
        "共同研究",
        "ペアプログラミング継続",
        "技術イベント参加",
        "世界を変える何かを作る"
    ]
};
```

◇◇◇◇

## 午後9時　今日の締めくくり

「今日も充実してた」

帰り支度をしながら。

「うん。勉強も楽しかった」

「それは隆弘くんと一緒だから」

駅まで送ってくれる隆弘くん。

「明日も頑張ろう」

「うん」

手を握って歩く。

春の夜風が心地いい。

◇◇◇◇

## 午後9時30分　帰宅後

家に帰って、今日の復習。

```javascript
// 今日の学習記録
const todaysStudy = {
    date: "2024-03-10",
    
    completed: {
        math: "微分方程式 10問",
        physics: "力学の演習 15問",
        english: "技術文書 3ページ",
        programming: "物理シミュレーション実装"
    },
    
    insights: [
        "数学とプログラミングの関連性",
        "シミュレーションで理解深まる",
        "一緒だと集中力UP"
    ],
    
    tomorrow: "継続は力なり",
    
    motivation: {
        level: 95,
        source: "隆弘くんの存在"
    }
};

studyLog.save(todaysStudy);
console.log("明日も頑張るぞ！");
```

ベッドに入る前に、隆弘くんにメッセージ。

『今日もありがとう。明日も一緒に頑張ろうね』

『こちらこそ。一緒に東工大行こう』

『うん！絶対！』

受験は大変。

でも、隆弘くんと一緒なら乗り越えられる。

プログラミングで培った論理的思考力。

二人で過ごす充実した時間。

全てが、私たちの力になる。

春は、新しい挑戦の季節。

一緒に、夢に向かって進もう。