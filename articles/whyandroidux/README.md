# Why Android UX

This article explains the difference between web, mobile web, iOS, and Android UX and why we do not use one for the other.

# TOC

* [Responsive Web](#Responsive Web)
* [Mobile Web](#Mobile Web)
* [iOS UX](#iOS UX)
* [Android UX](#Android UX)


##Responsive Web

Responsive web came about as the desktop PCs evolved to laptop computers and feature phones. It takes its name from using one massive stylesheet and media queries to
create a html layout that RESPONDS to the browser window size by resizing columns and rows.

Its advantage is that you can target device forms that only have one or two different
sizes among those device forms.  Which was fine when everyone was still being trained by experiencing webapps and the featur-phone appp marketplace was stillborn.

With over 2 billion iphones and Android devices it is no longer the case where users
are expecting web applications and want the web app UX experience.

##Mobile Web

Mobile web came about as the iPhone was introduced in 2007 as tools to do responsive
web started appearing in the form of css frameworks. Because there was only few sizes of iphone form factor and other devices such as blackberry, the tools took on a
responsive web bias.

Again, just like responsive web, it only works if the users are expecting a web application UX and if the targeted devices are only in a few sizes per form factor. Even than , you are forced either to have a phone bias,tablet bias, or desktop bias just in the choices of the number ofr columns the content adjusts to based on the size of the screen.

Big names like Facebook and Google have come up with a Form-Facotr-Device-Class-Adaptive approach for their mobile web properties in which users do expect the UX of a web application.

The disadvantages are in the form of such things as Apple-like double pixel displays in that the browser, both Safari UAs and Chrome UAs. will report a device width without
the double pixels.

##iOS UX

As far as hardware most iOS hardware is characterized by having one hardware system back button at the bottom of the device. In an iOS application the back button of the application is placed at the top of the application is labeled:


while the navigation of the application is placed in the form of tabs at the bottom of the application. The search bar in an iOS application is always at the top of the application. The action bar in an iOS application is place on top of the application on the right-hand side.

And due to the free default iOS applications that enforce this UX everyone supports this UX style on their iOS applications, even Google. And remember per the from factor of phone, tablet, watch there are only a few screen sizes.

And of course the Graphical UX design standards of iOS are now retreating away from
mirroring physical representations of buttons, etc. That was completed by Apple as they viewed representing physical buttons, etc as increasing the cognitive work of the iOS device user and wanted to reduce the cognitive work an iOS device user had to do in order to use an application.


##Android UX

Android UX is somewhat complicated by having a large variation of screen sizes per form factor. As such we get very terrible UX results when using an adjustable mobile web
layout. The same cannot be said for the native tools we have to adjust graphical asset
sizes to both the screen size and density of the screen. And that is true on iOS as well in that native applications because of the Apple native tools look better.

The Android phone and tablets as of Android Platform OS 4.0 have 3 hardware buttons; back, home, and recents. Thus application navigation, such as tabs, is place on top of the application on the actionbar.

Whereas the Android watch devices do not have those hardware buttons and Android TV devices also do not have those buttons. While we do a separate watch application without those application navigation features, the Android OS itself does adjust in the Android TV form-factor to supply those navigational features in virtual manner as often in the Android-TV case its the same application code base as the application on the phone and tablet form-factors.
