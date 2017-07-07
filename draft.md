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
* Developing/Testing Kotlin new features

# Setup

たぶん、ここが一番難しいです。

## JDK

信じられないと思いますが、KotlinはJDK1.6, 1.7, 1.8全てが必要です。

For apple JDK1.6
https://support.apple.com/kb/dl1572?locale=en_US

For Java SE Development Kit 7
http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html

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

Macで開発をする場合私はbrewを使いました。
`brew install ant`

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

ant -f update_dependencies.xml which will setup dependencies - e.g. My machine took "Total time: 32 minutes 35 seconds"
ant -f build.xml which will build the binaries of the compiler - e.g. My machine took "Total time: 6 minutes 20 seconds"

他にもオプショナルな設定がありますが、READMEを参考にしてください。
私はこれだけで十分でした。

## Kotlin plugin

Kotlinプラグインは開発版を使う必要があります。

Preferences -> Plugins -> Browse Repositories -> Manage Repositories...に遷移して、

https://teamcity.jetbrains.com/guestAuth/repository/download/bt345/bootstrap.tcbuildtag/updatePlugins.xml

を設定すればOKです。

わかりにくいので、動画にしてみました。

![git/install_kotlin_dev.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/install_kotlin_dev.gif)

この開発版は不定期に更新されます。毎回アップデートすることをオススメされていますが、
私はmasterを取り込んでビルド出来なくなった時点でアップデートしています。

**ここはIDEAが修正されない限りスルーする。**

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

## JDK, again!

ここまでがドキュメントに書いてあることです。

実はドキュメントに一つ書かれていない設定があります。
ここまでの経緯でJDK1.6はすでにインストール済みだと思いますが、IDE側にも設定します。

![sdk1.6missing](https://github.com/shiraji/KotlinConf2017/raw/master/images/sdk1.6missing.png)

設定方法は

File -> Project Stucture... -> Module SDK 1.6 [Invalid] "New..." button -> Set 1.6 Home

![setup_1.6sdk.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/setup_1.6sdk.gif)

## Finally...Run!

これでようやく環境周りは整いました。
最後に、ビルドしたKotlinプロジェクトを動かしてみましょう。
いくつかRunの設定がありますが、あとでそれは解説するとして、ここではIDEAを選び、実行します。

そして、数秒待つと・・・

IDEが立ち上がります。これでOKです。

![run_idea](https://github.com/shiraji/KotlinConf2017/raw/master/images/run_idea.gif)

あとはこの立ち上がった子IDEでKotlinを書いてみて下さい。ビルドしたKotlinプロジェクトを利用することが出来ます。
子IDEでは実プロジェクトに対して動くので、空のKotlin用テストプロジェクトを作って、そこでIssueの再現をすると良いと思いますよ。

DebugやBreakpointsも機能しますよ。

**ここに動画入れるとわかりやすいかも？**

## Setupまとめ

JDKの設定
Antの設定
Antでのビルド
Kotlinプラグインの設定

これらを揃えればRun出来るはずです。

もし、この時点で動かない！となった場合、他の開発者に確認をしたほうが早いです。
では、その他の開発者とのコミュニケーションの方法を説明していきたいと思います。

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

担当したいIssueがあったら、これをやるね！とコメントすれば良いです。
外部のコントリビューターはIssueにアサインされません。が、JetBrainsの人たちはコメントが入っていれば誰が対応しているのか把握しています。

また、後でも話しますが、GitHubでpull requestを出した時に対象のIssueのコメントにpull requestのURLを貼り付けて下さい。

どれをやれば良いのかわからない！という方は

https://youtrack.jetbrains.com/issues/KT?q=tag:%20%7BUp%20For%20Grabs%7D%20%23Unresolved

ここに、外部コントリビュータ向けの"up-for-grabs"というタグがついたIssueがあるので、ここから選ぶのが良いです。

あとKotlin pluginの新機能はここでIssueをあげることができます。
バグ報告などもここで受け付けています。重複するとJetBrainsの人が大変なので、同じような報告がないかはしっかり調べて報告してくださいね。

## GitHub

GitHub上でのやり取りはコードレビューくらいしかありません。Kotlinレポジトリにはissueは受け付けていないためです。issueはyoutrackで受け付けています。
Kotlinの開発フローですが、ブランチ戦略はGitHub flowと同じです。
masterからそれぞれがブランチを作成し、masterへマージされていきます。

ドキュメントの修正であればいきなりPRを送りつけてもマージされます。
しかし、バグfixではなく、新機能などをいきなり送りつけてもReviewerがアサインされず最終的にマージされなかったりします。
例えbug fixでもしっかり説明を書かなくてはなりません。
そこでオススメなのが、もしバグfixをしたいのであれば、youtrackにissueを上げ（もちろんすでに上がってないことを確認して下さいね。）その後、自分で対応してしまいましょう。
それであればみんなハッピーです。

最近外部のコントリビューターが増えてきたため、レビューしてくれる人たちも大変そうです。
ですので、いきなりGitHubにPRを送るのはKotlinのためにならないので、しっかり手順を踏んで、Kotlinのためになる開発をしてくれたら良いなと自分が思っています。

**これ削って良いかも？**

別件になりますが、KotlinのKEEPというレポジトリを紹介します。
https://github.com/Kotlin/KEEP
このレポジトリはKotlin Evolution and Enhancement Processと言い、Kotlinの新機能のproposalが議論されています。
私は読むだけしかしていませんが、議論を読むだけでも非常に楽しいです。色々ルールがあるので、しっかりREADMEは読んで下さい。

## まとめ

コミュニケーションは基本的にSlackで実施されています。
youtrackでIssueは管理されています。
GitHubでPull Requestを受け付けています。会話はコードレビューのやり取りのみです。ドキュメントの修正以外であれば、youtrackに報告してから対応した方が良いです。

# Developing one of Kotlin plugin features

私は主にKotlinプラグインのほうの開発をしています。"up-for-grab"のIssueも主にKotlin pluginの開発に関するIssueです。
外部のコントリビューターの多くが以下のカテゴリの対応を実施しています。

* Inspection
* Intention
* Quickfix

他にもJ2Kだったり、Quickdocだったり、フォーマッタだったりといくつかあります。

ところで、KotlinというよりIntellijの機能の話ですが、Inspection, Intention, Quickfixの違いってわかりますか？

InspectionはIntelliJが内蔵している静的コード解析ツールでバグの可能性がある箇所を探し出したり、利用されていないコードやパフォーマンス問題となりそうな箇所を指摘したりすることでコード全体を改善する手助けになります。
Intentionはコードの潜在的な問題点の修正案の提示や、修正を行ってくれる機能です。カーソル位置に適用されるインテンションのリストを見るにはOption + Enter(WindowsではAlt + Enter)を押します。

A quick fix allows to apply an automatic changes to the code via Show Intention Actions 

well, you don't understand this, right? Don't worry. You will figure this out after contributing Kotlin several times.

## Inspection

実際にやることは以下

* idea/src/org/jetbrains/kotlin/idea/inspections/にXxxInspection.ktを追加
* idea/src/META-INF/plugin.xmlにlocalInspectionのタグを追加
* idea/resources/inspectionDescriptions/Xxx.htmlのInspectionの説明を追加
* idea/testData/inspectionsLocal/xxxにテストデータを作成

**ここはスライドというより、細かくやることを記載。だけど、実際には説明しない。やるときにこのスライド見てね！くらいでいいと思う。**

**一つわかりにくいところがTestデータを作成し、テストを作成するところ。手順が複雑なので、そこはgifを作成し、紹介する。**

## Intention

https://github.com/JetBrains/kotlin/commit/83169ad78106ad407559492f617e6efa3107c020

```kotlin
open class Foo {
  fun foo() {} // can be "open"
}
```

![make_open.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/make_open.gif)

実際にやることは以下

* idea/src/org/jetbrains/kotlin/idea/intentions/XxxIntention.ktを追加
* idea/src/META-INF/plugin.xmlにintentionActionタグを追加
* idea/resources/intentionDescriptions/XxxIntention/description.htmlにIntentionの説明を追加
* idea/resources/intentionDescriptions/XxxIntention/before.kt.templateとidea/resources/intentionDescriptions/XxxIntention/after.kt.templateを追加
* idea/testData/intentions/xxxにテストデータを作成

**ここもやるときに見てね！くらいに止める。before/afterの<spot>タグの説明くらいはしてもいいかも？こいつもGenerate Testでいけることも再度言う**
**Generate Testへのテスト追加方法も記述**

## Quickfix

https://github.com/JetBrains/kotlin/commit/ab4eb1dd2098e1ea631ce7dbf8e63b8573eb2460

```kotlin
fun foo(text: String?) {
    text.let { 
        it.length + 1 // Syntax error. "it" is nullable.
    }
}
```

![quickfix_scope_fun.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/quickfix_scope_fun.gif)

* idea/src/org/jetbrains/kotlin/idea/quickfix/XxxFix.ktを作成する
* idea/src/org/jetbrains/kotlin/idea/quickfix/QuickFixRegistrar.ktの対象のエラーにregisterする
* idea/testData/quickfix/xxxにテストデータを作成

**ここもやるときに見てね！くらいに止める。こいつもGenerate Testでいけることも再度言う**


さて、これだけだとわからないので、Inspectionを作ってみましょう。

* idea/src/org/jetbrains/kotlin/idea/inspections/にXxxInspection.ktを追加
* idea/src/META-INF/plugin.xmlにlocalInspectionのタグを追加
* idea/resources/inspectionDescriptions/Xxx.htmlのInspectionの説明を追加
* idea/testData/inspectionsLocal/xxxにテストデータを作成

https://github.com/JetBrains/kotlin/commit/cbccf932a78d43e37d24a77f2ba178f866d383dc

![spread_intention.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/spread_intention.gif)

Inspetion/Intentionを作る場合、Kotlinのどの部分がどのクラスに値するのか？を知らないと作ることが出来ません。

```kotlin
fun foo(vararg bars: Int) {
}

fun main(args: Array<String>) {
    foo(*intArrayOf(1, 2, 3)) // "*intArrayOf" is useless!
}
```

この文法、どういう構成で作られているかわかりますか？解析するために全ての構文が何かしらの表現クラスで表現されています。
そのクラスに対してIntention/Inspectionを作るのですが、実際問題`*intArrayOf(1,2,3)`がどういうクラスで表現されているかわかりますか？
見ただけではわからないです。自分は慣れちゃったので、だいたいわかるんですけどね。
しかし、Intention/Inspectionを作るときに一番最初に知らないといけないのが対象の表現はどのクラスで認識されているのか？ということです。

そこで、PSI Viewerを使います。子IDEのTools -> View PSI Structure of Current Fileで起動します。cmd+shift+aで探すのが簡単なので動画ではそれで探しています。

実際にどうなるか見てみましょう。

![psi_viewer.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/psi_viewer.gif)

つまり、これからKtValueArgumentに対してInspectionを作ることになります。AbstractKotlinInspectionを継承します。
buildVisitorをoverrideし、KtVisitorVoidを実装します。

```kotlin
class RemoveRedundantSpreadOperatorInspection : AbstractKotlinInspection() {
    override fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): PsiElementVisitor {
        return object : KtVisitorVoid() {
        }
    }
}
```

KtVisitorVoidクラス内には多くのVisitメソッドがあります。このメソッドはさっき見た、表現にvisitする度に呼ばれます。KtValueArgumentのVisitメソッドはvisitArgumentメソッドなのでvisitArgumentメソッドを継承します。

```kotlin
class RemoveRedundantSpreadOperatorInspection : AbstractKotlinInspection() {
    override fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): PsiElementVisitor {
        return object : KtVisitorVoid() {
            override fun visitArgument(argument: KtValueArgument) {
                println(argument.text)
            }
        }
    }
}
```

さて、実際にKtValueArgumentのvisitごとにこのメソッドが呼ばれるのかを確認したいです。その前に、plugin.xmlにlocalInspectionのタグを追加しておきます。

```xml
    <localInspection implementationClass="org.jetbrains.kotlin.idea.inspections.RemoveRedundantSpreadOperatorInspection"
                     displayName="Redundant spread operator"
                     groupName="Kotlin"
                     enabledByDefault="true"
                     level="WARNING"
                     language="kotlin"
    />
```

display nameはalt+enterの時に出てくるラベルで、groupNameは説明のグループ、enabledByDefaultは絶対true。levelはそれぞれに合わせたものにします。

これで準備が整いました。ブレークポイントを入れて、デバッグモードで動かしてみましょう。

**実際に動かしてみる**

はい。どんどん来ます。。。さっき見せた通り、全てのKtValueArgumentの訪問時にメソッドが呼ばれます。


