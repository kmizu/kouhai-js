# 第9話 評価器（Evaluator）の仕組み

　四月二十八日、日曜日。午後二時。

　今日も美久が時間通りにやってきた。

「お邪魔します」

「いらっしゃい」

　昨日とは違って、今日の美久はジーンズにTシャツというカジュアルな格好だ。リラックスした雰囲気がいい。

「昨日の続き、楽しみにしてた」

「僕も」

　正直、週末に美久と過ごす時間が、何よりも楽しみになっていた。

◇◇◇◇

「今日は評価器を作るんだよね」

　美久が昨日のコードを開きながら言った。

「そう。ASTを実際に実行する部分」

「評価器って、どんな役割なの？」

　僕はホワイトボードに図を描いた。

```
AST → 評価器（Evaluator） → 結果
```

「ASTは設計図みたいなもの。評価器は、その設計図を読んで実際に作業をする職人さん」

「なるほど！　料理のレシピと料理人みたいな関係？」

　また的確な例えだ。

◇◇◇◇

「じゃあ、昨日作ったEnvironmentクラスを使って、評価器を実装していこう」

```javascript
class Evaluator {
  constructor(env = new Environment()) {
    this.env = env;
  }
  
  evaluate(node) {
    switch (node.type) {
      case ASTTypes.Program:
        return this.evaluateProgram(node);
      case ASTTypes.NumberLiteral:
        return node.value;
      case ASTTypes.StringLiteral:
        return node.value;
      case ASTTypes.BooleanLiteral:
        return node.value;
      case ASTTypes.Identifier:
        return this.env.get(node.name);
      case ASTTypes.BinaryExpression:
        return this.evaluateBinaryExpression(node);
      case ASTTypes.AssignmentExpression:
        return this.evaluateAssignment(node);
      default:
        throw new Error(`Unknown node type: ${node.type}`);
    }
  }
}
```

「クラスで作るんだ」

「そう。状態（Environment）を持つから、クラスの方が管理しやすい」

　美久が真剣にコードを見つめている。

◇◇◇◇

「次は、各評価メソッドを実装していこう」

```javascript
evaluateProgram(node) {
  let result = null;
  for (const statement of node.body) {
    result = this.evaluate(statement);
  }
  return result;
}

evaluateBinaryExpression(node) {
  const left = this.evaluate(node.left);
  const right = this.evaluate(node.right);
  
  switch (node.operator) {
    case '+':
      return left + right;
    case '-':
      return left - right;
    case '*':
      return left * right;
    case '/':
      if (right === 0) throw new Error('ゼロ除算エラー');
      return left / right;
    case '>':
      return left > right;
    case '<':
      return left < right;
    case '>=':
      return left >= right;
    case '<=':
      return left <= right;
    case '===':
      return left === right;
    case '!==':
      return left !== right;
    default:
      throw new Error(`Unknown operator: ${node.operator}`);
  }
}
```

「わあ、演算子がたくさん」

「基本的な演算子は全部サポートしよう」

◇◇◇◇

　作業を続けていると、美久が質問してきた。

「ねえ、隆弘先輩」

「ん？」

「代入はどうやって評価するの？」

「いい質問。やってみよう」

```javascript
evaluateAssignment(node) {
  const value = this.evaluate(node.right);
  const name = node.left.name;
  this.env.set(name, value);
  return value;
}
```

「シンプル！」

「代入は、右辺を評価して、その値を環境に保存するだけ」

　美久がメモを取る。

「でも、これだけ？」

「基本はこれだけ。でも、エラー処理とか、型チェックとか、本格的な言語にはもっと機能がある」

◇◇◇◇

「実際に動かしてみよう」

　僕は昨日のASTを使って、評価器をテストした。

```javascript
// 「好感度 を 0 にする」
const ast1 = createAssignment('好感度', createNumber(0));

const evaluator = new Evaluator();
console.log(evaluator.evaluate(ast1)); // 0
console.log(evaluator.env.get('好感度')); // 0

// 「好感度 を 10 にする」
const ast2 = createAssignment('好感度', createNumber(10));
console.log(evaluator.evaluate(ast2)); // 10
console.log(evaluator.env.get('好感度')); // 10
```

「おお、変数の値が更新されてる！」

　美久が感動している。

「これが状態を持つプログラムの基本」

◇◇◇◇

「ちょっと休憩しよう」

　三時になったので、お茶を入れることにした。

「手伝う」

　美久がキッチンについてくる。

「大丈夫だよ」

「でも、お邪魔してるし」

　狭いキッチンで二人並んでお茶の準備をする。距離が近くて、なんだかドキドキする。

「隆弘先輩、料理とかするの？」

「一人暮らしだから、そこそこは」

「へー、意外」

「意外？」

「研究ばっかりしてるイメージだったから」

　美久の中の僕のイメージに、少し苦笑する。

◇◇◇◇

　リビングに戻って、お茶を飲みながら話す。

「プログラミング言語って、思ってたより生き物みたい」

　美久がポツリと言った。

「生き物？」

「だって、状態を持って、それが変化していくでしょ？」

　その洞察に驚く。

「確かに。プログラムは生きているとも言える」

「MikuLangも、生きてる感じがする」

　美久が嬉しそうに言う。その表情を見ていると、こちらまで幸せな気持ちになる。

◇◇◇◇

「さて、続きをやろうか」

「はい！」

　今度は、もっと複雑な例を作ってみる。

```javascript
// if文の評価
evaluateIfStatement(node) {
  const condition = this.evaluate(node.condition);
  
  if (condition) {
    return this.evaluate(node.then);
  } else if (node.else) {
    return this.evaluate(node.else);
  }
  
  return null;
}

// ブロック文の評価
evaluateBlockStatement(node) {
  let result = null;
  for (const statement of node.body) {
    result = this.evaluate(statement);
  }
  return result;
}
```

「if文もできるんだ」

「そう。条件分岐は言語の基本機能」

◇◇◇◇

　実際にif文を含むプログラムを作ってみる。

```javascript
// 「もしも 好感度 が 80 以上 ならば」
const ifAst = {
  type: ASTTypes.Program,
  body: [
    createAssignment('好感度', createNumber(85)),
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
          createAssignment('結果', createString('告白成功！'))
        ]
      },
      else: {
        type: 'BlockStatement',
        body: [
          createAssignment('結果', createString('もっと頑張ろう'))
        ]
      }
    }
  ]
};

const evaluator2 = new Evaluator();
evaluator2.evaluate(ifAst);
console.log(evaluator2.env.get('結果')); // 告白成功！
```

「やった！　条件分岐が動いた！」

　美久が手を叩いて喜ぶ。

「これで本格的なプログラムが書けるようになったね」

◇◇◇◇

　しばらく作業を続けていると、美久がふと手を止めた。

「ねえ、隆弘先輩」

「どうした？」

「print文も実装したい」

「いいね。やってみよう」

```javascript
// print文の評価
evaluatePrintStatement(node) {
  const value = this.evaluate(node.value);
  console.log(value);
  return value;
}

// ヘルパー関数
function createPrint(value) {
  return {
    type: ASTTypes.PrintStatement,
    value: value
  };
}
```

「これで出力もできる」

◇◇◇◇

　完成した評価器で、簡単なプログラムを実行してみる。

```javascript
const program = {
  type: ASTTypes.Program,
  body: [
    createAssignment('名前', createString('美久')),
    createAssignment('好感度', createNumber(0)),
    createPrint(createString('ゲーム開始！')),
    createPrint(createIdentifier('名前')),
    createAssignment(
      '好感度',
      createBinaryExpression(
        '+',
        createIdentifier('好感度'),
        createNumber(10)
      )
    ),
    createPrint(createIdentifier('好感度'))
  ]
};

const evaluator3 = new Evaluator();
evaluator3.evaluate(program);
// 出力:
// ゲーム開始！
// 美久
// 10
```

「すごい！　本当のプログラムみたい！」

　美久の興奮が伝わってくる。

◇◇◇◇

　夕方になり、外が薄暗くなってきた。

「今日もあっという間だったね」

「うん」

　美久が片付けながら言った。

「でも、すごく充実してた」

「MikuLangが、どんどん本物の言語になっていく」

「そうだね」

　二人で窓の外を眺める。夕焼けが綺麗だ。

「隆弘先輩」

「ん？」

「明日も……来ていい？」

　遠慮がちな質問に、胸が熱くなる。

「もちろん。むしろ来てほしい」

　美久の顔が、夕日に照らされて赤く染まる。それとも、照れているのだろうか。

◇◇◇◇

　美久を送り出した後、一人でコードを見返していた。

　評価器ができて、MikuLangは本当に動く言語になった。まだ機能は少ないけれど、確実に成長している。

　そして、美久との関係も——

　明日は何を実装しよう。ループ？　関数？　それとも——

　美久の喜ぶ顔を想像しながら、僕は明日の準備を始めた。

　プログラミング言語を作ることが、こんなに楽しいなんて。

　いや、美久と一緒だから楽しいのかもしれない。

　そんなことを考えながら、僕は夜遅くまでコードを書き続けた。