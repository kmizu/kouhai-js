# 第27話 次のステージへ

## 11月10日（金）午後4時

文化祭から一週間。

日常が戻ってきたけど、何かが確実に変わってた。

「美久、今日も部活？」

クラスメイトが聞いてくる。

「うん。新しいプロジェクト始めるの」

「へぇ、今度は何作るの？」

「Webアプリ！」

胸を張って答える。

もう、隠す必要はない。

私はプログラマーで、隆弘くんの恋人。

◇◇◇◇

## 午後4時30分　プログラミング部室

「新しい技術の勉強始めようか」

隆弘くんがホワイトボードに書く。

『React入門』

「React？」

「モダンなWebアプリを作るためのフレームワーク」

```javascript
// Reactの基本
import React from 'react';
import ReactDOM from 'react-dom';

// 最初のコンポーネント
function App() {
    return (
        <div>
            <h1>Hello, React!</h1>
            <p>美久と隆弘の新しい冒険</p>
        </div>
    );
}

// レンダリング
ReactDOM.render(<App />, document.getElementById('root'));
```

「なんか、今までと違う書き方」

「そう。でも基本は同じ。JavaScriptだから」

新しい挑戦にワクワクする。

◇◇◇◇

## 午後5時　コンポーネント思考

「Reactは、コンポーネントという単位で考えるんだ」

```javascript
// コンポーネントの例
function Header({ title }) {
    return (
        <header>
            <h1>{title}</h1>
        </header>
    );
}

function MemoryCard({ memory }) {
    return (
        <div className="memory-card">
            <h3>{memory.title}</h3>
            <p>{memory.date}</p>
            <p>{memory.description}</p>
            {memory.image && <img src={memory.image} alt={memory.title} />}
        </div>
    );
}

function MemoryList({ memories }) {
    return (
        <div className="memory-list">
            {memories.map(memory => (
                <MemoryCard key={memory.id} memory={memory} />
            ))}
        </div>
    );
}
```

「部品を組み合わせて作るんだね」

「そう。レゴブロックみたいに」

隆弘くんの説明は、いつも分かりやすい。

◇◇◇◇

## 午後5時45分　状態管理

「アプリには『状態』がある」

```javascript
import React, { useState } from 'react';

function MemoryApp() {
    // 状態の定義
    const [memories, setMemories] = useState([]);
    const [newMemory, setNewMemory] = useState({
        title: '',
        description: '',
        date: new Date()
    });
    
    // 思い出を追加
    const addMemory = () => {
        if (newMemory.title && newMemory.description) {
            setMemories([...memories, {
                ...newMemory,
                id: Date.now()
            }]);
            
            // フォームをリセット
            setNewMemory({
                title: '',
                description: '',
                date: new Date()
            });
        }
    };
    
    return (
        <div className="memory-app">
            <h1>Memory Keeper</h1>
            
            <div className="add-memory">
                <input
                    type="text"
                    placeholder="タイトル"
                    value={newMemory.title}
                    onChange={(e) => setNewMemory({
                        ...newMemory,
                        title: e.target.value
                    })}
                />
                
                <textarea
                    placeholder="思い出を書いてください"
                    value={newMemory.description}
                    onChange={(e) => setNewMemory({
                        ...newMemory,
                        description: e.target.value
                    })}
                />
                
                <button onClick={addMemory}>
                    思い出を保存
                </button>
            </div>
            
            <MemoryList memories={memories} />
        </div>
    );
}
```

「状態が変わると、画面も自動で更新される」

「便利！」

でも、まだ難しい部分もある。

隆弘くんと一緒なら、きっと理解できる。

◇◇◇◇

## 午後6時30分　共同開発の計画

「役割分担しよう」

隆弘くんが提案する。

```javascript
// プロジェクト構成
const projectStructure = {
    name: "Memory Keeper",
    
    frontend: {
        developer: "美久",
        technologies: ["React", "CSS", "デザイン"],
        responsibilities: [
            "UI/UXデザイン",
            "コンポーネント実装",
            "スタイリング",
            "アニメーション"
        ]
    },
    
    backend: {
        developer: "隆弘",
        technologies: ["Node.js", "Express", "MongoDB"],
        responsibilities: [
            "API設計",
            "データベース設計",
            "認証システム",
            "セキュリティ"
        ]
    },
    
    collaboration: {
        tools: ["Git", "GitHub", "Discord"],
        meetingSchedule: "毎日放課後",
        deadline: "2ヶ月後",
        goal: "二人の思い出を永遠に残すアプリ"
    }
};
```

「私、デザイン頑張る！」

「期待してる」

◇◇◇◇

## 午後7時15分　Git入門

「共同開発にはGitが必須」

```bash
# Gitの基本コマンド
git init  # リポジトリを初期化
git add . # 変更をステージング
git commit -m "初回コミット" # コミット
git push origin main # リモートにプッシュ

# ブランチを使った開発
git checkout -b feature/add-memory-form
# 機能を実装...
git add .
git commit -m "思い出追加フォームを実装"
git push origin feature/add-memory-form

# プルリクエストを作成してマージ
```

「なんか本格的」

「プロの開発者と同じ方法だよ」

隆弘くんが誇らしそうに言う。

私たちも、本物の開発者になれるかな。

◇◇◇◇

## 午後8時　将来の話

「美久は、将来どうしたい？」

部活が終わって、二人で帰り道。

突然の質問に戸惑う。

「うーん……」

「プログラマーになりたい？」

「それも素敵だけど……」

考えながら答える。

「隆弘くんと一緒に、何か作り続けたい」

「仕事として？」

「仕事でも、趣味でも」

隆弘くんが優しく微笑む。

「僕も同じこと考えてた」

◇◇◇◇

## 午後8時30分　大学の話

「大学、どうする？」

現実的な話題。

でも、避けては通れない。

「情報系の学部がいいかな」

「僕もそう思ってた」

「同じ大学行けたらいいね」

「うん」

手を繋いで歩きながら、未来を語る。

```javascript
// 私たちの未来計画
const futurePlan = {
    highSchool: {
        remaining: "1年半",
        goals: [
            "プログラミングスキル向上",
            "ポートフォリオ作成",
            "大学受験準備"
        ]
    },
    
    university: {
        target: "情報工学部",
        dream: "同じ大学",
        subjects: [
            "コンピュータサイエンス",
            "ソフトウェア工学",
            "人工知能",
            "Webシステム"
        ]
    },
    
    career: {
        options: [
            "スタートアップ創業",
            "フリーランス",
            "IT企業就職",
            "研究者"
        ],
        requirement: "隆弘と一緒"
    },
    
    life: {
        partnership: "永遠",
        projects: "無限",
        happiness: "最大"
    }
};
```

◇◇◇◇

## 午後9時　新しい挑戦

「来月、プログラミングコンテストがあるんだ」

隆弘くんが教えてくれる。

「コンテスト？」

「U-18の全国大会。出てみない？」

「え、でも……」

「二人でチーム組めるよ」

迷う。

でも、隆弘くんとなら。

「やってみる」

「よし！」

新しい挑戦が始まる。

◇◇◇◇

## 午後9時30分　帰宅

「今日もありがとう」

マンションのエントランスで。

「こちらこそ」

「React、難しいけど楽しい」

「美久なら、すぐマスターできるよ」

「隆弘くんが教えてくれるから」

照れ笑いを交わす。

「じゃあ、また明日」

「うん」

エレベーターで別れる前に、軽くキス。

もう日常になった、愛情表現。

◇◇◇◇

## 午後10時　部屋で復習

```javascript
// 今日学んだこと
const todayILearned = {
    React: {
        concepts: ["コンポーネント", "状態管理", "Props"],
        understood: 0.7,
        needsMoreStudy: true
    },
    
    Git: {
        commands: ["init", "add", "commit", "push"],
        understood: 0.6,
        practice: "必要"
    },
    
    future: {
        university: "決意",
        career: "希望",
        withTakahiro: "確信"
    },
    
    feelings: {
        excitement: 100,
        anxiety: 30,
        love: Infinity
    }
};

console.log("今日も充実した一日だった");
console.log("明日も頑張ろう");
console.log("隆弘くん、大好き");
```

ノートPCを閉じて、ベッドに入る。

文化祭は終わったけど、私たちの物語は続く。

新しい技術、新しい挑戦、新しい未来。

全部、隆弘くんと一緒に。

それが、私の幸せ。

プログラミングが教えてくれた、大切なこと。

それは、一人じゃできないことも、二人ならできるということ。

そして、愛する人と同じ夢を見られる幸せ。

明日も、きっと素敵な一日になる。

そう信じて、眠りについた。