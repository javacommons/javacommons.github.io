https://www.nuget.org/packages/ServiceStack

[Simplified Common Lisp reference](https://jtra.cz/stuff/lisp/sclr/)

[#Script Lisp](https://sharpscript.net/lisp/)

`#Script`は、小さく、表現力があり、手首にやさしい動的スクリプト言語として設計されており、世界で最も人気のあるユビキタススクリプト言語であるJavaScriptをモデルにしています。その最小限の構文は、言語構成に対して異なる特注の構文を定義する大規模な言語文法を採用する代わりに、関数を多用する他の小さくても強力な言語からヒントを得ました。

Smalltalkのような小さな言語は、歴史上最も影響力のある言語の一つであるにもかかわらず、ポストカードに収まる最小限の構文で有名である。Smalltalkの発明者であるアラン・ケイは、Lispを、これまでに設計されたプログラミング言語の中で、最も優れた言語であると評価しています。

また、既存のCommon Lispソースコードとの互換性を高めるため、[Simplified Common Lisp Reference](https://jtra.cz/stuff/lisp/sclr/)の大部分と、C# LINQ 101 ExamplesをLispで実装するために必要な全ての不足関数を実装しています。

[Lisp LINQ Examples](https://sharpscript.net/linq/restriction-operators?lang=lisp)

また、読みやすさと親しみやすさを向上させるために、データリストやマップリテラルの定義、匿名関数、.NET Interop用のJava Interopの構文、コレクションのインデックス付けとインデックスアクセッサへのアクセスのためのキーワード構文、Clojureの人気の高いfn, def, defnの短いエイリアスなどのClojure構文を多数採用して、Clojureのソースコード互換性を向上させています。

### Lisp REPL

Lispは`#Script`の第1級言語であると同時に、そのダイナミズムと拡張性から、特に説明的なプログラミングに適しており、REPLによるアクセスは最新のxおよびapp dotnetツールに組み込まれており、Windows、macOS、Linux OS（.NET Core搭載）に素早くインストールすることができます。

```
$ dotnet tool install -g x
```

また、以前のバージョンをインストールしている場合は、以下の方法で最新バージョンにアップデートしてください。

```
$ dotnet tool update -g x
```

これで、Lisp REPLをすぐに立ち上げることができます。

```
$ x lisp
```

下のデモでは、scriptMethodsの問い合わせ、オブジェクトのpropの問い合わせ、Lispインタープリタのグローバルシンボルテーブルの問い合わせ、Lisp関数、マクロ、変数などのグローバルな状態の問い合わせが可能です。

https://www.youtube.com/watch?v=RR7yk6ReNnQ

<iframe width="1000" height="700" src="https://www.youtube.com/embed/RR7yk6ReNnQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Annotated REPL Walk through

以下に、それぞれの表現が何を行っているかを説明したデモの注釈版を掲載します。

Sharp Scripts や Sharp Apps と同様に、Lisp REPL は #Script Pages ScriptContext サンドボックス内で実行され、Sharp App フォルダから実行すると、完全に設定された .NET Core App をシミュレートする .NET Core App Server を起動します。この場合、redis Sharp App ディレクトリで実行され、静的な Web 資産と app.settings で設定された redis サーバー接続にアクセスすることができました。

```
【省略】
```

#### Enable features and access resources with app.settings

### Lisp REPL TCP Server

上記のコンソールからLisp REPLを起動するだけでなく、LispReplTcpServer ServiceStackプラグインで設定されたServiceStackアプリからLisp REPLを起動することができます。これにより、ServiceStack アプリに「プログラム可能なゲートウェイ」が開かれ、ライブクエリの実行、IOC 依存性のアクセス、内部サーバ機能の呼び出し、実行中のサーバの状態に関するクエリなどが可能になり、リモートサーバの問題を診断する際に Debug Inspector と同様に貴重な情報を提供することができます。

このアプリはVuetify SPAアプリで、サーバーサイドスクリプティング機能を使用していないため、空のSharpPagesFeatureで構成されているだけです。

アプリの appsettings.Production.json で DebugMode を設定すると、TCP ソケットサーバが起動し、デフォルトではループバック IP のポート 5005 をリッスンするように設定されます。

```
if (Config.DebugMode)
{
    Plugins.Add(new LispReplTcpServer {
        ScriptMethods = {
            new DbScripts()
        },
        ScriptNamespaces = {
            nameof(TechStacks),
            $"{nameof(TechStacks)}.{nameof(ServiceInterface)}",
            $"{nameof(TechStacks)}.{nameof(ServiceModel)}",
        },
    });
}
```

ScriptNamespaces は、C# の using Namespace; 文のように動作し、完全修飾された Namespace の代わりに Name で Types を参照できるようにします。

基本的なtelnetで接続できるようになりましたが、rlwrap readline wrapユーティリティを使用すると、行の編集、持続的な履歴、補完などの機能が強化され、より快適な使用感が得られます。

```
$ sudo apt-get install rlwrap
```

そして、新しいLisp REPLに接続するためのTCP Connectionを開くことができます。

```
$ rlwrap telnet localhost 5005
```



