# 第8話 見えないバグと、言えない言葉

「よし、じゃあ早速試してみようか」

　変数という、強力な記憶装置を手に入れた僕たちの言語。僕は少し興奮した気持ちを抑えながら、テストケースを打ち込んでいった。

```javascript
const env = new Environment();
evaluate(['define', 'x', 10], env);
console.log(evaluate(['+', 'x', 5], env)); // -> 15

evaluate(['define', 'y', ['*', 'x', 2]], env);
console.log(evaluate(['/', 'y', 4], env)); // -> 5
```

　画面には、僕の期待通りの数字が次々と表示される。`x`に`10`を記憶させ、それを使って`y`を計算する。完璧な動作だ。

「おお……！　ちゃんと覚えてる……！」

　隣で見ていた美久も、感心したように声を上げた。でも、その笑顔は、先週までの彼女と比べて、どこか少しだけぎこちなく見えた。

「じゃあ、次は少し意地悪なテストだ」

　僕は、にやりと笑いながら、新しいコードを打ち込んだ。

`evaluate(['define', 'x', 20], env);`
`console.log(evaluate('x', env));`

　一度定義した変数`x`を、新しい値で上書きする。いわゆる「再定義」だ。当然、画面には`20`と表示されるはずだった。
　しかし。

`10`

「……あれ？」

　表示されたのは、古い値のままの`10`だった。僕たちの作った`define`は、新しい値で上書きできず、最初に入れられた値を頑なに守り続けてしまっている。

「おかしいな……。なんでだ？」

　僕は眉をひそめ、完全に自分の世界に入り込んでしまった。ぶつぶつと独り言を呟きながら、コンソールに大量のデバッグ情報を出力し、コードのあちこちを睨みつける。頭の中は、この予期せぬバグの原因究明で、完全に占拠されてしまっていた。

　隣に、美久がいることも忘れて。

　彼女は、そんな僕の姿を、ただ黙って見つめているだけだった。僕の口から漏れる「スコープが汚染されてるのか？」「いや、参照のタイミングか？」なんていう言葉は、きっと今の彼女には、意味不明な呪文のようにしか聞こえないだろう。
　手伝おうにも、何をすればいいのか分からない。「共同開発者」のはずが、いつの間にか、僕は彼女をただの「見学者」にしてしまっていた。

「あの、先輩……」

　どれくらい時間が経っただろうか。美久が、おそるおそる、か細い声で僕に話しかけた。

「何か、私にできること、ありますか……？」

　その声に含まれた、不安と遠慮の色に、僕は気づくべきだった。でも、バグのことで頭がいっぱいだった僕は、最悪の返事をしてしまう。

「ああ、ごめん。今ちょっと、集中させてくれ。もうすぐ原因、分かると思うから」

　彼女の方を見ることさえせずに。悪気なんて、全くなかった。ただ、純粋に、問題解決に没頭していただけなんだ。でも、その言葉が、彼女の心をどれだけ深く傷つけたか、この時の僕には、想像もできなかった。

「……そう、ですか」

　美久の声から、感情が消えた。

「すみません、先輩。今日、ちょっと用事があるの、思い出しました。なので……お先に、失礼します」

　静かに立ち上がった彼女の言葉に、僕はまだ、画面に視線を釘付けにしたまま、生返事を返した。

「ん？　ああ、そうか。わかった。お疲れ」

　ぱたん、と静かに教室のドアが閉まる音で、僕はようやく顔を上げた。そこに、彼女の姿はもうなかった。

（あれ？　帰ったのか）

　一瞬、そう思っただけですぐに意識はコードの海へと戻っていく。一人になった教室に、僕のキーボードを叩く音だけが、虚しく響き渡っていた。

　僕たちの言語に生まれた、小さなバグ。
　そして、僕たちの間に生まれた、目には見えない、もっと深刻なバグ。

　その存在に、僕はまだ、気づいてすらいなかった。