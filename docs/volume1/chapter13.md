# 第13話 ブロックと、二人だけの「世界」

「スコープって、なんだか秘密基地みたいですね」

　バグが直った安堵感からか、美久がぽつりとそんなことを言った。机に肘をつき、楽しそうに足をぶらぶらさせている。

「その場所だけで使える、特別な合言葉がある、みたいな」
「秘密基地、か。……いい例えだな」

　僕は、彼女のそのユニークな発想に感心した。そして、その言葉から、新しいアイデアが閃いた。

「じゃあ、その『秘密基地』を、僕たちの言語で、いつでも自由に作れるようにしてみないか？」
「え、できるんですか？」
「ああ。`begin`っていう、新しい『お願い』を追加するんだ」

　僕は、新しい構文の例をエディタに書いてみせた。

`['begin', ['define', 'x', 10], ['+', 'x', 5]]`

「`begin`は、『ここからここまでが、一つのまとまりだよ』ってコンピュータに教えてあげる合図だ。そして、この`begin`の中で作られた変数は、その中でしか使えない、まさに『秘密の合言葉』になる」
「おお……！　面白そう！」

　美久が、ぱっと顔を輝かせた。その好奇心に満ちた表情を見ると、僕まで嬉しくなってくる。

「方針はこうだ。`evaluate`関数が`begin`を見つけたら、まず、今の『世界』を親とする、新しい『子どもの世界』を作ってあげる。そして、`begin`の中の処理は、全部その新しい世界の中で実行するんだ」

　僕は、`evaluate`関数に`begin`のルールを書き足そうとして、ふと手を止めた。

「……ここの処理は、少し長くなりそうだな。こういう時は、処理を別の小さな関数に切り出すのが、良い習慣なんだ。コードを整理整頓する、って感じかな」

　そう言って、僕は`evaluateBlock`という、新しい関数の定義を書き始めた。そして、その実装を、僕たちは二人で進めていった。まさに、ペアプログラミングだ。

「まず、新しい『子どもの世界』を作るから……`const blockEnv = new Environment(env);`でいいんですよね？」と美久が言う。
「その通り。じゃあ、次はこの`blockEnv`を使って、`begin`の中の式を、上から順番に評価していく」
「最後の式の答えが、ブロック全体の答えになる……。じゃあ、ループで回して、最後の結果だけを`return`すればいいんですね？」
「完璧だ、美久」

　僕が相槌を打つより先に、彼女が答えに辿り着く。彼女の成長の速さには、本当に驚かされる。僕たちは、もう教える、教えられるの関係じゃない。対等な、開発パートナーだ。

　そして、完成した`evaluateBlock`を使って、テストを実行する。

```javascript
console.log(evaluate(['begin',
  ['define', 'x', 10],
  ['define', 'y', 20],
  ['+', 'x', 'y']
], env)); // -> 30
```

　画面に`30`と表示されるのを確認し、僕たちは顔を見合わせて頷いた。そして、本命のテスト。

```javascript
console.log(evaluate(['begin',
  ['define', 'message', 'hello']
], env));

console.log(evaluate('message', env)); // -> Error!
```

　`begin`の外で`message`を使おうとすると、画面にはっきりと「Undefined variable: message（そんな変数、知らないよ）」というエラーが表示された。

「おお……！　すごい！　本当に、秘密基地になってる！」

　美久が、子どものようにはしゃいでいる。その純粋な喜びに、僕の心も温かくなる。

「これで、僕たちの言語に、また一つ、大切な概念が加わったな」
「はい！」

　美久が、満面の笑みで僕を見た。僕たちは、一つのPC画面を一緒に覗き込みながら、自然と次のアイデアについて話し始めていた。

　僕たちが作っているのは、もう単なるプログラムじゃない。僕と彼女の、二人だけの言葉で、二人だけのルールで動く、特別な「世界」そのものなんだ。そんな確信が、僕の胸を満たしていた。