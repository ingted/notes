静的に型付けされた関数型プログラミング言語を使用するべきではない10の理由
========================================================================
僕がそれを使わない理由について

原文：<http://fsharpforfunandprofit.com/posts/ten-reasons-not-to-use-a-functional-programming-language/>

----

関数型プログラミングに関する誇大広告にうんざりしてない？
僕もだよ！
じゃあ僕らみたいな分別のある人間がそれに近づかないほうがいい理由をいくつかぶっちゃけてみようと思う。

> 一応補足しておくと、以下で「静的に型付けされた関数型言語」と言った場合、型推論だとかデフォルトで不変だとかいう機能を持った言語のことを指す。
> これはたとえばHaskellだったりML系言語（OCamlとかF#とか）のこと。

理由1：最新の流行には乗りたくない
---------------------------

たいていのプログラマがそうだと思うんだけど、僕は生まれつき保守的で、新しいものなんて勉強したくないんだ。
IT業界に就職したのもそれが理由。

たくさんの「リア充」が最新の流行に乗ってるけど、彼らの真似なんてまっぴらごめんだよ。
いい感じに枯れてきて、それが何なのかがよく見えてくるようになるまでは待ちだよね。

関数型言語にしても、僕にとってはまだ出てきたばっかりで、それに乗っかるべきかどうかまだ決めかねるところがある。

もちろん、一部の学者様達は [ML][link1] だとか [Haskell][link2] だとかはもうJavaとかPHPみたいに十分歴史があるって言うんだろうけど、僕にしてみればHaskellなんてついこの間初めて聞いたばっかりだし、そんな議論をしたいわけでもないんだ。

その中でも生まれたての [F#][link3] っていうのもある。
これはまだたった7歳だよ。
勘弁してくれ！
地質学者にしてみれば7年は十分長い時間かもしれないけど、インターネット業界では7年なんて瞬き一つで過ぎ去る程度だ。

というわけでまとめると、昨今の関数型プログラミング言語の流行が果たして長続きするのか、あるいは一時の流行で消えていくのか、僕は慎重にもう数年様子を見るつもりだ。

理由2：行数分だけ稼ぎになる
----------------------

これを読んでる人がどうだかわからないけど、僕はコードをたくさん書けば書くほど生産してるって感じがするんだ。
1日に500行もコードを書いたらそれはもうやりきった感がすごいよね。
コミットもたくさんするし、上司もそれをみてああこいつ頑張ってるなって思ってるんじゃないかな。

だけど古き良きCスタイルのコードと関数型言語の[コードを比べてみる][link4]と、関数型言語のそれはもう身の毛もよだつようなシンプルさだ。

たとえばおなじみの言語で書かれたこんなコードがあるとする：

```csharp
public static class SumOfSquaresHelper
{
    public static int Square(int i)
    {
        return i * i;
    }

    public static int SumOfSquares(int n)
    {
        int sum = 0;
        for (int i = 1; i <= n; i++)
        {
            sum += Square(i);
        }
        return sum;
    }
}
```

一方、関数型言語だとこうだ：

```fsharp
let square x = x * x
let sumOfSquares n = [1..n] |> List.map square |> List.sum
```

17行と2行。
[これがプロジェクト全体に及んだら、果たしてどういうことになるのか想像つくだろう！][link5]

このアプローチを採用しようものなら、僕の生産性はだだ下がりだ。
申し訳ないけどお断りだよね。

理由3：波括弧が好きすぎる
---------------------

まだある。
関数型言語はどうして波括弧を無かったことにしたがるんだろうか。
それほんとにプログラミング言語って言えるの？

要するに何が言いたいか。
こんな波括弧付きのコードがあるとしよう。

```csharp
public class Squarer
{
    public int Square(int input)
    {
        var result = input * input;
        return result;
    }

    public void PrintSquare(int input)
    {
        var result = this.Square(input);
        Console.WriteLine("入力={0} 結果={1}", input, result);
    }
}
```

同じコードを波括弧無しで書くとこうだ。

```fsharp
type Squarer() =

    let Square input =
        let result = input * input
        result

    let PrintSquare input =
        let result = Square input
        printf "入力=%i 結果=%i" input result
```

こんなに違うんだよ！
これを読んでる人がどうかはわからないけど、僕にはこの2番目のコードには何か大切なものが足りない感じがして落ち着かない。

正直言って、波括弧のガイドがないと少し不安なんだよね。

理由4：型は見ればわかるようになっていてほしい
-------------------------------------

関数型言語なんてものを作り出した人いわく、型推論さえあればコードに型をわざわざ書く必要はないよねっていうことらしい。

うーん。でも僕は型の宣言って好きなんだよね。
引数全部の型がわからないとちょっと不安なんだよ。
だから[Java][link6]なんかはまさにお気に入りの言語だ。

MLっぽいコードだと関数のシグネチャはこんな感じになる。
型を書く必要はなくて、自動的に推論されるわけだ。

```fsharp
let GroupBy source keySelector =
    ...
```

同じことをC#でかくと、型の宣言がついてこんな感じ。

```csharp
public IEnumerable<IGrouping<TKey, TSource>> GroupBy<TSource, TKey>(
    IEnumerable<TSource> source,
    Func<TSource, TKey> keySelector
    )
    ...
```

あんまり賛同は得られないかもしれないけど、僕はこっちのほうが好きだ。
返り値が`IEnumerable<IGrouping<TKey, TSource>>`だっていうことがちゃんとわかることが重要。

もちろん型のミスマッチがあればコンパイラがチェックして警告してくれる。
でもどうして頭の中で出来ることをコンパイラにわざわざお願いしないといけないんだろう？

ジェネリックやラムダ式、関数を返す関数とかの最新機能を使う時には確かに型の宣言がえらいことになるという指摘はごもっともだ。
で、それをきちんと宣言するのも確かに難しい。

でもそんなのはたいした話じゃない。
ジェネリックとかそんな類いの機能なんて使わなければいいだけだ。
そうすればシグネチャはすこぶるシンプルになる。
簡単でしょ？

理由5：バグを直すのが好き
---------------------

僕にしてみると狩り、つまりちっちゃなバグを見つけてはつぶしていく作業のスリルは他では味わえないものだ。
製品コードにバグを見つけたりしたらそれはもうすごいよね。
なんと言ってもヒーローになれるチャンスなんだし。

だけど型付けされた関数型言語だとバグが入り込む隙間がないって[聞いたことがある][link7]。

がっかりだよね。

理由6：デバッガーが手放せない
------------------------

バグ修正の話を続けると、僕はコードをステップ実行するだけで1日が終わっていく。
もちろん単体テストをした方がいいって話もあるけど、でも実際動かしてみればわかるよって話でしょ？

それはおいといて、静的に型付けされた関数型言語の場合、[コードがコンパイルできたってことはそれが動くってことだ][link8]。

型を前もって一致させる作業には結構時間がかかるけど、それさえ終わってコンパイルが通れば後はデバッグの必要もないっていう話は聞いたことがある。
でもそれって面白い？

それはつまり(ｒｙ

理由7：細かいことは考えたくない
--------------------------

型のつじつま合わせだけですべてがうまくいくなんてことは僕にしてみれば退屈以外の何物でも無い。

実際、ありとあらゆるコーナーケースに注意して、エラーケースに対応して、他にも問題が起こりそうなところがあれば必ず手を入れないといけないって話だし。
しかもそれをサボったり後回しにすることもできないだなんて。

僕なら（ほとんど）うまく動くケースが通ればバグの修正なんて後回しにしたいけどね。

理由8：やっぱりnullチェックしないと。
-------------------------------

メソッドでは毎回[nullチェック][link9]が欠かせないよね。
そうすればコードがちゃんと守られてるっていう感じで満たされるし。

```csharp
void someMethod(SomeClass x)
{
    if (x == null) { throw new NullArgumentException(); }

    x.doSomething();
}
```

冗談だよ(笑)
いつでもどこでもnullチェックなんてさすがに退屈するよね。
こんなコードは実際に書いたことなんてないってば。

だけどヌルポでひどいクラッシュが起きるときだけはガッするようにはしてる。
そうすれば僕がエラーを探している間もビジネス的な損失はそれほど起こらないからね。
なのでnullの処理ってそんなに[たいした話][link10]じゃないと思う。

理由9：いつでもどこでもデザインパターン使いたい
---------------------------------------

[デザパタ本][link11]でデザインパターンを知った口（何か理由があってGoF本を読まないといけなくなったんだけど、理由は覚えてない）なんだけど、これはありとあらゆる問題を解決する神アイテムだと思ってる。
デザインパターンを使っておけば、コードがまじめで「エンタープライズっぽい」感じになるし、上司受けもいいからね。

だけど関数型言語向けのパターンって全然見かけない。
StrategyとかAbstractFactoryとかDecoratorとかProxyみたいな便利なパターンを使わずにどうしてるんだろう？

多分関数型のプログラマー達はそういうの知らないのかな。

理由10：数学っぽすぎる
-------------------

２乗の総和は以下のコードでも計算できる。
でもこれだと奇妙な記号が並んでいるばっかりで、わかりづらすぎるよ。

```fsharp
ss=: +/ @: *:
```

おおっと！
ごめんこれは[J用のコード][link12]だった。

だけどやっぱり関数型プログラムだと`<*>`とか`>>=`とかいうへんてこな記号を使ったり、「モナド」とか「函手」とかいうぼんやりとした概念を使ったりしているんだって話は聞いたことがある。

関数畑の人たちはどうして僕と同じ目線で話ができないんだろう？
`++`とか`!=`みたいな記号だったり、「継承」とか「多態性」っていう概念のほうがわかりやすいのに。

まとめ：アリかナシかでいえばナシ
--------------------------

理由はわかるよね。
僕は使わない。
関数型プログラミングが便利だとも思わないし。

とにかく、あれこれいろんなことを言われても正直よくわからないんで、誰か3行で[説明して][link13]くれってことだけ言いたかったんだ。

アップデート：なので「everything you need to know on one page」のページを見たけどちょっと短すぎてわかんなかった。

誰か僕が興味を持てるように[kw][link14][s][link15][k][link16]教えてくれないかな。

だけど[チュートリアル][link17]嫁とか[サンプル動かせ][link18]とかコード書けとかはナシで。
仕事じゃないのにそんなことしたくも無いんで。

新しいパラダイムを勉強するためだけにこれまでのポリシーを曲げたくはないんだ。

[link1]: <http://en.wikipedia.org/wiki/ML_(programming_language)>
[link2]: <http://en.wikipedia.org/wiki/Haskell_(programming_language)>
[link3]: <http://fsharp.org/>
[link4]: <http://fsharpforfunandprofit.com/posts/fvsc-sum-of-squares/>
[link5]: <http://www.simontylercousins.net/journal/2013/2/22/does-the-language-you-choose-make-a-difference.html>
[link6]: <http://steve-yegge.blogspot.co.uk/2006/03/execution-in-kingdom-of-nouns.html>
[link7]: <http://www.simontylercousins.net/journal/2013/3/7/why-bugs-dont-like-f.html>
[link8]: <http://www.haskell.org/haskellwiki/Why_Haskell_just_works>
[link9]: <http://stackoverflow.com/questions/7585493/null-parameter-checking-in-c-sharp>
[link10]: <http://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare>
[link11]: <http://www.amazon.com/First-Design-Patterns-Elisabeth-Freeman/dp/0596007124>
[link12]: <http://en.wikipedia.org/wiki/J_(programming_language)>
[link13]: <http://fsharpforfunandprofit.com/why-use-fsharp/>
[link14]: <http://fsharpforfunandprofit.com/posts/designing-for-correctness/>
[link15]: <http://fsharpforfunandprofit.com/series/designing-with-types.html>
[link16]: <http://fsharpforfunandprofit.com/posts/computation-expressions-intro/>
[link17]: <http://learnyouahaskell.com/>
[link18]: <http://www.tryfsharp.org/Learn>
