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
