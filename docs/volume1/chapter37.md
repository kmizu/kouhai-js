# 第37話 春のハッカソン

## 4月20日（土）午前9時

「学生ハッカソンに出よう」

隆弘くんの提案。

24時間で何かを作り上げるイベント。

「面白そう！」

「テーマは『未来の教育』だって」

CodeFirstの経験が活きるかも。

◇◇◇◇

## 午前10時　会場到着

都内の大きなイベントスペース。

全国から集まった高校生・大学生たち。

みんな目が輝いてる。

「チーム名は？」

「Forever Loopで」

私たちの定番。

```javascript
// ハッカソン参加登録
const teamRegistration = {
    teamName: "Forever Loop",
    members: [
        { name: "嵐山隆弘", role: "バックエンド・設計" },
        { name: "河内美久", role: "フロントエンド・UI/UX" }
    ],
    
    experience: [
        "WinterLang開発",
        "Connect Hearts（コンテスト優勝）",
        "CodeFirst（開発中）"
    ],
    
    motivation: "技術で教育を革新したい"
};
```

◇◇◇◇

## 午前11時　アイデア出し

「24時間で何を作る？」

ブレインストーミング開始。

```javascript
// アイデアリスト
const ideas = [
    {
        name: "AI家庭教師",
        description: "生徒の理解度に合わせて教え方を変えるAI",
        feasibility: 0.6
    },
    {
        name: "VR実験室",
        description: "危険な実験も安全に体験できるVR環境",
        feasibility: 0.4
    },
    {
        name: "ペアプログラミング学習",
        description: "リアルタイムで協力して学べる環境",
        feasibility: 0.9
    }
];

// 決定
const chosenIdea = {
    name: "StudyBuddy",
    concept: "AIが最適な学習パートナーをマッチング",
    features: [
        "学習スタイル診断",
        "相性の良いパートナー検索",
        "リアルタイム協同学習",
        "進捗の可視化"
    ]
};
```

◇◇◇◇

## 午後12時　開発開始

役割分担して、コーディング開始。

周りのチームも真剣そのもの。

```typescript
// StudyBuddyの基本設計
interface User {
    id: string;
    name: string;
    learningStyle: LearningStyle;
    subjects: Subject[];
    availability: TimeSlot[];
}

interface LearningStyle {
    pace: 'fast' | 'medium' | 'slow';
    preference: 'visual' | 'auditory' | 'kinesthetic';
    social: 'collaborative' | 'independent';
}

interface Match {
    user1: User;
    user2: User;
    compatibility: number;
    commonSubjects: Subject[];
    suggestedActivities: Activity[];
}

class MatchingAlgorithm {
    calculateCompatibility(user1: User, user2: User): number {
        let score = 0;
        
        // 学習ペースの相性
        if (user1.learningStyle.pace === user2.learningStyle.pace) {
            score += 30;
        }
        
        // 学習スタイルの補完性
        if (this.isComplementary(user1.learningStyle, user2.learningStyle)) {
            score += 40;
        }
        
        // 共通科目
        const commonSubjects = this.findCommonSubjects(user1, user2);
        score += commonSubjects.length * 10;
        
        return Math.min(score, 100);
    }
}
```

◇◇◇◇

## 午後3時　困難に直面

「マッチングアルゴリズムが思ったより複雑」

隆弘くんが頭を抱える。

「一緒に考えよう」

ペアプログラミングの本領発揮。

```javascript
// 改良版マッチングアルゴリズム
class ImprovedMatcher {
    match(users: User[]): Match[] {
        const matches = [];
        
        // 全組み合わせを評価
        for (let i = 0; i < users.length; i++) {
            for (let j = i + 1; j < users.length; j++) {
                const compatibility = this.evaluate(users[i], users[j]);
                
                if (compatibility > 70) {
                    matches.push({
                        users: [users[i], users[j]],
                        score: compatibility,
                        reasons: this.getMatchReasons(users[i], users[j])
                    });
                }
            }
        }
        
        // スコアで並び替え
        return matches.sort((a, b) => b.score - a.score);
    }
    
    private evaluate(user1: User, user2: User): number {
        const factors = {
            schedule: this.scheduleCompatibility(user1, user2) * 0.2,
            style: this.styleCompatibility(user1, user2) * 0.3,
            goals: this.goalAlignment(user1, user2) * 0.3,
            level: this.levelMatch(user1, user2) * 0.2
        };
        
        return Object.values(factors).reduce((sum, val) => sum + val, 0);
    }
}
```

◇◇◇◇

## 午後6時　夕食タイム

提供されたピザを食べながら、作戦会議。

「UI、もっと直感的にしたい」

「うん、初心者でも使えるように」

エネルギー補給して、後半戦へ。

◇◇◇◇

## 午後9時　深夜の追い込み

他のチームも必死。

でも、私たちには強みがある。

息の合ったペアプログラミング。

```javascript
// リアルタイム協同学習機能
class CollaborativeSession {
    constructor(roomId: string) {
        this.roomId = roomId;
        this.participants = [];
        this.sharedState = {
            code: '',
            notes: '',
            whiteboard: null
        };
    }
    
    // 画面共有
    shareScreen(userId: string) {
        this.broadcast('screen-share', {
            userId,
            stream: this.getUserStream(userId)
        });
    }
    
    // 同期的コード編集
    updateCode(changes: CodeChange) {
        this.sharedState.code = this.applyChanges(changes);
        this.broadcast('code-update', this.sharedState.code);
    }
    
    // 音声チャット
    enableVoiceChat() {
        return new VoiceChannel(this.roomId);
    }
    
    // 学習進捗の記録
    recordProgress(milestone: Milestone) {
        this.participants.forEach(user => {
            user.progress.add(milestone);
        });
    }
}
```

◇◇◇◇

## 午前2時　エネルギー切れ

「眠い...」

でも、まだやることがある。

「ちょっと休憩しよう」

隆弘くんが缶コーヒーを買ってきてくれる。

「ありがとう」

「一緒に頑張ろう」

手を握って、元気をもらう。

◇◇◇◇

## 午前5時　デモ準備

「あと4時間」

最後の仕上げ。

```javascript
// デモシナリオ
const demoScript = {
    introduction: "StudyBuddyは最適な学習パートナーを見つけるアプリです",
    
    flow: [
        {
            step: 1,
            action: "学習スタイル診断",
            duration: "30秒",
            points: ["簡単な質問に答えるだけ", "AIが分析"]
        },
        {
            step: 2,
            action: "マッチング",
            duration: "20秒",
            points: ["相性スコア表示", "マッチング理由の説明"]
        },
        {
            step: 3,
            action: "協同学習開始",
            duration: "40秒",
            points: ["リアルタイム画面共有", "音声通話", "進捗管理"]
        }
    ],
    
    closing: "StudyBuddyで、一人じゃない学習体験を"
};
```

◇◇◇◇

## 午前9時　プレゼンテーション

ついに発表の時間。

緊張する。

でも、隆弘くんが隣にいる。

「皆さん、こんにちは。チームForever Loopです」

堂々と発表する隆弘くん。

私もデモを担当。

24時間の成果を、3分間に凝縮。

◇◇◇◇

## 午前11時　結果発表

「優秀賞は...」

ドキドキ。

「チームForever Loop！」

やった！

「技術力と、実用性のバランスが素晴らしかった」

審査員からの講評。

◇◇◇◇

## 午後12時　振り返り

「疲れたけど、楽しかった」

帰り道、二人で話す。

```javascript
// ハッカソンの学び
const hackathonLearnings = {
    technical: [
        "時間制限下での設計判断",
        "MVPの重要性",
        "新技術への挑戦"
    ],
    
    teamwork: [
        "役割分担の大切さ",
        "コミュニケーション",
        "お互いの強みを活かす"
    ],
    
    personal: {
        confidence: "+100",
        experience: "貴重",
        motivation: "次も挑戦したい"
    },
    
    together: "最高のチーム"
};

console.log("また一つ、素敵な思い出ができた");
```

24時間、ほとんど寝ずに頑張った。

でも、不思議と充実感でいっぱい。

隆弘くんと一緒なら、どんな挑戦も楽しい。

これからも、二人で新しいことに挑戦していこう。

春の陽射しを浴びながら、そう思った。