# 第13話 オブジェクト指向

　五月二日、木曜日。ゴールデンウィーク四日目。

　美久との日々が、当たり前のようになってきた。でも、それが嬉しい。

「今日はオブジェクトだね」

　美久が準備万端といった様子で言う。

「昨日調べてきた？」

「うん。でも、よく分からなかった……」

　困った顔の美久が可愛い。

「大丈夫。ゆっくり理解していこう」

◇◇◇◇

「オブジェクトは、関連するデータをまとめたもの」

　ホワイトボードに例を書く。

```javascript
// キャラクターのデータ
{
  名前: "美久",
  レベル: 15,
  HP: 100,
  MP: 50,
  スキル: ["回復魔法", "攻撃魔法"]
}
```

「あ、ゲームのステータス画面みたい！」

「そう。複数の属性を一つにまとめられる」

◇◇◇◇

「オブジェクトのASTを設計しよう」

```javascript
// オブジェクトリテラルのAST
const ObjectLiteral = {
  type: 'ObjectLiteral',
  properties: [
    {
      key: { type: 'Identifier', name: '名前' },
      value: { type: 'StringLiteral', value: '美久' }
    },
    {
      key: { type: 'Identifier', name: 'レベル' },
      value: { type: 'NumberLiteral', value: 15 }
    }
  ]
};

// プロパティアクセス
const PropertyAccess = {
  type: 'MemberExpression',
  object: { type: 'Identifier', name: 'キャラクター' },
  property: { type: 'Identifier', name: '名前' },
  computed: false
};
```

「keyとvalueのペアになってるんだ」

「そう。連想配列とも呼ばれる」

◇◇◇◇

　実装を始める。

```javascript
// ASTTypesに追加
ASTTypes.ObjectLiteral = 'ObjectLiteral';

// ヘルパー関数
function createObject(properties) {
  return {
    type: ASTTypes.ObjectLiteral,
    properties: properties.map(([key, value]) => ({
      key: createIdentifier(key),
      value: value
    }))
  };
}

function createPropertyAccess(object, property) {
  return {
    type: ASTTypes.MemberExpression,
    object: object,
    property: createIdentifier(property),
    computed: false
  };
}
```

「computedって何？」

「obj.nameかobj['name']かの違い」

◇◇◇◇

「評価器も拡張しよう」

```javascript
// オブジェクトリテラルの評価
evaluateObjectLiteral(node) {
  const obj = {};
  
  for (const prop of node.properties) {
    const key = prop.key.name;
    const value = this.evaluate(prop.value);
    obj[key] = value;
  }
  
  return obj;
}

// プロパティアクセスの評価（拡張）
evaluateMemberExpression(node) {
  const object = this.evaluate(node.object);
  
  if (Array.isArray(object)) {
    // 配列の場合
    const index = this.evaluate(node.property);
    return object[index];
  } else if (typeof object === 'object' && object !== null) {
    // オブジェクトの場合
    const key = node.computed 
      ? this.evaluate(node.property)
      : node.property.name;
    return object[key];
  }
  
  throw new Error('配列またはオブジェクトではありません');
}
```

◇◇◇◇

　美久が質問してきた。

「オブジェクトと配列の違いは？」

「配列は順番、オブジェクトは名前でアクセス」

　具体例を示す。

```javascript
// 配列：インデックスでアクセス
const items = ["剣", "盾", "薬草"];
items[0]; // "剣"

// オブジェクト：プロパティ名でアクセス
const player = { name: "美久", level: 15 };
player.name; // "美久"
```

「なるほど！　用途によって使い分けるんだ」

◇◇◇◇

「実際にオブジェクトを使ったプログラムを書いてみよう」

```javascript
const characterProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment(
      'キャラクター',
      createObject([
        ['名前', createString('美久')],
        ['職業', createString('魔法使い')],
        ['レベル', createNumber(15)],
        ['HP', createNumber(100)],
        ['MP', createNumber(50)]
      ])
    ),
    createPrint(createString('=== キャラクター情報 ===')),
    createPrint(createPropertyAccess(
      createIdentifier('キャラクター'),
      '名前'
    )),
    createPrint(createPropertyAccess(
      createIdentifier('キャラクター'),
      '職業'
    )),
    createPrint(createString('レベル:')),
    createPrint(createPropertyAccess(
      createIdentifier('キャラクター'),
      'レベル'
    ))
  ]
};

mikuLang.run(characterProgram);
// 出力:
// === キャラクター情報 ===
// 美久
// 魔法使い
// レベル:
// 15
```

「わあ、本当にゲームみたい！」

　美久の目が輝く。

◇◇◇◇

　休憩時間。美久がお茶を飲みながら言った。

「オブジェクトって、現実世界に似てる」

「どういうこと？」

「だって、人にも名前とか年齢とか、いろんな属性があるでしょ？」

　その洞察に感心する。

「その通り。オブジェクト指向は現実世界をモデル化したもの」

◇◇◇◇

「オブジェクトの中にオブジェクトも入れられる」

　新しい例を書く。

```javascript
const gameData = createObject([
  ['プレイヤー', createObject([
    ['名前', createString('隆弘')],
    ['レベル', createNumber(20)],
    ['装備', createObject([
      ['武器', createString('光の剣')],
      ['防具', createString('勇者の鎧')]
    ])]
  ])],
  ['パートナー', createObject([
    ['名前', createString('美久')],
    ['レベル', createNumber(18)]
  ])]
]);
```

「入れ子構造！」

「複雑なデータも表現できる」

◇◇◇◇

「メソッドも実装してみよう」

```javascript
// メソッド呼び出しのAST
const MethodCall = {
  type: 'CallExpression',
  callee: {
    type: 'MemberExpression',
    object: { type: 'Identifier', name: 'キャラクター' },
    property: { type: 'Identifier', name: 'レベルアップ' }
  },
  arguments: []
};
```

「オブジェクトの中に関数を入れるの？」

「そう。データと処理をまとめられる」

◇◇◇◇

　美久がプログラムを書き始めた。

```javascript
// 美久のオブジェクトプログラム
const mikuObjectProgram = {
  type: ASTTypes.Program,
  body: [
    createAssignment(
      'ゲーム状態',
      createObject([
        ['好感度', createNumber(0)],
        ['イベント数', createNumber(0)],
        ['現在地', createString('教室')]
      ])
    ),
    createFunction(
      'イベント発生',
      ['場所', '相手'],
      {
        type: 'BlockStatement',
        body: [
          createAssignment(
            'ゲーム状態',
            createObject([
              ['好感度', createBinaryExpression(
                '+',
                createPropertyAccess(
                  createIdentifier('ゲーム状態'),
                  '好感度'
                ),
                createNumber(10)
              )],
              ['イベント数', createBinaryExpression(
                '+',
                createPropertyAccess(
                  createIdentifier('ゲーム状態'),
                  'イベント数'
                ),
                createNumber(1)
              )],
              ['現在地', createIdentifier('場所')]
            ])
          ),
          createPrint(createBinaryExpression(
            '+',
            createIdentifier('場所'),
            createString('でイベント発生！')
          )),
          createPrint(createString('好感度が上がった！'))
        ]
      }
    ),
    createCall('イベント発生', [
      createString('屋上'),
      createString('隆弘先輩')
    ]),
    createPrint(createString('現在の好感度:')),
    createPrint(createPropertyAccess(
      createIdentifier('ゲーム状態'),
      '好感度'
    ))
  ]
};
```

「これ、実際のゲームロジックに近い」

　美久の理解力と応用力に驚く。

◇◇◇◇

　夕方になってきた。今日も充実した一日だった。

「オブジェクト、難しかったけど面白い」

　美久が満足そうに言う。

「データをまとめる方法が分かると、プログラムの幅が広がる」

「うん。ゲームのキャラクターデータとか、これで管理できるんだね」

◇◇◇◇

「隆弘先輩」

　片付けながら、美久が言った。

「MikuLangで、本格的なゲームが作れる気がしてきた」

「きっと作れるよ」

「一緒に作ろうね」

　その言葉に、胸が熱くなる。

「もちろん」

◇◇◇◇

　美久を見送る時、彼女が言った。

「明日は何を実装するの？」

「エラー処理かな。プログラムを安全にする仕組み」

「また新しいことが学べるんだ」

　美久の向学心が嬉しい。

「美久の成長が早くて、教えがいがある」

「隆弘先輩のおかげ」

　また褒め合いになってしまう。

「じゃあ、また明日」

「うん」

　美久が帰った後、今日のコードを振り返る。

　オブジェクト指向の基礎を、美久は見事に理解した。

　MikuLangは、もはや本格的なプログラミング言語に近づいている。

　そして、美久との距離も——

　明日もまた、新しい発見がありそうだ。