# Java 9+ Support

With the rise of [Project Jigsaw](https://www.baeldung.com/project-jigsaw-java-modularity), Java has become much more encapsulated. This includes locking away internal code behind the <abbr title="Java Platform Module System">JPMS</abbr>, thankfully, the compiler contains a few options to allow you to access internal parts of the code.

## New TreeMaker Location

TreeMaker no longer exists as part of `tools.jar` as the file is no longer shipped with the JDK, but instead is part of the `jdk.compiler` module (**NOT** `java.compiler`!).

## Compiler Options

`add-exports` is a new compiler option that allows you to export a module and it's members, this makes it available to be used in your code.

The settings you'll want to use are:<br>
`--add-exports jdk.compiler/com.sun=ALL-UNNAMED`

This is due to our project requiring `com.sun.tools` and `com.sun.source`.