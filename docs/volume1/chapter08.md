# 第8話 JavaScriptでASTを作る

　四月二十七日、土曜日。午後二時。

　約束の時間きっかりに、美久が僕の部屋のインターホンを鳴らした。

「こんにちは、お邪魔します」

　今日の美久は、薄いピンクのワンピースに白いカーディガン。休日らしい、柔らかい雰囲気だ。

「いらっしゃい。緊張してる？」

「ち、ちょっとだけ……」

　美久が照れたように笑う。確かに、学校以外で二人きりというのは、なんだか特別な感じがする。

「飲み物は？　コーヒーでいい？」

「あ、お水で大丈夫です」

　キッチンに向かいながら、僕も少し緊張していることに気づく。部屋の片付けは完璧だし、資料も準備万端。でも、なぜかそわそわしてしまう。

◇◇◇◇

「今日は、昨日考えた構文をJavaScriptで実装していこう」

　僕たちは机を挟んで向かい合って座った。美久のノートPCも持参してきている。

「はい！　楽しみです」

　美久がノートを開く。昨日のアイデアがびっしりと書き込まれている。

「まず、基本的なAST構造を作ろう。JavaScriptのオブジェクトとして」

```javascript
// MikuLangのAST定義
const ASTTypes = {
  Program: 'Program',
  NumberLiteral: 'NumberLiteral',
  StringLiteral: 'StringLiteral',
  BooleanLiteral: 'BooleanLiteral',
  Identifier: 'Identifier',
  BinaryExpression: 'BinaryExpression',
  UnaryExpression: 'UnaryExpression',
  AssignmentExpression: 'AssignmentExpression',
  PrintStatement: 'PrintStatement',
  IfStatement: 'IfStatement',
  WhileStatement: 'WhileStatement'
};
```

「わあ、たくさんある」

「これが言語の『部品』になるんだ」

◇◇◇◇

「次は、これらを作るヘルパー関数を定義しよう」

　僕は丁寧にコードを書いていく。美久も自分のPCで同じように打ち込んでいる。

```javascript
// 数値リテラル
function createNumber(value) {
  return {
    type: ASTTypes.NumberLiteral,
    value: value
  };
}

// 文字列リテラル
function createString(value) {
  return {
    type: ASTTypes.StringLiteral,
    value: value
  };
}

// 識別子（変数名）
function createIdentifier(name) {
  return {
    type: ASTTypes.Identifier,
    name: name
  };
}
```

「なるほど、関数で包むことで作りやすくなるんだ」

「そう。これで手動でもASTが作りやすくなる」

　美久が真剣な表情でコードを見つめている。その横顔を見ていると、なんだか嬉しくなる。

◇◇◇◇

「じゃあ、昨日考えた『好感度を0にする』を実装してみよう」

```javascript
// 「好感度 を 0 にする」のAST
function createAssignment(name, value) {
  return {
    type: ASTTypes.AssignmentExpression,
    left: createIdentifier(name),
    right: value
  };
}

// 実際に作ってみる
const setAffection = createAssignment(
  '好感度',
  createNumber(0)
);

console.log(JSON.stringify(setAffection, null, 2));
```

　実行すると、きれいな構造が表示される。

```json
{
  "type": "AssignmentExpression",
  "left": {
    "type": "Identifier",
    "name": "好感度"
  },
  "right": {
    "type": "NumberLiteral",
    "value": 0
  }
}
```

「すごい！　日本語の変数名も使えるんだ」

「JavaScriptは寛容だからね」

◇◇◇◇

　しばらく作業を続けていると、美久がふと手を止めた。

「ねえ、隆弘先輩」

「ん？」

「これって、実際のプログラミング言語と同じ仕組み？」

　いい質問だ。

「基本的には同じ。JavaScriptもPythonも、内部ではこういうASTを作ってる」

「へー。じゃあ、私たちがやってることって……」

「本物のプログラミング言語開発と同じだよ」

　美久の目が輝く。

「すごい……私、本物を作ってるんだ」

　その感動が伝わってきて、僕も嬉しくなる。

◇◇◇◇

「次は、もっと複雑な構文を作ってみよう」

　僕は新しいコードを書き始めた。

```javascript
// 「もしも 好感度 が 80 以上 ならば」
function createIfStatement(condition, thenBranch, elseBranch = null) {
  return {
    type: ASTTypes.IfStatement,
    condition: condition,
    then: thenBranch,
    else: elseBranch
  };
}

// 比較演算
function createComparison(operator, left, right) {
  return {
    type: ASTTypes.BinaryExpression,
    operator: operator,
    left: left,
    right: right
  };
}

// 実際の if 文を作る
const ifStatement = createIfStatement(
  createComparison(
    '>=',
    createIdentifier('好感度'),
    createNumber(80)
  ),
  {
    type: 'BlockStatement',
    body: [
      createPrint(createString('告白成功！'))
    ]
  }
);
```

「複雑になってきた……」

　美久が眉をひそめる。

「大丈夫。一つずつ分解して考えればいい」

　僕は図を描きながら説明する。美久の肩に触れそうなくらい近づいて、画面を指差す。

（近い……）

　美久のシャンプーの香りがふわっと漂ってくる。集中、集中。

◇◇◇◇

　休憩時間。僕はお茶を入れて戻ってきた。

「疲れた？」

「ううん、楽しい」

　美久が伸びをする。その仕草が妙に色っぽくて、目を逸らしてしまう。

「でも、隆弘先輩の部屋、落ち着くね」

「そう？」

「うん。隆弘先輩の匂いがする」

　ドキッとする発言。

「に、匂い？」

「あ、いや、変な意味じゃなくて！　なんていうか、安心する匂い？」

　美久が慌てて弁解する。顔が真っ赤だ。

（かわいい）

　また、そんなことを考えてしまう。

◇◇◇◇

「そ、それより、続きをやろう」

　僕は話題を変えた。

「今度は、実際に動くインタープリタの基礎を作る」

```javascript
// 環境（変数を保存する場所）
class Environment {
  constructor() {
    this.variables = {};
  }
  
  set(name, value) {
    this.variables[name] = value;
  }
  
  get(name) {
    if (!(name in this.variables)) {
      throw new Error(`変数 '${name}' が見つかりません`);
    }
    return this.variables[name];
  }
}
```

「環境？」

「変数の値を覚えておく場所。メモリみたいなもの」

「なるほど。好感度=100 みたいな情報を保存するんだ」

　美久の理解が早い。

◇◇◇◇

　作業を続けていると、外が暗くなってきた。

「もう六時か」

「え、もうそんな時間？」

　美久も驚いている。集中していると時間があっという間だ。

「お腹空いた？」

「あ、ちょっと……」

　タイミングよく、美久のお腹が小さく鳴った。

「きゃっ」

　美久が顔を真っ赤にして俯く。

「ピザでも頼もうか」

「え、いいの？」

「一緒に食べながら作業しよう」

　なんだか、デートみたいな展開になってきた。

◇◇◇◇

　ピザを食べながら、今日の成果を振り返る。

「JavaScriptでASTを作る方法、理解できた？」

「うん。最初は難しかったけど、だんだん分かってきた」

　美久がピザを頬張りながら答える。その姿が可愛くて、つい見とめてしまう。

「隆弘先輩？」

「あ、いや、なんでもない」

　慌てて自分のピザに集中する。

「ねえ」

　美久が真剣な顔で言った。

「私、本当にプログラミング言語が作れるかな」

「作れるよ」

　即答する。

「根拠は？」

「美久の熱意と、理解力があれば大丈夫」

　それに、僕がついてるから——とは言えなかった。

「ありがとう」

　美久が優しく微笑む。

「隆弘先輩と一緒なら、きっとできる気がする」

　その言葉に、胸が熱くなる。

◇◇◇◇

　夜八時。そろそろお開きの時間だ。

「今日は、ありがとうございました」

「こちらこそ。楽しかった」

　本当に楽しかった。プログラミングを教えるのも、美久と過ごす時間も。

「明日も……いい？」

　美久が上目遣いで聞いてくる。

「もちろん」

　即答してしまってから、ちょっと恥ずかしくなる。

「やった！　じゃあ、明日は評価器を作るんだよね」

「そう。いよいよMikuLangが動き始める」

　美久の期待に満ちた表情を見て、僕も楽しみになる。

　玄関で靴を履きながら、美久が振り返った。

「隆弘先輩」

「ん？」

「今日は、特別な一日でした」

　そう言って、美久は部屋を出ていった。

　一人になった部屋で、僕は今日のことを振り返る。

　JavaScriptでASTを作る。それは単なる技術的な作業のはずだった。

　でも、美久と一緒だと、すべてが特別な意味を持つ。

　明日もまた、特別な一日になる予感がした。