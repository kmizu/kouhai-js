# 第19話 日本語構文の設計

　五月八日、水曜日。

　放課後の空き教室。美久との開発も、いよいよ最終段階に入った。

「今日は日本語構文をもっと自然にしよう」

　美久が興味深そうに聞く。

「どういうこと？」

「MikuLangを、より日本語らしく書けるようにする」

　ホワイトボードに例を書く。

```
// 現在の構文
もし (好感度 >= 80) {
  返す "告白成功"
}

// より自然な日本語構文
もし 好感度が80以上なら
  「告白成功」を返す
```

◇◇◇◇

「助詞を使った構文を設計しよう」

```javascript
// 日本語の助詞をトークンとして追加
const JapaneseTokens = {
  GA: 'が',      // 主語
  WO: 'を',      // 目的語
  NI: 'に',      // 対象
  DE: 'で',      // 手段・場所
  TO: 'と',      // 並列・引用
  KARA: 'から',   // 起点
  MADE: 'まで',   // 終点
  NO: 'の',      // 所有
  WA: 'は',      // 主題
  NARA: 'なら',   // 条件
  TOKI: 'とき'    // 時
};

// 比較演算子の日本語版
const JapaneseComparison = {
  '以上': '>=',
  '以下': '<=',
  'より大きい': '>',
  'より小さい': '<',
  '等しい': '===',
  '等しくない': '!=='
};
```

「助詞があると、本当に日本語みたい！」

　美久が感心している。

◇◇◇◇

「文法規則を拡張しよう」

```javascript
// 日本語構文のパーサー拡張
class JapaneseParser extends Parser {
  // 条件文のパース
  parseJapaneseIf() {
    this.consume('もし');
    
    // 条件式（日本語スタイル）
    const condition = this.parseJapaneseCondition();
    
    this.consume('なら');
    
    // then節
    const then = this.parseJapaneseBlock();
    
    // else節（オプション）
    let elsePart = null;
    if (this.peek().value === 'そうでなければ') {
      this.consume('そうでなければ');
      elsePart = this.parseJapaneseBlock();
    }
    
    return {
      type: ASTTypes.IfStatement,
      condition,
      then,
      else: elsePart
    };
  }
  
  // 日本語スタイルの条件式
  parseJapaneseCondition() {
    const left = this.parseExpression();
    
    if (this.peek().value === 'が') {
      this.consume('が');
      
      // 数値との比較
      const value = this.parseExpression();
      const comparison = this.peek().value;
      
      if (JapaneseComparison[comparison]) {
        this.advance();
        return {
          type: ASTTypes.BinaryExpression,
          operator: JapaneseComparison[comparison],
          left,
          right: value
        };
      }
    }
    
    return left;
  }
}
```

◇◇◇◇

　美久が質問してきた。

「動詞も日本語にできる？」

「もちろん。動詞活用も実装しよう」

```javascript
// 日本語の動詞活用
const JapaneseVerbs = {
  // 基本動詞
  '表示する': 'print',
  '追加する': 'push',
  '削除する': 'pop',
  '計算する': 'calculate',
  '保存する': 'save',
  '読み込む': 'load',
  
  // 状態動詞
  'である': 'is',
  'になる': 'become',
  'を含む': 'includes',
  'で始まる': 'startsWith',
  'で終わる': 'endsWith'
};

// 活用形の処理
function parseVerbConjugation(verb) {
  // 「〜して」形（連用形）
  if (verb.endsWith('して')) {
    const stem = verb.slice(0, -2);
    return JapaneseVerbs[stem + 'する'];
  }
  
  // 「〜した」形（過去形）
  if (verb.endsWith('した')) {
    const stem = verb.slice(0, -2);
    return JapaneseVerbs[stem + 'する'];
  }
  
  return JapaneseVerbs[verb];
}
```

◇◇◇◇

　休憩時間。美久がお茶を飲みながら言った。

「日本語でプログラミングって、新鮮だね」

「プログラミングを身近に感じてもらえるかもしれない」

「お年寄りや子供にも優しいかも」

　美久の発想が素晴らしい。

◇◇◇◇

「より自然な日本語プログラムを書いてみよう」

```javascript
// 美久の日本語プログラム
const japaneseProgram = `
  美久の好感度を0にする
  
  デートの関数を定義する（）｛
    美久の好感度を10増やす
    「デートで好感度が上がりました」を表示する
  ｝
  
  プレゼントをあげる関数を定義する（アイテム）｛
    もし アイテムが「花」なら
      美久の好感度を5増やす
    そうでなければ アイテムが「本」なら
      美久の好感度を8増やす
    そうでなければ
      美久の好感度を3増やす
  ｝
  
  美久の好感度が80未満の間｛
    デートする
    「花」をプレゼントする
  ｝
  
  もし 美久の好感度が80以上なら
    「告白に成功しました！」を表示する
    「美久：『はい、お付き合いしてください』」を表示する
`;
```

「すごい！本当に日本語の文章みたい」

　美久が興奮している。

◇◇◇◇

「数え方の単位も実装しよう」

```javascript
// 日本語の助数詞
const Counters = {
  '個': (n) => n,
  '人': (n) => n,
  '回': (n) => n,
  '度': (n) => n,
  '歳': (n) => n,
  '年': (n) => n,
  '月': (n) => n,
  '日': (n) => n,
  '時間': (n) => n,
  '分': (n) => n,
  '秒': (n) => n
};

// 使用例のパース
// "3回" → { value: 3, unit: '回' }
// "18歳" → { value: 18, unit: '歳' }

function parseNumberWithCounter(token) {
  const match = token.match(/^(\d+)(.)$/);
  if (match && Counters[match[2]]) {
    return {
      type: 'NumberWithCounter',
      value: parseInt(match[1]),
      counter: match[2]
    };
  }
  return null;
}
```

◇◇◇◇

　美久が実践的なプログラムを書き始めた。

```javascript
// 美久の恋愛シミュレーション（完全日本語版）
const loveSimulation = `
  隆弘との出会いから1年が経過した
  
  私の気持ちを「好き」に設定する
  隆弘の気持ちを「不明」に設定する
  
  告白する勇気の関数を定義する（）｛
    もし 私の気持ちが「好き」で
       かつ 季節が「春」なら
      「告白する」を返す
    そうでなければ
      「もう少し待つ」を返す
  ｝
  
  隆弘の反応の関数を定義する（告白）｛
    もし 告白が「告白する」なら
      隆弘の気持ちを「実は前から好きでした」に設定する
      「両想い」を返す
    ｝
  
  季節を「春」に設定する
  行動を告白する勇気（）
  
  もし 行動が「告白する」なら
    結果を隆弘の反応（行動）
    「めでたしめでたし」を表示する
`;
```

「恋愛要素まで入れちゃった」

　美久が照れながら言う。

◇◇◇◇

「エラーメッセージも日本語化しよう」

```javascript
// 日本語のエラーメッセージ
const JapaneseErrors = {
  SYNTAX_ERROR: '構文エラー',
  TYPE_ERROR: '型エラー',
  REFERENCE_ERROR: '参照エラー',
  RANGE_ERROR: '範囲エラー'
};

class JapaneseMikuLangError extends Error {
  constructor(type, message, location) {
    const jpType = JapaneseErrors[type] || type;
    const fullMessage = `${jpType}: ${message}`;
    
    if (location) {
      fullMessage += `\n場所: ${location.line}行目`;
    }
    
    super(fullMessage);
    this.name = 'MikuLangエラー';
  }
}

// 使用例
// throw new JapaneseMikuLangError(
//   'SYNTAX_ERROR',
//   '「なら」が見つかりません',
//   { line: 10 }
// );
```

◇◇◇◇

　夕方になってきた。日本語構文の設計も完成に近づいた。

「MikuLang、本当に日本語のプログラミング言語になったね」

　美久が感慨深そうに言う。

「誰でも書けるプログラミング言語」

「隆弘先輩の夢？」

「美久と一緒に作った夢」

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「明日で、MikuLang完成？」

「そうだね。最後の仕上げをすれば」

　美久が少し寂しそうな顔をした。

「完成したら、開発終わっちゃうの？」

「そんなことない。完成は始まりだよ」

「始まり？」

「これから、MikuLangでいろんなものを作れる」

　美久の顔が明るくなる。

「そっか。終わりじゃないんだ」

◇◇◇◇

　教室を出る時、夕日が美しかった。

「隆弘先輩」

「ん？」

「明日、大事な日だね」

　美久の言葉に、胸が高鳴る。

「そうだね」

「楽しみにしてる」

　何を楽しみにしているのか。

　MikuLangの完成か、それとも——

　明日、すべてが明らかになる。

　僕の気持ちも、美久の気持ちも。

　そして、二人の未来も。