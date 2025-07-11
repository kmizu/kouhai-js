# 第15話 文字列処理

　五月四日、土曜日。みどりの日。

　ゴールデンウィークも残すところあと三日。美久との時間が、かけがえのないものになっている。

「今日は文字列処理だね」

　美久がいつものように準備万端で言う。

「テキストを扱う機能。ゲーム作りには必須」

「文字列って、今まで使ってたStringLiteralとは違うの？」

「基本は同じ。今日はその操作方法を増やす」

◇◇◇◇

「文字列の基本操作から始めよう」

　ホワイトボードに例を書く。

```javascript
// 文字列の連結
"Hello, " + "World!"

// 文字列の長さ
"美久".length // 2

// 文字列の一部を取得
"プログラミング".substring(0, 4) // "プログ"
```

「JavaScriptの機能をMikuLangにも実装するんだ」

「そう。便利な機能は取り入れていく」

◇◇◇◇

「文字列操作のASTを設計しよう」

```javascript
// 文字列メソッド呼び出しのAST
const StringMethod = {
  type: 'CallExpression',
  callee: {
    type: 'MemberExpression',
    object: { type: 'StringLiteral', value: 'こんにちは' },
    property: { type: 'Identifier', name: 'length' },
    computed: false
  },
  arguments: []
};

// 文字列の連結
const StringConcat = {
  type: 'BinaryExpression',
  operator: '+',
  left: { type: 'StringLiteral', value: 'Hello, ' },
  right: { type: 'StringLiteral', value: 'World!' }
};
```

「メソッド呼び出しは関数と似てる」

「オブジェクトのメソッドと同じ仕組み」

◇◇◇◇

　実装を始める。

```javascript
// 組み込み文字列メソッド
const stringMethods = {
  length: (str) => str.length,
  toUpperCase: (str) => str.toUpperCase(),
  toLowerCase: (str) => str.toLowerCase(),
  substring: (str, start, end) => str.substring(start, end),
  indexOf: (str, search) => str.indexOf(search),
  replace: (str, search, replace) => str.replace(search, replace)
};

// 評価器の拡張
evaluateMemberExpression(node) {
  const object = this.evaluate(node.object);
  
  // 文字列の場合
  if (typeof object === 'string') {
    const methodName = node.property.name;
    if (methodName in stringMethods) {
      return (args) => {
        const evaluatedArgs = args.map(arg => this.evaluate(arg));
        return stringMethods[methodName](object, ...evaluatedArgs);
      };
    }
    // lengthプロパティ
    if (methodName === 'length') {
      return object.length;
    }
  }
  
  // 既存の処理...
}
```

◇◇◇◇

　美久が質問してきた。

「どんな時に文字列処理を使うの？」

「例えば、プレイヤーの名前を入力してもらって、それを使うとか」

　具体例を書く。

```javascript
const nameProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment('プレイヤー名', createString('美久')),
    createAssignment(
      '挨拶',
      createBinaryExpression(
        '+',
        createString('こんにちは、'),
        createBinaryExpression(
          '+',
          createIdentifier('プレイヤー名'),
          createString('さん！')
        )
      )
    ),
    createPrint(createIdentifier('挨拶')),
    
    // 名前の長さをチェック
    createAssignment(
      '名前の長さ',
      createPropertyAccess(
        createIdentifier('プレイヤー名'),
        'length'
      )
    ),
    createPrint(createString('名前の文字数:')),
    createPrint(createIdentifier('名前の長さ'))
  ]
};

mikuLang.run(nameProgram);
// 出力:
// こんにちは、美久さん！
// 名前の文字数:
// 2
```

「名前を組み込んで、カスタマイズされたメッセージが作れる！」

◇◇◇◇

　休憩時間。美久がお茶を飲みながら言った。

「文字列って、コミュニケーションの基本だよね」

「どういうこと？」

「言葉で気持ちを伝えるのも、文字列の組み合わせ」

　その発想に感心する。

「確かに。プログラムも人とのコミュニケーション」

◇◇◇◇

「文字列の検索と置換も実装しよう」

```javascript
// 文字列検索と置換の例
const textProcessProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment(
      'メッセージ',
      createString('隆弘先輩が大好きです')
    ),
    
    // 文字列に特定の単語が含まれているかチェック
    createFunction(
      '含まれているか',
      ['文字列', '検索語'],
      {
        type: 'BlockStatement',
        body: [
          createReturn(
            createComparison(
              '>=',
              createCall('indexOf', [
                createIdentifier('文字列'),
                createIdentifier('検索語')
              ]),
              createNumber(0)
            )
          )
        ]
      }
    ),
    
    // チェック
    createAssignment(
      '結果',
      createCall('含まれているか', [
        createIdentifier('メッセージ'),
        createString('好き')
      ])
    ),
    
    createPrint(createIdentifier('結果')) // true
  ]
};
```

「メッセージに『好き』が含まれてるかチェックしてる」

「そう。テキストアドベンチャーゲームで、キーワード検索に使える」

◇◇◇◇

　美久がプログラムを書き始めた。

```javascript
// 美久の文字列処理プログラム
const mikuStringProgram = {
  type: ASTTypes.Program,
  body: [
    // テンプレート機能
    createFunction(
      'メッセージ生成',
      ['名前', '好感度'],
      {
        type: 'BlockStatement',
        body: [
          createAssignment('基本メッセージ', createString('')),
          
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '>=',
              createIdentifier('好感度'),
              createNumber(80)
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createAssignment(
                  '基本メッセージ',
                  createBinaryExpression(
                    '+',
                    createIdentifier('名前'),
                    createString('先輩、ずっと一緒にいたいです...')
                  )
                )
              ]
            },
            else: {
              type: ASTTypes.IfStatement,
              condition: createComparison(
                '>=',
                createIdentifier('好感度'),
                createNumber(50)
              ),
              then: {
                type: 'BlockStatement',
                body: [
                  createAssignment(
                    '基本メッセージ',
                    createBinaryExpression(
                      '+',
                      createIdentifier('名前'),
                      createString('先輩といると楽しいです')
                    )
                  )
                ]
              },
              else: {
                type: 'BlockStatement',
                body: [
                  createAssignment(
                    '基本メッセージ',
                    createBinaryExpression(
                      '+',
                      createIdentifier('名前'),
                      createString('先輩、よろしくお願いします')
                    )
                  )
                ]
              }
            }
          },
          
          createReturn(createIdentifier('基本メッセージ'))
        ]
      }
    ),
    
    // 使用例
    createPrint(createCall('メッセージ生成', [
      createString('隆弘'),
      createNumber(85)
    ]))
  ]
};

mikuLang.run(mikuStringProgram);
// 出力: 隆弘先輩、ずっと一緒にいたいです...
```

「好感度によってメッセージが変わる！」

　美久の顔が赤くなる。

「実際のゲームみたいでしょ？」

◇◇◇◇

「文字列の分割と結合も便利」

```javascript
// split と join の実装
createFunction(
  'セリフ分割',
  ['長文'],
  {
    type: 'BlockStatement',
    body: [
      // 「。」で分割
      createAssignment(
        '文リスト',
        createCall('split', [
          createIdentifier('長文'),
          createString('。')
        ])
      ),
      
      // 各文を処理
      {
        type: 'ForStatement',
        init: createAssignment('i', createNumber(0)),
        test: createComparison(
          '<',
          createIdentifier('i'),
          createLength(createIdentifier('文リスト'))
        ),
        update: createAssignment(
          'i',
          createBinaryExpression('+', createIdentifier('i'), createNumber(1))
        ),
        body: {
          type: 'BlockStatement',
          body: [
            createPrint(createArrayAccess(
              createIdentifier('文リスト'),
              createIdentifier('i')
            ))
          ]
        }
      }
    ]
  }
)
```

◇◇◇◇

　夕方になってきた。文字列処理の実装も順調に進んだ。

「文字列処理、ゲーム作りには欠かせないね」

　美久が満足そうに言う。

「テキストはゲームの命。特にノベルゲームでは」

「物語を紡ぐのも、プログラミングなんだね」

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「MikuLangで、本当に物語が書けるようになってきた」

「そうだね。文字列処理があれば、複雑なテキスト処理もできる」

「私たちの物語も、きっと素敵なものになる」

　美久の言葉の意味を考える。

　ゲームの物語？　それとも——

◇◇◇◇

　美久を見送る時、彼女が振り返った。

「明日は何を実装するの？」

「標準ライブラリかな。便利な機能をまとめたもの」

「楽しみ」

　美久が少し躊躇してから言った。

「隆弘先輩」

「ん？」

「好感度85のメッセージ、本当の気持ちです」

　そう言って、美久は急いで部屋に入っていった。

　僕は、しばらくその場に立ち尽くした。

（ずっと一緒にいたい、か）

　僕も同じ気持ちだ。

　明日、いや、ゴールデンウィークが終わる前に、ちゃんと伝えよう。

　文字列では表現しきれない、本当の気持ちを。