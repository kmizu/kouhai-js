# 第23話 非同期処理と待つ心

## 11月1日（水）午前7時

文化祭まで、あと2日。

緊張と期待で、昨夜はあまり眠れなかった。

でも、隆弘くんと一緒なら大丈夫。

そう思いながら、朝の準備をする。

スマホが震えた。

『おはよう。今日も頑張ろう』

隆弘くんからのメッセージ。

『おはよう。うん、頑張る！』

今日も素敵な一日になりそう。

◇◇◇◇

## 午後4時　プログラミング部室

「今日は、非同期処理について教えるよ」

隆弘くんが新しい概念を説明し始める。

「非同期処理？」

「時間のかかる処理を、待たずに実行する仕組み」

```javascript
// 同期処理（順番に実行される）
console.log("1番目");
console.log("2番目");
console.log("3番目");

// 非同期処理（順番が変わる）
console.log("1番目");
setTimeout(() => {
    console.log("2番目（1秒後）");
}, 1000);
console.log("3番目");

// 実行結果：
// 1番目
// 3番目
// 2番目（1秒後）
```

「えっ、順番が変わっちゃった！」

「そう。JavaScriptは待ってくれないんだ」

◇◇◇◇

## 午後4時30分　Promiseの基本

「そこで使うのがPromise」

```javascript
// Promiseの基本
const myPromise = new Promise((resolve, reject) => {
    // 非同期処理
    setTimeout(() => {
        const success = true;
        
        if (success) {
            resolve("成功しました！");
        } else {
            reject("失敗しました...");
        }
    }, 1000);
});

// Promiseの使い方
myPromise
    .then(result => {
        console.log(result); // "成功しました！"
    })
    .catch(error => {
        console.error(error);
    });
```

「約束みたいな名前」

「そう。『処理が終わったら教えるよ』っていう約束」

隆弘くんの説明が分かりやすい。

◇◇◇◇

## 午後5時　ゲームへの実装

「ゲームのデータ読み込みに使ってみよう」

```javascript
// 画像読み込みクラス
class ImageLoader {
    static loadImage(src) {
        return new Promise((resolve, reject) => {
            const img = new Image();
            
            img.onload = () => {
                console.log(`画像読み込み成功: ${src}`);
                resolve(img);
            };
            
            img.onerror = () => {
                console.error(`画像読み込み失敗: ${src}`);
                reject(new Error(`Failed to load image: ${src}`));
            };
            
            img.src = src;
        });
    }
    
    // 複数の画像を読み込む
    static loadImages(imageList) {
        const promises = imageList.map(src => this.loadImage(src));
        return Promise.all(promises);
    }
}

// セーブデータ管理
class SaveDataManager {
    // セーブ
    static save(slotNumber, gameData) {
        return new Promise((resolve) => {
            setTimeout(() => {
                const saveData = {
                    slot: slotNumber,
                    data: gameData,
                    timestamp: new Date().toISOString()
                };
                
                localStorage.setItem(`save_${slotNumber}`, JSON.stringify(saveData));
                resolve("セーブ完了");
            }, 500); // セーブアニメーションの時間
        });
    }
    
    // ロード
    static load(slotNumber) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                const savedString = localStorage.getItem(`save_${slotNumber}`);
                
                if (savedString) {
                    const saveData = JSON.parse(savedString);
                    resolve(saveData);
                } else {
                    reject(new Error("セーブデータが見つかりません"));
                }
            }, 300); // ロードアニメーションの時間
        });
    }
}
```

「画像もセーブも、全部Promise！」

「そう。これで画面が固まらない」

◇◇◇◇

## 午後5時45分　async/await

「もっと簡単な書き方もあるよ」

```javascript
// async/awaitを使った書き方
async function startGame() {
    try {
        // ローディング画面を表示
        showLoadingScreen("ゲームを起動中...");
        
        // 画像を読み込む
        updateLoadingText("画像を読み込んでいます...");
        const images = await ImageLoader.loadImages([
            'images/bg_classroom.jpg',
            'images/bg_rooftop.jpg',
            'images/character_miku.png',
            'images/character_miku_blush.png'
        ]);
        
        // BGMを読み込む
        updateLoadingText("音楽を読み込んでいます...");
        const bgm = await loadBGM('bgm/main_theme.mp3');
        
        // セーブデータをチェック
        updateLoadingText("セーブデータを確認中...");
        const hasSaveData = await checkSaveData();
        
        // ローディング完了
        hideLoadingScreen();
        
        // タイトル画面を表示
        if (hasSaveData) {
            showTitleScreen({ continueEnabled: true });
        } else {
            showTitleScreen({ continueEnabled: false });
        }
        
    } catch (error) {
        console.error("ゲームの起動に失敗しました:", error);
        showErrorScreen("ゲームの起動に失敗しました");
    }
}

// BGM読み込み
function loadBGM(src) {
    return new Promise((resolve, reject) => {
        const audio = new Audio(src);
        
        audio.addEventListener('canplaythrough', () => {
            resolve(audio);
        });
        
        audio.addEventListener('error', () => {
            reject(new Error(`BGM読み込み失敗: ${src}`));
        });
        
        audio.load();
    });
}

// セーブデータの存在確認
async function checkSaveData() {
    for (let i = 1; i <= 5; i++) {
        try {
            await SaveDataManager.load(i);
            return true; // セーブデータが見つかった
        } catch (e) {
            // このスロットは空
        }
    }
    return false; // セーブデータなし
}
```

「asyncとawait？」

「Promiseを同期処理みたいに書ける魔法」

「すごい！順番通りに見える」

◇◇◇◇

## 午後6時30分　実践的な使用例

「実際のゲームシーンで使ってみよう」

```javascript
// シーン遷移システム
class SceneTransition {
    static async fadeOut(duration = 1000) {
        const overlay = document.createElement('div');
        overlay.className = 'fade-overlay';
        overlay.style.opacity = '0';
        document.body.appendChild(overlay);
        
        // フェードアウトアニメーション
        overlay.style.transition = `opacity ${duration}ms`;
        
        return new Promise(resolve => {
            setTimeout(() => {
                overlay.style.opacity = '1';
                setTimeout(() => {
                    resolve(overlay);
                }, duration);
            }, 10);
        });
    }
    
    static async fadeIn(overlay, duration = 1000) {
        overlay.style.transition = `opacity ${duration}ms`;
        overlay.style.opacity = '0';
        
        return new Promise(resolve => {
            setTimeout(() => {
                overlay.remove();
                resolve();
            }, duration);
        });
    }
    
    static async changeTo(sceneName) {
        try {
            // フェードアウト
            const overlay = await this.fadeOut();
            
            // シーンを読み込む
            const sceneData = await this.loadScene(sceneName);
            
            // 背景を変更
            await this.changeBackground(sceneData.background);
            
            // キャラクターを配置
            if (sceneData.characters) {
                await this.placeCharacters(sceneData.characters);
            }
            
            // BGMを変更
            if (sceneData.bgm) {
                await this.changeBGM(sceneData.bgm);
            }
            
            // フェードイン
            await this.fadeIn(overlay);
            
            // シーンの開始
            return sceneData;
            
        } catch (error) {
            console.error("シーン遷移エラー:", error);
            throw error;
        }
    }
    
    static loadScene(sceneName) {
        // 実際はサーバーから読み込んだりする
        return new Promise(resolve => {
            setTimeout(() => {
                const scenes = {
                    confession: {
                        name: "告白シーン",
                        background: "images/bg_rooftop_sunset.jpg",
                        characters: [
                            { name: "miku", image: "miku_blush.png", position: "center" }
                        ],
                        bgm: "bgm/love_theme.mp3"
                    }
                };
                resolve(scenes[sceneName]);
            }, 500);
        });
    }
}

// 告白シーンの実行
async function playConfessionScene() {
    try {
        // シーン遷移
        const scene = await SceneTransition.changeTo('confession');
        
        // セリフを表示
        await showDialogue("美久", "あの……ずっと言いたかったことがあるの");
        
        // 少し間を置く
        await wait(1000);
        
        // 続きのセリフ
        await showDialogue("美久", "隆弘くん……好き");
        
        // ハートエフェクト
        await showHeartEffect();
        
        // 選択肢
        const choice = await showChoices([
            { text: "僕も好きだよ", value: "accept" },
            { text: "ごめん…", value: "reject" }
        ]);
        
        if (choice === "accept") {
            await showDialogue("隆弘", "僕もずっと美久のことが好きだった");
            await unlockAchievement("true_love");
        }
        
    } catch (error) {
        console.error("シーンエラー:", error);
    }
}

// ユーティリティ関数
function wait(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

function showDialogue(speaker, text) {
    return new Promise(resolve => {
        gameScreen.showMessage(speaker, text);
        
        // クリック待ち
        document.addEventListener('click', function clickHandler() {
            document.removeEventListener('click', clickHandler);
            resolve();
        });
    });
}
```

「全部がPromiseで繋がってる！」

「そう。これで複雑な演出も簡単に書ける」

◇◇◇◇

## 午後7時15分　エラーハンドリング

「エラーの処理も大事」

```javascript
// エラーハンドリングの例
async function robustGameStart() {
    const maxRetries = 3;
    let retryCount = 0;
    
    while (retryCount < maxRetries) {
        try {
            await startGame();
            break; // 成功したらループを抜ける
            
        } catch (error) {
            retryCount++;
            console.error(`起動失敗 (${retryCount}/${maxRetries}):`, error);
            
            if (retryCount >= maxRetries) {
                // リトライ上限に達した
                showFatalError("ゲームの起動に失敗しました。ページを更新してください。");
                return;
            }
            
            // リトライ前に少し待つ
            await wait(1000 * retryCount);
            
            showRetryMessage(`再試行中... (${retryCount}/${maxRetries})`);
        }
    }
}

// Promise.allの活用
async function preloadAllAssets() {
    try {
        const [images, sounds, scripts] = await Promise.all([
            loadAllImages(),
            loadAllSounds(),
            loadAllScripts()
        ]);
        
        console.log("全リソース読み込み完了");
        return { images, sounds, scripts };
        
    } catch (error) {
        // どれか1つでも失敗したらここに来る
        console.error("リソース読み込みエラー:", error);
        
        // 部分的な読み込みを試みる
        return await loadEssentialAssetsOnly();
    }
}
```

「失敗しても諦めない！」

「そう。ユーザーのために最善を尽くす」

◇◇◇◇

## 午後7時45分　約束の意味

「Promise、理解できた？」

「うん。約束を守るプログラム」

部室には、また二人きり。

外はもう暗い。

「ねえ、隆弘くん」

「なに？」

「私たちも、Promise作ろう」

「え？」

```javascript
// 私たちのPromise
const ourPromise = new Promise((resolve) => {
    const promise = {
        from: "美久",
        to: "隆弘",
        content: "ずっと一緒にゲームを作り続ける",
        date: new Date(),
        forever: true
    };
    
    resolve(promise);
});

ourPromise.then(promise => {
    console.log("約束:", promise.content);
    console.log("期限:", promise.forever ? "永遠" : promise.date);
});
```

隆弘くんが優しく微笑む。

「いいね。僕からも」

```javascript
const myPromise = new Promise((resolve) => {
    resolve({
        from: "隆弘",
        to: "美久",
        content: "美久の夢を全力でサポートする",
        feelings: "愛してる",
        async: false // 同期的に、ずっと
    });
});
```

涙が出そうになる。

「ありがとう」

「こちらこそ」

手を繋いで、誓い合う。

プログラムの約束も、私たちの約束も、きっと守られる。

そう信じて、明日を迎える。

文化祭まで、あと2日。

でも、私たちの未来は、もっとずっと先まで続いていく。

Promiseのように、確実に。