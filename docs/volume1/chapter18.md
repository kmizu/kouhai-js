# 第18話 パーサーの実装

　五月七日、火曜日。

　ゴールデンウィーク明けの学校。久しぶりの授業を終えて、放課後の空き教室で美久と開発を続ける。

「今日はパーサーだね」

　美久が期待に満ちた顔で言う。

「文字列からASTを作る。これで本物のプログラミング言語になる」

「ついに最後のピース？」

「そう。今まではASTを手動で作ってたけど、これからはコードを書けるようになる」

◇◇◇◇

「パーサーの基本から始めよう」

　ホワイトボードに流れを書く。

```
ソースコード（文字列）
    ↓
字句解析（トークン化）
    ↓
構文解析（パース）
    ↓
AST（抽象構文木）
```

「字句解析って？」

「文字列を意味のある単位に分解すること」

　具体例を示す。

```javascript
// 入力: "x = 10 + 20"
// 字句解析の結果（トークン）:
[
  { type: 'IDENTIFIER', value: 'x' },
  { type: 'ASSIGN', value: '=' },
  { type: 'NUMBER', value: '10' },
  { type: 'PLUS', value: '+' },
  { type: 'NUMBER', value: '20' }
]
```

◇◇◇◇

「まず、トークナイザーを作ろう」

```javascript
// トークンの種類
const TokenType = {
  NUMBER: 'NUMBER',
  STRING: 'STRING',
  IDENTIFIER: 'IDENTIFIER',
  PLUS: 'PLUS',
  MINUS: 'MINUS',
  STAR: 'STAR',
  SLASH: 'SLASH',
  ASSIGN: 'ASSIGN',
  LPAREN: 'LPAREN',
  RPAREN: 'RPAREN',
  LBRACE: 'LBRACE',
  RBRACE: 'RBRACE',
  SEMICOLON: 'SEMICOLON',
  IF: 'IF',
  ELSE: 'ELSE',
  WHILE: 'WHILE',
  FUNCTION: 'FUNCTION',
  RETURN: 'RETURN',
  EOF: 'EOF'
};

// トークナイザークラス
class Tokenizer {
  constructor(input) {
    this.input = input;
    this.position = 0;
  }
  
  // 次の文字を見る（消費しない）
  peek() {
    return this.input[this.position] || '';
  }
  
  // 次の文字を消費
  advance() {
    return this.input[this.position++] || '';
  }
  
  // 空白をスキップ
  skipWhitespace() {
    while (/\s/.test(this.peek())) {
      this.advance();
    }
  }
}
```

「文字を一つずつ見ていくんだ」

「そう。パターンを認識してトークンに変換する」

◇◇◇◇

　美久が質問してきた。

「日本語も扱えるの？」

「もちろん。識別子に日本語を使えるようにしよう」

```javascript
// 数値トークンの読み取り
readNumber() {
  let value = '';
  while (/[0-9]/.test(this.peek())) {
    value += this.advance();
  }
  if (this.peek() === '.') {
    value += this.advance();
    while (/[0-9]/.test(this.peek())) {
      value += this.advance();
    }
  }
  return {
    type: TokenType.NUMBER,
    value: parseFloat(value)
  };
}

// 識別子の読み取り（日本語対応）
readIdentifier() {
  let value = '';
  // 日本語、英数字、アンダースコアを許可
  while (/[\p{L}\p{N}_]/u.test(this.peek())) {
    value += this.advance();
  }
  
  // キーワードチェック
  const keywords = {
    'if': TokenType.IF,
    'もし': TokenType.IF,
    'else': TokenType.ELSE,
    'でなければ': TokenType.ELSE,
    'while': TokenType.WHILE,
    'の間': TokenType.WHILE,
    'function': TokenType.FUNCTION,
    '関数': TokenType.FUNCTION,
    'return': TokenType.RETURN,
    '返す': TokenType.RETURN
  };
  
  const type = keywords[value] || TokenType.IDENTIFIER;
  return { type, value };
}
```

「日本語でプログラミングできる！」

　美久の目が輝く。

◇◇◇◇

　休憩時間。美久がお茶を飲みながら言った。

「パーサーって、人の話を理解するのに似てるね」

「どういうこと？」

「単語を認識して、文法を理解して、意味を把握する」

　鋭い洞察だ。

「確かに。自然言語処理とプログラミング言語処理は似てる」

◇◇◇◇

「次は構文解析器を作ろう」

```javascript
// パーサークラス
class Parser {
  constructor(tokens) {
    this.tokens = tokens;
    this.current = 0;
  }
  
  // 現在のトークンを見る
  peek() {
    return this.tokens[this.current] || { type: TokenType.EOF };
  }
  
  // トークンを消費
  consume(expectedType) {
    const token = this.peek();
    if (token.type !== expectedType) {
      throw new Error(`期待: ${expectedType}, 実際: ${token.type}`);
    }
    this.current++;
    return token;
  }
  
  // プログラムのパース
  parseProgram() {
    const statements = [];
    while (this.peek().type !== TokenType.EOF) {
      statements.push(this.parseStatement());
    }
    return {
      type: ASTTypes.Program,
      body: statements
    };
  }
}
```

◇◇◇◇

「式のパースは再帰下降法を使う」

```javascript
// 式のパース（優先順位を考慮）
parseExpression() {
  return this.parseAssignment();
}

parseAssignment() {
  let left = this.parseAdditive();
  
  if (this.peek().type === TokenType.ASSIGN) {
    this.consume(TokenType.ASSIGN);
    const right = this.parseAssignment();
    return {
      type: ASTTypes.AssignmentExpression,
      left,
      right
    };
  }
  
  return left;
}

parseAdditive() {
  let left = this.parseMultiplicative();
  
  while (this.peek().type === TokenType.PLUS || 
         this.peek().type === TokenType.MINUS) {
    const op = this.consume(this.peek().type).value;
    const right = this.parseMultiplicative();
    left = {
      type: ASTTypes.BinaryExpression,
      operator: op,
      left,
      right
    };
  }
  
  return left;
}
```

「なんで関数が関数を呼んでるの？」

「演算子の優先順位を表現するため。掛け算は足し算より先に計算される」

◇◇◇◇

　美久がプログラムを書き始めた。

```javascript
// 美久の最初のMikuLangプログラム（文字列）
const mikuFirstCode = `
  好感度 = 0
  
  関数 デート() {
    好感度 = 好感度 + 10
    返す 好感度
  }
  
  関数 告白() {
    もし (好感度 >= 80) {
      返す "告白成功！"
    } でなければ {
      返す "もう少し仲良くなってから..."
    }
  }
  
  // デートを繰り返す
  の間 (好感度 < 80) {
    デート()
  }
  
  結果 = 告白()
  表示(結果)
`;

// パースして実行
const tokenizer = new Tokenizer(mikuFirstCode);
const tokens = tokenizer.tokenize();
const parser = new Parser(tokens);
const ast = parser.parseProgram();
const evaluator = new MikuLangEvaluator();
evaluator.run(ast);
```

「日本語でプログラムが書けた！」

　美久が感動している。

◇◇◇◇

「文字列リテラルのパースも追加しよう」

```javascript
// 文字列トークンの読み取り
readString() {
  const quote = this.advance(); // 開始のクォート
  let value = '';
  let escaped = false;
  
  while (this.position < this.input.length) {
    const char = this.peek();
    
    if (escaped) {
      // エスケープシーケンス
      const escapeMap = {
        'n': '\n',
        't': '\t',
        '\\': '\\',
        '"': '"',
        "'": "'"
      };
      value += escapeMap[char] || char;
      escaped = false;
      this.advance();
    } else if (char === '\\') {
      escaped = true;
      this.advance();
    } else if (char === quote) {
      this.advance(); // 終了のクォート
      break;
    } else {
      value += this.advance();
    }
  }
  
  return {
    type: TokenType.STRING,
    value
  };
}
```

◇◇◇◇

「関数定義のパースも実装しよう」

```javascript
// 関数定義のパース
parseFunctionDeclaration() {
  this.consume(TokenType.FUNCTION);
  const name = this.consume(TokenType.IDENTIFIER).value;
  
  this.consume(TokenType.LPAREN);
  const params = [];
  
  while (this.peek().type !== TokenType.RPAREN) {
    params.push(this.consume(TokenType.IDENTIFIER).value);
    if (this.peek().type === TokenType.COMMA) {
      this.consume(TokenType.COMMA);
    }
  }
  
  this.consume(TokenType.RPAREN);
  const body = this.parseBlockStatement();
  
  return {
    type: ASTTypes.FunctionDeclaration,
    id: { type: ASTTypes.Identifier, name },
    params: params.map(p => ({ type: ASTTypes.Identifier, name: p })),
    body
  };
}
```

◇◇◇◇

　夕方になってきた。パーサーの実装も順調に進んだ。

「これで、MikuLangが本物のプログラミング言語になった」

　美久が感慨深そうに言う。

「文字列でプログラムを書けるようになった」

「しかも日本語で」

　美久の達成感に満ちた表情が嬉しい。

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「私たち、本当に言語を作っちゃったんだね」

「そうだね。ゼロから作り上げた」

「すごい。信じられない」

　美久の目が潤んでいる。

「美久がいたからできた」

「隆弘先輩こそ」

◇◇◇◇

　教室を出る時、美久が振り返った。

「隆弘先輩」

「ん？」

「明日、何をする？」

「日本語の構文をもっと自然にしたい。それと——」

　僕は少し躊躇してから続けた。

「MikuLangの完成を祝おう」

　美久の顔が明るくなる。

「うん！楽しみ！」

　二人で廊下を歩く。

　夕日が窓から差し込んで、美久の髪を金色に染める。

（明日こそ）

　MikuLangの完成と同時に、美久に伝えよう。

　ずっと心に秘めていた、本当の気持ちを。