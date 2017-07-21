Hi, Today, I am talking about "How to Kontribute?"

How many people already contribute kotlin/kotlin plugin?

OK, get out this room. You have nothing to learn from me

Before I'm going to give a talk about how to contribute kotlin, I want to introduce myself

I have been contributing kotlin since July 2016. I made about 50 commits by now. I love kotlin same as you guys. I have a beautiful wife and cute son. 

I work for ASICS and Runkeeper which has office in here US/Japan. So, if you want to work with me, please contact me @shiraj_i at Twitter or @shiraji at Github

OK, so let's start my talk.

Questions for you guys

Can you use git?
Do you have github account?
Can you write Koltin?

Yes??? Yea, after this talk you are able to contribute Kotlin!

Because Kotlin use git as version control system. The repository is in Github. And Kotlin is written in Kotlin!!! Old part is still java but you need to send PR with only koltin code.

And I have to apologize to anyone who use windows or linux, because I only have mac OS, this talk is based on mac os. but I'm sure you can develop kotlin in windows or linux.

So, here is outline

* Setup environment (mac)
* Communicating with others
* Describe Intention/Inspection/Quickfix
* And if I have time, develop kotlin plugin features

Kotlin plugin??? yes, kotlin project also contains Kotlin plugin. Most of external contributors contributed to Koltin plugin. However, if you understand how to contirbuite kotlin plugin you are able to contribute kotlin language.

Setup!

This is the hardest part. So, please don't give up here.

There are 5 parts of setup. The first setup is JDK

You don't believe this but kotlin use JDK 1.8 and 1.7...AND 1.6!
Yes, you need to install 3 JDKs!

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
At the first time, you can build. See this red lines. IDE's sdk setting is missing. So Go File -> Project Stucture... -> Module SDK 1.6 [Invalid] "New..." button -> Set 1.6 Home

Again, it's hard to see what's going on, I made a movie. 

Yea. Now everything is set. Let's run it. Hit >

Explain what gif does.

To sum up, as for setup, this is the hardest part, and you need to setup JDK, And and kotlin plugin.

If you have a problem, ask other contributors like me.

How? OK, let's talk about how to communicate with others.

For communication, we mainly uses 3 tools, slack, youtrack, and github.

Let's talk about the use cases of these tools.

The first one is slack. I believe most of people in this room already use the team. There is public team call kotlinlang.

You can get invitation from this URL.

kontributor channel is the one that most of contributors and JetBrains stuff stay.
If you have trouble building Kotlin project or anything that is related to contributing Kotlin project, ask in the channel.
In most time, people answer within one business day. 

The second one is youtrack

youtrack is the issue management tool that JetBrains developed. Kotlin's public issues are listed there.
Have you visit this site? https://youtrack.jetbrains.com/issues/KT
You can work anything if there is no assignee.
If you don't have any idea which issue you are looking to, then check "up-for-grabs" tag.

https://youtrack.jetbrains.com/issues/KT?q=tag:%20%7BUp%20For%20Grabs%7D%20%23Unresolved

These issues are free to contribute. If you want to work the issue, comment "I am going to do this!". Then, the issue will be yours. One point, since we are external contributors, JetBrains stuff won't assign your name as assignee, but if you comment the issue, you are treated as assignee.

