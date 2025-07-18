# 第7話 世界を創るということ

「じゃあ、まず何から始めるんですか？」

　`Environment`という、まだ空っぽのクラスを前にして、美久が期待と不安の入り混じった声で尋ねた。その横顔は真剣そのものだ。

「まずは、この『世界』に、変数を記録できるようにしよう。`define`メソッドの実装からだ」

　僕はそう言って、`Environment`クラスの中にコードを書き込んだ。

```javascript
  define(name, value) {
    this.variables[name] = value;
  }
```

「たったこれだけ？」
「ああ。`this.variables`っていうのが、変数を実際に記録しておくための、ただのオブジェクトだ。そこに、`name`っていう名前の引き出しを作って、`value`っていう中身を入れてあげる。やってることは、すごくシンプルだろ？」
「なるほど……」

　美久は、コードと僕の説明を交互に見比べて、こくりと頷いた。

「じゃあ、今度はその逆。記録した変数の値を、名前を使って探し出す方をやってみようか。`lookup`メソッドだ。美久、どうすればいいか分かるか？」

　僕がそう促すと、彼女は少し戸惑ったような顔を見せた。でも、すぐに`define`のコードに視線を落とし、何かを考えるように指先でテーブルを軽く叩いている。
　やがて、意を決したように顔を上げると、おそるおそる口を開いた。

「えっと……`return this.variables[name];`……ですか？」
「……完璧だ」

　僕は、素直な称賛の言葉を口にした。彼女は、ちゃんと自分の頭で考えて、答えを導き出したんだ。美久は「良かった……」と、ほっと胸を撫で下ろしている。その表情に、確かな自信の光が灯ったのを、僕は見逃さなかった。

「さて、これで『世界』の準備はできた。次はこの`Environment`を、僕たちの心臓部である`evaluate`関数と繋いであげる必要がある」

　僕は`evaluate`関数の定義を、`function evaluate(exp, env)`という形に書き換えた。第二の引数として、今作った`env`（環境）を受け取れるようにするためだ。

「そして、`evaluate`関数の中に、新しいお願い`define`のルールを追加する」

```javascript
    // もし式がリストなら
    if (Array.isArray(exp)) {
      // もし、それがdefine文なら
      if (exp[0] === 'define') {
        const name = exp[1];
        const value = evaluate(exp[2], env); // 値を先に評価する
        env.define(name, value);
        return; // define文は何も返さない
      }
      // ...四則演算の処理は続く
    }
```

「最後に、変数を参照するルールだ。『もし、`exp`が`'x'`みたいな文字列だったら、それは変数名だと判断して、`env`に問い合わせる』」

```javascript
  // もし式が文字列なら、変数を探す
  if (typeof exp === 'string') {
    return env.lookup(exp);
  }
```

　これで、一通りの実装は完了だ。僕は達成感に浸りながら、キーボードから手を離した。

「これで、僕たちの言語は『記憶』を手に入れた。`['define', 'x', 10]`とお願いすれば、`x`が10だってことを、ちゃんと覚えてくれるようになったんだ」
「はあ……」

　僕の弾んだ声とは対照的に、美久の返事はどこか上の空だった。彼女の表情が、さっきまでの快活さを失い、少しだけ硬くなっていることに、僕は気づかなかった。

「……先輩は、すごいですね」

　ぽつりと、彼女が呟いた。

「こんなこと、全部スラスラと出てくるなんて。まるで、頭の中に完成図が全部入ってるみたい」
「まあ、何度もやってることだからな」

　僕は、それをただの称賛として、額面通りに受け取ってしまった。その言葉の裏に、自分との圧倒的な差に対する、小さな絶望と焦りが滲んでいることにも気づかずに。

　僕たちの言語は、確かに記憶を手に入れた。でも、その代償のように、僕たちの間には、今までなかったはずの、見えない壁が生まれ始めていたのかもしれない。
　そんな不穏な予感を、この時の僕は、まだ知る由もなかった。