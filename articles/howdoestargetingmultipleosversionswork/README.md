# How Does Targeting Multiple OS Versions Work?

Unlike iOS devices on the Android OS Platform we have a multitude of devices with different Android OS versions.  Some of those OS versions introduce new features and or new UX design standards.  This article attempts to explain how it works.

# TOC

* [Android OS SDK Stubbing](#Android OS SDK Stubbing)
    * [Lazy Class and Method Calls](#Lazy Class and Method Calls)
* [Back Porting](#Back Porting)



##Android OS SDK Stubbing

On the Android OS Platform Google came up with some things so that we could target
multiple OS versions on different devices. The first part of is something that automatically occurs as part of the SDK and IDE tool-set. The maximum SDK OS version target is loaded into the IDE project as jar package containing classes that have method stubs.

That way we can refer to right class that exists at the minimum level OS version and have it still compile right for the device with the highest OS version.

###Lazy Class and Method Calls

Obviously, we cannot call the class and or method that does not exist on the lower OS version.  We use the trick of wrapping those calls in IF THEN statement so that they are only called on the OS version that they exist in. The compiler produces no error as we have already linked the project to the highest OS version using the SDK stubs.

Because it is never called in the lower OS version the compiler correctly compiles it
to never be called. The automatic part of the process of lazy calling class and method calls is that the IDE gives warnings about direct calls to those classes and methods based on the applicaiton manifest and buidl file minimum target SDk setting.

##Back Porting
