# Buildsystem

It is easier to keep powerful build scripts from breaking if they are modular.
Obviously, there will be two sets of gradle build files to use as plugins. The ones where we  have no product flavors and the ones with product flavors.

# TOC

* [Gradle Plugins](#Gradle Plugins)
* [Android Configuration](#Android Configuration)
* [Dependencies](#Dependencies)
* [Dex Incremental](#Dex Incremental)
* [App Special Gradle Stuff](#App Special Gradle Stuff)
* [Test Special Gradle Stuff](#Test Special Gradle Stuff)
* [Library Special Gradle Stuff](#Library Special Gradle Stuff)
* [Lombok](#Lombok)
* [ApplicationVariantsAll](#ApplicationVariantsAll)

##Gradle Plugins

The full set of plugins to load:

```gradle
dependencies {
        classpath 'com.android.tools.build:gradle:1.3.0-beta4'
        classpath 'com.jakewharton.hugo:hugo-plugin:1.2.1'
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.5.1'
        classpath 'com.stanfy.spoon:spoon-gradle-plugin:1.0.3'
        classpath 'org.jetbrains.kotlin:kotlin-gradle-plugin:0.12.613'
        classpath 'org.moallemi.gradle.advanced-build-version:gradle-plugin:1.5.0'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
        classpath 'de.hannesstruss:godot:0.2'
        classpath "com.android.databinding:dataBinder:1.0-rc0"
}

```

I can load variables representing the classpath strings in a fake-proxy plugin that
gets loaded in the root build script as part of an ext gradle code block:

```gradle
ext{
     androidGoogleGradlePlugin 'com.android.tools.build:gradle:1.3.0-beta4'
     jakesHugoGradlePlugin 'com.jakewharton.hugo:hugo-plugin:1.2.1'
     androidAPTGradlePlugin 'com.neenbedankt.gradle.plugins:android-apt:1.5.1'

     spoonGradlePlugin 'com.stanfy.spoon:spoon-gradle-plugin:1.0.3'
     spoonClientAndroidTestCompile 'com.squareup.spoon:spoon-client:1.1.9'

     //versions must be the same for plugin and STDlib
     intellijKotlinGradlePlugin 'org.jetbrains.kotlin:kotlin-gradle-plugin:0.12.613'
     kotlinSTDLibCompile 'org.jetbrains.kotlin:kotlin-stdlib:0.12.613'
     kotterKnifeCompile ''


     gradleAdvanceBuildVersion 'org.moallemi.gradle.advanced-build-version:gradle-plugin:1.5.0'
     androidMavenGradlePlugin 'com.github.dcendents:android-maven-gradle-plugin:1.3'
     bintrayGradlePlugin 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
     godotGradleBuildStatsPlugin 'de.hannesstruss:godot:0.2'
     googleDataBindingGradlePlugin 'com.android.databinding:dataBinder:1.0-rc0'

}
```
Notice that we also load the module deps for a plugin if it has any as far as the string variables are concerned.

So now in the root build script we have:

```gradle
apply from: 'buildsystem/gradleplugins.gradle'

```
Now in our root build script after all the build apply declarations:

```gradle
def gradlePlugins = rootProject.ext

```
and in the buildscript dependencies block:

```gradle
dependencies {
  classpath gradlePlugins.androidGoogleGradlePlugin
}
```

Also note that doing it this way turns off the IDE little notice if you updated
to a new gradle plugin version via a new IDE release or other means. It just means that if you get a build error using the IDE that you will check the gradleplugins.gradle and androidconfig.gradle files first to see if versions are out of whack.

##Android Configuration

With the androidconfiguration.gradle fake plugin gradle file we are just concerning ourselves with the first part of the android code block in the module build file. In the ext code block:

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
And in the module build file between the apply declarations and the android block:

```gradle
def globalConfiguration = rootProject.extensions.getByName("ext")


```

And in the android block of the module gradle build file:

```gradle
android{
  compileSdkVersion globalConfiguration.getAt("androidCompileSdkVersion")
  buildToolsVersion globalConfiguration.getAt("androidBuildToolsVersion")

  defaultConfig{

    minSdkVersion globalConfiguration.getAt("androidMinSdkVersion")
    targetSdkVersion globalConfiguration.getAt("androidTargetSdkVersion")

  }

}
```

##Dependencies

And now we have rest of the dependencies in a dependencies.gralde file as:

```gradle
ext{
  butterKnife  'com.jakewharon:butterknife:6.1.0'
  daggerTwoCompile  'com.google.dagger:dagger:2.0'
  daggerCompilerTwoApt  'com.google.dagger:dagger-compiler:2.0'

  renderersCompile  'com.github.pedrovgs:renderers:1.5'
  coloursLibraryCompile  'com.github.matthewyork:ColoursLibrary:1.0.0'



  dexMakerAndroidTestCompile  'com.go0gle.dexmaker:dexmaker:1.0'
  dexMakerMockitoAndroidTestCompile  'com.google.dexmaker:dexmaker-mockito:1.0'
  mockitoAndroidTestCompile  'org.mockito:mockito-core:1.10.19'


  espressoAndroidTestCompile  'com.android.support.test:espresso-core:2.0'
  espressoContribAndroidTestCompile  'com.android.support.test:espresso-contrib:2.0'
  testingSupportLibAndroidTestCompile  'com.android.support.test:testing-support-lib:0.2'

  madgeCompile  'com.jakewharton.madge:madge:1.1.2'

  androidJodaCompile 'net.dnlew:android-joda:2.7.2'


  jacksonCompile  ''

  javaxAnnotationCompile  'org.glassfish:javax.annotation:10.0-b28'
  findbugsAnnotationsCompile  'com.google.code.findbugs:annotations:3.0.0'




  leakCanaryDebug 'com.squareup.leakcanary:leakcanary-android:1.3'
  leakCanaryRelease 'com.squareup.leakcanary:leakcanary-android-no-op:1.3'

  okHttpCompile 'com.squareup.okhttp:okhttp:2.3.0'
  okMockWebServiceDebugCompile 'com.squareup.okhttp:mockwebserver:2.3.0'
  okUrlConnectionCompile 'com.squareup.okhttp:okhttp-urlconnection:2.3.0'
  okioCompile 'com.squareup.okio:okio:1.3.0'
  retrofitCompile 'com.squareup.retrofit:retrofit:1.9.0'
  retrofitMockDebugCompile 'com.squareup.retrofit:retrofit-mock:1.9.0'

  picassoCompile 'com.squreup.picasso:picasso:2.5.2'


  rxAndroidCompile 'io.reactivex:rxandroid:0.24.0'
  sqlBriteCompile 'com.squeareup.sqlbrite:sqlbrite:0.1.0'
  scapelDebugCompile 'com.jakewharton.scapel:scapel:1.1.2'



  timberCompile 'com.jakewharton.timber:timber:3.1.0'
  timberLintCompile  'com.jakewharton.timber:timber-lint:3.1.0'

  googlePlusPlayServicesCompile 'com.google.android.gms:play-services-plus:'
  goolgeAccountLoginPlayServicesCompile 'com.google.android.gms:play-services-identity:'
  googleActivityrecognitionPlayServicesCompile 'com.google.android.gms:play-services-location:'
  googleAppIndexingPlayServicesCompile 'com.google.android.gms:play-services-appindexing:'
  googleCastPlayServicesCompile 'com.google.android.gms:play-services-cast:'
  googleDrivePlayServicesCompile 'com.google.android.gms:play-services-drive:'
  googleFitPlayServicesCompile 'com.google.android.gms:play-services-fitness:'
  googleMapsPlayServicesCompile 'com.google.android:gms:play-services-maps:'
  googleMobileAdsPlayServicesCompile 'com.google.android.gms:play-services-ads:'
  googlePanoramaViewerPlayServicesCompile 'com.google.android.gsm:play-services-panorama:'
  googlePlayGameServicesPlayServicesCompile 'com.google.android.gms:play-services-games:'
  googleWalletPlayServicesCompile 'com.google.android.gms:play-services-wallet:'
  googleAndroidWearPlayServicesCompile 'com.google.android.gms:play-services-wearable:'
  googleSafetyNetPlayServicesCompile 'com.google.android.gms:play-services-safetynet:7.5.0'
  googleGCMPlayServicesCompile 'com.google.android.gms:play-services-gcm:7.5.0'
  googleNearby PlayServicesCompile 'com.google.android.gms:play-services-nearby:7.5.0'
  googleAllWearPlayServicesCompile 'com.google.android.gms:play-services-all-wear:7.5.0'
  googleAnalyticsPlayServicesCompile 'com.google.android.gms:play-services-analytics:7.5.0'
  googleAppInvitePlayServicesCompile 'com.google.android.gms:play-services-appinvite:7.5.0'
  googleAppStatePlayServicesCompile 'com.google.android.gms:play-ervices-appstate:7.5.0'
  googleBasePlayServicesCompile 'com.google.android.gms:play-services-base:7.5.0'



  lombokProvided 'org.projectlombok:lombok:1.12.6'




}
```
Than in the module build file at top:

```gradle
apply from: 'buildsystem/dependencies.gradle'
```

than right before the android code block:

```gradle
def myDependencies = rootProject.ext
```

than in the dependency block:

```gradle
dependencies{
  compile myDependencies.butterKnifeCompile
}
```

Nice and simple.

##Dex Incremental

dexincremental.gradle has:

```gradle
applicationVariants.all { variant ->
    if (variant.buildType.name == 'debug'){
        variant.dex.enableIncremental = true
        variant.dex.dexOptions.incremental = true
        variant.dex.dexOptions.preDexLibraries = true
    }
}
```

than in app and library modules your apply from declaration:

```gradle
apply from: 'buildsystem/dexincremental.gradle'

```

##App Special Gradle Stuff

###Compile Options

To enable diamond operator, multi-catch, and strings in switches

```gradle
compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }
```

###Setting Version and VersionName

After the Gradle Advance Build Version plugin is applied in the module build file:

```gradle
apply plugin: 'org.moallemi.advanced-build-version'

```
and than at the top of the moduel build file:

```gradle
advancedVersioning {
    nameOptions { }
    codeOptions { }
    outputOptions { }
}

def appVersionName = advancedVersioning.versionName
def appVersionCode = advancedVersioning.versionCode

```
and in defaultConfig you use those two defined variables.

my typical settings are:

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

because I use auto increment we need to add a version.properties file to the module
folder that has this:

```gradle
AI_VERSION_CODE=0
```

Just that simple.

##Test Special Gradle Stuff

###Espresso

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


###Logging

In the module build file in the android code block:

```gradle
testOptions.unitTests.all {
    testLogging {
      events 'passed', 'skipped', 'failed', 'standardOut', 'standardError'
    }
  }
```

##Library Special Gradle Stuff

###Compile Options

To enable diamond operator, multi-catch, and strings in switches

```gradle
compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }
```

###DebugLogs

To get debug logs one has to do two things, first one has to have a library where this setting is made in the android code block of the build file:

```gradle
publishNonDefault true
```

of course the hugo plugin has ot be enabled(loaded in root buidl file) and applied in the library module build file:

```gradle
apply 'hugo'
```

Third, one has to put the DebugLog annotation in all the library class methods:

```gradle
  @DebugLog
   someMethod {

   }
```

Fourth, load both library variants in your app in the dependencies block, such as:

```gradle
    compile project(path: ":gwsindiainklibrary", configuration:"release")
    debugCompile project(path:":gwsindiainklibrary", configuration:"debug")

```
If its already an aar and up somewhere than compile and debugCompile and make sure to specify the aar variant.

Note, that you have to label both parts the url path part and the configuration part.

###Renderscript

Updating my libraries to stop using the rs support library as we are now past api 11
but until the defaultConfig block will still contain the two lines to use
the v8 support lib for renderscript:

```gradle

```

###Setting Version and VersionName

After the Gradle Advance Build Version plugin is applied in the module build file:

```gradle
apply plugin: 'org.moallemi.advanced-build-version'

```
and than at the top of the moduel build file:

```gradle
advancedVersioning {
    nameOptions { }
    codeOptions { }
    outputOptions { }
}

def appVersionName = advancedVersioning.versionName
def appVersionCode = advancedVersioning.versionCode

```
and in defaultConfig you use those two defined variables.

my typical settings are:

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

because I use auto increment we need to add a version.properties file to the module
folder that has this:

```gradle
AI_VERSION_CODE=0
```

Just that simple.

#Lombok



##ApplicationVariantsAll

We can only probably call applicationVariants.all once. ie only have one code block so we cannot do a dexincemtnakl.gradle and do the same block in the app module to set
up tests so we do a dexincrementaltest.gradle and dexincrementallib.gradle files instead(oen for the app module one for the lib module):


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


applicationVariants.all { variant ->
    if (variant.buildType.name == 'debug'){
        variant.dex.enableIncremental = true
        variant.dex.dexOptions.incremental = true
        variant.dex.dexOptions.preDexLibraries = true
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

than the one for the lib module, dexincrementallib.gradle is somewhat smaller:


```gradle
libraryVariants.all{ variant ->
  if (variant.buildType.name == 'debug'){
      variant.dex.enableIncremental = true
      variant.dex.dexOptions.incremental = true
      variant.dex.dexOptions.preDexLibraries = true
    }
}

```

Than one gets apply from in the app module and one gets apply from in the library module.
