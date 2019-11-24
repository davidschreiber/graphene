# Graphene

_Graphene_ is a Gradle build plugin to visualize the task execution graph of your build. 

## Installation

Inside your top-level Gradle file add the Graphene plugin:

**In a `build.gradle` file (Groovy):**
```groovy
plugins {
    id 'at.droiddave.graphene' version '1.0.0-SNAPSHOT'
}
```

**In a `build.gradle.kts` Kotlin DSL file:**

```kotlin 
plugins {
    id("at.droiddave.graphene") version "1.0.0-SNAPSHOT"
}
```

## Usage

Simply run a build, and Graphene will write a GraphViz `.dot` file to `build/reports/taskGraph/graph.dot`.

```shell
$ ./gradlew assembleDebug
$ cat build/reports/taskGraph/graph.dot

strict digraph G {
  1 [ label=":someOtherTask" ];
  2 [ label=":someTask" ];
  2 -> 1;
}
```

You can also print the entire task dependency graph to the console:

```shell 
$ ./gradlew assembleDebug -Dat.droiddave.graphene.consoleOutput=TREE

 ── :someTask
    └── :someOtherTask

> Task :someOtherTask UP-TO-DATE
> Task :someTask UP-TO-DATE

BUILD SUCCESSFUL in 92ms

```

## Configuration

The plugin registers an extension called `graphene` on your project which can be used to configure the output:

```groovy
graphene {
    outputFile = new File(buildDir, 'reports/my-task-graph.dot')
    consoleOutput = at.droiddave.graphene.ConsoleOutput.TREE
}
```

### `outputFile`

Configures the path of the `.dot` output file containing the information about the task graph that was executed. Defaults to `reports/taskGraph/graph.dot`.  

```groovy
graphene {
    outputFile = new File(buildDir, 'reports/my-task-graph.dot')
}
```

### `consoleOutput`

Configures the type of console output produced by the plugin.

* `TREE` will print the entire task dependency tree before after the configuration phase of your build.
* `NONE` will not print any console output. This is the default.

```groovy
graphene {
    consoleOutput = at.droiddave.graphene.ConsoleOutput.TREE
}
```