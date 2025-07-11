# 第10話 初めての言語実行

　四月二十九日、月曜日。昭和の日。

　祝日だけど、美久と会う約束をしていた。もはや、週末の恒例行事になりつつある。

「今日はいよいよMikuLangを動かすんだよね」

　部屋に入るなり、美久が興奮気味に言った。

「そう。初めての実行」

「ドキドキする」

　美久の期待に満ちた表情を見て、僕も気が引き締まる。

◇◇◇◇

「まず、昨日作った評価器をもう一度確認しよう」

　画面にコードを表示する。

「これまでに作ったものを整理すると……」

```javascript
// MikuLang インタープリタ
class MikuLang {
  constructor() {
    this.evaluator = new Evaluator();
  }
  
  run(program) {
    try {
      return this.evaluator.evaluate(program);
    } catch (error) {
      console.error('実行エラー:', error.message);
      return null;
    }
  }
}
```

「わあ、MikuLangクラス！」

「これが僕たちの言語の入り口」

◇◇◇◇

「じゃあ、最初のプログラムを実行してみよう」

　僕は簡単なプログラムを書いた。

```javascript
// 最初のMikuLangプログラム
const firstProgram = {
  type: ASTTypes.Program,
  body: [
    createPrint(createString('こんにちは、MikuLang！'))
  ]
};

const mikuLang = new MikuLang();
mikuLang.run(firstProgram);
// 出力: こんにちは、MikuLang！
```

「動いた！」

　美久が歓声を上げる。

「私たちの言語が、本当に動いてる！」

　その感動が伝わってきて、僕も嬉しくなる。

◇◇◇◇

「もっと複雑なプログラムも書いてみよう」

```javascript
// 変数を使った計算
const calcProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment('x', createNumber(10)),
    createAssignment('y', createNumber(20)),
    createAssignment(
      'sum',
      createBinaryExpression(
        '+',
        createIdentifier('x'),
        createIdentifier('y')
      )
    ),
    createPrint(createString('計算結果:')),
    createPrint(createIdentifier('sum'))
  ]
};

mikuLang.run(calcProgram);
// 出力:
// 計算結果:
// 30
```

「計算もできてる！」

「変数も正しく動作してるね」

◇◇◇◇

　美久が突然立ち上がった。

「私も書いてみたい！」

「いいよ。何を書く？」

　美久は少し考えて、キーボードに向かった。

```javascript
// 美久の最初のプログラム
const mikuProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment('メッセージ', createString('隆弘先輩、ありがとう')),
    createPrint(createIdentifier('メッセージ')),
    createAssignment('気持ち', createNumber(100)),
    createPrint(createString('感謝の気持ち:')),
    createPrint(createIdentifier('気持ち'))
  ]
};
```

「実行してみる？」

「うん」

　緊張した面持ちで、美久が実行ボタンを押す。

```
隆弘先輩、ありがとう
感謝の気持ち:
100
```

「できた！」

　美久が満面の笑みを浮かべる。

（なんか照れるな……）

◇◇◇◇

「次は、条件分岐を使ったプログラムを書いてみよう」

　僕は新しい例を作り始めた。

```javascript
// 成績判定プログラム
const gradeProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment('点数', createNumber(85)),
    {
      type: ASTTypes.IfStatement,
      condition: createComparison(
        '>=',
        createIdentifier('点数'),
        createNumber(90)
      ),
      then: {
        type: 'BlockStatement',
        body: [createPrint(createString('優'))]
      },
      else: {
        type: ASTTypes.IfStatement,
        condition: createComparison(
          '>=',
          createIdentifier('点数'),
          createNumber(80)
        ),
        then: {
          type: 'BlockStatement',
          body: [createPrint(createString('良'))]
        },
        else: {
          type: 'BlockStatement',
          body: [createPrint(createString('可'))]
        }
      }
    }
  ]
};

mikuLang.run(gradeProgram);
// 出力: 良
```

「else if みたいな構造も作れるんだ」

「そう。入れ子にすることで実現できる」

◇◇◇◇

　休憩時間。お茶を飲みながら、美久が言った。

「ねえ、隆弘先輩」

「ん？」

「MikuLangで、ゲームとか作れるかな」

　意外な質問だった。

「ゲーム？」

「うん。簡単な文字だけのゲームとか」

　美久の発想に感心する。

「作れると思う。ループと配列があれば、簡単なゲームなら」

「楽しみ！」

◇◇◇◇

「そうだ、エラー処理も確認しておこう」

　わざとエラーになるプログラムを書く。

```javascript
// エラーになるプログラム
const errorProgram = {
  type: ASTTypes.Program,
  body: [
    createPrint(createIdentifier('未定義の変数'))
  ]
};

mikuLang.run(errorProgram);
// 実行エラー: 変数 '未定義の変数' が見つかりません
```

「ちゃんとエラーメッセージが出る」

「エラー処理も大事な機能だからね」

◇◇◇◇

　夕方になってきた。今日は祝日なので、いつもより長く作業できる。

「MikuLangに、もっと機能を追加したい」

　美久が意欲的に言う。

「どんな機能？」

「うーん、繰り返しとか」

「じゃあ、簡単なwhileループを実装してみようか」

```javascript
// while文の評価
evaluateWhileStatement(node) {
  let result = null;
  while (this.evaluate(node.condition)) {
    result = this.evaluate(node.body);
  }
  return result;
}

// ヘルパー関数
function createWhile(condition, body) {
  return {
    type: ASTTypes.WhileStatement,
    condition: condition,
    body: body
  };
}
```

◇◇◇◇

「while文を使ったプログラムを書いてみよう」

```javascript
// カウントダウンプログラム
const countdownProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment('カウント', createNumber(5)),
    createWhile(
      createComparison(
        '>',
        createIdentifier('カウント'),
        createNumber(0)
      ),
      {
        type: 'BlockStatement',
        body: [
          createPrint(createIdentifier('カウント')),
          createAssignment(
            'カウント',
            createBinaryExpression(
              '-',
              createIdentifier('カウント'),
              createNumber(1)
            )
          )
        ]
      }
    ),
    createPrint(createString('発射！'))
  ]
};

mikuLang.run(countdownProgram);
// 出力:
// 5
// 4
// 3
// 2
// 1
// 発射！
```

「すごい！　ループも動いてる！」

　美久の興奮が最高潮に達している。

◇◇◇◇

「隆弘先輩」

　作業の合間に、美久が真剣な顔で言った。

「私、プログラミング言語を作るのが、こんなに楽しいなんて思わなかった」

「僕も」

　正直な気持ちを伝える。

「研究では作ったことあるけど、誰かと一緒に作るのは初めてで」

「私も初めて」

　美久が微笑む。

「でも、隆弘先輩となら、もっと難しいことにも挑戦できそう」

　その言葉に、胸が熱くなる。

◇◇◇◇

　夜七時。外はすっかり暗くなった。

「今日は本当に充実してた」

　美久が満足そうに言う。

「MikuLangが動いて、感動した」

「まだまだこれから」

　僕は今後の計画を話す。

「関数、配列、オブジェクト……追加したい機能はたくさんある」

「全部やりたい！」

　美久の意欲に笑ってしまう。

「少しずつね」

「はい」

◇◇◇◇

　美久を送り出す時、玄関で彼女が振り返った。

「隆弘先輩」

「ん？」

「今日、MikuLangが初めて動いた日」

「そうだね」

「記念日みたい」

　美久の言葉に、僕も同じことを考えていたことに気づく。

「確かに、記念日かも」

「じゃあ、来年の今日は、MikuLangの誕生日」

　来年も一緒に——そんな未来を当然のように語る美久。

「そうだね。一周年には、もっとすごい言語になってるよ」

「楽しみ」

　美久が嬉しそうに帰っていく。

　一人になった部屋で、今日のコードを見返す。

　MikuLangが動いた。僕たちが作った言語が、確かに生きている。

　そして、美久との関係も、また一歩前進した気がする。

　明日からは、もっと高度な機能に挑戦だ。

　美久と一緒なら、どんな困難も乗り越えられる。

　そんな確信を胸に、僕は明日の準備を始めた。