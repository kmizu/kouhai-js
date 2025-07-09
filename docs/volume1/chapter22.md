# 第22話 DOMで描く未来

## 10月31日（火）午前7時

ハロウィンの朝。

でも、私たちには文化祭の方が大事。

あと3日で本番。

ゲームはほぼ完成してるけど、まだやることがある。

「おはよう、美久」

エレベーターで隆弘くんと会った。

「おはよう」

「今日の放課後も、頑張ろうね」

「うん」

二人だけの秘密の時間。

学校では普通の先輩後輩。

でも、心はいつも繋がってる。

◇◇◇◇

## 午後4時　プログラミング部室

「今日は、DOM操作について詳しく教えるよ」

隆弘くんがホワイトボードに大きく「DOM」と書く。

「DOM？」

「Document Object Model。HTMLを操作する仕組みだよ」

```javascript
// DOM要素の取得方法いろいろ
const element1 = document.getElementById('game-title');
const element2 = document.querySelector('.character-name');
const elements = document.querySelectorAll('.choice-button');

// 要素の作成
const newDiv = document.createElement('div');
newDiv.textContent = '新しい要素';
newDiv.className = 'message-box';

// 要素の追加
document.body.appendChild(newDiv);

// 要素の削除
const oldElement = document.getElementById('old-element');
if (oldElement) {
    oldElement.remove();
}
```

「なるほど！HTMLを動的に変えられるんだ」

「そう。これでゲーム画面を自由に操作できる」

◇◇◇◇

## 午後4時45分　ゲーム画面の構築

「実際にゲーム画面を作ってみよう」

```javascript
// ゲーム画面の基本構造を作る
class GameScreen {
    constructor() {
        this.container = document.getElementById('game-container');
        this.init();
    }
    
    init() {
        // ゲーム画面をクリア
        this.container.innerHTML = '';
        
        // 背景レイヤー
        this.backgroundLayer = this.createElement('div', 'background-layer');
        
        // キャラクターレイヤー
        this.characterLayer = this.createElement('div', 'character-layer');
        
        // UIレイヤー
        this.uiLayer = this.createElement('div', 'ui-layer');
        
        // テキストボックス
        this.textBox = this.createElement('div', 'text-box');
        this.nameBox = this.createElement('div', 'name-box');
        this.messageBox = this.createElement('div', 'message-box');
        
        // 要素を組み立てる
        this.textBox.appendChild(this.nameBox);
        this.textBox.appendChild(this.messageBox);
        this.uiLayer.appendChild(this.textBox);
        
        // コンテナに追加
        this.container.appendChild(this.backgroundLayer);
        this.container.appendChild(this.characterLayer);
        this.container.appendChild(this.uiLayer);
    }
    
    createElement(tag, className) {
        const element = document.createElement(tag);
        if (className) {
            element.className = className;
        }
        return element;
    }
    
    // 背景を変更
    setBackground(imagePath) {
        this.backgroundLayer.style.backgroundImage = `url(${imagePath})`;
        this.backgroundLayer.style.backgroundSize = 'cover';
    }
    
    // キャラクターを表示
    showCharacter(imagePath, position = 'center') {
        const character = this.createElement('img', 'character');
        character.src = imagePath;
        character.style.position = 'absolute';
        
        // 位置を設定
        switch(position) {
            case 'left':
                character.style.left = '10%';
                break;
            case 'right':
                character.style.right = '10%';
                break;
            default:
                character.style.left = '50%';
                character.style.transform = 'translateX(-50%)';
        }
        
        this.characterLayer.appendChild(character);
    }
    
    // メッセージを表示
    showMessage(name, text) {
        this.nameBox.textContent = name;
        this.messageBox.textContent = '';
        
        // タイプライター効果
        let index = 0;
        const typeWriter = () => {
            if (index < text.length) {
                this.messageBox.textContent += text[index];
                index++;
                setTimeout(typeWriter, 50);
            }
        };
        typeWriter();
    }
}
```

「すごい！レイヤー構造になってる」

「そう。背景、キャラクター、UIを別々に管理できる」

美久が興奮気味に画面を見つめる。

◇◇◇◇

## 午後5時30分　選択肢システム

「選択肢も動的に作ろう」

```javascript
// 選択肢システム
class ChoiceSystem {
    constructor(gameScreen) {
        this.gameScreen = gameScreen;
        this.choiceContainer = null;
    }
    
    // 選択肢を表示
    showChoices(choices) {
        // 既存の選択肢を削除
        if (this.choiceContainer) {
            this.choiceContainer.remove();
        }
        
        // 選択肢コンテナを作成
        this.choiceContainer = document.createElement('div');
        this.choiceContainer.className = 'choice-container';
        
        // 各選択肢を作成
        choices.forEach((choice, index) => {
            const button = this.createChoiceButton(choice, index);
            this.choiceContainer.appendChild(button);
        });
        
        // UIレイヤーに追加
        this.gameScreen.uiLayer.appendChild(this.choiceContainer);
        
        // フェードインアニメーション
        this.choiceContainer.style.opacity = '0';
        setTimeout(() => {
            this.choiceContainer.style.transition = 'opacity 0.5s';
            this.choiceContainer.style.opacity = '1';
        }, 100);
    }
    
    // 選択肢ボタンを作成
    createChoiceButton(choice, index) {
        const button = document.createElement('button');
        button.className = 'choice-button';
        button.textContent = choice.text;
        
        // ホバー効果
        button.addEventListener('mouseenter', () => {
            button.style.transform = 'scale(1.05)';
            button.style.backgroundColor = '#ff6b6b';
        });
        
        button.addEventListener('mouseleave', () => {
            button.style.transform = 'scale(1)';
            button.style.backgroundColor = '#4ecdc4';
        });
        
        // クリック処理
        button.addEventListener('click', () => {
            this.selectChoice(choice, index);
        });
        
        // 遅延表示
        button.style.opacity = '0';
        setTimeout(() => {
            button.style.transition = 'opacity 0.3s';
            button.style.opacity = '1';
        }, index * 200);
        
        return button;
    }
    
    // 選択肢を選んだ時の処理
    selectChoice(choice, index) {
        console.log(`選択: ${choice.text}`);
        
        // 選択エフェクト
        const buttons = this.choiceContainer.querySelectorAll('.choice-button');
        buttons.forEach((btn, i) => {
            if (i === index) {
                btn.style.backgroundColor = '#ff4757';
                btn.style.transform = 'scale(1.1)';
            } else {
                btn.style.opacity = '0.3';
            }
        });
        
        // 選択肢を消す
        setTimeout(() => {
            this.choiceContainer.style.opacity = '0';
            setTimeout(() => {
                this.choiceContainer.remove();
                this.choiceContainer = null;
                
                // 選択後の処理を実行
                if (choice.callback) {
                    choice.callback();
                }
            }, 500);
        }, 500);
    }
}
```

「選択肢にアニメーションがついてる！」

「ユーザー体験を良くするためだよ」

隆弘くんの細やかな配慮が嬉しい。

◇◇◇◇

## 午後6時15分　エフェクトシステム

「特別な演出も作ろう」

```javascript
// エフェクトシステム
class EffectSystem {
    constructor(gameScreen) {
        this.gameScreen = gameScreen;
    }
    
    // 画面を揺らす
    shake(duration = 500, intensity = 10) {
        const container = this.gameScreen.container;
        const originalTransform = container.style.transform;
        
        const shakeAnimation = () => {
            const x = (Math.random() - 0.5) * intensity;
            const y = (Math.random() - 0.5) * intensity;
            container.style.transform = `translate(${x}px, ${y}px)`;
        };
        
        const interval = setInterval(shakeAnimation, 50);
        
        setTimeout(() => {
            clearInterval(interval);
            container.style.transform = originalTransform;
        }, duration);
    }
    
    // フラッシュ効果
    flash(color = 'white', duration = 200) {
        const flashDiv = document.createElement('div');
        flashDiv.style.cssText = `
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: ${color};
            opacity: 0;
            z-index: 9999;
            pointer-events: none;
        `;
        
        this.gameScreen.container.appendChild(flashDiv);
        
        // フラッシュアニメーション
        flashDiv.style.transition = `opacity ${duration/2}ms`;
        setTimeout(() => {
            flashDiv.style.opacity = '1';
            setTimeout(() => {
                flashDiv.style.opacity = '0';
                setTimeout(() => {
                    flashDiv.remove();
                }, duration/2);
            }, duration/2);
        }, 10);
    }
    
    // ハートエフェクト
    showHearts(count = 5) {
        for (let i = 0; i < count; i++) {
            setTimeout(() => {
                this.createHeart();
            }, i * 200);
        }
    }
    
    createHeart() {
        const heart = document.createElement('div');
        heart.className = 'heart-effect';
        heart.innerHTML = '❤️';
        heart.style.cssText = `
            position: absolute;
            font-size: 30px;
            z-index: 9999;
            pointer-events: none;
            left: ${Math.random() * 80 + 10}%;
            bottom: -50px;
            animation: floatUp 3s ease-out forwards;
        `;
        
        this.gameScreen.container.appendChild(heart);
        
        setTimeout(() => {
            heart.remove();
        }, 3000);
    }
    
    // 暗転
    fadeToBlack(duration = 1000, callback) {
        const blackScreen = document.createElement('div');
        blackScreen.style.cssText = `
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: black;
            opacity: 0;
            z-index: 9998;
        `;
        
        this.gameScreen.container.appendChild(blackScreen);
        
        blackScreen.style.transition = `opacity ${duration}ms`;
        setTimeout(() => {
            blackScreen.style.opacity = '1';
            setTimeout(() => {
                if (callback) callback();
            }, duration);
        }, 10);
    }
}
```

「ハートエフェクト！」

美久が目を輝かせる。

「告白シーンで使えそうでしょ？」

「うん！絶対使う！」

◇◇◇◇

## 午後7時　統合テスト

「全部組み合わせてテストしてみよう」

```javascript
// ゲームの初期化
const gameScreen = new GameScreen();
const choiceSystem = new ChoiceSystem(gameScreen);
const effectSystem = new EffectSystem(gameScreen);

// 告白シーンのデモ
function confessionScene() {
    // 背景を夕暮れの屋上に
    gameScreen.setBackground('images/rooftop_sunset.jpg');
    
    // キャラクターを表示
    gameScreen.showCharacter('images/miku_blush.png', 'center');
    
    // セリフ
    gameScreen.showMessage('美久', 'あの……隆弘くん……');
    
    // 3秒後に選択肢
    setTimeout(() => {
        const choices = [
            {
                text: '美久、好きだ',
                callback: () => {
                    effectSystem.showHearts(10);
                    gameScreen.showMessage('美久', '私も……ずっと好きでした！');
                }
            },
            {
                text: '大切な友達だよ',
                callback: () => {
                    effectSystem.shake(300, 5);
                    gameScreen.showMessage('美久', 'そう……だよね……');
                }
            }
        ];
        
        choiceSystem.showChoices(choices);
    }, 3000);
}

// デモ実行
confessionScene();
```

画面に表示される告白シーン。

ハートが舞い上がる演出。

「きれい……」

美久が呟く。

「これ、私たちの告白シーンみたい」

隆弘くんが照れくさそうに笑う。

◇◇◇◇

## 午後7時45分　最後の調整

「DOM操作、理解できた？」

「うん。HTMLを自由に操れるんだね」

「そう。これで思い通りの画面が作れる」

二人で画面を見つめる。

私たちが作ったゲーム。

もうすぐ完成。

「隆弘くん」

「なに？」

「このゲーム、私たちの思い出だね」

「うん」

「DOMで作った画面に、私たちの気持ちが詰まってる」

隆弘くんが私の手を握る。

「文化祭が終わっても、この思い出は消えない」

「うん」

「次は、もっとすごいゲーム作ろう」

「約束」

小指を絡める。

約束のしるし。

プログラミングが繋いだ私たち。

DOMで描く未来は、きっと明るい。

そう信じて、今日も一緒に帰る。

手を繋いで、同じ未来を見つめながら。