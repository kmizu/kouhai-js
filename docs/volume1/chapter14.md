# 第14話 エラー処理

　五月三日、金曜日。憲法記念日。

　ゴールデンウィークも後半に入った。美久との日々は、もはや生活の一部になっている。

「今日はエラー処理だね」

　美久がノートを開きながら言う。

「プログラムを安全にする大事な機能」

「エラーって、悪いものじゃないの？」

「エラーは情報。何が間違っているかを教えてくれる」

◇◇◇◇

「まず、try-catch の仕組みを実装しよう」

　ホワイトボードに構造を描く。

```
try {
    // エラーが起きるかもしれない処理
} catch (エラー) {
    // エラーが起きた時の処理
}
```

「あ、昨日まで使ってたやつ」

「そう。今日はこれをMikuLangに実装する」

◇◇◇◇

「try-catch のASTを設計しよう」

```javascript
// try-catch文のAST
const TryStatement = {
  type: 'TryStatement',
  block: {
    type: 'BlockStatement',
    body: []  // tryブロックの中身
  },
  handler: {
    type: 'CatchClause',
    param: { type: 'Identifier', name: 'error' },
    body: {
      type: 'BlockStatement',
      body: []  // catchブロックの中身
    }
  },
  finalizer: null  // finallyブロック（今回は実装しない）
};
```

「handlerがcatchの部分？」

「正解。エラーを捕まえる処理」

◇◇◇◇

　実装を始める。

```javascript
// ASTTypesに追加
ASTTypes.TryStatement = 'TryStatement';
ASTTypes.CatchClause = 'CatchClause';
ASTTypes.ThrowStatement = 'ThrowStatement';

// ヘルパー関数
function createTry(tryBlock, catchParam, catchBlock) {
  return {
    type: ASTTypes.TryStatement,
    block: tryBlock,
    handler: {
      type: ASTTypes.CatchClause,
      param: createIdentifier(catchParam),
      body: catchBlock
    },
    finalizer: null
  };
}

function createThrow(argument) {
  return {
    type: ASTTypes.ThrowStatement,
    argument: argument
  };
}
```

◇◇◇◇

「評価器も拡張しよう」

```javascript
// try-catch文の評価
evaluateTryStatement(node) {
  try {
    // tryブロックを実行
    return this.evaluate(node.block);
  } catch (error) {
    if (node.handler) {
      // エラーを変数として設定
      const catchEnv = new Environment(this.env);
      catchEnv.set(node.handler.param.name, error);
      
      // catchブロックを実行
      const prevEnv = this.env;
      this.env = catchEnv;
      
      try {
        return this.evaluate(node.handler.body);
      } finally {
        this.env = prevEnv;
      }
    }
    // catchがない場合は再スロー
    throw error;
  }
}

// throw文の評価
evaluateThrowStatement(node) {
  const value = this.evaluate(node.argument);
  throw value;
}
```

「環境を切り替えてる」

「catchブロック内でエラー変数を使えるようにするため」

◇◇◇◇

　美久が質問してきた。

「どんな時にエラーを投げるの？」

「例えば、不正な入力があった時とか」

　具体例を書く。

```javascript
const errorProgram = {
  type: ASTTypes.Program,
  body: [
    createFunction(
      '年齢チェック',
      ['年齢'],
      {
        type: 'BlockStatement',
        body: [
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '<',
              createIdentifier('年齢'),
              createNumber(0)
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createThrow(createString('年齢は0以上である必要があります'))
              ]
            }
          },
          createReturn(createString('年齢OK'))
        ]
      }
    ),
    createTry(
      {
        type: 'BlockStatement',
        body: [
          createCall('年齢チェック', [createNumber(-5)])
        ]
      },
      'エラー',
      {
        type: 'BlockStatement',
        body: [
          createPrint(createString('エラーが発生:')),
          createPrint(createIdentifier('エラー'))
        ]
      }
    )
  ]
};
```

◇◇◇◇

　実行してみる。

```
エラーが発生:
年齢は0以上である必要があります
```

「エラーがちゃんと捕まえられた！」

　美久が感動している。

「プログラムがクラッシュせずに、エラーを処理できる」

◇◇◇◇

　休憩時間。美久がお茶を飲みながら言った。

「エラー処理って、人生にも必要だよね」

「どういうこと？」

「失敗した時の対処法を用意しておくって意味で」

　その洞察に感心する。

「確かに。予期しないことは必ず起きるから」

◇◇◇◇

「カスタムエラー型も作ってみよう」

　新しい機能を追加する。

```javascript
// エラーオブジェクトの作成
function createError(type, message) {
  return createObject([
    ['type', createString(type)],
    ['message', createString(message)],
    ['timestamp', createNumber(Date.now())]
  ]);
}

// 使用例
const customErrorProgram = {
  type: ASTTypes.Program,
  body: [
    createFunction(
      'ゲームロード',
      ['セーブデータ'],
      {
        type: 'BlockStatement',
        body: [
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '===',
              createIdentifier('セーブデータ'),
              createString('')
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createThrow(createError(
                  'LoadError',
                  'セーブデータが見つかりません'
                ))
              ]
            }
          },
          createReturn(createString('ロード成功'))
        ]
      }
    )
  ]
};
```

「エラーにも種類があるんだ」

「エラーの種類によって、対処法を変えられる」

◇◇◇◇

　美久がプログラムを書き始めた。

```javascript
// 美久のエラー処理プログラム
const mikuErrorProgram = {
  type: ASTTypes.Program,
  body: [
    createFunction(
      '告白',
      ['好感度'],
      {
        type: 'BlockStatement',
        body: [
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '<',
              createIdentifier('好感度'),
              createNumber(80)
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createThrow(createError(
                  'LoveError',
                  'まだ好感度が足りません'
                ))
              ]
            }
          },
          createReturn(createString('告白成功！おめでとう！'))
        ]
      }
    ),
    createAssignment('現在の好感度', createNumber(70)),
    createTry(
      {
        type: 'BlockStatement',
        body: [
          createAssignment(
            '結果',
            createCall('告白', [createIdentifier('現在の好感度')])
          ),
          createPrint(createIdentifier('結果'))
        ]
      },
      'e',
      {
        type: 'BlockStatement',
        body: [
          createPrint(createString('告白失敗...')),
          createPrint(createPropertyAccess(
            createIdentifier('e'),
            'message'
          )),
          createPrint(createString('もっと頑張ろう！'))
        ]
      }
    )
  ]
};
```

「実行してみよう」

```
告白失敗...
まだ好感度が足りません
もっと頑張ろう！
```

「ゲームっぽい！」

　美久の発想力はいつも面白い。

◇◇◇◇

「複数のcatchも実装できる」

　より高度な例を示す。

```javascript
// 複数のエラータイプを処理
const multiCatchProgram = {
  type: ASTTypes.Program,
  body: [
    createFunction(
      'ゲーム進行',
      ['コマンド'],
      {
        type: 'BlockStatement',
        body: [
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '===',
              createIdentifier('コマンド'),
              createString('save')
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createThrow(createError('SaveError', 'セーブに失敗しました'))
              ]
            }
          },
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '===',
              createIdentifier('コマンド'),
              createString('load')
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createThrow(createError('LoadError', 'ロードに失敗しました'))
              ]
            }
          },
          createReturn(createString('コマンド実行成功'))
        ]
      }
    )
  ]
};
```

◇◇◇◇

　夕方になってきた。エラー処理の実装も順調に進んだ。

「エラー処理、思ったより奥が深い」

　美久が感想を述べる。

「プログラムの品質を上げる重要な要素だから」

「失敗を想定して、対策を用意する。人生の教訓みたい」

　美久の言葉に、また感心する。

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「MikuLangが、どんどん実用的になってきた」

「そうだね。エラー処理があれば、安全なプログラムが書ける」

「私たちの言語で、本当にゲームが作れそう」

　美久の期待に満ちた表情が嬉しい。

「明日は何を実装する？」

「文字列処理かな。テキストを扱う機能」

「楽しみ！」

◇◇◇◇

　美久を見送った後、今日のコードを振り返る。

　エラー処理の実装で、MikuLangはより堅牢な言語になった。

　美久の理解も、日に日に深まっている。

　そして、二人の距離も——

（そろそろ、ちゃんと伝えないと）

　ゴールデンウィークも残りわずか。

　この特別な時間の中で、美久に気持ちを伝えたい。

　プログラミング言語の完成と同時に、僕たちの関係も新しいステージに進めたら——

　そんなことを考えながら、明日の準備を始めた。