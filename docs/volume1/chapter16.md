# 第16話 標準ライブラリ

　五月五日、日曜日。こどもの日。

　ゴールデンウィークも残り二日。MikuLangの完成が近づいている。そして、美久との関係も——

「今日は標準ライブラリだね」

　美久が期待に満ちた表情で言う。

「便利な機能をまとめたもの。これがあれば実用的な言語になる」

「どんな機能を入れるの？」

「数学関数、日付処理、乱数生成……ゲーム作りに必要なものを」

◇◇◇◇

「まず、数学関数から始めよう」

　ホワイトボードに一覧を書く。

```javascript
// 数学関数の標準ライブラリ
const mathLib = {
  abs: (x) => Math.abs(x),        // 絶対値
  floor: (x) => Math.floor(x),    // 切り捨て
  ceil: (x) => Math.ceil(x),      // 切り上げ
  round: (x) => Math.round(x),    // 四捨五入
  max: (a, b) => Math.max(a, b),  // 最大値
  min: (a, b) => Math.min(a, b),  // 最小値
  pow: (a, b) => Math.pow(a, b),  // べき乗
  sqrt: (x) => Math.sqrt(x),      // 平方根
  random: () => Math.random()      // 乱数（0-1）
};
```

「randomってゲームでよく使うよね」

「そう。敵の出現とか、アイテムのドロップとか」

◇◇◇◇

「標準ライブラリの実装方法を考えよう」

```javascript
// グローバル環境に標準ライブラリを登録
class StandardLibrary {
  static initialize(env) {
    // 数学関数
    Object.entries(mathLib).forEach(([name, func]) => {
      env.setFunction(name, {
        params: func.length === 1 ? ['x'] : ['a', 'b'],
        body: null,  // ネイティブ関数
        native: func
      });
    });
    
    // 配列操作関数
    env.setFunction('push', {
      params: ['array', 'item'],
      native: (array, item) => {
        array.push(item);
        return array;
      }
    });
    
    env.setFunction('pop', {
      params: ['array'],
      native: (array) => array.pop()
    });
    
    // 文字列操作関数
    env.setFunction('concat', {
      params: ['str1', 'str2'],
      native: (str1, str2) => str1 + str2
    });
  }
}
```

「nativeって？」

「JavaScript側で実装された組み込み関数」

◇◇◇◇

　美久が質問してきた。

「乱数を使った例を見せて」

「じゃあ、サイコロゲームを作ってみよう」

```javascript
const diceGame = {
  type: ASTTypes.Program,
  body: [
    // 1-6のサイコロを振る関数
    createFunction(
      'サイコロ',
      [],
      {
        type: 'BlockStatement',
        body: [
          createReturn(
            createBinaryExpression(
              '+',
              createCall('floor', [
                createBinaryExpression(
                  '*',
                  createCall('random', []),
                  createNumber(6)
                )
              ]),
              createNumber(1)
            )
          )
        ]
      }
    ),
    
    // ゲーム本体
    createAssignment('プレイヤー', createCall('サイコロ', [])),
    createAssignment('コンピュータ', createCall('サイコロ', [])),
    
    createPrint(createString('プレイヤー:')),
    createPrint(createIdentifier('プレイヤー')),
    createPrint(createString('コンピュータ:')),
    createPrint(createIdentifier('コンピュータ')),
    
    {
      type: ASTTypes.IfStatement,
      condition: createComparison(
        '>',
        createIdentifier('プレイヤー'),
        createIdentifier('コンピュータ')
      ),
      then: {
        type: 'BlockStatement',
        body: [createPrint(createString('あなたの勝ち！'))]
      },
      else: {
        type: ASTTypes.IfStatement,
        condition: createComparison(
          '<',
          createIdentifier('プレイヤー'),
          createIdentifier('コンピュータ')
        ),
        then: {
          type: 'BlockStatement',
          body: [createPrint(createString('あなたの負け...'))]
        },
        else: {
          type: 'BlockStatement',
          body: [createPrint(createString('引き分け！'))]
        }
      }
    }
  ]
};
```

「実行するたびに結果が変わる！」

　美久が何度も実行して楽しんでいる。

◇◇◇◇

　休憩時間。美久がお茶を飲みながら言った。

「標準ライブラリって、先人の知恵の結晶だね」

「どういうこと？」

「みんなが必要とする機能を、使いやすくまとめてある」

　その視点に感心する。

「確かに。車輪の再発明を避けられる」

◇◇◇◇

「日付処理も追加しよう」

```javascript
// 日付関連の標準ライブラリ
const dateLib = {
  now: () => Date.now(),
  year: () => new Date().getFullYear(),
  month: () => new Date().getMonth() + 1,
  day: () => new Date().getDate(),
  hour: () => new Date().getHours(),
  minute: () => new Date().getMinutes(),
  second: () => new Date().getSeconds(),
  dayOfWeek: () => new Date().getDay()
};

// 曜日を日本語で返す関数
createFunction(
  '曜日',
  [],
  {
    type: 'BlockStatement',
    body: [
      createAssignment(
        '曜日配列',
        createArray([
          createString('日'),
          createString('月'),
          createString('火'),
          createString('水'),
          createString('木'),
          createString('金'),
          createString('土')
        ])
      ),
      createReturn(
        createArrayAccess(
          createIdentifier('曜日配列'),
          createCall('dayOfWeek', [])
        )
      )
    ]
  }
)
```

◇◇◇◇

　美久がプログラムを書き始めた。

```javascript
// 美久の記念日プログラム
const anniversaryProgram = {
  type: ASTTypes.Program,
  body: [
    // 記念日リスト
    createAssignment(
      '記念日',
      createObject([
        ['初めて会った日', createString('2023-04-22')],
        ['プログラミングを始めた日', createString('2023-04-23')],
        ['手を繋いだ日', createString('2023-05-01')],
        ['告白した日', createString('????-??-??')]
      ])
    ),
    
    createFunction(
      '今日の日付',
      [],
      {
        type: 'BlockStatement',
        body: [
          createReturn(
            createBinaryExpression(
              '+',
              createBinaryExpression(
                '+',
                createBinaryExpression(
                  '+',
                  createBinaryExpression(
                    '+',
                    createCall('year', []),
                    createString('年')
                  ),
                  createCall('month', [])
                ),
                createString('月')
              ),
              createCall('day', [])
            ),
            createString('日')
          )
        ]
      }
    ),
    
    createPrint(createString('今日は')),
    createPrint(createCall('今日の日付', [])),
    createPrint(createString('大切な記念日がまた増えますように...'))
  ]
};
```

「告白した日が????になってる」

　美久の顔が赤くなる。

「だって、まだ……」

「そうだね。でも、きっと近いうちに」

　僕の言葉に、美久の瞳が潤む。

◇◇◇◇

「ゲーム用の便利関数も作ろう」

```javascript
// ゲーム開発用ライブラリ
const gameLib = {
  // 範囲内の整数乱数
  randomInt: (min, max) => {
    return Math.floor(Math.random() * (max - min + 1)) + min;
  },
  
  // 配列からランダムに選択
  randomChoice: (array) => {
    return array[Math.floor(Math.random() * array.length)];
  },
  
  // 配列をシャッフル
  shuffle: (array) => {
    const result = [...array];
    for (let i = result.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [result[i], result[j]] = [result[j], result[i]];
    }
    return result;
  },
  
  // 確率判定
  chance: (probability) => {
    return Math.random() < probability;
  }
};
```

「chanceって面白い」

「50%の確率で成功、みたいな処理に使える」

◇◇◇◇

　夕方になってきた。標準ライブラリの実装も大部分が完成した。

「標準ライブラリがあると、プログラミングが楽になるね」

　美久が満足そうに言う。

「そう。よく使う機能を再利用できる」

「MikuLangが、本当の言語みたいになってきた」

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「明日で、MikuLangは完成？」

「基本的な部分はね。でも、言語は永遠に成長し続ける」

「私たちみたいに？」

　美久の言葉にドキッとする。

「そうだね。一緒に成長していく」

◇◇◇◇

　美久を見送る時、彼女が振り返った。

「明日は何を実装するの？」

「デバッグツール。プログラムの問題を見つけやすくする機能」

「最後の仕上げだね」

　美久が深呼吸をして言った。

「隆弘先輩」

「ん？」

「明日、MikuLangが完成したら……」

　言いかけて、美久は首を振った。

「ううん、なんでもない。明日のお楽しみ」

　そう言って、美久は部屋に入っていった。

　明日。MikuLangの完成の日。

　そして、僕が美久に告白する日。

　胸の高鳴りを抑えながら、僕は明日の準備を始めた。

　標準ライブラリの実装で、MikuLangは実用的な言語になった。

　あとは、最後の仕上げ。そして——