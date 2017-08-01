# slides

TODO: テンプレートをもらったら実際に作る。

# QR code of slide URL(CA.kt/KotlinConf)

# How to Kontribute

# How many people already contributed kotlin?

# Who am I

* Kontributing Kotlin since July 2016
* Love Kotlin!
* With beautiful wife + cute son
* Work for ASICS/Runkeeper

# Questions?

* Can you use git? (add, commit, push)
* Do you have github account?
* Can you write Kotlin?

# YES???

Contribute Kotlin!!!

# Outline

* Setup development environment
* Communicating with other developers
* Intention/Inspection/Quickfix
* Developing/Testing Kotlin plugin features 

# Setup

* Hardest part!!!

# Setup - JDK

* Need JDK1.8
* Need JDK1.7!
* Need JDK1.6!!!

# Setup - JDK - what doc says

```
  JAVA_HOME="path to JDK 1.8"
  JDK_16="path to JDK 1.6"
  JDK_17="path to JDK 1.7"
  JDK_18="path to JDK 1.8"
```

# Setup - JDK - What I did

```
export JAVA_HOME=`/usr/libexec/java_home -v "1.8"`
export JDK_16=`/usr/libexec/java_home -v "1.6"`
export JDK_17=`/usr/libexec/java_home -v "1.7"`
export JDK_18=`/usr/libexec/java_home -v "1.8"`
```

# Setup - Ant

* Apache Ant 1.9.4
* `brew install ant`
* Memory setting `export ANT_OPTS="-Xmx1024m -XX:MaxPermSize=512m"`

* "Kotlin Meets Gradle"???

# Setup - Build

* Run command
```
ant -f update_dependencies.xml
ant -f build.xml
```

# Setup - Kotlin plugin

* Requires dev version of Koltin plugin
* Preferences -> Plugins -> Browse Repositories -> Manage Repositories...
* https://teamcity.jetbrains.com/guestAuth/repository/download/bt345/bootstrap.tcbuildtag/updatePlugins.xml

# Setup - Kotlin plugin

![git/install_kotlin_dev.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/install_kotlin_dev.gif)

# Setup - JDK, again!!!

* IDE's sdk is missing

![sdk1.6missing](https://github.com/shiraji/KotlinConf2017/raw/master/images/sdk1.6missing.png)

* File -> Project Stucture... -> Module SDK 1.6 [Invalid] "New..." button -> Set 1.6 Home

# Setup - JDK, again!!!

![setup_1.6sdk.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/setup_1.6sdk.gif)

## Finally...Run!

* Hit >

![run_idea](https://github.com/shiraji/KotlinConf2017/raw/master/images/run_idea.gif)

# Setup

* JDK
* Ant
* Kotlin plugin
* Have problem? Ask!

# Outline

* Setup development environment
* Communicating with other developers
* Intention/Inspection/Quickfix
* Developing/Testing Kotlin plugin features 

# Communication

* Slack
* youtrack
* GitHub

# Communication - slack

* Kotlin lang http://slack.kotlinlang.org/
* #kontributors

# youtrack

https://youtrack.jetbrains.com/issues/KT
* Kotlin issue manager
* "up-for-grabs" tag
https://youtrack.jetbrains.com/issues/KT?q=tag:%20%7BUp%20For%20Grabs%7D%20%23Unresolved
* Comment "I'm gonna do this!"

# GitHub

* Code Review
* ❌　new feature without issue
* ⭕️　Docs/comments

# Communication Summary

* Ask questions in Slack
* Talk about issue with YouTrack
* Send PR Github

# image

want to try???

# Intention/Inspection/Quickfix

* Inspection
* Intention
* Quickfix

# Inspection

* Add XxxInspection.kt idea/src/org/jetbrains/kotlin/idea/inspections/
* Add localInspection tag to idea/src/META-INF/plugin.xml
* Add Inspection descritption idea/resources/inspectionDescriptions/Xxx.html
* Add test data idea/testData/inspectionsLocal/xxx

# Intention

* Add XxxIntention.kt idea/src/org/jetbrains/kotlin/idea/intentions/
* Add intentionAction tag idea/src/META-INF/plugin.xml
* Add intention description idea/resources/intentionDescriptions/XxxIntention/description.html
* Add idea/resources/intentionDescriptions/XxxIntention/before.kt.template and idea/resources/intentionDescriptions/XxxIntention/after.kt.template
* Add test data idea/testData/intentions/xxx

# Quickfix

* Add XxxFix.kt idea/src/org/jetbrains/kotlin/idea/quickfix/
* Register quick fix at idea/src/org/jetbrains/kotlin/idea/quickfix/QuickFixRegistrar.kt
* Add test data idea/testData/quickfix/xxx

# Developing/Testing Kotlin plugin features

Well...I don't think this is enough...Let's make PR!

# Inspection

* Add XxxInspection.kt idea/src/org/jetbrains/kotlin/idea/inspections/
* Add localInspection tag to idea/src/META-INF/plugin.xml
* Add Inspection descritption idea/resources/inspectionDescriptions/Xxx.html
* Add test data idea/testData/inspectionsLocal/xxx

# Remove redundant spread op

```kotlin
fun foo(vararg bars: Int) {
}

fun main(args: Array<String>) {
    foo(*intArrayOf(1, 2, 3)) // "*intArrayOf" is useless!
}
```

https://github.com/JetBrains/kotlin/commit/cbccf932a78d43e37d24a77f2ba178f866d383dc

# Remove redundant spread op

![spread_intention.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/spread_intention.gif)

# Where to start???

* Find out what Expression class
* Impossible with looking at code
* Use PSI Viewer!

# PSI Viewer

* Identify the structure
* Tools -> View PSI Structure of Current File

![psi_viewer.gif](https://github.com/shiraji/KotlinConf2017/raw/master/images/psi_viewer.gif)

# Create Inspection file

```kotlin
class RemoveRedundantSpreadOperatorInspection : AbstractKotlinInspection() {
}
```

# Create Inspection file

```kotlin
class RemoveRedundantSpreadOperatorInspection : AbstractKotlinInspection() {
    override fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): PsiElementVisitor {
        TODO("Do it later")
    }
}
```

# Create Inspection file

```kotlin
class RemoveRedundantSpreadOperatorInspection : AbstractKotlinInspection() {
    override fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): PsiElementVisitor {
        return object : KtVisitorVoid() {
        }
    }
}
```

# Create Inspection file

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

# Inspection

* Add XxxInspection.kt idea/src/org/jetbrains/kotlin/idea/inspections/
* Add localInspection tag to idea/src/META-INF/plugin.xml
* Add Inspection descritption idea/resources/inspectionDescriptions/Xxx.html
* Add test data idea/testData/inspectionsLocal/xxx

# Add localInspection

```xml
    <localInspection implementationClass="org.jetbrains.kotlin.idea.inspections.RemoveRedundantSpreadOperatorInspection"
                     displayName="Redundant spread operator"
                     groupName="Kotlin"
                     enabledByDefault="true"
                     level="WARNING"
                     language="kotlin"
    />
```

# Debug

**gif**

# Implement Inspection

```kotlin
class RemoveRedundantSpreadOperatorInspection : AbstractKotlinInspection() {
    override fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): PsiElementVisitor {
        return object : KtVisitorVoid() {
            override fun visitArgument(argument: KtValueArgument) {
                super.visitArgument(argument)
                // Is there spread operator?
                val spreadElement = argument.getSpreadElement() ?: return
                // Is there name?
                if (argument.isNamed()) return
                val argumentExpression = argument.getArgumentExpression() as? KtCallExpression ?: return
                // the function name is arrayOf method?
                if (argumentExpression.isArrayOfMethod()) {
                    // OK, let's register problem
                    val argumentOffset = argument.startOffset
                    val problemDescriptor = holder.manager.createProblemDescriptor(
                            argument,
                            TextRange(spreadElement.startOffset - argumentOffset,
                                      argumentExpression.calleeExpression!!.endOffset - argumentOffset),
                            "Remove redundant spread operator",
                            ProblemHighlightType.LIKE_UNUSED_SYMBOL,
                            isOnTheFly,
                            RemoveRedundantSpreadOperatorQuickfix() // register quick fix
                    )
                    holder.registerProblem(problemDescriptor)
                }
            }
        }
    }
}
```

# Inspection

✅ Add XxxInspection.kt idea/src/org/jetbrains/kotlin/idea/inspections/
✅ Add localInspection tag to idea/src/META-INF/plugin.xml
* Add Inspection descritption idea/resources/inspectionDescriptions/Xxx.html
* Add test data idea/testData/inspectionsLocal/xxx

# Add description

idea/resources/inspectionDescriptions/RemoveRedundantSpreadOperator.html

```html
<html>
<body>
This inspection reports the redundant spread operator for arrayOf call
</body>
</html>
```

# Inspection

✅ Add XxxInspection.kt idea/src/org/jetbrains/kotlin/idea/inspections/
✅ Add localInspection tag to idea/src/META-INF/plugin.xml
✅ Add Inspection descritption idea/resources/inspectionDescriptions/Xxx.html
* Add test data idea/testData/inspectionsLocal/xxx

# Add test data

* create folder idea/testData/inspectionsLocal/removeRedundantSpreadOperator
* Add .inspection file
* create test cases (a.kt -> a.kt.after)
* idea/testData/inspection is old ways to test inspection

