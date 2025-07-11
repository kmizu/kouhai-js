# 第36話 新プロジェクト始動

## 4月10日（水）午後3時30分

新学期が始まって数日。

東京工業大学の新1年生として、新しい生活に慣れ始めた頃。

「美久、今日の講義どうだった？」

大学の中庭で、隆弘くんと待ち合わせ。

桜はもう散って、新緑が眩しい。

「AI論の基礎、面白かった！」

「僕も受けたかったな」

◇◇◇◇

## 午後4時　新しい挑戦

「実は、新しいアイデアがあるんだ」

隆弘くんが切り出す。

「受験勉強で大変だったでしょ？」

「うん。特に時間管理とか」

「AI使った学習支援アプリ、作らない？」

目が輝く隆弘くん。

その情熱に、私も引き込まれる。

```javascript
// AI学習支援アプリ - StudyBuddy
const studyBuddySpec = {
    name: "StudyBuddy",
    concept: "受験生のためのAI学習パートナー",
    
    features: [
        {
            name: "問題推薦",
            description: "個人の弱点に合わせた問題選択",
            tech: ["機械学習", "協調フィルタリング"]
        },
        {
            name: "進捗管理",
            description: "最適な学習ペースを提案",
            tech: ["データ分析", "可視化"]
        },
        {
            name: "学習計画",
            description: "志望校に向けた計画生成",
            tech: ["最適化アルゴリズム", "スケジューリング"]
        },
        {
            name: "モチベーション維持",
            description: "励ましメッセージと成果表示",
            tech: ["感情分析", "ゲーミフィケーション"]
        }
    ],
    
    targetUsers: "高校生",
    
    motivation: "自分たちの経験を活かしたい"
};
```

「すごい案！」

◇◇◇◇

## 午後4時30分　技術選定

「まず、技術スタックを決めよう」

隆弘くんがホワイトボードに書き始める。

```javascript
// 技術スタック
const techStack = {
    frontend: {
        framework: "React + TypeScript",
        ui: "Material-UI",
        state: "Redux Toolkit",
        charts: "Chart.js"
    },
    
    backend: {
        runtime: "Node.js",
        framework: "Express",
        database: "PostgreSQL",
        orm: "Prisma"
    },
    
    ai: {
        framework: "TensorFlow.js",
        nlp: "Natural",
        recommendation: "協調フィルタリング"
    },
    
    infrastructure: {
        hosting: "Vercel + Heroku",
        storage: "AWS S3",
        ci_cd: "GitHub Actions"
    },
    
    language: {
        primary: "TypeScript",
        secondary: "Python (AI部分)"
    }
};
```

「TypeScript使いたかったんだ」

「型があると安心だよね」

「一緒に勉強しよう」

◇◇◇◇

## 午後5時　基本設計

「まずは基本的な設計から」

二人でカフェに移動。

```typescript
// StudyBuddy 基本設計
interface User {
    id: string;
    name: string;
    grade: number;
    targetUniversity: string;
    subjects: Subject[];
}

interface Subject {
    name: string;
    currentLevel: number;  // 現在の実力
    targetLevel: number;
    weakPoints: string[];
    studyHours: number;
}

interface StudyPlan {
    userId: string;
    dailyTasks: Task[];
    weeklyGoals: Goal[];
    monthlyMilestones: Milestone[];
}

class StudyBuddyAI {
    private userModel: UserModel;
    private recommendationEngine: RecommendationEngine;
    
    constructor() {
        this.userModel = new UserModel();
        this.recommendationEngine = new RecommendationEngine();
    }
    
    async analyzePerformance(userId: string, testResults: TestResult[]) {
        const analysis = {
            strengths: [],
            weaknesses: [],
            trends: [],
            recommendations: []
        };
        
        // 正答率の分析
        const accuracy = this.calculateAccuracy(testResults);
        
        // 時間効率の分析
        const timeEfficiency = this.analyzeTimeUsage(testResults);
        
        // 弱点の特定
        const weakPoints = this.identifyWeakPoints(testResults);
        
        // 改善提案の生成
        const improvements = await this.generateImprovements(
            accuracy,
            timeEfficiency,
            weakPoints
        );
        
        return {
            ...analysis,
            accuracy,
            timeEfficiency,
            weakPoints,
            improvements
        };
    }
    
    async generateOptimalPlan(user: User): Promise<StudyPlan> {
        // 現在の実力を分析
        const currentStatus = await this.analyzeCurrentStatus(user);
        
        // 目標とのギャップ
        const gaps = this.calculateGaps(currentStatus, user.targetUniversity);
        
        // 最適な計画生成
        const plan = this.optimizePlan(gaps, user.subjects);
        
        return plan;
    }
}
```

「AIが学習計画を立ててくれるなんて」

「僕たちも欲しかったよね」

◇◇◇◇

## 午後5時45分　UI設計

「UIも大事だよね」

美久がFigmaを開く。

「使いやすくて、見やすくて」

```javascript
// UIコンポーネント設計
const uiComponents = {
    dashboard: {
        layout: "グリッドレイアウト",
        widgets: [
            {
                name: "今日のタスク",
                type: "TaskList",
                priority: "high"
            },
            {
                name: "学習時間グラフ",
                type: "TimeChart",
                data: "週間/月間"
            },
            {
                name: "科目別達成度",
                type: "RadarChart",
                subjects: ["数学", "物理", "化学", "英語"]
            },
            {
                name: "モチベーションメーター",
                type: "ProgressBar",
                gamification: true
            }
        ]
    },
    
    studySession: {
        timer: "ポモドーロタイマー",
        problemDisplay: "問題表示エリア",
        answerInput: "解答入力フォーム",
        hintButton: "ヒント表示",
        submitButton: "提出"
    },
    
    analytics: {
        charts: [
            "成績推移グラフ",
            "時間配分円グラフ",
            "弱点ヒートマップ",
            "予測スコア"
        ]
    }
};
```

「かわいいデザインにしたい」

「でも、シンプルで集中できるように」

「隆弘くんらしいね」

◇◇◇◇

## 午後6時30分　データベース設計

「データ構造も重要」

一緒にERDを描き始める。

```sql
-- StudyBuddy データベース設計
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL,
    grade INTEGER NOT NULL,
    target_university VARCHAR(200),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(50) NOT NULL,
    category VARCHAR(50) NOT NULL
);

CREATE TABLE user_subjects (
    user_id UUID REFERENCES users(id),
    subject_id UUID REFERENCES subjects(id),
    current_level DECIMAL(3,1),
    target_level DECIMAL(3,1),
    study_hours INTEGER DEFAULT 0,
    PRIMARY KEY (user_id, subject_id)
);

CREATE TABLE study_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    subject_id UUID REFERENCES subjects(id),
    start_time TIMESTAMP NOT NULL,
    end_time TIMESTAMP,
    efficiency_score DECIMAL(3,2),
    notes TEXT
);

CREATE TABLE problems (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_id UUID REFERENCES subjects(id),
    difficulty INTEGER CHECK (difficulty BETWEEN 1 AND 10),
    topic VARCHAR(100),
    content TEXT NOT NULL,
    solution TEXT NOT NULL,
    explanation TEXT
);

CREATE TABLE user_problem_history (
    user_id UUID REFERENCES users(id),
    problem_id UUID REFERENCES problems(id),
    attempted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_correct BOOLEAN,
    time_spent INTEGER, -- seconds
    confidence_level INTEGER CHECK (confidence_level BETWEEN 1 AND 5),
    PRIMARY KEY (user_id, problem_id, attempted_at)
);
```

「正規化もちゃんとできてる」

◇◇◇◇

## 午後7時15分　AI部分の検討

「機械学習の部分は」

「Pythonで書く？」

```python
# AI推薦エンジン（Python）
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from sklearn.preprocessing import StandardScaler
import tensorflow as tf

class StudyRecommendationEngine:
    def __init__(self):
        self.performance_model = self._build_performance_model()
        self.recommendation_model = self._build_recommendation_model()
        self.scaler = StandardScaler()
    
    def _build_performance_model(self):
        """成績予測モデル"""
        model = tf.keras.Sequential([
            tf.keras.layers.Dense(128, activation='relu', input_shape=(10,)),
            tf.keras.layers.Dropout(0.2),
            tf.keras.layers.Dense(64, activation='relu'),
            tf.keras.layers.Dropout(0.2),
            tf.keras.layers.Dense(32, activation='relu'),
            tf.keras.layers.Dense(1, activation='linear')
        ])
        
        model.compile(
            optimizer='adam',
            loss='mse',
            metrics=['mae']
        )
        
        return model
    
    def _build_recommendation_model(self):
        """問題推薦モデル"""
        return RandomForestRegressor(
            n_estimators=100,
            max_depth=10,
            random_state=42
        )
    
    def predict_performance(self, user_features):
        """成績の予測"""
        features = self._extract_features(user_features)
        scaled_features = self.scaler.transform([features])
        prediction = self.performance_model.predict(scaled_features)
        return prediction[0][0]
    
    def recommend_problems(self, user_id, subject, n_recommendations=5):
        """最適な問題の推薦"""
        user_history = self._get_user_history(user_id, subject)
        user_level = self._estimate_user_level(user_history)
        
        # 協調フィルタリング + コンテンツベース
        similar_users = self._find_similar_users(user_id)
        content_scores = self._calculate_content_scores(user_level, subject)
        collaborative_scores = self._calculate_collaborative_scores(
            similar_users, subject
        )
        
        # スコアの統合
        final_scores = 0.6 * content_scores + 0.4 * collaborative_scores
        
        # トップN問題の選択
        recommendations = self._get_top_problems(final_scores, n_recommendations)
        
        return recommendations
    
    def _extract_features(self, user_data):
        """特徴量抽出"""
        return [
            user_data['study_hours'],
            user_data['accuracy_rate'],
            user_data['consistency_score'],
            user_data['difficulty_preference'],
            user_data['time_efficiency'],
            user_data['review_frequency'],
            user_data['weak_points_count'],
            user_data['strength_areas_count'],
            user_data['motivation_level'],
            user_data['days_until_exam']
        ]
```

「複雑だけど...」

「でも、面白そう」

◇◇◇◇

## 午後8時　開発スケジュール

「いつまでに作る？」

「受験シーズンまでには」

```javascript
// 開発スケジュール
const developmentSchedule = {
    phase1: {
        period: "4月 - 5月",
        goals: [
            "基本機能の実装",
            "データベース構築",
            "問題データの収集",
            "簡易UIの作成"
        ],
        hours_per_week: 10
    },
    
    phase2: {
        period: "6月 - 7月",
        goals: [
            "AI機能の実装",
            "推薦システム",
            "成績予測",
            "学習計画生成"
        ],
        hours_per_week: 8
    },
    
    phase3: {
        period: "8月 - 9月",
        goals: [
            "UIの改善",
            "性能最適化",
            "テスト",
            "ベータ版リリース"
        ],
        hours_per_week: 5
    },
    
    phase4: {
        period: "10月以降",
        goals: [
            "フィードバック収集",
            "機能改善",
            "正式リリース"
        ],
        hours_per_week: 3
    }
};
```

◇◇◇◇

## 午後8時30分　決意

「大変そうだけど」

「隆弘くんとなら」

「受験で苦労した経験を活かせる」

「そうだね」

「自分たちみたいな人を助けたい」

```javascript
// プロジェクトの意義
const projectMission = {
    vision: "全ての受験生に最適な学習体験を",
    
    values: [
        "個別最適化 - 一人ひとりに合った学習",
        "科学的アプローチ - データに基づく改善",
        "継続性 - モチベーションを保つ工夫",
        "公平性 - 誰もが使える価格設定"
    ],
    
    personalMotivation: {
        隆弘: "技術で教育を変えたい",
        美久: "みんなの夢を応援したい"
    },
    
    impact: {
        short_term: "受験生の負担軽減",
        long_term: "教育格差の是正"
    }
};

console.log("新しい挑戦の始まり");
```

◇◇◇◇

## 午後9時　最初のコミット

駅に向かいながら。

```bash
git init study-buddy
cd study-buddy
npm init -y

# package.jsonの設定
npm install --save react typescript @types/react
npm install --save-dev @vitejs/plugin-react vite

# 最初のコミット
touch README.md
echo "# StudyBuddy - AI学習支援アプリ" > README.md

git add .
git commit -m "feat: StudyBuddy プロジェクト開始 🚀

- React + TypeScriptでセットアップ
- 基本的なプロジェクト構造
- READMEの追加

受験生のための新しい挑戦を始めます"
```

「最初のコミット」

◇◇◇◇

## 午後9時30分　帰り道

「新しいプロジェクト、楽しみ」

「大学の勉強と両立できるかな」

「大丈夫」

「一緒だから」

手を繋いで歩く。

春の夜風が心地いい。

「美久」

「ん？」

「このプロジェクトを通して」

「もっと成長したい」

「高校生の時より、もっと大きなものを作りたい」

「隆弊くんとなら、きっと」

「うん」

```javascript
// 新しい章の始まり
const newBeginning = {
    project: "StudyBuddy",
    started: new Date("2024-04-10"),
    
    challenges: [
        "大学との両立",
        "AIの実装",
        "ユーザーのニーズ理解"
    ],
    
    opportunities: [
        "社会貢献",
        "技術力向上",
        "新しい仲間"
    ],
    
    promise: {
        from: "美久",
        to: "隆弘",
        content: "一緒に素晴らしいものを作ろう"
    },
    
    future: "無限の可能性"
};

saveToMemory(newBeginning);
```

新しいプロジェクトの始まり。

WinterLangとは違う、より実用的な挑戦。

でも、根っこは同じ。

誰かの役に立ちたい、という想い。

そして、隆弘くんと一緒に創り上げる喜び。

大学生活、まだ始まったばかり。

これから、どんな未来が待ってるんだろう。