# BuildSystem

We have some tradeoffs in designing a module android gradle build-system and hopefully I have made the right choices.



# TOC

* [Gradle Plugins](#Gradle Plugins)
* [Dependencies](#Dependencies)
* [Android Config](#Android Config)
* [Module Root Config Block](#Module Root Config Block)
* [DexOptions](#DexOptions)
* [TestOptions](#TestOptions)
* [SnapShots](#SnapShots)
* [MyBintTray](#MyBinTray)
* [GIT SHA](#GTI SHA)
* [Espresso SetUp](#Espresso SetUp)
* [Jacoco Coverage](#Jacoco Coverage)
* [BuildConfigField](#BuildConfigField)
* [AaptOptions](#AaptOptions)

##Gradle Plugins

gradleplugindeps. gradle

```gradle
ext{
     androidGradlePlugin 'com.android.tools.build:gradle:1.3.0-beta4'
     hugoGradlePlugin 'com.jakewharton.hugo:hugo-plugin:1.2.1'
     androidAPTGradlePlugin 'com.neenbedankt.gradle.plugins:android-apt:1.5.1'

     spoonGradlePlugin 'com.stanfy.spoon:spoon-gradle-plugin:1.0.3'
     spoonClient 'com.squareup.spoon:spoon-client:1.1.9'

     //versions must be the same for plugin and STDlib
     intellijKotlinGradlePlugin 'org.jetbrains.kotlin:kotlin-gradle-plugin:0.12.613'
     kotlinSTDLibCompile 'org.jetbrains.kotlin:kotlin-stdlib:0.12.613'
     kotterKnife ''



     gradleAdvanceBuildVersion 'org.moallemi.gradle.advanced-build-version:gradle-plugin:1.5.0'
     androidMavenGradlePlugin 'com.github.dcendents:android-maven-gradle-plugin:1.3'
     bintrayGradlePlugin 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
     godotGradleBuildStatsPlugin 'de.hannesstruss:godot:0.2'
     googleDataBindingGradlePlugin 'com.android.databinding:dataBinder:1.0-rc0'

}

```


##Dependencies

Let's group dependencies into their own group gradle build files:

playservicesdeps.gradle

```gradle
ext{
  //all these are compile
  //proguard stuff is already taken care by the library itself
  //as there is a proguard.txt file with each lib  aar
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


}
```

Next up is our support libs, ie

androidsupportlibsdeps.gradle

```gradle
ext{
  //all these are compile deps
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

  espresso 'com.android.support.test.espresso:espresso-core:2.2'
  espressoContributions 'com.android.test.espresso:espresso-contrib:2.2'
  espressoIntents 'com.android.test.espresso:espresso-intents:2.2'
  espressoWeb 'com.android.test.espresso:espresso-web:2.2'
}
```
Now for other annotations


```gradle
ext{

  javaxAnnotations 'org.glassfish:javax.annotation:10.0-b28'
  findbugsAnnotations 'com.google.code.findbugs:annotations:3.0.0'

}

```

Now for other google libs

```gradle
ext{
  googleWearable 'com.google.android.support:wearable:1.2.0'
  googleWearWear 'com.google.android.wearable:wearable:1.0.0'
}
```

Now for other test libs

testlibsdeps.gradle

```gradle
ext{

  assertjAndroid 'com.squareup.assert:assertj-android:'
  assertjAndroidAppCompat 'com.squareup.assert:assertj-android-appcompat-v7:'
  assertAndroidCardView 'com.squareup.assertj:assertj-android-cardview-v7:'
  assertjAndroidGridLayout 'com.squareup.assertj:assertj-android-gridlayout-v7:'
  assertjAndroidMediaRouter 'com.squareup.assertj:assertj-android-mediarouter-v7:'
  assertjAndroidPalette 'com.squareup.assertj:assertj-android-palette-v7:'
  assertjAndroidPlayServices 'com.squareup.assertj:assertj-android-play-services:'
  assertjAndroidRecyclerView 'com.squareup.assertj:assertj-android-recyclerview-v7:'
  assertjAndroidSupportFour 'com.squareup.assertj:assertj-android-support-v4:'

  junit junit:junit:4.12'
  dexMaker 'com.google.dexmaker:dexmaker:1.0'
  dexMakerMockito 'com.google.dexmaker:dexmaker-mockito:1.0'
  mockito   'org.mockito:mockito-core:1.10.19'
  hamcrestLibrary 'org.hamcrest:hamcrest-library:1.3'
  hamcrestIntegration 'org.hamcrest:hamcrest-integration:1.3'
  hamcrestCore 'org.hamcrest:hamcrest-core:1.3'
  truthLib 'com.google.truth:truth:0.27'



  madge 'com.jakewharton.madge:madge:1.1.2'
  scalpel 'com.jakewharton.scalpel:scalpel:1.1.2'
}
```

Now dagger

daggerdeps.gradle

```gradle
ext{
  dagger 'com.google.dagger:dagger:2.0'
  daggerCompile 'com.google.dagger:dagger-compiler:2.0'

}
```

Now leakcanary

leakcanarydeps.gradle

```gradle
ext{
  leakCanary 'com.squareup.leakcanary:leakcanary-android:1.3'
  leakCanaryNOOP 'com.squareup.leakcanary:leakcanary-android-no-op:1.3'

}
```

Now for retrofit and okhttp

retrofitokhttpdeps.gradle

```gradle
ext{
  okHttp 'com.squareup.okhttp:okhttp:2.3.0'

  okMockWebService 'com.squareup.okhttp:mockwebserver:2.3.0'

  okUrlConnection 'com.squareup.okhttp:okhttp-urlconnection:2.3.0'
  okio 'com.squareup.okio:okio:1.3.0'
  retrofit 'com.squareup.retrofit:retrofit:1.9.0'

  retrofitMock 'com.squareup.retrofit:retrofit-mock:1.9.0'

}
```

Now for timber libs

timberdeps.gradle

```gradle
ext{
  timber 'com.jakewharton.timber:timber:3.1.0'
  timberLint  'com.jakewharton.timber:timber-lint:3.1.0'

}
```

Now for butterknife

butterknifedeps.gradle

```gradle
ext{
  butterKnife  'com.jakewharon:butterknife:6.1.0'

}
```

Now for renderers lib

renderersdeps.gradle

```gradle
ext{
  renderers 'com.github.pedrovgs:renderers:1.5'

}
```

Now for colours lib

coloursdeps.gradle

```gradle
ext{
  colours 'com.github.matthewyork:ColoursLibrary:1.0.0'

}
```

Now for androidjoda libs

androidjodadeps.gradle

```gradle
ext{
  androidJoda 'net.dnlew:android-joda:2.7.2'

}
```

Now for rxandroid deps

rxandroiddeps.gradle

```gradle
ext{
  rxAndroid 'io.reactivex:rxandroid:0.24.0'
  rxKotlin 'io.reactivex:rxkotlin:0.21.0'
}
```

Now for picasso

picassodeps.gradle

```gradle
ext{
  picasso 'com.squreup.picasso:picasso:2.5.2'

}
```

Now for Jacoco

jacocodeps.gradle

```gradle
ext{
  jacocoAgent 'org.jacoco:org.jacoco.agent:0.7.4.201502262128'
  jacocoAnt   'org.jacoco:org.jacoco.ant:0.7.4.201502262128'
}
```

Now for codeqa

codeqadeps.gradle

```gradle
ext{
  findbugs 'com.google.code.findbugs:findbugs:3.0.0'
  checkstyle 'com.puppycrawl.tools:checkstyle:6.1'
  javancss 'org.codehaus.javancss:javancss:33.54'
  jdepend 'jdepend:jdepend:2.9.1'
  jdependAnt 'org.apache.ant:ant-jdepend:1.8.2'
  pmdJava 'net.sourceforge.pmd:pmd-java:5.2.1'
}
```

Now for jackson deps

jacksondeps.gradle

```gradle
ext{
  jacksonAnnotations 'com.fasterxml.jackson.core:jackson-annotations:2.5.3'
  jacksonCore 'com.fasterxml.jackson.core:jackson-core:2.5.3'
  jacksonDatabind 'com.fasterxml.jackson.core:jackson-databind:2.5.3'
}
```

That should be it for my dependencies.

##Android Config

androidconfig.gradle

```gradle

```gradle
ext{
    //Android
    androidCompileSdkVersion = 22
    androidBuildToolsVersion = "22.0.1"
    //Android.defaultConfig
    androidMinSdkVersion = 15
    androidTargetSdkVersion = 22


}

```


##Module Root Config Block

modulerootconfigblocknonflavor.gradle

```gradle
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

```

##DexOptions

At root build:

project.ext.preDexLibs = !project.hasProperty('disablePreDex')


dexoptionsappmodulenonproductflavors.gradle

```gradle
applicationVariants.all { variant ->
  if (rootProject.ext.preDexLibs == true){
    if (variant.buildType.name == 'debug'){
        variant.dex.enableIncremental = true
        variant.dex.dexOptions.incremental = true
        variant.dex.dexOptions.preDexLibraries = true
    }
  }

}
```

Than we can disable to run CI server with:

```gradle
./gradlew clean assemble -PdisablePreDex
```

##TestOptions

###TestLogging

```gradle
testOptions.unitTests.all {
    testLogging {
      events 'passed', 'skipped', 'failed', 'standardOut', 'standardError'
    }
  }
```

##SnapShots

I use OSS sonatype snapshots, mainly assertj. So at the root build script
for all subprojects we set:

```gradle

allprojects {
    repositories {
        jcenter()
        maven {
            url 'http://oss.sonatype.org/content/repositories/snapshots'
        }
    }

}


```

##MyBinTray

At top of module build file:

```gradle
apply plugin: 'com.github.dcendents.android-maven'
apply plugin: "com.jfrog.bintray"

version = android.versionName

```

Than at bottom of build file:

```gradle
def siteUrl = ''
def gitUrl = ''
group = ''

install {
    repositories.mavenInstaller {
        // This generates POM.xml with proper parameters
        pom {
            project {
                packaging 'aar'

                // Add your description here
                name 'Android Update Checker'
                description = 'The project aims to provide a reusable instrument to check asynchronously if exists any newer released update of your Android app on the Store.'
                url siteUrl

                // Set your license
                licenses {
                    license {
                        name 'The Apache Software License, Version 2.0'
                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
                    }
                }
                developers {
                    developer {
                        id 'danielemaddaluno'
                        name 'Daniele Maddaluno'
                        email 'daniele.maddaluno@gmail.com'
                    }
                }
                scm {
                    connection gitUrl
                    developerConnection gitUrl
                    url siteUrl

                }
            }
        }
    }
}

task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
}

task javadoc(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}
artifacts {
    archives javadocJar
    archives sourcesJar
}

Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

// https://github.com/bintray/gradle-bintray-plugin
bintray {
    user = properties.getProperty("bintray.user")
    key = properties.getProperty("bintray.apikey")

    configurations = ['archives']
    pkg {
        repo = "maven"
        // it is the name that appears in bintray when logged
        name = "androidupdatechecker"
        websiteUrl = siteUrl
        vcsUrl = gitUrl
        licenses = ["Apache-2.0"]
        publish = true
        version {
          gpg {
              sign = false
              passphrase = properties.getProperty("bintray.gpg.password") //Optional. The passphrase for GPG signing'
          }
//            mavenCentralSync {
//                sync = true //Optional (true by default). Determines whether to sync the version to Maven Central.
//                user = properties.getProperty("bintray.oss.user") //OSS user token
//                password = properties.getProperty("bintray.oss.password") //OSS user password
//                close = '1' //Optional property. By default the staging repository is closed and artifacts are released to Maven Central. You can optionally turn this behavior off (by putting 0 as value) and release the version manually.
//            }
    }
  }
}

```
in local.properties:

```gradle
bintray.user=<your bintray username>
bintray.apikey=<your bintray api key>

bintray.gpg.password=<your gpg signing password>
bintray.oss.user=<your sonatype username>
bintray.oss.password=<your sonatype password>
```
##GIT SHA

At top before android block in module build file:

```gradle
def gitSha = 'git rev-parse --short HEAD'.execute([], project.rootDir).text.trim()
```

in the android block:

```gradle
buildConfigField "String", "GIT_SHA", "\"${gitSha}\""

```

If using Crashlytics in the onCreate of the app:

```java
Crashlytics.setString("git_sha", BuildConfig.GIT_SHA);
```

Than in a different directory from the git dev branch of project,
for example tempogit for example:

```java
git checkout git_sha_goes_here
```

Than you will be detached state and can see what is causing the crash.
Than you hot fix in the git dev branch.


##Espresso SetUp

Espresso needs some special stuff enabled so that it is flake free when executing tests.  First the manifest needs an entry that we will manipulate via gradle tasks:

```android
<?xml version="1.0" encoding="utf-8"?>
<manifest
  xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.novoda.espresso">

  <!-- For espresso testing purposes, this is removed in live builds, but not in dev builds -->
  <uses-permission android:name="android.permission.SET_ANIMATION_SCALE" />

  <!-- ... -->

</manifest>
```
Than we modify the main test class to:

```java
public class MyInstrumentationTestCase extends ActivityInstrumentationTestCase2<MyActivity> {

    private SystemAnimations systemAnimations;

    public MyInstrumentationTestCase() {
        super(MyActivity.class);
    }

    @Override
    protected void setUp() throws Exception {
        super.setUp();
        systemAnimations = new SystemAnimations(getInstrumentation().getContext());
        systemAnimations.disableAll();
        getActivity();
    }

    @Override
    protected void tearDown() throws Exception {
        super.tearDown();
        systemAnimations.enableAll();
    }
}

class SystemAnimations {

    private static final String ANIMATION_PERMISSION = "android.permission.SET_ANIMATION_SCALE";
    private static final float DISABLED = 0.0f;
    private static final float DEFAULT = 1.0f;

    private final Context context;

    SystemAnimations(Context context) {
        this.context = context;
    }

    void disableAll() {
        int permStatus = context.checkCallingOrSelfPermission(ANIMATION_PERMISSION);
        if (permStatus == PackageManager.PERMISSION_GRANTED) {
            setSystemAnimationsScale(DISABLED);
        }
    }

    void enableAll() {
        int permStatus = context.checkCallingOrSelfPermission(ANIMATION_PERMISSION);
        if (permStatus == PackageManager.PERMISSION_GRANTED) {
            setSystemAnimationsScale(DEFAULT);
        }
    }

    private void setSystemAnimationsScale(float animationScale) {
        try {
            Class<?> windowManagerStubClazz = Class.forName("android.view.IWindowManager$Stub");
            Method asInterface = windowManagerStubClazz.getDeclaredMethod("asInterface", IBinder.class);
            Class<?> serviceManagerClazz = Class.forName("android.os.ServiceManager");
            Method getService = serviceManagerClazz.getDeclaredMethod("getService", String.class);
            Class<?> windowManagerClazz = Class.forName("android.view.IWindowManager");
            Method setAnimationScales = windowManagerClazz.getDeclaredMethod("setAnimationScales", float[].class);
            Method getAnimationScales = windowManagerClazz.getDeclaredMethod("getAnimationScales");

            IBinder windowManagerBinder = (IBinder) getService.invoke(null, "window");
            Object windowManagerObj = asInterface.invoke(null, windowManagerBinder);
            float[] currentScales = (float[]) getAnimationScales.invoke(windowManagerObj);
            for (int i = 0; i < currentScales.length; i++) {
                currentScales[i] = animationScale;
            }
            setAnimationScales.invoke(windowManagerObj, new Object[]{currentScales});
        } catch (Exception e) {
            Log.e("SystemAnimations", "Could not change animation scale to " + animationScale + " :'(");
        }
    }
}
```

Now the gradle tasks, this the no productFlavor version..modify if you have productFlavors:


espressstuffappmodule.gradle

```gradle
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

android.applicationVariants.all { variant ->
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
}
```

Yes, it is a pain the ass.

##Jacoco Coverage

In app module build file in the android block:

```gradle
debug {
            testCoverageEnabled true
        }
```

than from terminal:

```gradle
gradlew createDebugCoverageReport
```

the report will be in build/reports/coverage/buildType

##BuildConfigField

So let's standardize the fields we set and probably we should put the def blaock at the top of the buildTypes block:

```gradle
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

```

Than the in debug buildType block we have:

```gradle
buildConfigField STRING, gitshaString, gitShaValue
buildConfigField STRING, buildTimeStampString, buildTimeStampValue
buildConfigField BOOLEAN, LOG_HTTP_REQUESTS, TRUE
buildConfigField BOOLEAN, REPORT_CRASHES, FALSE
buildConfigField BOOLEAN, ENABLE_VIEW_SERVER, TRUE
buildConfigField BOOLEAN, ENABLE_SHARING, TRUE
buildConfigField BOOLEAN, DEBUG_IMAGES, TRUE


```

Than in  the release buildType block we have:

```gradle
buildConfigField STRING, gitshaString, gitShaValue
buildConfigField STRING, buildTimeStampString, buildTimeStampValue
buildConfigField BOOLEAN, LOG_HTTP_REQUESTS, FALSE
buildConfigField BOOLEAN, REPORT_CRASHES, TRUE
buildConfigField BOOLEAN, ENABLE_VIEW_SERVER, FALSE
buildConfigField BOOLEAN, ENABLE_SHARING, FALSE
buildConfigField BOOLEAN, DEBUG_IMAGES, FALSE

```

##AaptOption

```gradle
aaptOptions {
        noCompress 'txt'
        ignoreAssetsPattern "!.svn:!.git:!.ds_store:!*.scc:.*:<dir>_*:!CVS:!thumbs.db:!picasa.ini:!*~"
    }

```

##SigningConfigs

```gradle
signingConfigs {

        release {
            //props stored in gradle.properties at userhome .gradle subfolder

            storeFile file(FREDGROTT_RELEASE_STORE_FILE)
            storePassword FREDGROTT_RELEASE_STORE_PASSWORD
            keyAlias FREDGROTT_RELEASE_KEY_ALIAS
            keyPassword FREDGROTT_RELEASE_KEY_PASSWORD
        }

    }

```

The gradle.properties file at userhome .gradle:


```gradle


```

##CompileOptions

```gradle
/*
    Enables diamond operator, multi-catch, strings in switches for our
    minSdkVersion to compileSdkVersion range.
     */
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }

```

##BuildTypes Debug

Our settings:

```gradle
debuggable true
minifyEnabled false
// dagger jacaco conflict issue is resolved, dx change after buildtoolsversion 21
testCoverageEnabled true

// here we go, versionNameSuffix with build date!
applicationIdSuffix '.dev'
versionNameSuffix '-dev-'

```

##BuildTypes Release

Our settings:

```gradle
minifyEnabled true
proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro',  'proguard-butterknife.txt', 'proguard-daggertwo.txt', 'proguard.project.txt'
applicationIdSuffix '.release'
versionNameSuffix '-release-'
signingConfig signingConfigs.release
```

##LintOptions

```gradle
lintOptions {
        lintConfig file("my-custom-lint.xml")

        // set to true to turn off analysis progress reporting by lint
        quiet false
        // if true, stop the gradle build if errors are found
        abortOnError false
        // if true, only report errors
        ignoreWarnings true
        // if true, emit full/absolute paths to files with errors (true by default)
        //absolutePaths true
        // if true, check all issues, including those that are off by default
        checkAllWarnings true
        // if true, treat all warnings as errors
        warningsAsErrors true
        // turn off checking the given issue id's
        //disable 'TypographyFractions', 'TypographyQuotes'
        // turn on the given issue id's
        //enable 'RtlHardcoded', 'RtlCompat', 'RtlEnabled'
        // check *only* the given issue id's
        check 'NewApi', 'InlinedApi'
        // if true, don't include source code lines in the error output
        noLines false
        // if true, show all locations for an error, do not truncate lists, etc.
        showAll true

        // if true, generate a text report of issues (false by default)
        textReport true
        // location to write the output; can be a file or 'stdout'
        textOutput 'stdout'
        // if true, generate an XML report for use by for example Jenkins
        xmlReport true
        // file to write report to (if not specified, defaults to lint-results.xml)
        xmlOutput file("${rootProject.ext.lintReportsDir}/lint-report.xml")
        // if true, generate an HTML report (with issue explanations, sourcecode, etc)
        htmlReport true
        // optional path to report (default will be lint-results.html in the builddir)
        htmlOutput file("${rootProject.ext.lintReportsDir}/lint-report.html")

        // set to true to have all release builds run lint on issues with severity=fatal
        // and abort the build (controlled by abortOnError above) if fatal issues are found
        checkReleaseBuilds true
        // Set the severity of the given issues to fatal (which means they will be
        // checked during release builds (even if the lint target is not included)
        //fatal 'NewApi', 'InlineApi'
        // Set the severity of the given issues to error
        //error 'Wakelock', 'TextViewEdits'
        // Set the severity of the given issues to warning
        //warning 'ResourceAsColor'
        // Set the severity of the given issues to ignore (same as disabling the check)
        //ignore 'TypographyQuotes'


    }

```
##Spoon Settings

```gradle
spoon {
    // devices = ['333236E9AE5800EC']
    debug = true
    //className = 'fully.qualified.TestCase'

    //methodName = 'testMyApp'

    baseOutputDir = file("$buildDir/spoon")
    // Enable setting test class/method-to-be-run from command line. E.g.:
  // $> ../gradlew spoonFreeDebugTest -PspoonClassName=com.stanfy.spoon.example.test.MainActivityTest -PspoonMethodName=testSetText
  if (project.hasProperty('spoonClassName')) {
    className = project.spoonClassName

    if (project.hasProperty('spoonMethodName')) {
      methodName = project.spoonMethodName
    }
  }

}

```

##Copy Apk To Artifacts

We want to have the apk of the release build copied to the artifacts subfolder
as we provide it to stakeholders as proof of milestone.



```gradle
def releasePath = file("${rootDir}/archive/${project.name}")

def releaseTask = tasks.create(name: 'release') {
    group 'Build'
    description "Assembles and archives all Release builds"
}

android.applicationVariants.all { variant ->
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
```









##DefaultConfig For testing

testApplicationId defaults to applicationId + ".test"  should never need to
set it to something else. testPackageName is left never declared in the sample
app under test in Google's Espresso examples. Thus, should be okay not to
declare unless we have something real custom.


```gradle

testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
testHandleProfiling true
testFunctionalTest true
```

It is the same for library projects as well.

##Codeqa

The general idea is I do not tie these to the assembleDebug unless the CI
server is the one doing the build as I can always instruct AndroiidStudio to save a command shortcut that assembleDebug and executes the codeqa stuff. The Google Android plugin still does not due a standard java plugin integration, the gradle codeqa plugins run but its not tied into the Google Android report merging, etc.

To merge the reports I have to execute ant tasks and do an xslt style merge.





javancsscodeqanonproductflavor.gradle

```gradle
// Configure JavaNCSS

configurations {
  javancss
}

dependencies {
  // v32.53
  javancss 'org.codehaus.javancss:javancss:32.53'
}

task javancss() {
  description = 'execute JavaNCSS tool on project source code'
  group = 'Code Quality'

  def ignoreFailures = true

  // create output folders
  def javancssReportDir = file("$project.buildDir/reports/javancss")
  javancssReportDir.mkdirs()

  // exclude auto-generated code and 3rd party libs
  def exclude = ['**/build/generated/**', '**/build/source/**',
                 '**/com/android/**', '**/com/google/**', '**/android/support/**']

  ant {
    taskdef name: 'javancss',
            classname: 'javancss.JavancssAntTask',
            classpath: configurations.javancss.asPath

    javancss srcdir: 'src',
            packageMetrics: "yes",
            excludes: exclude,
            classMetrics: "yes",
            functionMetrics: "yes",
            abortOnFail: !ignoreFailures,
            generateReport: true,
            outputfile: "$javancssReportDir/javancss.xml",
            format: 'xml'
  }
}
```

checkstylenonproductflavors.gradle

```gradle
apply plugin: 'checkstyle'

configurations {
  checkstyle
}

dependencies{
  checkstyle ''
}

task checkstyle(type: Checkstyle){
  source 'src'
  include '**/*.java'
  exclude '**/gen/**'
  // empty classpath
  classpath = files()
  //Do not fail build
}
```

pmdnonproductflavors.gradle

```gradle
apply plugin: 'pmd'

configurations{
  pmd
}

dependencies{
  pmd ''
}

task pmd(type: Pmd){
  source 'src'
  include '**/*.java'
  exclude '**/gen/**'
  ruleSets = ["android"]
}
```

findbugsnonproductflavors.gradle

```gradle
apply plugin: 'findbugs'

configurations{
  findbugs
}

dependencies{
  findbugs ''
}

task findbugs(type: FindBugs){
  ignoreFailures = true
  classes = fileTree('build/intermediates/classes/debug')
  source = fileTree('src/main/java')
  classpath = files()
  effort = 'max'
}

tasks.withType(findBugs) {
  excludeFilter = file("config/findbugs/excludeFilter.xml")
}
```
