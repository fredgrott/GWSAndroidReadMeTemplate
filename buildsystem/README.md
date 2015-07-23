# Build System

To use the advance techniques of dagger, OOP and Compound Patterns, kotlin, RX, etc
one has to go beyond the simplistic build.gradle templates that the Android Studio
IDE uses.  I adapt a somewhat modular approach while at the same time future proofing
my gradle coding blocks so that it is updatable to the future Google Android Gradle
Plugin versions.

This document is intended to document all the gradle coding and other techniques so that this build system can be maintained with very limited effort. You are more than welcomed to borrow and steal my techniques and gradle code.

Why go through all the extra settings to debug log the libraries we use and use
enhanced logging libraries in our test code?  Because the way Google has implemented testing on the Cloud is by sending the test apk and the optional tested apk to the
test server rather than a CI server approach. Hence, our best practices approach is
to ensure we get the maximum amount of test feedback.

# TOC

* [Root Build File](#Root Build File)
* [ProGuard Files](#ProGuard Files)
* [Lombok Config Files](#Lombok Config Files)
* [Application Module Non Product Flavors Build File](#Application Module Non Product Flavors Build File)
* [Library Module Non Product Flavors Build File](#Library Module Non Product Flavors Build File)

##Root Build File

We start out with the root build file in that we do not want this long file of all the dependencies we use as ext variables, etc. We instead want one fake plugin gradle build file listing all our possible ext variables that gets loaded into the root build file.

And we block off distinct sections of the ext block so that its easy to update a portion of said ext block.

The trade-off we make is that by using a variable for things like the Google Android
Gradle plugin entry in the root build file is that we turn off IDE notification of an update to that plugin. This instead forces the developer to think about whether the
update of that plugin is required for the project in terms of the gradle code used and
prevent updating the plugin based upon impulse.

we also group together the direct dependencies and indirect of the plugin, if there are any, as sometimes the versions have to match the version of the gradle plugin.

Thus we have:

rootext.gradle

```gradle
ext{

  //Configure stuff
  ourReportsDir = 'build/reports'
  //Android
  //def globalConfiguration = rootProject.extensions.getByName("ext")
  //than globalConfiguration.getAt("androidCompileSdkVersion")
  androidCompileSdkVersion = 22
  androidBuildToolsVersion = "22.0.1"
  //Android.defaultConfig
  androidMinSdkVersion = 15
  androidTargetSdkVersion = 22





  //Our Gradle plugins
  androidGradlePlugin 'com.android.tools.build:gradle:1.3.0-beta4'
  hugoGradlePlugin 'com.jakewharton.hugo:hugo-plugin:1.2.1'
  androidAPTGradlePlugin 'com.neenbedankt.gradle.plugins:android-apt:1.5.1'

  spoonGradlePlugin 'com.stanfy.sppon:spoon-gradle-plugin:1.0.3'
  spoonClient 'com.squareup.spoon:spoon-client:1.1.9'

  intellijKotlinGradlePlugin 'org.jetbrains.kotlin:kotlin-gradle-plugin:0.12.613'
  kotlinSTDLibCompile 'org.jetbrains.kotlin:kotlin-stdlib:0.12.613'
  kotterKnife 'com.jakewharton:kotterknife:0.1.0-SNAPSHOT'

  gradleAdvanceBuildVersion 'org.moallemi.gradle.advanced-build-version:gradle-plugin:1.5.0'
  androidMavenGradlePlugin 'com.github.dcendents:android-maven-gradle-plugin:1.3'
  bintrayGradlePlugin 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
  godotGradleBuildStatsPlugin 'de.hannesstruss:godot:0.2'
  googleDataBindingGradlePlugin 'com.android.databinding:dataBinder:1.0-rc0'

  //rest of our possible dependencies

  //play services, proguard.txt is included in the aars thus no need for a separate
  //set of proguard files at module subfolder
  playServicesAll 'com.google.android.gms:play-services:7.5.0'
  playServicesAds 'com.google.android.gms:play-services-ads:7.5.0'
  playServicesAllWear 'com.google.android.gms:play-services-all-wear:7.5.0'
  playServicesAnalytics 'com.google.android.gms:play-services-analytics:7.5.0'
  playServicesAppIndexing 'com.google.android.gms:play-services-appindexing:7.5.0'
  playServicesAppInvite 'com.google.android.gms:play-services-appinvite:7.5.0'
  playServicesAppState 'com.google.android.gms:play-services-appstate:7.5.0'
  playServicesBase 'com.google.android.gms:play-services-base:7.5.0'
  playServicesCast 'com.google.android.gms:play-services-cast:7.5.0'
  playServicesDrive 'com.google.android.gms:play-services-drive:7.5.0'
  playServicesFitness 'com.google.android.gms:play-services-fitness:7.5.0'
  playServicesGames 'com.google.android.gms:play-services-games:7.5.0'
  playServicesGCM 'com.google.android.gms:play-services-gcm:7.5.0'
  playServicesIdentity 'com.google.android.gms:play-services-identity:7.5.0'
  playServicesLocation 'com.google.android.gms:play-services-location:7.5.0'
  playServicesMaps 'com.google.android.gms:play-services-maps:7.5.0'
  playServicesNearby 'com.google.android.gms:play-services-nearby:7.5.0'
  playServicesPanorama 'com.google.android.gms:play-services-panorama:7.5.0'
  playServicesPlus 'com.google.android.gms:play-services-plus:7.5.0'
  playServicesSafetynet 'com.google.android.gms:play-services-safetynet:7.5.0'
  playServicesWallet 'com.google.android.gms:play-services-wallet:7.5.0'
  playServicesWearable 'com.google.android.gms:play-services-wearable:7.5.0'


  //android support libs, same thing as the AndroidGradlePlugin in that
  //using the ext var turns off the IDE update notification but forces the
  //dev to update based on project needs rather than impulse
  appCompat 'com.android.support:appcompat-v7:22.2.1'
  cardView 'com.android.support:cardview-v7:22.2.1'
  design 'com.android.support:design:22.2.1'
  gridLayout 'com.android.support:gridlayout-v7:22.2.1'
  leanBack 'com.android.support:leanback-v17:22.2.1'
  mediaRouter 'com.android.support:mediarouter-v7:22.2.1'
  multidex 'com.android.support:multidex:22.2.1'
  multidexInstrumentation 'com.android.support:multidex-instrumentation:22.2.1'
  palette 'com.android.support:palette-v7:22.2.1'
  percent 'com.android.support:percent:22.2.1'
  recyclerView 'com.android.support:recyclerview-v7:22.2.1'
  supportAnnotations 'com.android.support:support-annotations:22.2.1'
  supportFour  'com.android.support:support-v4:22.2.1'
  supportThirteen 'com.android.support:support-v13:22.2.1'

  testEspresso 'com.android.support.test:testing-support-lib:0.1'
  testRunner 'com.android.support.test:runner:0.3'
  testRules 'com.android.support.test:rules:0.3'
  testUiAutomator 'com.android.support.test:uiautomator:uiautomator-v18:2.1.1'
  testExposedInstrumentationApiPublish 'com.android.support.test:exposed-instrumentation-api-publish:0.3'

  //should strive to have version number match the 2nd digit of major and
  //the minor and patch numbers of the support libs as it has that
  //dependency however Google doesn't always update these concurrently with
  //the support libs like they should and you may have to force a dependency to
  //right things
  espresso 'com.android.support.test.espresso:espresso-core:2.2'
  espressoContributions 'com.android.test.espresso:espresso-contrib:2.2'
  espressoIntents 'com.android.test.espresso:espresso-intents:2.2'
  espressoWeb 'com.android.test.espresso:espresso-web:2.2'

  //other google libs
  googleWearable 'com.google.android.support:wearable:1.2.0'
  googleWearWear 'com.google.android.wearable:wearable:1.0.0'

  //assertj libs for testing
  assertjAndroid 'com.squareup.assert:assertj-android:1.0.1-SNAPSHOT'
  assertjAndroidAppCompat 'com.squareup.assert:assertj-android-appcompat-v7:1.0.1-SNAPSHOT'
  assertAndroidCardView 'com.squareup.assertj:assertj-android-cardview-v7:1.0.1-SNAPSHOT'
  assertjAndroidGridLayout 'com.squareup.assertj:assertj-android-gridlayout-v7:1.0.1-SNAPSHOT'
  assertjAndroidMediaRouter 'com.squareup.assertj:assertj-android-mediarouter-v7:1.0.1-SNAPSHOT'
  assertjAndroidPalette 'com.squareup.assertj:assertj-android-palette-v7:1.0.1-SNAPSHOT'
  assertjAndroidPlayServices 'com.squareup.assertj:assertj-android-play-services:1.0.1-SNAPSHOT'
  assertjAndroidRecyclerView 'com.squareup.assertj:assertj-android-recyclerview-v7:1.0.1-SNAPSHOT'
  assertjAndroidSupportFour 'com.squareup.assertj:assertj-android-support-v4:1.0.1-SNAPSHOT'

  //other testing libs
  junit junit:junit:4.12'
  dexMaker 'com.google.dexmaker:dexmaker:1.0'
  dexMakerMockito 'com.google.dexmaker:dexmaker-mockito:1.0'
  mockito   'org.mockito:mockito-core:1.10.19'
  hamcrestLibrary 'org.hamcrest:hamcrest-library:1.3'
  hamcrestIntegration 'org.hamcrest:hamcrest-integration:1.3'
  hamcrestCore 'org.hamcrest:hamcrest-core:1.3'
  truthLib 'com.google.truth:truth:0.27'

  //debug libs
  madge 'com.jakewharton.madge:madge:1.1.2'
  scalpel 'com.jakewharton.scalpel:scalpel:1.1.2'
  leakCanary 'com.squareup.leakcanary:leakcanary-android:1.3'
  leakCanaryNOOP 'com.squareup.leakcanary:leakcanary-android-no-op:1.3'


  //dagger libs
  dagger 'com.google.dagger:dagger:2.0'
  daggerCompiler 'com.google.dagger:dagger-compiler:2.0'
  javaxAnnotations 'org.glassfish:javax.annotation:10.0-b28'

  //retrofit libs
  okHttp 'com.squareup.okhttp:okhttp:2.3.0'

  okMockWebService 'com.squareup.okhttp:mockwebserver:2.3.0'

  okUrlConnection 'com.squareup.okhttp:okhttp-urlconnection:2.3.0'
  okio 'com.squareup.okio:okio:1.3.0'
  retrofit 'com.squareup.retrofit:retrofit:1.9.0'

  retrofitMock 'com.squareup.retrofit:retrofit-mock:1.9.0'


  //log libs
  timber 'com.jakewharton.timber:timber:3.1.0'
  timberLint  'com.jakewharton.timber:timber-lint:3.1.0'

  //butterknife libs
  butterKnife  'com.jakewharon:butterknife:6.1.0'

  //rxandroid
  rxAndroid 'io.reactivex:rxandroid:0.24.0'
  rxKotlin 'io.reactivex:rxkotlin:0.21.0'

  //other dependencies
  renderers 'com.github.pedrovgs:renderers:1.5'
  colours 'com.github.matthewyork:ColoursLibrary:1.0.0'
  androidJoda 'net.dnlew:android-joda:2.7.2'
  picasso 'com.squreup.picasso:picasso:2.5.2'

  //codeqa
  findbugs 'com.google.code.findbugs:findbugs:3.0.0'
  findbugsAnnotations 'com.google.code.findbugs:annotations:3.0.0'
  checkstyle 'com.puppycrawl.tools:checkstyle:6.1'
  javancss 'org.codehaus.javancss:javancss:33.54'
  jdepend 'jdepend:jdepend:2.9.1'
  jdependAnt 'org.apache.ant:ant-jdepend:1.8.2'
  pmdJava 'net.sourceforge.pmd:pmd-java:5.2.1'

  jacocoAnt 'org.jacoco:org.jacoco.ant:0.7.4.201502262128'
  jacoocAgent 'org.jacoco:org.jacoco.agent:0.7.4.201502262128'


}
```

In our root build file:

```gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.

apply from: 'buildsystem/rootext.gradle'

def gradlePlugins = rootProject.ext

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath gradlePlugins.androidGradlePlugin
        classpath gradlePlugins.hugoGradlePlugin
        classpath gradlePlugins.androidAPTGradlePlugin
        classpath gradlePlugins.spoonGradlePlugin
        classpath gradlePlugins.intellijKotlinGradlePlugin
        classpath gradlePlugin.gradleAndvanceBuildVersion
        classpath gradlePlugins.androidMavenGradlePlugin
        classpath gradlePlugins.bintrayGradlePlugin
        classpath gradlePlugins.godotGradleBuildStatsPlugin
        classpath gradlePlugins.googleDataBindingGradlePlugin



        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
//The first part of our configuration to turn off preDexing
//when build is executed on a CI server, its also how
//we turn on codeqa report generation
project.ext.preDexLibs = !project.hasProperty('disablePreDex')


allprojects {
    repositories {
        jcenter()
        //oss snapshots
        maven {
            url 'http://oss.sonatype.org/content/repositories/snapshots'
        }

    }
    //sets the jvmArgs so that tests will not steal focus as we have
    //the forked process launched as jvm headless
    tasks.withType(JavaForkOptions) {
    // Forked processes like GradleWorkerMain for tests won't steal focus!
    jvmArgs '-Djava.awt.headless=true'
    }
}

apply plugin: 'de.hannesstruss.godot'


```

And so that takes care of the needed additions to the root build file.

##ProGuard Files

We need:



proguard-butterknife.txt

```gradle
-dontwarn butterknife.internal.**
-keep class butterknife.** { *; }
-keep class **$$ViewInjector { *; }

-keepclasseswithmembernames class * {
    @butterknife.InjectView <fields>;
}

-keepclasseswithmembernames class * {
    @butterknife.OnClick <methods>;
    @butterknife.OnEditorAction <methods>;
    @butterknife.OnItemClick <methods>;
    @butterknife.OnItemLongClick <methods>;
    @butterknife.OnLongClick <methods>;
}
```

proguard-daggertwo.txt  

```gradle
-keep class dagger.** { *; }
-keep interface dagger.** { *; }
# not sure this is needed, uncomment if needed
# -keepnames class com.ourcompany.**

-keep class **$$ModuleAdapter { *; }
-keep class **$$InjectAdapter { *; }
-keep class **$$StaticInjection

-keepclassmembers class * {
    @javax.inject.* <fields>;
    @javax.inject.* <init>(...);
    @dagger.* *;
}
-adaptclassstrings
-keepnames class dagger.Lazy
```

proguard.project.txt

```gradle
-keep class
com.companyname.projectname.BuildConfig[*;]
```


##Lombok Config Files

Yes, I use Lombok. We need two lombok.config files so at project root:

lombok.config

```gradle
config.stopBubbling = true
lombok.anyConstructor.suppressConstructorProperties = true
```
Than if we need any module specific settings its a lombok.config file at the module root

##Application Module Non Product Flavors Build File

This is an example of what the application module build file should look like with
our stuff and lots of notes and comments:

application module build.gradle file:

```gradle
apply plugin: 'com.android.application'
//test collating plugin
apply plugin: 'spoon'
//debug annotation plugin
apply plugin: 'hugo'
//build version and renaming apk plugin
apply plugin: 'org.moallemi.advanced-build-version'

//Our App Module Configuration Set-Up
def globalConfiguration = rootProject.extensions.getByName("ext")
//our build version and apk renaming Configuration, it requires a
//version.properties file at module root with this:
//AI_VERSION_CODE=0
buildTime = new Date().format("yyyy-MM-dd'T'HH:mm", TimeZone.getTimeZone("CST"))


advancedVersioning {
    nameOptions {
        versionMajor 1
        versionMinor 0
        versionPatch 0
        versionBuild 0
    }
    codeOptions {
        versionCodeType org.moallemi.gradle.internal.VersionCodeType.AUTO_INCREMENT_ONE_STEP
        dependsOnTasks 'release, debug'

    }
    outputOptions {
        renameOutput true
        // if product flavors exist than include $flavorName in this string
        nameFormat '$appName-$projectName-$buildType-$buildTime-$versionName-$versionCode'
    }
}

def appVersionName = advancedVersioning.versionName
def int appVersionCode = advancedVersioning.versionCode

// After looking at Google Android Gradle Plugin source and
// Gradle 2.4 source the sourceSets we have for a
// dagger app module project are:
//
// sourceSets.main.java
// sourceSets.debug.java
// sourceSets.release.java
// sourceSets.test.java   unit tests
// sourceSets.androidTest.java android unit tests
//
// Thus codeqa gradle plugins tasks should work and be
//     CheckstyleMain
//     CheckstyleTest
//     CheckstyleAndroidTest
//     CheckstyleDebug
//     CheckstyleRelease
//
//   ie we set up tasks for the sourceSets of interest
//
//   simplest test would be
//
//   sourceSets.all { sourceSet ->
//        def name = sourceSet.name
//        println '${name}'
//
//}
//
//  that should give the output of
//       main
//       androidTest
//       debug
//       release
//       test
//
// if only main and androidTest are setup than the
//output should be main and androidTest
//Well my test app does not print the names not even after the corrections
//so we have to fake it I guess as far as sourceSets.
//
// That means that the codeqa tasks have to use ant and we still do
// codeqa tasks by codeqaNameSourceSetName naming conventions.
//
//sourceSets {
//  
//   debug {
//        java {
//           srcDir 'src/java'
//        }
//        resources {
//            srcDir 'src/resources'
//        }
//   }
//
//   release {
//       java {
//         srcDir 'src/java'
//       }
//       resources {
//         srcDir 'src/resources'
//       }
//   }
//}



android {
    compileSdkVersion globalConfiguration.getAt("androidCompileSdkVersion")
    buildToolsVersion globalConfiguration.getAt("androidBuildToolsVersion")


    defaultConfig {
        //required so that the applicationId is decoupled from the
        //packageName node in manifest so refactoring works
        applicationId "com.grottworkshop.gwsindiainkapp"
        minSdkVersion globalConfiguration.getAt("androidMinSdkVersion")
        targetSdkVersion globalConfiguration.getAt("androidMinSdkVersion")
        versionCode appVersionCode
        versionName appVersionName

        //testApplicationId defaults to applicationId = '.test'
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        testHandleProfiling true
        testFunctionalTest true



    }

    aaptOptions {
        noCompress 'txt'
        ignoreAssetsPattern "!.svn:!.git:!.ds_store:!*.scc:.*:<dir>_*:!CVS:!thumbs.db:!picasa.ini:!*~"
    }

    //to enable logging of tests
    testOptions.unitTests.all {
        testLogging {
             events 'passed', 'skipped', 'failed', 'standardOut', 'standardError'
        }
    }
    testOptions{
      //xml reports
      resultsDir = "${globalConfiguration.ourReportsDir}"
      //html reports
      reportDir = "${globalConfiguration.ourReportsDir}"

    }

    signingConfigs {

        release {
            //props stored in gradle.properties at userhome .gradle subfolder
            //for example in my gradle.properties in my .gradle subfolder
            //FREDGROTT_RELEASE_STORE_FILE=C:\\Users\\fgrott\\fredgrott.keystore
            //easy huh?
            storeFile file(FREDGROTT_RELEASE_STORE_FILE)
            storePassword FREDGROTT_RELEASE_STORE_PASSWORD
            keyAlias FREDGROTT_RELEASE_KEY_ALIAS
            keyPassword FREDGROTT_RELEASE_KEY_PASSWORD
        }

    }
    /*
    Enables diamond operator, multi-catch, strings in switches for our
    minSdkVersion to compileSdkVersion range.
     */
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }

    packagingOptions{

    }

    //coverage reports will be generated with the command:
    // ./gradlew createDebugCoveragereport or
    // ./gradlew connectCheck
    //reports will be found in build/reports/coverage/BuildType
    jacoco{
        version = '0.7.4.201502262128'
    }



    buildTypes {
      //our buildConfigField defs
      def STRING = "String"
      def gitshaString = "GIT_SHA"
      def gitSha = 'git rev-parse --short HEAD'.execute([], project.rootDir).text.trim()
      def gitShaValue = "\"${gitSha}\""
      def buildTimeStampString = "Build_TIME_STAMP"
      def buildTimeStampValue = "\"${buildTime}\""

      def BOOLEAN = "boolean"
      def TRUE = "true"
      def FALSE = "false"
      def LOG_HTTP_REQUESTS = "LOG_HTTP_REQUESTS"
      def REPORT_CRASHES = "REPORT_CRASHES"
      def ENABLE_VIEW_SERVER = "ENABLE_VIEW_SERVER"
      def ENABLE_SHARING = "ENABLE_SHARING"
      def DEBUG_IMAGES = "DEBUG_IMAGES"

      debug {
        debuggable true
        minifyEnabled false

        testCoverageEnabled true
        applicationIdSuffix '.dev'
        versionNameSuffix '-dev-'

      }

        release {
          minifyEnabled true
          proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro',  'proguard-butterknife.txt', 'proguard-daggertwo.txt', 'proguard.project.txt'
           applicationIdSuffix '.release'
           versionNameSuffix '-release-'
           signingConfig signingConfigs.release

        }
    }
    //options are listed to the order in the LintOptions.java class file
    lintOptions {
        lintConfig file("my-custom-lint.xml")

        //if true, stop the gradle build if errors are found (true by default)
        abortOnError false
        // if true, emit full/absolute paths to files with errors (true by default)
        //absolutePaths true
        //if true, no source lines will be included in output (true by default)
        noLines false
        // set to true to turn off analysis progress reporting by lint
        quiet false
        // if true, check all issues, including those that are off by default
        checkAllWarnings true
        // if true, only report errors
        ignoreWarnings true
        // if true, treat all warnings as errors
        warningsAsErrors true
        // if true, show all locations for an error, do not truncate lists, etc.
        showAll true
        // and abort the build (controlled by abortOnError above) if fatal issues are // found (true by default)
        //checkReleaseBuilds true
        // explain issues (true by default) because xml and html reports
        // ignores this setting and explains issues anyway this setting is just
        // to explain issues per the stdout output
        //explainIssues true

        // turn off checking the given issue id's
        //disable 'TypographyFractions', 'TypographyQuotes'
        // turn on the given issue id's
        //enable 'RtlHardcoded', 'RtlCompat', 'RtlEnabled'
        // check *only* the given issue id's
        check 'NewApi', 'InlinedApi'



        // if true, generate a text report of issues (false by default)
        textReport true
        // location to write the output; can be a file or 'stdout'
        textOutput 'stdout'
        // if true, generate an XML report for use by for example Jenkins
        xmlReport true
        // file to write report to (if not specified, defaults to lint-results.xml)
        xmlOutput file("${globalConfiguration.ourReportsDir}/lint-report.xml")
        // if true, generate an HTML report (with issue explanations, sourcecode, etc)
        htmlReport true
        // optional path to report (default will be lint-results.html in the builddir)
        htmlOutput file("${globalConfiguration.ourReportsDir}/lint-report.html")



        // Set the severity of the given issues to fatal (which means they will be
        // checked during release builds (even if the lint target is not included)
        //fatal 'NewApi', 'InlineApi'
        // Set the severity of the given issues to error
        //error 'Wakelock', 'TextViewEdits'
        // Set the severity of the given issues to warning
        //warning 'ResourceAsColor'
        // Set the severity of the given issues to ignore (same as disabling the //check)
        //ignore 'TypographyQuotes'


    }


}

spoon {
    // devices = ['333236E9AE5800EC']
    debug = true
    //className = 'fully.qualified.TestCase'

    //methodName = 'testMyApp'

    baseOutputDir = file("${globalConfiguration.ourReportsDir}/spoon")
    // Enable setting test class/method-to-be-run from command line. E.g.:
    // $> ../gradlew spoonFreeDebugTest //-PspoonClassName=com.stanfy.spoon.example.test.MainActivityTest //-PspoonMethodName=testSetText
  if (project.hasProperty('spoonClassName')) {
    className = project.spoonClassName

    if (project.hasProperty('spoonMethodName')) {
      methodName = project.spoonMethodName
    }
  }

}

configurations{
  //apt, provided, compile, androidTestCompile, androidJacocoAnt, androidJacocoAgent
  //are already setup
  codeqa
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile globalConfiguration.appCompat
    //annotations
    compile globalConfiguration.supportAnnotations
    provided globalConfiguration.lombok
    apt globalConfiguration.lombok
    compile globalConfiguration.findbugsAnnotations


    //we can get a debug log of libraries by publishNonDefault true of
    //lib variants and using the gradle hugo plugin and marking all
    //lib methods @DebugLog and loading both variants as
    //deps in our app  module, if project lib as aar than use the release and
    //debug qualifiers
    compile project(path: ":gwsindiainklibrary", configuration:"release")
    debugCompile project(path:":gwsindiainklibrary", configuration:"debug")

    //dagger Set-Up
    //if we are using kotlin than we have:
    //kapt globalConfiguration.daggerCompiler
    //compile globalConfiguration.dagger
    //provided globalConfiguration.javaxAnnotations
    // and kotlin and the kotlin gradle plugin snapshot instead
    apt globalConfiguration.daggerCompiler
    compile globalConfiguration.dagger
    provided globalConfiguration.javaxAnnotations

    //butterknife
    compile globalConfiguration.butterKnife

    //our log libs
    compile globalConfiguration.timber
    compile globalConfiguration.timberLint

    //compile globalConfiguration.retrofit
    //compile globalConfiguration.okhttp
    //compile globalConfiguration.okUrlConnection
    //compile globalConfiguration.okio

    //debug stuff I use
    debugCompile globalConfiguration.madge
    debugCompile globalConfiguration.scalpel
    //debugCompile globalConfiguration.retrofitMock
    //debugCompile globalConfiguration.okMockWebService

    //jacoco
    androidJacocoAnt globalConfiguration.jacocoAnt
    androidJacocoAgent globalConfiguration.jacoocAgent


    //unit testing, under test
    testCompile globalConfiguration.junit
    testCompile globalConfiguration.mockito

    //uiautomator and espresso
    androidTestCompile globalConfiguration.runner
    androidTestCompile globalConfiguration.rules
    androidTestCompile globalConfiguration.uiautomator
    androidTestCompile globalConfiguration.hamcrestIntegration
    androidTestCompile globalConfiguration.espresso
    androidTestCompile globalConfiguration.espressoContributions
    //if testing for intents or web
    //androidTestCompile globalConfiguration.espressoIntents
    //androidTestCompile globalConfiguration.espressoWeb

    //google's truth framework for unittesting
    androidTestCompile globalConfiguration.truth

    //our assertj android stuff
    //gives depends on jar in AS console on builds so not using until
    //can get rid of error
    //androidTestCompile globalConfiguration.assertjAndroid
    //androidTestCompile globalConfiguration.assertjAndroidSupportFour
    //androidTestCompile globalConfiguration.assertjAndroidAppCompat

    //our codeqa deps
    codeqa globalConfiguration.checkstyle
    //updated classycel past 1.4.2 does not have ant tasks ie the one in maven central
    //thus we stick the lib in our config subfolder and load it
    //we did something similar for the lombok.jar that was needed just for the
    //ant task to delombok things for the javadoc task as we should not load it in
    //libs as it adds stuff that will generate errors.

    codeqa globalConfiguration.findbugs
    codeqa globalConfiguration.checkstyle
    codeqa globalConfiguration.jdepend
    codeqa globalConfiguration.pmdJava

}

//Our codeqa assumptions are:
// Anddroid Gradle Plugin still not fully compatible with
// Gradle source setts so CodeQANameSourceSetName tasks are
// not automatically triggered so we have to manually state
// what they are and the checkSourceSetName task that they
// should be triggered by
// and if than wrap them chekcing if we are PreDexing PreDexing false
// is the trigger codeqa condition


//so if we do the commandline as part of CI server execution or
//in our AS terminal:
//./gradlew clean assemble -PdisablePreDex
//than our codeqa reports will be generated

def releasePath = file("${rootDir}/archive/${project.name}")

def releaseTask = tasks.create(name: 'release') {
    group 'Build'
    description "Assembles and archives all Release builds"
}

//Our Lombok Stuff, warning the updated lombok jar matching the
//dependency loaded must be placed in the rootProject/config/lombok/lib subfolder of //the module and you should have your AS lombok plugin installed
def srcJava = 'src/main/java'
def srcDelomboked = 'build/src-delomboked/main/java'

task delombok {
    inputs.files file(srcJava)
    outputs.dir file(srcDelomboked)

    doLast {
        FileCollection collection = files(configurations.compile)
        FileCollection sumTree = collection + fileTree(dir: 'bin')

        ant.taskdef(name: 'delombok', classname: 'lombok.delombok.ant.DelombokTask', classpath: '${rootProject}/config/lombok/lib/lombok.jar')
        ant.delombok(from:srcJava, to:srcDelomboked, classpath: sumTree.asPath)
    }
}


//Our last part of our turn off predexing when executed on CI server Set-Up
//the commandline to turn off predexing when executing on CI server is
//    ./graddlew clean assemble -PdisablePreDex
//we also use rootProject.ext.preDexLibs == false to enable our
//codeqa report runs so that it runs on CI server but on IDE the
//default is not to run and than to run it we do the commandline of
// ./gradlew clean assemble -PdisablePreDex
applicationVariants.all { variant ->
  //for our javadoc tasks
  def name = variant.buildType.name

  //I do not skip the debug variants as I am placing output at
  //project module root/javadocs/compiled/variantBaseName
  task("javadoc${variant.name.capitalize()}", type: Javadoc) {

        description "Generates Javadoc for $variant.name."
        title = "Documentation for ${project.name} $android.defaultConfig.versionName b$android.defaultConfig.versionCode"
        destinationDir = new File("${project.getProjectDir()}/javadocs/compiled/", variant.baseName)
        setDependsOn(['delombok'])
        source = srcDelomboked
        ext.androidJar = "${android.sdkDirectory}/platforms/${android.compileSdkVersion}/android.jar"
        classpath = files(variant.javaCompile.classpath.files) + ext.androidJar
        options.memberLevel = org.gradle.external.javadoc.JavadocMemberLevel.PRIVATE
        options.links('http://docs.oracle.com/javase/7/docs/api/');
        options.linksOffline('http://d.android.come/reference' '[sdkDir]/docs/reference');
        exclude '**/BuildConfig.java'
        exclude '**/R.java'
        exclude '**/internal/**'
        failOnError false
    }

    task("bundleJavadoc${variant.name.capitalize()}", type: Jar) {
        description "Bundles Javadoc into zip for $variant.name."
        classifier = "javadoc"
        from tasks["javadoc${variant.name.capitalize()}"]
    }

  if (rootProject.ext.preDexLibs == true){
    if (variant.buildType.name == 'debug'){
        variant.dex.enableIncremental = true
        variant.dex.dexOptions.incremental = true
        variant.dex.dexOptions.preDexLibraries = true
    }
  }

  //espresso set up stuff
  if (variant.name.startsWith('debug')) {
        System.out.println("Not removing the SET_ANIMATION_SCALE permission for $variant.name")
        return
    }

    System.out.println("Removing the SET_ANIMATION_SCALE permission for $variant.name")
    variant.processManifest.doLast {
        copyAndReplaceText(manifestOutputFile, manifestOutputFile) {
            def replaced = it.replace('<uses-permission android:name="android.permission.SET_ANIMATION_SCALE"/>', '');
            if (replaced.contains('SET_ANIMATION_SCALE')) {
                // For security, imagine an extra space is added before closing tag, then the replace would fail - TODO use regex
                throw new RuntimeException("Don't ship with this permission! android.permission.SET_ANIMATION_SCALE")
            }
            replaced
        }
    }
    //archive
    if (variant.buildType.name == 'release') {
         def build = variant.name.capitalize()

         def releaseBuildTask = tasks.create(name: "release${build}", type: Zip) {
             group 'Build'
             description "Assembles and archives apk and its proguard mapping for the $build build"
             destinationDir releasePath
             baseName variant.packageName
             if (!variant.buildType.packageNameSuffix) {
                 appendix variant.buildType.name
             }
             if (variant.versionName) {
                 version "${variant.versionName}_${variant.versionCode}"
             } else {
                 version "$variant.versionCode"
             }
             def archiveBaseName = archiveName.replaceFirst(/\.${extension}$/, '')
             from(variant.outputFile.path) {
                 rename '.*', "${archiveBaseName}.apk"
             }
             if (variant.buildType.runProguard) {
                 from(variant.processResources.proguardOutputFile.parent) {
                     include 'mapping.txt'
                     rename '(.*)', "${archiveBaseName}-proguard_\$1"
                 }
             }
         }
         releaseBuildTask.dependsOn variant.assemble

         variant.productFlavors.each { flavor ->
             def flavorName = flavor.name.capitalize()
             def releaseFlavorTaskName = "release${flavorName}"
             def releaseFlavorTask
             if (tasks.findByName(releaseFlavorTaskName)) {
                 releaseFlavorTask = tasks[releaseFlavorTaskName]
             } else {
                 releaseFlavorTask = tasks.create(name: releaseFlavorTaskName) {
                     group 'Build'
                     description "Assembles and archives all Release builds for flavor $flavorName"
                 }
                 releaseTask.dependsOn releaseFlavorTask
             }
             releaseFlavorTask.dependsOn releaseBuildTask
         }
     }


}

//dep resolution strategies, sometimes we have to force things if Google or others
//did not update a library correctly with one of the dependencies so
//we have a block where we handle such things
configurations.all {
    // Currently espresso is dependent on support-annotations:22.2.0
    //it assumes that we do have a dep entry for support-annotations 22.2.1
    resolutionStrategy.force 'com.android.support:support-annotations:22.2.1'
}





//espresso set up stuff
task grantAnimationPermission(type: Exec, dependsOn: 'installDevDebug') {
  commandLine "adb shell pm grant $android.defaultConfig.applicationId android.permission.SET_ANIMATION_SCALE".split(' ')
}

tasks.whenTaskAdded { task ->
    if (task.name.startsWith('connectedAndroidTest')) {
        task.dependsOn grantAnimationPermission
    }
}

def copyAndReplaceText(source, dest, Closure replaceText) {
    dest.write(replaceText(source.text))
}



```

##Library Module Non Product Flavors Build File

It's similar to the Application Module with just a few changes:

```gradle

```
