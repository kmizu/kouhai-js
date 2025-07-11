# 第17話 デバッグツール

　五月六日、月曜日。振替休日。

　ゴールデンウィークも最後の一日。学校は休みだが、美久との開発は続く。

「今日はデバッグツールだね」

　美久が期待に満ちた顔で言う。

「プログラムの問題を見つけて直すための道具」

「デバッグって、虫を取り除くって意味？」

「そう。昔、コンピュータに本物の虫が入って故障したことから」

◇◇◇◇

「まず、実行トレース機能から作ろう」

　ホワイトボードに設計を書く。

```javascript
// デバッグ情報を保持するクラス
class DebugContext {
  constructor() {
    this.enabled = false;
    this.trace = [];
    this.breakpoints = new Set();
    this.stepMode = false;
    this.callStack = [];
  }
  
  log(type, info) {
    if (this.enabled) {
      this.trace.push({
        type,
        info,
        timestamp: Date.now(),
        callStack: [...this.callStack]
      });
    }
  }
  
  enterFunction(name) {
    this.callStack.push(name);
    this.log('function_enter', { name });
  }
  
  exitFunction(name) {
    this.callStack.pop();
    this.log('function_exit', { name });
  }
}
```

「トレースって何を記録するの？」

「どの順番で何が実行されたか。プログラムの足跡みたいなもの」

◇◇◇◇

「評価器にデバッグ機能を組み込もう」

```javascript
// デバッグ機能付き評価器
class DebugEvaluator extends MikuLangEvaluator {
  constructor() {
    super();
    this.debug = new DebugContext();
  }
  
  evaluate(node) {
    // デバッグ情報を記録
    if (this.debug.enabled) {
      this.debug.log('evaluate', {
        type: node.type,
        location: node.location
      });
    }
    
    // ブレークポイントチェック
    if (this.debug.breakpoints.has(node.location?.line)) {
      this.handleBreakpoint(node);
    }
    
    // 通常の評価
    return super.evaluate(node);
  }
  
  handleBreakpoint(node) {
    console.log('ブレークポイント:', node.location);
    console.log('現在の変数:', this.env.getAll());
    // 実際の実装では、ここで実行を一時停止
  }
}
```

「ブレークポイントって？」

「プログラムを一時停止したい場所。その時点での状態を確認できる」

◇◇◇◇

　美久が質問してきた。

「どんな時にデバッグツールを使うの？」

「プログラムが思った通りに動かない時」

　具体例を示す。

```javascript
// バグのあるプログラム例
const buggyProgram = {
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
            }
          },
          // バグ: n * 階乗(n) となっている（正しくは n-1）
          createReturn(
            createBinaryExpression(
              '*',
              createIdentifier('n'),
              createCall('階乗', [createIdentifier('n')])
            )
          )
        ]
      }
    ),
    createPrint(createCall('階乗', [createNumber(5)]))
  ]
};
```

◇◇◇◇

「実行トレースを見てみよう」

```javascript
// デバッグ実行
const debugEval = new DebugEvaluator();
debugEval.debug.enabled = true;

try {
  debugEval.run(buggyProgram);
} catch (e) {
  console.log('エラー発生:', e.message);
}

// トレースを表示
console.log('実行トレース:');
debugEval.debug.trace.forEach((entry, i) => {
  console.log(`${i}: ${entry.type} - ${JSON.stringify(entry.info)}`);
});
```

「無限ループになってる！」

　美久が気づいた。

「そう。階乗(n)が階乗(n)を呼び続けてる」

◇◇◇◇

　休憩時間。美久がお茶を飲みながら言った。

「デバッグって、探偵みたいだね」

「どういうこと？」

「証拠を集めて、犯人（バグ）を見つける」

　面白い例えだ。

「確かに。推理力が必要」

◇◇◇◇

「変数ウォッチ機能も作ろう」

```javascript
// 変数の変化を監視
class VariableWatcher {
  constructor() {
    this.watches = new Map();
  }
  
  watch(varName, callback) {
    if (!this.watches.has(varName)) {
      this.watches.set(varName, []);
    }
    this.watches.get(varName).push(callback);
  }
  
  notify(varName, oldValue, newValue) {
    const callbacks = this.watches.get(varName) || [];
    callbacks.forEach(cb => cb(varName, oldValue, newValue));
  }
}

// 環境クラスを拡張
class WatchableEnvironment extends Environment {
  constructor(parent, watcher) {
    super(parent);
    this.watcher = watcher;
  }
  
  set(name, value) {
    const oldValue = this.get(name);
    super.set(name, value);
    
    if (this.watcher) {
      this.watcher.notify(name, oldValue, value);
    }
  }
}
```

◇◇◇◇

　美久がプログラムを書き始めた。

```javascript
// 美久のデバッグ練習プログラム
const mikuDebugProgram = {
  type: ASTTypes.Program,
  body: [
    createFunction(
      '好感度計算',
      ['現在値', 'イベント'],
      {
        type: 'BlockStatement',
        body: [
          createAssignment('増加量', createNumber(0)),
          
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '===',
              createIdentifier('イベント'),
              createString('デート')
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createAssignment('増加量', createNumber(10))
              ]
            }
          },
          
          {
            type: ASTTypes.IfStatement,
            condition: createComparison(
              '===',
              createIdentifier('イベント'),
              createString('プレゼント')
            ),
            then: {
              type: 'BlockStatement',
              body: [
                createAssignment('増加量', createNumber(5))
              ]
            }
          },
          
          createAssignment(
            '新しい値',
            createBinaryExpression(
              '+',
              createIdentifier('現在値'),
              createIdentifier('増加量')
            )
          ),
          
          // デバッグ出力
          createCall('debug', [
            createString('好感度変化:'),
            createIdentifier('現在値'),
            createString('→'),
            createIdentifier('新しい値')
          ]),
          
          createReturn(createIdentifier('新しい値'))
        ]
      }
    ),
    
    // テスト実行
    createAssignment('好感度', createNumber(50)),
    createAssignment(
      '好感度',
      createCall('好感度計算', [
        createIdentifier('好感度'),
        createString('デート')
      ])
    ),
    createPrint(createIdentifier('好感度'))
  ]
};
```

◇◇◇◇

「プロファイリング機能も追加しよう」

```javascript
// 実行時間を計測
class Profiler {
  constructor() {
    this.timings = new Map();
  }
  
  start(label) {
    this.timings.set(label, {
      start: performance.now(),
      count: (this.timings.get(label)?.count || 0) + 1
    });
  }
  
  end(label) {
    const timing = this.timings.get(label);
    if (timing && timing.start) {
      const duration = performance.now() - timing.start;
      timing.total = (timing.total || 0) + duration;
      timing.start = null;
    }
  }
  
  report() {
    console.log('=== プロファイリング結果 ===');
    this.timings.forEach((timing, label) => {
      console.log(`${label}:`);
      console.log(`  呼び出し回数: ${timing.count}`);
      console.log(`  合計時間: ${timing.total?.toFixed(2)}ms`);
      console.log(`  平均時間: ${(timing.total / timing.count).toFixed(2)}ms`);
    });
  }
}
```

◇◇◇◇

「エラーの詳細情報も表示できるようにしよう」

```javascript
// エラー情報の拡張
class MikuLangError extends Error {
  constructor(message, node, context) {
    super(message);
    this.name = 'MikuLangError';
    this.node = node;
    this.context = context;
  }
  
  getDetailedMessage() {
    let details = this.message;
    
    if (this.node?.location) {
      details += `\n場所: ${this.node.location.line}行目`;
    }
    
    if (this.context?.callStack.length > 0) {
      details += '\nコールスタック:';
      this.context.callStack.forEach((func, i) => {
        details += `\n  ${i}: ${func}`;
      });
    }
    
    return details;
  }
}
```

◇◇◇◇

　夕方になってきた。デバッグツールの実装も順調に進んだ。

「デバッグツールがあると、プログラミングが楽になるね」

　美久が感想を述べる。

「バグは必ず発生するから、見つけやすくすることが大事」

「人間関係のデバッグツールもあればいいのに」

　美久の冗談に笑ってしまう。

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「今日で連休も終わりか」

「そうだね。でも、開発は続く」

「学校でも続けられる？」

「もちろん。放課後に」

　美久の顔が明るくなる。

「よかった。まだまだ一緒に作りたいものがあるから」

◇◇◇◇

　美久を見送る時、彼女が振り返った。

「隆弘先輩」

「ん？」

「この一週間、本当に充実してた」

「僕もだよ」

「明日からも、よろしくお願いします」

　美久が深々と頭を下げる。

「こちらこそ」

　僕も頭を下げる。

　明日から、また日常が始まる。

　でも、美久との開発は続いていく。

　そして、MikuLangの完成も、もうすぐそこまで来ている。

（次はパーサーか）

　本物のプログラミング言語への、最後のピース。

　美久と一緒なら、きっと作れる。