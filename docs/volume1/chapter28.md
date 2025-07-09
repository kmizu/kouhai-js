# 第28話 プログラミングコンテスト

## 12月15日（土）午前9時

U-18プログラミングコンテスト当日。

会場は、都内の大きなイベントホール。

「緊張する」

隆弘くんの隣で、心臓がバクバクしてる。

「大丈夫。練習通りにやれば」

隆弘くんが優しく手を握ってくれる。

その温もりで、少し落ち着いた。

◇◇◇◇

## 午前9時30分　開会式

「皆さん、ようこそU-18プログラミングコンテストへ！」

司会者の声が会場に響く。

参加者は全国から集まった高校生たち。

みんな自信に満ちた表情。

「今年のテーマは『繋がり』です」

テーマ発表。

隆弘くんと目が合う。

私たちにぴったりのテーマ。

◇◇◇◇

## 午前10時　開発開始

「よし、始めよう」

与えられた時間は6時間。

その間に、テーマに沿ったWebアプリを作る。

```javascript
// プロジェクト初期設定
const projectConfig = {
    name: "Connect Hearts",
    theme: "繋がり",
    description: "大切な人との絆を可視化するアプリ",
    team: {
        frontend: "河内美久",
        backend: "嵐山隆弘"
    },
    technologies: {
        frontend: ["React", "TypeScript", "Tailwind CSS"],
        backend: ["Node.js", "Express", "Socket.io"],
        database: ["MongoDB"],
        deployment: ["Vercel", "Heroku"]
    }
};

console.log("開発開始！");
```

◇◇◇◇

## 午前11時　アイデアの実装

「リアルタイムで心が繋がるアプリはどう？」

私のアイデアに、隆弘くんが頷く。

「いいね。技術的にはWebSocketを使えば」

二人で設計を詰めていく。

```javascript
// フロントエンド（美久担当）
import React, { useState, useEffect } from 'react';
import io from 'socket.io-client';

const HeartConnection = () => {
    const [socket, setSocket] = useState(null);
    const [partnerCode, setPartnerCode] = useState('');
    const [connected, setConnected] = useState(false);
    const [heartbeat, setHeartbeat] = useState(0);
    
    useEffect(() => {
        const newSocket = io('http://localhost:3000');
        setSocket(newSocket);
        
        newSocket.on('partner-connected', () => {
            setConnected(true);
        });
        
        newSocket.on('heartbeat', (data) => {
            setHeartbeat(data.intensity);
            animateHeart(data.intensity);
        });
        
        return () => newSocket.close();
    }, []);
    
    const connectToPartner = () => {
        socket.emit('connect-partner', partnerCode);
    };
    
    const sendHeartbeat = () => {
        socket.emit('send-heartbeat', {
            intensity: Math.random() * 100,
            message: 'Thinking of you'
        });
    };
    
    return (
        <div className="heart-app">
            {!connected ? (
                <div className="connect-screen">
                    <h1>Connect Hearts</h1>
                    <input
                        type="text"
                        placeholder="パートナーコードを入力"
                        value={partnerCode}
                        onChange={(e) => setPartnerCode(e.target.value)}
                    />
                    <button onClick={connectToPartner}>
                        繋がる
                    </button>
                </div>
            ) : (
                <div className="connected-screen">
                    <Heart intensity={heartbeat} />
                    <button onClick={sendHeartbeat}>
                        ハートを送る
                    </button>
                </div>
            )}
        </div>
    );
};
```

◇◇◇◇

## 午後0時　バックエンド実装

隆弘くんがサーバー側を実装。

私は横で見守りながら、デザインを進める。

```javascript
// バックエンド（隆弘担当）
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server, {
    cors: {
        origin: "http://localhost:3001",
        methods: ["GET", "POST"]
    }
});

// 接続管理
const connections = new Map();
const rooms = new Map();

io.on('connection', (socket) => {
    console.log('新しい接続:', socket.id);
    
    // パートナーコードを生成
    const partnerCode = generatePartnerCode();
    connections.set(socket.id, { partnerCode, partnerId: null });
    
    socket.emit('your-code', partnerCode);
    
    // パートナーと接続
    socket.on('connect-partner', (targetCode) => {
        // パートナーを探す
        for (let [id, data] of connections) {
            if (data.partnerCode === targetCode && id !== socket.id) {
                // 相互接続を確立
                connections.get(socket.id).partnerId = id;
                connections.get(id).partnerId = socket.id;
                
                // ルームを作成
                const roomId = generateRoomId();
                socket.join(roomId);
                io.sockets.sockets.get(id).join(roomId);
                rooms.set(roomId, [socket.id, id]);
                
                // 両者に通知
                io.to(roomId).emit('partner-connected');
                break;
            }
        }
    });
    
    // ハートビートを送信
    socket.on('send-heartbeat', (data) => {
        const connection = connections.get(socket.id);
        if (connection && connection.partnerId) {
            io.to(connection.partnerId).emit('heartbeat', data);
            
            // データベースに記録
            saveHeartbeat({
                from: socket.id,
                to: connection.partnerId,
                intensity: data.intensity,
                message: data.message,
                timestamp: new Date()
            });
        }
    });
    
    // 切断処理
    socket.on('disconnect', () => {
        const connection = connections.get(socket.id);
        if (connection && connection.partnerId) {
            io.to(connection.partnerId).emit('partner-disconnected');
        }
        connections.delete(socket.id);
    });
});

function generatePartnerCode() {
    return Math.random().toString(36).substring(2, 8).toUpperCase();
}

function generateRoomId() {
    return `room_${Date.now()}_${Math.random().toString(36).substring(2, 8)}`;
}

server.listen(3000, () => {
    console.log('サーバー起動 port:3000');
});
```

◇◇◇◇

## 午後1時30分　UI/UXの洗練

「もっと感動的なデザインにしたい」

私がCSSを調整していく。

```css
/* ハートのアニメーション */
.heart {
    width: 100px;
    height: 90px;
    position: relative;
    animation: heartbeat 1s infinite;
}

.heart:before,
.heart:after {
    position: absolute;
    content: "";
    left: 50px;
    top: 0;
    width: 50px;
    height: 80px;
    background: #ff6b6b;
    border-radius: 50px 50px 0 0;
    transform: rotate(-45deg);
    transform-origin: 0 100%;
}

.heart:after {
    left: 0;
    transform: rotate(45deg);
    transform-origin: 100% 100%;
}

@keyframes heartbeat {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
}

/* パートナーからのハートビート */
.partner-heartbeat {
    animation: glow 2s ease-in-out;
}

@keyframes glow {
    0% { box-shadow: 0 0 5px #ff6b6b; }
    50% { box-shadow: 0 0 20px #ff6b6b, 0 0 30px #ff6b6b; }
    100% { box-shadow: 0 0 5px #ff6b6b; }
}
```

◇◇◇◇

## 午後2時45分　機能追加

「メッセージ機能も追加しよう」

時間との戦い。

でも、二人なら大丈夫。

```javascript
// メッセージ機能
const MessageFeature = () => {
    const [messages, setMessages] = useState([]);
    const [newMessage, setNewMessage] = useState('');
    
    const sendMessage = () => {
        if (newMessage.trim()) {
            socket.emit('send-message', {
                text: newMessage,
                timestamp: new Date()
            });
            
            setMessages([...messages, {
                text: newMessage,
                from: 'me',
                timestamp: new Date()
            }]);
            
            setNewMessage('');
        }
    };
    
    return (
        <div className="message-container">
            <div className="messages">
                {messages.map((msg, index) => (
                    <div 
                        key={index} 
                        className={`message ${msg.from}`}
                    >
                        <p>{msg.text}</p>
                        <span>{formatTime(msg.timestamp)}</span>
                    </div>
                ))}
            </div>
            
            <div className="message-input">
                <input
                    type="text"
                    value={newMessage}
                    onChange={(e) => setNewMessage(e.target.value)}
                    onKeyPress={(e) => e.key === 'Enter' && sendMessage()}
                    placeholder="メッセージを入力..."
                />
                <button onClick={sendMessage}>送信</button>
            </div>
        </div>
    );
};
```

◇◇◇◇

## 午後3時30分　最終調整

「あと30分！」

ラストスパート。

デバッグ、デプロイ、プレゼン準備。

```javascript
// デプロイ設定
const deploymentConfig = {
    frontend: {
        platform: "Vercel",
        url: "https://connect-hearts.vercel.app",
        env: {
            REACT_APP_API_URL: process.env.BACKEND_URL
        }
    },
    backend: {
        platform: "Heroku",
        url: "https://connect-hearts-api.herokuapp.com",
        env: {
            MONGODB_URI: process.env.MONGODB_URI,
            PORT: process.env.PORT || 3000
        }
    }
};

// プレゼン資料の要点
const presentationPoints = [
    "リアルタイムで心が繋がる体験",
    "WebSocketによる双方向通信",
    "感情を可視化するUI",
    "大切な人との絆を深めるツール"
];
```

◇◇◇◇

## 午後4時　プレゼンテーション

「次は、チーム『Forever Loop』です」

私たちの番が来た。

隆弘くんと一緒に壇上へ。

「私たちが作ったのは『Connect Hearts』」

「大切な人と、いつでも心で繋がれるアプリです」

デモを実演。

会場から「おお」という声が上がる。

◇◇◇◇

## 午後5時　結果発表

「優勝は……」

ドキドキしながら待つ。

隆弘くんの手をぎゅっと握る。

「チーム『Forever Loop』！」

え？

私たち？

「技術力とアイデア、そして何より『繋がり』というテーマを見事に表現していました」

信じられない。

隆弘くんと抱き合う。

◇◇◇◇

## 午後6時　表彰式

トロフィーを受け取る。

二人で持つ、重いトロフィー。

「これも、二人で作った作品」

隆弘くんが嬉しそうに言う。

「うん。最高の思い出」

写真撮影。

笑顔が止まらない。

◇◇◇◇

## 午後7時　帰り道

「信じられない」

まだ実感が湧かない。

「美久のデザインが素晴らしかったから」

「隆弘くんの実装があったから」

お互いを褒め合う。

でも、本当は分かってる。

二人だから、できたこと。

◇◇◇◇

## 午後8時　お祝い

「今日はお祝いしよう」

レストランで乾杯。

「優勝おめでとう」

「隆弘くんも」

グラスを合わせる。

「これからも、一緒に」

「うん、ずっと」

◇◇◇◇

## 午後9時30分　エピローグ

```javascript
// 今日の記録
const todaysMilestone = {
    event: "U-18プログラミングコンテスト",
    result: "優勝",
    project: "Connect Hearts",
    team: ["嵐山隆弘", "河内美久"],
    
    lessons: [
        "二人の力は無限大",
        "愛がコードを強くする",
        "繋がりは最高のテーマ"
    ],
    
    future: {
        nextProject: "もっと大きな夢",
        together: "永遠に",
        love: Infinity
    }
};

console.log("最高の一日だった");
console.log("これからも、ずっと一緒に");
```

部屋に戻って、トロフィーを見つめる。

これは、私たちの愛の証。

プログラミングが繋いだ、運命の糸。

明日からも、新しい挑戦が待ってる。

でも、怖くない。

隆弘くんと一緒なら、どんなことでもできる。

そう信じて、幸せな気持ちで眠りについた。