# Getting Started with TreeMaker

## What is TreeMaker?

[`com.sun.tools.javac.tree.TreeMaker`](https://www.javadoc.io/doc/org.kohsuke.sorcerer/sorcerer-javac/latest/com/sun/tools/javac/tree/TreeMaker.html) is an internal class used by the Java compiler. It allows the creation of [`com.sun.tools.javac.tree.JCTree`](https://www.javadoc.io/static/org.kohsuke.sorcerer/sorcerer-javac/0.11/com/sun/tools/javac/tree/JCTree.html) instances. JCTree is a class representing a node of the Java compiler's <abbr title="Abstract Syntax Tree">AST</abbr>.

This is a *very* powerful class as it provides a way to do compile-time transformation, as in, add new nodes to the AST before it is compiled. This essentially gives you a way to add *new* methods, fields, and other elements to a Java class, as well as modify or remove existing ones.

It can be used by compiler plugins, annotation processors (like with [Project Lombok](https://projectlombok.org/)), and etc. However, due to it being an internal class, it has scarce documentation and it's hard to find examples of it online.

This guide was created as part of [**Project Transparent**](https://github.com/project-transparent) to help newcomers to Javac's internals and embrace some of the hacky projects out there.

## Prerequisites

You require an installation of JDK 8 (specifically a JDK, [as JREs do not contain the necessary `tools.jar` file](https://stackoverflow.com/a/5730851)), newer versions may work [but are more difficult to get working with this internal code](java-9-support.md).

## How to get TreeMaker

There are multiple ways to get an instance of TreeMaker, the two most common are via annotation processors and via compiler plugins.

### Annotation Processor Entrypoint

Using an annotation processor, you can modify annotated elements via TreeMaker. This can be done in the `AbstractProcessor.init` method.

```java
import com.sun.source.util.Trees;
import com.sun.tools.javac.processing.JavacProcessingEnvironment;
import com.sun.tools.javac.tree.TreeMaker;
import com.sun.tools.javac.util.Context;
import com.sun.tools.javac.util.Names;

...

protected Trees trees;
protected TreeMaker maker;
protected Names names;

...

@Override
public synchronized void init(ProcessingEnvironment env) {
    super.init(env);

    this.trees = Trees.instance(env);
    final Context context = ((JavacProcessingEnvironment) env).getContext();
    this.maker = TreeMaker.instance(context);
    this.names = Names.instance(context);
}
```

We have now introduced 2 new utility classes!
* **Trees** - This allows you to instantiate a `com.sun.source.tree.Tree` (which is then casted to `JCTree`) from an instance of `javax.lang.model.element.Element`.
* **Names** - This allows you to instantiate a `com.sun.tools.javac.util.Name` from a `String`, Name is used everywhere in TreeMaker as identifiers.

**TODO**

### Compiler Plugin Entrypoint

**TODO**
