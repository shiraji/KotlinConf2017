Hi, Today, I am talking about "How to Kontribute?"

Before I'm going to give a talk about how to contribute kotlin, I want to introduce myself

I have been contributing kotlin since July 2016. I made about 50 commits by now. I love kotlin same as you guys. I have a beautiful wife and cute son. 

I work for ASICS and Runkeeper which has office in here US/Japan. So, if you want to work with me, please contact me @shiraj_i at Twitter or @shiraji at Github

OK, so let's start my talk.

Questions for you guys

Can you use git?
Do you have github account?
Can you write Koltin?

How many people say yes to all these questions? After this talk you guys are able to contribute Kotlin!

Because Kotlin use git as version control system. The repository is in Github. And Kotlin is written in Kotlin!!! Old part is still java but you need to send PR with only koltin code.

One thing you must know is kotlin project consist the many features including Kotlin language, kotlin js, kotlin plugins and so on. The most external contributors mainly work on Kotlin plugin.
Since all codes are in one repository, if you contribute kotlin plugin, you are kotlin contributor. And you can contribute kotlin languages, too if you want.

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

And it kinda hard to setup, so here is my setting.
Don't worry I will post blog entry so that you can copy and paste my setting.

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

https://youtrack.jetbrains.com/issues/KT?q=tag:%20%7BUp%20For%20Grabs%7D%20%23Unresolved

These issues are free to contribute. If you want to work the issue, comment "I am going to do this!". Then, the issue will be yours. One point, since we are external contributors, JetBrains stuff won't assign your name as assignee, but if you comment the issue, you are treated as assignee.

The last one is GitHub

GitHub is basically where JetBrains stuff give us feed back of your PR. Not much communication happens in Github. As far as I know, you should not send new feature without creating issues in youtrack. The small documentation or comment change can be merge without issues. If you are not sure, again, ask question in slack.

After sending PR, you should comment PR URL in youslack. This is because JetBrains stuff didn't look at GitHub. They constantly check youtrack but not github.

So, to conclude communication part,

Ask questions or communicate with others use slack. Find and assign task in youtrack. and then send PR in GitHub.

Are you ready?

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


