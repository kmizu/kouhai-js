# 第6話 ツリー構造の不思議

　四月二十五日、木曜日。放課後。

　今日は美久が珍しく遅刻してきた。

「ごめんなさい！　委員会が長引いて」

　息を切らせながら駆け込んでくる美久。

「大丈夫。まだ準備してたところだから」

「本当？　良かった……」

　美久が安堵の表情を浮かべる。その頬がほんのり赤いのは、走ってきたからだろうか。

「今日は、昨日の続きでASTのバリエーションを増やすって言ってたよね」

「覚えててくれたんだ」

「当たり前でしょ。ちゃんと予習もしてきたよ」

　美久がノートを広げる。単項演算子や真偽値について、自分なりに調べてきたらしい。

◇◇◇◇

「じゃあ、まずは真偽値から始めよう」

　僕は新しいコードを書き始めた。

```javascript
// 真偽値リテラルのAST
function createBooleanLiteral(value) {
  return {
    type: 'BooleanLiteral',
    value: value
  };
}

// 評価関数に追加
function evaluate(node) {
  if (node.type === 'NumberLiteral') {
    return node.value;
  }
  
  if (node.type === 'StringLiteral') {
    return node.value;
  }
  
  if (node.type === 'BooleanLiteral') {
    return node.value;
  }
  
  // ... 他の処理
}
```

「真偽値って、trueとfalseのことだよね」

「そう。プログラミングでは『真』と『偽』を使って条件を表現するんだ」

　美久がメモを取る。

「でも、これだけじゃ面白くないよね」

「え？」

「だって、ただtrueやfalseを返すだけでしょ？」

　美久の指摘は的確だ。

「じゃあ、比較演算を実装してみよう」

◇◇◇◇

```javascript
// 比較演算のAST
const comparison = {
  type: 'BinaryExpression',
  operator: '>',
  left: createNumberLiteral(5),
  right: createNumberLiteral(3)
};

// 評価関数を拡張
switch (node.operator) {
  case '+':
    return left + right;
  case '-':
    return left - right;
  case '*':
    return left * right;
  case '/':
    if (right === 0) throw new Error('Division by zero');
    return left / right;
  case '>':
    return left > right;
  case '<':
    return left < right;
  case '===':
    return left === right;
  case '!==':
    return left !== right;
  default:
    throw new Error(`Unknown operator: ${node.operator}`);
}
```

「おお、比較もできるようになった！」

　実行してみると、ちゃんと true が返ってくる。

「これで条件分岐が作れるようになるね」

　美久の発言に驚く。まだ教えていないのに、次のステップを予測している。

◇◇◇◇

「次は単項演算子をやってみよう」

「単項演算子って、値を一つだけ取るやつだよね」

「そう。例えばマイナス記号とか」

```javascript
// 単項演算のAST
function createUnaryExpression(operator, argument) {
  return {
    type: 'UnaryExpression',
    operator: operator,
    argument: argument
  };
}

// -5 を表現
const negativeNumber = createUnaryExpression(
  '-',
  createNumberLiteral(5)
);

// 評価関数に追加
if (node.type === 'UnaryExpression') {
  const argument = evaluate(node.argument);
  
  switch (node.operator) {
    case '-':
      return -argument;
    case '!':
      return !argument;
    default:
      throw new Error(`Unknown unary operator: ${node.operator}`);
  }
}
```

「なるほど、二項演算と似てるけど、引数が一つなんだ」

「その通り。構造は似てるけど、微妙に違う」

　美久がコードを見つめながら考え込む。

「ねえ、隆弘先輩」

「ん？」

「ASTって、どんどん複雑になっていくけど、基本的な考え方は同じだよね」

◇◇◇◇

「いい気づきだね」

　僕は美久の洞察力に感心する。

「実は、それがASTの美しさなんだ。どんな複雑な式も、シンプルなルールの組み合わせで表現できる」

「レゴブロックみたい」

　美久がまた的確な例えを出す。

「そう。小さな部品を組み合わせて、大きな構造を作る」

　僕はホワイトボードに新しい図を描き始めた。

```
        !
        |
        >
       / \
      x   0
```

「これは !（x > 0）のAST」

「わあ、入れ子になってる！」

「単項演算子の中に二項演算子が入ってる。こうやって組み合わせることで、複雑な式も表現できるんだ」

◇◇◇◇

　しばらく作業を続けていると、美久が突然声を上げた。

「あ、分かった！」

「何が？」

「ASTって、入れ子人形みたい」

　また面白い例えだ。

「マトリョーシカ？」

「そう！　大きな人形の中に小さな人形が入ってて、その中にまた……」

　美久が手でジェスチャーをしながら説明する。

「確かに、再帰的な構造という意味では似てるかも」

「再帰的？」

「自分の中に自分と同じ構造が含まれること」

　美久が首を傾げる。

「うーん、難しい」

「じゃあ、実際に見てみよう」

◇◇◇◇

　僕は printAst 関数を改良して、もっと視覚的に分かりやすくした。

```javascript
function printAstVisual(node, indent = '', isLast = true) {
  const prefix = indent + (isLast ? '└── ' : '├── ');
  
  if (node.type === 'NumberLiteral') {
    console.log(prefix + `Number: ${node.value}`);
  } else if (node.type === 'BooleanLiteral') {
    console.log(prefix + `Boolean: ${node.value}`);
  } else if (node.type === 'BinaryExpression') {
    console.log(prefix + `Binary: ${node.operator}`);
    const newIndent = indent + (isLast ? '    ' : '│   ');
    printAstVisual(node.left, newIndent, false);
    printAstVisual(node.right, newIndent, true);
  } else if (node.type === 'UnaryExpression') {
    console.log(prefix + `Unary: ${node.operator}`);
    const newIndent = indent + (isLast ? '    ' : '│   ');
    printAstVisual(node.argument, newIndent, true);
  }
}
```

　複雑な式を表示してみる。

```
└── Binary: &&
    ├── Binary: >
    │   ├── Number: 10
    │   └── Number: 5
    └── Binary: <
        ├── Number: 20
        └── Number: 30
```

「すごい！　本当に木みたい！」

　美久が目を輝かせる。

◇◇◇◇

「ねえ、隆弘先輩」

　休憩中、美久が窓の外を眺めながら言った。

「プログラミング言語を作るって、思ってたより創造的なんだね」

「どういうこと？」

「だって、新しい表現方法を作ってるわけでしょ？　芸術みたい」

　その発想は新鮮だった。

「確かに、言語設計にはセンスが必要かもしれない」

「隆弘先輩のセンス、好きだよ」

　さらりと言われた言葉に、心臓が跳ねる。

「え？」

「あ、いや、プログラミングのセンスが！」

　美久が慌てて言い直す。その顔が真っ赤だ。

（今のは……）

　気まずい沈黙が流れる。

◇◇◇◇

「そ、それより」

　僕は咳払いをして話題を変えた。

「明日はもっと面白いことをやろうと思うんだ」

「面白いこと？」

「パーサーは作らないって言ったけど、簡単な構文を定義してみようかなって」

　美久の目が輝く。

「構文？」

「MikuLangの見た目を決めるんだ。どんな風に書くか」

「わあ、楽しみ！」

　美久の反応に、僕も楽しくなってくる。

「例えば、日本語を使えるようにするとか」

「本当に？」

「美久が言ってたでしょ。日本語で書けるプログラミング言語って」

　美久が嬉しそうに頷く。

「覚えててくれたんだ」

「もちろん」

◇◇◇◇

　夕方六時。今日も時間があっという間に過ぎた。

「明日が楽しみで眠れないかも」

　荷物をまとめながら美久が言う。

「大げさだよ」

「だって、私たちの言語に形ができるんでしょ？」

　『私たちの言語』という表現に、胸が温かくなる。

　いつの間にか、これは二人のプロジェクトになっていた。

「じゃあ、今晩は構文のアイデアを考えてきて」

「はい！」

　教室を出て、廊下を歩く。

「隆弘先輩」

「ん？」

「今日も、ありがとう」

　美久の素直な感謝の言葉に、照れくさくなる。

「こちらこそ。美久の発想、いつも面白いから」

「本当？」

「うん。マトリョーシカとか、芸術とか。新しい視点をくれる」

　美久が照れたように微笑む。

「それは、隆弘先輩が優しく聞いてくれるから」

　また『優しい』という言葉。美久はよくそう言う。

◇◇◇◇

　帰り道、僕は今日のことを振り返っていた。

　ASTのバリエーションが増えて、MikuLangは着実に成長している。

　そして、美久との関係も——

（『好きだよ』って言ったよな、確かに）

　プログラミングのセンスのことだと訂正されたけど、あの一瞬の表情は……。

　考えても仕方ない。でも、考えずにはいられない。

　明日は構文を決める。MikuLangに、初めて「顔」ができる日だ。

　美久はどんなアイデアを持ってくるだろう。

　きっと、また驚かされるに違いない。

　そんな期待を胸に、僕は家路を急いだ。