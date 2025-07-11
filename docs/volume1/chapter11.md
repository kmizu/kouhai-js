# 第11話 関数を作る

　四月三十日、火曜日。振替休日。

　ゴールデンウィークの初日も、美久と一緒にプログラミング。もはや日課になりつつある。

「今日は関数を実装するんだよね」

　美久が期待に満ちた顔で言う。

「そう。プログラミングの醍醐味の一つ」

「関数って、数学の関数とは違うの？」

　いい質問だ。

「基本的な考え方は同じ。入力を受け取って、出力を返す」

◇◇◇◇

「まず、関数の定義から始めよう」

　ホワイトボードに図を描く。

```
関数 挨拶(名前) {
    「こんにちは、」 + 名前 + 「さん」 を表示
}
```

「わあ、日本語で書くとわかりやすい」

「MikuLangらしくていいでしょ？」

　美久が嬉しそうに頷く。

◇◇◇◇

「じゃあ、ASTの構造を考えよう」

```javascript
// 関数定義のAST
const FunctionDeclaration = {
  type: 'FunctionDeclaration',
  name: '挨拶',
  params: ['名前'],
  body: {
    type: 'BlockStatement',
    body: [
      // 関数の中身
    ]
  }
};

// 関数呼び出しのAST
const FunctionCall = {
  type: 'CallExpression',
  callee: '挨拶',
  arguments: [
    { type: 'StringLiteral', value: '美久' }
  ]
};
```

「関数定義と関数呼び出し、別々のASTなんだ」

「そう。定義は設計図、呼び出しは実行命令」

◇◇◇◇

　実際に実装を始める。

```javascript
// ASTTypesに追加
ASTTypes.FunctionDeclaration = 'FunctionDeclaration';
ASTTypes.CallExpression = 'CallExpression';
ASTTypes.ReturnStatement = 'ReturnStatement';

// ヘルパー関数
function createFunction(name, params, body) {
  return {
    type: ASTTypes.FunctionDeclaration,
    name: name,
    params: params,
    body: body
  };
}

function createCall(name, args) {
  return {
    type: ASTTypes.CallExpression,
    callee: name,
    arguments: args
  };
}

function createReturn(value) {
  return {
    type: ASTTypes.ReturnStatement,
    value: value
  };
}
```

「returnって何？」

「関数から値を返すための命令」

　美久がメモを取る。

◇◇◇◇

「次は評価器を拡張しよう」

　僕は慎重にコードを書いていく。

```javascript
// 関数を保存するための環境を拡張
class Environment {
  constructor(parent = null) {
    this.variables = {};
    this.functions = {};
    this.parent = parent;
  }
  
  setFunction(name, func) {
    this.functions[name] = func;
  }
  
  getFunction(name) {
    if (name in this.functions) {
      return this.functions[name];
    }
    if (this.parent) {
      return this.parent.getFunction(name);
    }
    throw new Error(`関数 '${name}' が見つかりません`);
  }
}
```

「parent？」

「スコープのため。関数の中と外で変数を分けるんだ」

◇◇◇◇

　美久が首を傾げる。

「スコープって？」

「変数の有効範囲のこと」

　簡単な例を書いて説明する。

```javascript
// グローバルスコープ
let x = 10;

function test() {
  // ローカルスコープ
  let x = 20;
  console.log(x); // 20
}

console.log(x); // 10
```

「なるほど！　同じ名前でも別の変数になるんだ」

「そう。これがスコープの重要性」

◇◇◇◇

　評価器に関数の処理を追加する。

```javascript
// 関数定義の評価
evaluateFunctionDeclaration(node) {
  const func = {
    params: node.params,
    body: node.body,
    closure: this.env // クロージャ
  };
  this.env.setFunction(node.name, func);
  return null;
}

// 関数呼び出しの評価
evaluateCallExpression(node) {
  const func = this.env.getFunction(node.callee);
  
  // 引数を評価
  const args = node.arguments.map(arg => this.evaluate(arg));
  
  // 新しい環境を作成
  const funcEnv = new Environment(func.closure);
  
  // パラメータに引数を割り当て
  func.params.forEach((param, i) => {
    funcEnv.set(param, args[i]);
  });
  
  // 関数本体を実行
  const prevEnv = this.env;
  this.env = funcEnv;
  
  try {
    this.evaluate(func.body);
    return null; // return文がない場合
  } catch (e) {
    if (e.type === 'RETURN') {
      return e.value;
    }
    throw e;
  } finally {
    this.env = prevEnv;
  }
}
```

「複雑……」

　美久が困った顔をする。

「一つずつ理解していけば大丈夫」

◇◇◇◇

　休憩時間。美久がソファで質問してきた。

「クロージャって何？」

「関数が定義された時の環境を記憶する仕組み」

「どういうこと？」

　例を書いて説明する。

```javascript
function makeCounter() {
  let count = 0;
  
  return function() {
    count++;
    return count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

「わあ、countの値が保持されてる」

「これがクロージャの力」

◇◇◇◇

「実際に関数を使ったプログラムを書いてみよう」

```javascript
const funcProgram = {
  type: ASTTypes.Program,
  body: [
    // 関数定義
    createFunction(
      '二乗',
      ['x'],
      {
        type: 'BlockStatement',
        body: [
          createReturn(
            createBinaryExpression(
              '*',
              createIdentifier('x'),
              createIdentifier('x')
            )
          )
        ]
      }
    ),
    // 関数呼び出し
    createAssignment(
      '結果',
      createCall('二乗', [createNumber(5)])
    ),
    createPrint(createIdentifier('結果'))
  ]
};

mikuLang.run(funcProgram);
// 出力: 25
```

「動いた！」

　美久が歓声を上げる。

◇◇◇◇

「私も関数を作ってみたい」

　美久がキーボードに向かう。

```javascript
// 美久の関数
createFunction(
  '好感度チェック',
  ['値'],
  {
    type: 'BlockStatement',
    body: [
      {
        type: ASTTypes.IfStatement,
        condition: createComparison(
          '>=',
          createIdentifier('値'),
          createNumber(80)
        ),
        then: {
          type: 'BlockStatement',
          body: [
            createReturn(createString('大好き'))
          ]
        },
        else: {
          type: 'BlockStatement',
          body: [
            createReturn(createString('もっと頑張って'))
          ]
        }
      }
    ]
  }
)
```

「かわいい関数だね」

「えへへ」

　美久が照れる。

◇◇◇◇

　実行してみる。

```javascript
// 好感度チェック関数を使う
createAssignment(
  'メッセージ',
  createCall('好感度チェック', [createNumber(85)])
),
createPrint(createIdentifier('メッセージ'))
// 出力: 大好き
```

「やった！」

　美久が喜ぶ姿を見て、僕も嬉しくなる。

◇◇◇◇

「ねえ、隆弘先輩」

　作業を続けながら、美久が言った。

「再帰関数も作れる？」

　また鋭い質問だ。

「作れるよ。試してみよう」

```javascript
// 階乗を計算する再帰関数
const factorialProgram = {
  type: ASTTypes.Program,
  body: [
    createFunction(
      '階乗',
      ['n'],
      {
        type: 'BlockStatement',
        body: [
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '<=',
              createIdentifier('n'),
              createNumber(1)
            ),
            then: {
              type: 'BlockStatement',
              body: [createReturn(createNumber(1))]
            },
            else: {
              type: 'BlockStatement',
              body: [
                createReturn(
                  createBinaryExpression(
                    '*',
                    createIdentifier('n'),
                    createCall(
                      '階乗',
                      [createBinaryExpression(
                        '-',
                        createIdentifier('n'),
                        createNumber(1)
                      )]
                    )
                  )
                )
              ]
            }
          }
        ]
      }
    ),
    createPrint(createCall('階乗', [createNumber(5)]))
  ]
};

mikuLang.run(factorialProgram);
// 出力: 120
```

「すごい！　自分で自分を呼んでる」

　美久の理解の速さに感心する。

◇◇◇◇

　夕方になってきた。窓の外はオレンジ色に染まっている。

「関数があると、プログラムがすごく便利になるね」

　美久が満足そうに言う。

「そう。同じ処理を何度も書かなくて済む」

「DRY原則？」

「よく知ってるね」

「Don't Repeat Yourself。昨日調べた」

　美久の向学心に、また感心する。

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「MikuLangで、もっと複雑なプログラムが書けるようになってきた」

「そうだね」

「でも、まだ配列がないよね」

　鋭い指摘だ。

「明日は配列を実装しよう」

「やった！」

　美久の期待に応えたくなる。

◇◇◇◇

　美久を見送った後、一人で今日のコードを見返す。

　関数の実装は、言語にとって大きな一歩だ。これで本格的なプログラムが書けるようになる。

　そして、美久のプログラミング理解も、着実に深まっている。

　クロージャ、スコープ、再帰——難しい概念も、しっかり理解している。

　明日は配列。データ構造の基本だ。

　美久なら、きっとすぐに理解してくれるだろう。

　そんな確信と期待を胸に、僕は明日の準備を始めた。

　MikuLangも、美久との関係も、順調に成長している。

　このゴールデンウィークが、特別な時間になりそうだ。