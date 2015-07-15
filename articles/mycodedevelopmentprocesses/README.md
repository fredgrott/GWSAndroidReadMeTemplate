# My Code Development Processes

Because not everyone can take a look over my shoulder and watch how I develop code
and applications an explanation of the development processes I use is in order. A
person I know who use to be in the Chicago Start-up industry thought that Japan's
economic miracles both during the 1950s and the 1970s was due to a management process
created by the Japanese. But he was wrong and the market somewhat demonstrated that to
him with some company failures. The actual miracle of the Japanese was a little movement in the Gestalt shift of how to control manufacturing variations.

It can be said that I am an operationalist in that there is a set of systems in the
world and if we gain knowledge of those systems and apply human psychology and apply
a theory of knowledge we can gain some idea of how those systems together will act and
proceed. My choices in application of development processes reflect my operationalist
viewpoint.

# Unit Testing, OOP Patterns and OOP Compound Patterns, and Debugging

Unit testing and oop patterns and oop compound patterns came about during the same
time period in software development evolution and thus they are loosely coupled
partners and it is helpful to explain how I use them together as I use them as pairs.
While unit testing came about as a way to verify business logic of application code
oop patterns and oop compound patterns resulted from the realization of the imperfections of unit testing.

While I use unit testing to verify business logic code, I do not let unit testing
dictate a departure from the mobile application development process of early-on
optimizing GUI code to use less memory and render the GUI faster.  At the same time,
I use oop patterns and oop compound patterns to separate out application concerns and
address the imperfections of unit testing.

I use debugging to complete my systems knowledge of how a mobile application executes
and operates on the android OS platform on the devices I am targeting. Just like oop
patterns and oop compound patterns it is a tool I use to address the imperfections of
unit testing. As I started out computer programming with assembly language I tend
to break down coding into small blocks and observe outputs as my debug process.

# Agile Processes

The Agile Processes come in many shapes and sizes as to what can be used in particular
situations and I choose and modify agiel processes to fit the development situation. While testing is not done completely concurrently I still do unit test business logic early in the development cycle.

Certain Agile Processes lend themselves to mobile application development such as
Functional and Behavior Driven Testing which I use to assist in verifying that
the Android native java application has the same look, feel, and behavior on every
Android OS Platform version and device that is targeted. But again, that only works
due to the underlying processes and tools warn me about using different new APIs
and thus I can than choose to bracket the new method call by an IF Build > Build.OS.Version in order to have the method only called on a specific android OS version.

As the Android native java application is built, a automated testing framework for
the application is built that covers both system and integration testing and regression testing.  The purpose of this framework is to verify changes to the code do not
break something that has already been implemented.

## OOP Patterns and Compound Patterns

The goal is decouple things so that we get areas of concerns that handle only what
has been assigned to their area. While interface and abstract classes get used to assist in separating concerns, OOP Patterns and Compound Patterns and dependency
injection tools such as dagger2 are also used.

# Iteration

I use code repos in the form of Git repositories to store snapshots and assist me in
producing daily and weekly milestone builds. Having to produce daily milestones assists me in breaking down a programming solution into small parts that have working code.

# Mocking

At times it is useful to mock coding objects for testing. This usually occurs when
the application has to access a 3rd party service through an API. This can be done
in several different ways and I have Mockito, Dager2, etc at my disposal to choose which way to implement the mocks.  Integration testing comes into play as the verification that the Mocks set up do in-fact mirror the actual access behavior to
3rd party apis, etc.

# Unit Testing

I use the Google Espresso Testing Kit to unit test android native java applications
along with such accessories as asertj for android. As I stated beofre in this docuemnt, the testing framework for the Android native java application under development is
implemented and built over time concurrently with the android native java application.
That assists in the maintenance of the application code as your start-up decides to
build new versions with new features.

# Unofficial OEM Bug Device Database

I maintain an informal OEM Bug Device Database that assists me in developing coding
workarounds for bugs. It is smaller than it used to be as Google has improved their
CTS testing process so that OEMs can find more implementation mistakes on the OEM-side
of the Android OS Platform equation.


# Conclusion

This is meant to give an overview of the processes I use in the development process to
develop your Android native java application. Specifically, this describes the coding processes I use while the design article covers both my design processes and the
analytical and systems-analysis processes used to determine or predict which application features may be the core feature-set per market reaction, etc.
