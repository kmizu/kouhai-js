# 第49話 新しい始まり

　四月八日、火曜日。

　東京大学での新学期が始まった。二年生になった僕と美久は、それぞれ専門課程に進んだ。

「隆弘先輩、今日の講義どうだった？」

　駒場キャンパスの食堂で、美久と昼食。

「計算理論、面白かったよ」

「難しそう」

「でも、MikuLangの実装経験が活きてる」

　実際、講義で出てきた概念の多くは、すでに実践していたものだった。

◇◇◇◇

「美久の言語学はどう？」

「楽しい！自然言語処理の話とか」

　美久の目が輝く。

「プログラミング言語と自然言語の接点を研究したい」

「面白そう」

　お互いの専門が、いい形で交わり始めている。

◇◇◇◇

　午後、本郷キャンパスの研究室を見学。

「ようこそ、嵐山君」

　プログラミング言語研究室の教授が迎えてくれた。

「君のMikuLangの論文、読ませてもらったよ」

「ありがとうございます」

　高校時代に書いた論文が評価されていた。

◇◇◇◇

「今は何を研究してる？」

「型システムに興味があります」

「ほう、どんな？」

「依存型を使った検証可能な言語設計です」

　ホワイトボードに構想を描く。

```
-- 依存型の例
Vec : Nat → Type → Type
Vec 0 a = Nil
Vec (n+1) a = Cons a (Vec n a)

-- 長さが保証されたリスト操作
append : Vec n a → Vec m a → Vec (n+m) a
```

「面白い。ぜひうちで研究を続けてほしい」

◇◇◇◇

　夕方、美久と合流して帰路につく。

「研究室、決まりそう？」

「うん。教授も歓迎してくれた」

「よかった！」

　美久が嬉しそうに飛び跳ねる。

「美久の方は？」

「言語学の山田教授のところに行きたい」

「計算言語学の？」

「そう！プログラミングの知識も活かせそう」

◇◇◇◇

　二人のアパートの間にあるカフェで勉強。

　東京に来てから見つけた、お気に入りの場所だ。

「新しいプロジェクト、始めない？」

　美久が提案する。

「どんな？」

「自然言語でプログラミングできる言語」

「MikuLangの進化版？」

「もっと高度な」

◇◇◇◇

　アイデアをノートに書き出していく。

```javascript
// 自然言語プログラミングの例
"リストの中から偶数だけを取り出して、
 それぞれを2倍にした新しいリストを作る"

// 内部的に変換される
list.filter(x => x % 2 === 0)
    .map(x => x * 2)
```

「機械学習も使えそう」

「言語モデルで意図を理解する」

　二人のアイデアが次々に繋がっていく。

◇◇◇◇

「名前はどうする？」

「MikuLang 2.0？」

「新しい名前がいいかも」

　しばらく考える。

「NaturalScript」

「いいね！」

　新プロジェクトの誕生だ。

◇◇◇◇

　夜、それぞれの部屋に戻る前に。

「隆弘先輩」

「ん？」

「大学でも一緒に研究できて嬉しい」

「僕もだよ」

　美久が背伸びをして、キスをしてきた。

　相変わらず積極的だ。

◇◇◇◇

　自室でGitHubにリポジトリを作成。

```bash
$ git init naturalscript
$ cd naturalscript
$ echo "# NaturalScript" > README.md
$ git add .
$ git commit -m "Initial commit: Start of a new journey with Miku"
```

　コミットメッセージに美久の名前を入れる。

　これから始まる新しい挑戦の記録。

◇◇◇◇

　ベッドに入って、今日のことを振り返る。

　大学二年生。専門課程。新しい研究。

　そして、変わらず隣にいる美久。

```javascript
// 新しい章
const newBeginning = {
  university: "東京大学",
  year: 2,
  research: "プログラミング言語",
  project: "NaturalScript",
  partner: "美久",
  future: "明るい"
};
```

　高校時代に夢見た未来が、現実になっている。

　いや、想像以上に素晴らしい現実だ。

　明日も美久と一緒に、新しい言語を作っていこう。

　そう思いながら、幸せな気持ちで眠りについた。