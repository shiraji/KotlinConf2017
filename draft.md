# Title

How to Kontribute

# 最初に

皆さん自分が誰なのかわからないと思いますが、気にしないで下さい。

とりあえずここで必要なのは

* 2016/07からKotlinの外部コントリビューター
* Kotlinが大好き

ということです。このトークではKotlinにコントリビュートする場合の注意事項を伝えたいと思います。

KotlinはKotlinで書かれています。
私はKotlinを勉強したくて、最終的にKotlinを書くことを選びました。
きっとここにいる人たちもみんなKotlinが好きですよね？
だからここにいる人たちみんなが一緒にKotlinを盛り上げてくれたらいいなと思いこのトークをしたいと思います。

# Things I won't explain

* Git/GitHub
* Kotlinの文法
* WindowsやLinuxでの開発(私がMacユーザなので・・・。すまんね)

# Outline

アウトラインはこんな感じです。

* Setup development environment
* Communicating with other developers
* Developing one of Kotlin plugin features
* Implementing unit tests
* Sending a pull request

# Setup

たぶん、ここが一番難しいです。

## JDK

信じられないと思いますが、KotlinはJDK1.6, 1.7, 1.8全てが必要です。

私は色んなOSSに参加しているのですが、JDKを複数必要としているプロジェクトはこれが初めてです。

こんな感じで設定するそうです。

```
  JAVA_HOME="path to JDK 1.8"
  JDK_16="path to JDK 1.6"
  JDK_17="path to JDK 1.7"
  JDK_18="path to JDK 1.8"
```

JDK全てをインストールしたのであれば、私の設定ファイルを参考にすると簡単です。

```
export JAVA_HOME=`/usr/libexec/java_home -v "1.8"`
export JDK_16=`/usr/libexec/java_home -v "1.6"`
export JDK_17=`/usr/libexec/java_home -v "1.7"`
export JDK_18=`/usr/libexec/java_home -v "1.8"`
```

HotSpotを利用していますが、OpenJDKでも動くはずです。試したことないけど。

## 他のツール

Apache Ant 1.9.4以上が必要です。

"Gradle Meets Kotlin"

## Let's build!

The first build takes seriously long. My machine connect to wifi. It takes about 2 hours to download dependencies. So please please please don't give up here. Just wait. Downloading latest IDE takes time. TO: JetBrains, please please please make this download first!!!

ということで、コマンドですが実は普通に叩こうとするとまずメモリーが足らなくて失敗します。
そこで、antの設定をします。

```
export ANT_OPTS="-Xmx1024m -XX:MaxPermSize=512m"
```

この環境変数を設定した後、以下のコマンドを叩きます。

```
ant -f update_dependencies.xml
ant -f build.xml
```

ant -f update_dependencies.xml which will setup dependencies
ant -f build.xml which will build the binaries of the compiler

他にもオプショナルな設定がありますが、READMEを参考にしてください。
私はこれだけで十分でした。

## Kotlin plugin

Kotlinプラグインは開発版を使う必要があります。

*設定方法は動画で示す*

Preferences -> Plugins -> Browse Repositories -> Manage Repositories...

https://teamcity.jetbrains.com/guestAuth/repository/download/bt345/bootstrap.tcbuildtag/updatePlugins.xml

この開発版は不定期に更新されます。毎回アップデートすることをオススメされていますが、
私はmasterを取り込んでビルド出来なくなった時点でアップデートしています。

Kotlinの開発専用マシンを用意出来るのであれば、それでいいのですが、私のように奥様にバジェット握られているエンジニアは
IDEA_PROPERTIESによる設定で切り替えることが出来ます。

詳細はこちら。
https://intellij-support.jetbrains.com/hc/en-us/articles/207240985-Changing-IDE-default-directories-used-for-config-plugins-and-caches-storage

私はpluginのパスだけ設定しています。

```
% env | grep IDEA
IDEA_PROPERTIES=/Users/isogai/idea.properties
% cat idea.properties
idea.plugins.path=/Users/isogai/kotlin_dev/plugins
```

Kotlinのstable版とdev版を切り替えることが出来ます。

## Run!

これでようやく環境周りは整いました。
最後に、ビルドしたKotlinプロジェクトを動かしてみましょう。
いくつかRunの設定がありますが、あとでそれは解説するとして、ここではIDEAを選び、実行します。

そして、数秒待つと・・・

IDEが立ち上がります。これでOKです。
あとはこの立ち上がった子IDEでKotlinを書いてみて下さい。ビルドしたKotlinプロジェクトを利用することが出来ます。

## Setupまとめ

JDKの設定
Antの設定
Antでのビルド
Kotlinプラグインの設定

これらを揃えればRun出来るはずです。

もし、この時点で動かない！となった場合、他の開発者に確認をしたほうが早いです。
では、他の開発者とのコミュニケーションの方法を説明していきたいと思います。

# Communicating with other developers

他の開発者とのコミュニケーションにはいくつか手段があります。

* Slack
* youtrack
* GitHub

## Slack

たぶんここにいる人たちはみんなすでに登録しているだろうけど、Kotlinlangという公開チャンネルがあります。

http://slack.kotlinlang.org/

ここで招待を受けて下さい。

ここにある、#kontributorsというチャンネルにコントリビュートしている人たちが集まっています。

もし、さっきの説明でビルド出来ない。Run出来ないという状況に陥ってしまったら、ここで質問すると回答をもらえます。
自分も最初どうしてもビルド出来なくて、ここで質問しました。

他にもここでは、どういう実装をしたら良いのか？どこかにヒントはないのか？など実装に関する質問を受け付けています。
JetBrainsの人やすでにコントリビュートしている方たちが親切に教えてくれます。

## youtrack

youtrackはJetBrains製のIssue管理ツールです。Kotlinの公開されているissueはここで管理されています。

https://youtrack.jetbrains.com/issues/KT

