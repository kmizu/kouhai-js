# 第12話 配列とループ

　五月一日、水曜日。ゴールデンウィーク真っ只中。

　今日も美久がやってきた。連日のプログラミング三昧だけど、全く飽きない。むしろ、毎日が楽しみで仕方ない。

「今日は配列だよね」

　美久が期待に満ちた顔で言う。

「そう。データをまとめて扱うための重要な機能」

「配列って、数学で習ったやつとは違うの？」

「基本的な考え方は同じ。複数の値を順番に並べたもの」

◇◇◇◇

「まず、配列のASTから設計しよう」

```javascript
// 配列リテラルのAST
const ArrayLiteral = {
  type: 'ArrayLiteral',
  elements: [
    { type: 'NumberLiteral', value: 1 },
    { type: 'NumberLiteral', value: 2 },
    { type: 'NumberLiteral', value: 3 }
  ]
};

// 配列アクセスのAST
const ArrayAccess = {
  type: 'MemberExpression',
  object: { type: 'Identifier', name: 'numbers' },
  property: { type: 'NumberLiteral', value: 0 }
};
```

「elements に要素が入ってるんだ」

「そう。JavaScriptの配列と同じ構造」

◇◇◇◇

　実装を始める。

```javascript
// ASTTypesに追加
ASTTypes.ArrayLiteral = 'ArrayLiteral';
ASTTypes.MemberExpression = 'MemberExpression';

// ヘルパー関数
function createArray(elements) {
  return {
    type: ASTTypes.ArrayLiteral,
    elements: elements
  };
}

function createArrayAccess(array, index) {
  return {
    type: ASTTypes.MemberExpression,
    object: array,
    property: index
  };
}
```

「MemberExpressionって？」

「メンバーにアクセスする式。配列の要素取得に使う」

◇◇◇◇

「評価器も拡張しよう」

```javascript
// 配列リテラルの評価
evaluateArrayLiteral(node) {
  return node.elements.map(element => this.evaluate(element));
}

// 配列アクセスの評価
evaluateMemberExpression(node) {
  const object = this.evaluate(node.object);
  const property = this.evaluate(node.property);
  
  if (Array.isArray(object)) {
    if (property < 0 || property >= object.length) {
      throw new Error(`配列の範囲外: ${property}`);
    }
    return object[property];
  }
  
  throw new Error('配列ではありません');
}
```

「配列の範囲外エラーも考慮してるんだ」

「エラー処理は大事。プログラムを安全に保つため」

◇◇◇◇

　美久が質問してきた。

「配列って、どんな時に使うの？」

「例えば、ゲームのアイテムリストとか」

　具体的な例を書く。

```javascript
// アイテムリストの例
const itemProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment(
      'アイテム',
      createArray([
        createString('ポーション'),
        createString('エリクサー'),
        createString('フェニックスの尾')
      ])
    ),
    createPrint(createString('所持アイテム:')),
    createPrint(createArrayAccess(
      createIdentifier('アイテム'),
      createNumber(0)
    )),
    createPrint(createArrayAccess(
      createIdentifier('アイテム'),
      createNumber(1)
    ))
  ]
};

mikuLang.run(itemProgram);
// 出力:
// 所持アイテム:
// ポーション
// エリクサー
```

「わあ、RPGっぽい！」

　美久の反応が可愛い。

◇◇◇◇

「配列の長さを取得する機能も追加しよう」

```javascript
// 組み込み関数として長さを追加
function createLength(array) {
  return {
    type: 'LengthExpression',
    array: array
  };
}

// 評価器に追加
evaluateLengthExpression(node) {
  const array = this.evaluate(node.array);
  if (!Array.isArray(array)) {
    throw new Error('配列ではありません');
  }
  return array.length;
}
```

「これで配列の要素数が分かるんだ」

「そう。ループと組み合わせると便利」

◇◇◇◇

　休憩時間。美久がソファで考え込んでいる。

「どうした？」

「配列って、入れ子にできる？」

　また鋭い質問だ。

「できるよ。多次元配列って言うんだ」

```javascript
// 2次元配列の例
const matrix = createArray([
  createArray([createNumber(1), createNumber(2), createNumber(3)]),
  createArray([createNumber(4), createNumber(5), createNumber(6)]),
  createArray([createNumber(7), createNumber(8), createNumber(9)])
]);
```

「行列みたい！」

「まさにそう。ゲームのマップデータとかに使える」

◇◇◇◇

「次は、forループを実装しよう」

　新しいASTを設計する。

```javascript
// forループのAST
const ForStatement = {
  type: 'ForStatement',
  init: {
    type: 'AssignmentExpression',
    left: { type: 'Identifier', name: 'i' },
    right: { type: 'NumberLiteral', value: 0 }
  },
  test: {
    type: 'BinaryExpression',
    operator: '<',
    left: { type: 'Identifier', name: 'i' },
    right: { type: 'NumberLiteral', value: 10 }
  },
  update: {
    type: 'AssignmentExpression',
    left: { type: 'Identifier', name: 'i' },
    right: {
      type: 'BinaryExpression',
      operator: '+',
      left: { type: 'Identifier', name: 'i' },
      right: { type: 'NumberLiteral', value: 1 }
    }
  },
  body: {
    type: 'BlockStatement',
    body: []
  }
};
```

「複雑……」

「一つずつ見ていけば大丈夫」

◇◇◇◇

　forループの評価器を実装する。

```javascript
evaluateForStatement(node) {
  // 初期化
  if (node.init) {
    this.evaluate(node.init);
  }
  
  let result = null;
  
  // ループ
  while (true) {
    // 条件チェック
    if (node.test && !this.evaluate(node.test)) {
      break;
    }
    
    // 本体実行
    result = this.evaluate(node.body);
    
    // 更新
    if (node.update) {
      this.evaluate(node.update);
    }
  }
  
  return result;
}
```

「whileループを使ってforループを実装してるんだ」

「そう。forはwhileの便利な書き方とも言える」

◇◇◇◇

「配列とループを組み合わせてみよう」

```javascript
// 配列の全要素を表示
const arrayLoopProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment(
      '名前リスト',
      createArray([
        createString('隆弘'),
        createString('美久'),
        createString('太郎'),
        createString('花子')
      ])
    ),
    {
      type: 'ForStatement',
      init: createAssignment('i', createNumber(0)),
      test: createComparison(
        '<',
        createIdentifier('i'),
        createLength(createIdentifier('名前リスト'))
      ),
      update: createAssignment(
        'i',
        createBinaryExpression(
          '+',
          createIdentifier('i'),
          createNumber(1)
        )
      ),
      body: {
        type: 'BlockStatement',
        body: [
          createPrint(
            createArrayAccess(
              createIdentifier('名前リスト'),
              createIdentifier('i')
            )
          )
        ]
      }
    }
  ]
};

mikuLang.run(arrayLoopProgram);
// 出力:
// 隆弘
// 美久
// 太郎
// 花子
```

「すごい！　全部表示された！」

　美久が興奮している。

◇◇◇◇

「私も配列を使ったプログラムを書いてみたい」

　美久がキーボードに向かう。

```javascript
// 美久のプログラム：好感度の推移
const mikuArrayProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment(
      '好感度推移',
      createArray([
        createNumber(10),
        createNumber(25),
        createNumber(40),
        createNumber(65),
        createNumber(80),
        createNumber(95)
      ])
    ),
    createAssignment('月', createNumber(1)),
    {
      type: 'ForStatement',
      init: createAssignment('i', createNumber(0)),
      test: createComparison(
        '<',
        createIdentifier('i'),
        createLength(createIdentifier('好感度推移'))
      ),
      update: createAssignment(
        'i',
        createBinaryExpression(
          '+',
          createIdentifier('i'),
          createNumber(1)
        )
      ),
      body: {
        type: 'BlockStatement',
        body: [
          createPrint(createBinaryExpression(
            '+',
            createBinaryExpression(
              '+',
              createString(''),
              createIdentifier('月')
            ),
            createString('月目の好感度:')
          )),
          createPrint(
            createArrayAccess(
              createIdentifier('好感度推移'),
              createIdentifier('i')
            )
          ),
          createAssignment(
            '月',
            createBinaryExpression(
              '+',
              createIdentifier('月'),
              createNumber(1)
            )
          )
        ]
      }
    }
  ]
};
```

「実行してみよう」

```
1月目の好感度:
10
2月目の好感度:
25
3月目の好感度:
40
4月目の好感度:
65
5月目の好感度:
80
6月目の好感度:
95
```

「やった！　グラフみたいに推移が分かる」

　美久の発想力に感心する。

◇◇◇◇

　夕方になってきた。窓の外は夕焼けに染まっている。

「配列とループがあると、できることが一気に増えるね」

　美久が満足そうに言う。

「そう。プログラミングの基本中の基本」

「でも、難しかった」

「美久は十分理解してるよ」

　褒めると、美久が照れる。

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「MikuLangが、どんどん本格的になってきた」

「そうだね」

「私たちが作った言語で、本当にゲームが作れそう」

　美久の目が輝いている。

「きっと作れるよ」

「楽しみ」

◇◇◇◇

　美久を見送る時、玄関で彼女が振り返った。

「明日は何を実装するの？」

「オブジェクトかな。データをまとめる、もう一つの方法」

「難しそう」

「大丈夫。美久なら理解できる」

　美久が嬉しそうに微笑む。

「隆弘先輩がそう言ってくれるなら、頑張れる」

　その笑顔に、胸が熱くなる。

「じゃあ、また明日」

「うん、また明日」

　ドアが閉まった後も、美久の笑顔が頭から離れない。

　配列とループ。プログラミングの基本を、美久は見事に理解した。

　MikuLangも、美久も、そして僕たちの関係も、着実に成長している。

　明日はオブジェクト。さらに高度な概念だけど、美久となら乗り越えられる。

　そんな確信を胸に、僕は明日の準備を始めた。