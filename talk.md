 Hi, Today, I am talking about "How to Kontribute?"

Before I'm going to give a talk, I wanna introduce myself

My name is Yoshi. My github account name is shiraji.
I have been contributing kotlin since July 2016. I made about 50 commits by now. I love kotlin same as you guys. I have a beautiful wife and cute son. 

I work for ASICS and Runkeeper which has office in here US/Japan. So, if you want to work with me, please contact me @shiraj_i at Twitter or @shiraji at Github

OK, so let's start my talk.

Questions for you guys

Can you use git?
Do you have github account?
Can you write Koltin?

How many people say yes to all these questions? 

The purpose of this talk is someone who said yes to the questions become a contributor of kotlin!

Because Kotlin use git. The repository is in Github. And Kotlin is written in Kotlin!!! Old part is still java but you need to send PR with only koltin code.

One thing you must know is kotlin project consist the many features including Kotlin language, kotlin js, kotlin plugins and so on. The most external contributors mainly work on Kotlin plugin.
And this talk focus on contributing Kotlin plugin.

And I have to apologize to anyone who use windows or linux, because I only have mac OS, this talk is based on mac os. but I'm sure you can develop kotlin in windows or linux.

So, here is outline

* Setup environment (mac)
* Communicating with others
* Describe Intention/Inspection/Quickfix
* And if I have time, develop kotlin plugin features

Setup!

This is the hardest part. So, please don't give up here.

There are 5 parts of setup. The first setup is JDK

You don't believe this but kotlin use JDK 1.8 and 1.7...AND 1.6!
Yes, you need to install 3 version of JDKs!

According to documentation, we need to have this setting.

And it kinda hard to setup, so here is what I did.
Don't worry I will post this slide. 

The next setting is Ant

Kotlin use ant to build. Ant version must be higher than 1.9.4.
If you use mac, run brew command. 
In most case, you need memory setting. add ANT_OPTS environment variable like this.

Oops

The third one is buid command

Run this command.

First command will setup dependencies including download all dependencies. It took me 32 minites to complete
Second command will build the binaries of the compiler. It took me about 7 minites.

There are other setting that can support variety of development, but this is enough for the beginning. I haven't used any other settings.

The forth setting is Kotlin plugin

You must use development version Kotlin plugin to develop Kotlin.

In order to use development version of plugin, go to Preferences -> Plugins -> Browse Repositories -> Manage Repositories... and then hit plus sign. Copy this URL.

Well, it's hard to explain in doc, I brought gif images for how to setup dev plugin.

The last setting is...JDK, again!!!

After setup Kotlin plugin, you can open kotlin project in Intellij IDE. Both community version and IDEA version works fine.
At the first time, you got compile error. See this red lines. IDE's sdk setting is missing. So Go File -> Project Stucture... -> Module SDK 1.6 [Invalid] "New..." button -> Set 1.6 Home

Again, it's hard to see what's going on, I made a movie. 

Yea. Now everything is set. Let's run it. Hit >

Explain what gif does.

To sum up, as for setup, this is the hardest part, and you need to setup JDK, And and kotlin plugin.

If you have a problem, ask other contributors like me.

How? OK, let's go to the next topic. Communicating with other developers

For communication, we mainly uses 3 tools, slack, youtrack, and github.

Let's talk about the use cases of these tools.

The first one is slack. I believe most of people in this room already use the team. There is public team call kotlinlang.

You can get invitation from this URL.

kontributor channel is the one that most of contributors and JetBrains stuff are staying.
If you have trouble building Kotlin project or anything that is related to contributing Kotlin project, ask at the channel.
In most time, people answer within one business day. 

The second one is youtrack

youtrack is the issue management tool that JetBrains developed. 
Have you visit this site? https://youtrack.jetbrains.com/issues/KT
This is the youtrack site that Kotlin's public issues are listed.
You can work anything if there is no assignee.
If you don't have any idea which issue you are looking for, then check "up-for-grabs" tag.
These issues are free to contribute. If you want to work the issue, comment "I am going to do this!". Then, the issue will be yours. One point, since we are external contributors, JetBrains stuff won't assign your name as assignee, but if you comment the issue, you are treated as assignee.

The last one is GitHub

GitHub is basically where JetBrains stuff give us feed back of your PR. Not much communication happens in Github. As far as I know, you should not send new feature without creating issues in youtrack. The small documentation or comment change can be merge without issues. If you are not sure, again, ask question in slack.

After sending PR, you should comment PR URL in YouTrack's issue. This is because JetBrains stuff didn't look at GitHub. They constantly check youtrack but not github.

So, to conclude communication part,

Ask questions or communicate with others use slack. Find and assign task in youtrack. and then send PR in GitHub.

Are you ready?

Well...since we have a time, let me explain how to create inspection step by step.

As I said before, these are the things I have to do for creating inspections

This is what I have contributed before. Basically, this inspection reports astarisk and intArrayOf method call is useless. And then quickfix remove them.

And this one is the final product. 

So, where I should start working? First thing is I need to find what Expression class is used. 

It's imporssible to identify it by stealing the code. Well, if you spent more than 1 year, like me, you may be able to guess what expression is used. This gonna be KtValueArgument.

Anyway, it's hard, so use PSI Viewer which is bundled in child IDE.

PSI Viewer is the tool that identifies the structure of the code. You can go to Tools and click View PSI Structure of Current File.

***PSI Viewerの使い方***

Now, we understand the target class that the inspection applies is `KtValueArgument`. Let's create the inspection class for it.

First, create a class that inherits `AbstractKotlinInspection` class. Override `buildVisitor` method. 

`buildVisitor` returns `PsiElementVisitor` class. This class is a visitor which can be used to visit elements for all languages. We use `KtVisitor` which is a Kotlin version of visitor class. The visitor class has method `visitArgument` that has `KtValueArgument` as a parameter. Whenever the IDE's inspection visits `KtValueArgument`, IDE call registered inspections that implement `visitArgument` method.

So going back to code, this code uses `KtVisitorVoid` class. `KtVisitorVoid` class inherit `KtVisitor` class that the all visit method returns void. Well, it's written in Java, void is correct word.

OK, so I'm ready for hello world.

The next step is registering the inspection. To do so, we will add localInspection tag to plugin.xml file.

localInpection tag is something like this. As of implementationClass value is the FQN of the inspection class.
displayName is the text that appear when we press alt+enter

We registered the inspection, let's debug it.

***debug gif***

To debug the inspection, the same as normal project, we can use break point. Put it on this line. And hit the debug button.

Wait a few second...Child IDE popup.
Now, let's write the sample code to see the inspection is registered or not.

Create new file, copy and paste the code, and...there you go.

You can check the variables and expressions like this.

***end debug gif***

Cool. So the next step is implementing the inspection. I will explain this line by line very quickly.

Check whether argument has spread element
Check the argument has name
Check the argument expression is arrayOf method
Finally, show warning and register the quickfix

RemoveRedundantSpreadOperatorQuickfix is the quick fix that remove asterisk and the arrayOf method call.

Now, we have done these 2 steps. The next one is the description of inspection.

File name is the Inspection class name without "Inspection"

Last one is the test cases.

Create inspection folder with name lower camel without "Inspection"
By the way, kotlin plugin has 2 ways to test inspections. Don't add the folder under idea/testData/inspections. inspectionLocal is the correct one.

Add .inspection file that contains FQN of the inspection

Create test data. The test data file name can be anything but I recommend to set the descriptive one.

Let's create basic test case.

We needs 2 test data. First one is before applying quickfix. The extention is `.kt`
The test data must provides caret as triangle bracket with caret.

The second one represents applying the quickfix. The extention is `.kt.after`

There are so many options for these test data. For instance, when you want to use runtime dependencies, add comment WITH_RUNTIME at top of the both data files.
And I imagine lots of people want to ask "is there document???" Well, there is text files named AbstractInspectionTest and KotlinLightCodeInsightFixtureTestCase. It's written in Kotlin!

Yea, we have done all TODO.

Now, what? Sending PR.

Commit size should be 1 or 2, Commit comment should include "Issue number Fixed".
As I said before, JetBrains guys only watch YouTrack, not github. So, I recommend you to do is comment the pull request URL to the issue.
If no one assign as a reviewer to the pull request within a few business days, Ask them at Slack. 

Since JetBrains engineers are so busy, wait. Don't push them check the pull request again and again. Just wait.

After they review the pull request, they will eventually, merge the commit into the master branch.

If your pull request merge, your name will be listed Kotlin blog like this. The name come from GitHub profile. Change whatever you want them list.

Yes, that's it for my talk. Well, now you guys are ready to become External contributor!!!

Any questions?



---

If you go to youtrack and find the "up-for-grabs" tag, you will find there are sevral types of issues. The main types of issues are developing these 3 features. Inspection, Intention, and quickfix.

According to the official documentation, 

Inspection or Code Inspection is

> [Code Inspection] detects compiler and runtime errors, suggests corrections and improvements before you even compile.

Intention is

> IntelliJ IDEA recognizes code constructs that can be optimized or improved, and suggests appropriate intention actions

Quickfix is

> A quick fix allows to apply an automatic changes to the code

Briefly I will explain what you need to do for these three features.

This is the list of files you need to add when you create new Inspection, where Xxx is the Inspection name.

As I said before, don't worry about the detail because I will post this slide.

* Add XxxInspection.kt idea/src/org/jetbrains/kotlin/idea/inspections/
* Add localInspection tag to idea/src/META-INF/plugin.xml
* Add Inspection descritption idea/resources/inspectionDescriptions/Xxx.html
* Add test data idea/testData/inspectionsLocal/xxx

For creating new Intention, this is the list, where Xxx is the Intention name

* Add XxxIntention.kt idea/src/org/jetbrains/kotlin/idea/intentions/
* Add intentionAction tag idea/src/META-INF/plugin.xml
* Add intention description idea/resources/intentionDescriptions/XxxIntention/description.html
* Add idea/resources/intentionDescriptions/XxxIntention/before.kt.template and idea/resources/intentionDescriptions/XxxIntention/after.kt.template
* Add test data idea/testData/intentions/xxx

And finally, the quickfix. Quick fix is the easiest.

* Add XxxFix.kt idea/src/org/jetbrains/kotlin/idea/quickfix/
* Register quick fix at idea/src/org/jetbrains/kotlin/idea/quickfix/QuickFixRegistrar.kt
* Add test data idea/testData/quickfix/xxx
