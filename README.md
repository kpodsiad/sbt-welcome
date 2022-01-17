# SBT Welcome

*An SBT plugin for displaying a welcome message and commonly used tasks.*

## What is it?

Upon loading SBT, a welcome message is displayed with common tasks for that project. This is particularly useful
for first-time contributors who are not yet familiar with a project and its build setup. Rather than leave them wondering
"How do I run the benchmarks?", "How do I build the microsite/docs?", etc. you can display these commands on startup.
Here's an example of what it can look like:

![screenshot](assets/screenshot.png?raw=true "SBT Welcome screenshot")

The bullet points are aliases (which are configurable), meaning you can type `d` and it'll run the 4th line item. The
aliases are optional, but can be useful for particularly long commands.

## Installation

Add the following to `project/plugins.sbt`:

```scala
addSbtPlugin("com.github.reibitto" % "sbt-welcome" % "0.2.2")
```

## Commands

You can type `welcome` to re-print the welcome message (rather than reloading the project).

## Configuration

An example configuration:

```scala
import sbtwelcome._

logo :=
  s"""
     |       ______ _____                   ______
     |__________  /___  /_   ___      _________  /__________________ ________
     |__  ___/_  __ \\  __/   __ | /| / /  _ \\_  /_  ___/  __ \\_  __ `__ \\  _ \\
     |_(__  )_  /_/ / /_     __ |/ |/ //  __/  / / /__ / /_/ /  / / / / /  __/
     |/____/ /_.___/\\__/     ____/|__/ \\___//_/  \\___/ \\____//_/ /_/ /_/\\___/
     |
     |${version.value}
     |
     |${scala.Console.YELLOW}Scala ${scalaVersion.value}${scala.Console.RESET}
     |
     |""".stripMargin

usefulTasks := Seq(
  UsefulTask("a", "~compile", "Compile with file-watch enabled"),
  UsefulTask("b", "fmt", "Run scalafmt on the entire project"),
  UsefulTask("c", "publishLocal", "Publish the sbt plugin locally so that you can consume it from a different project"),
  UsefulTask("d", "cli-client/graalvm-native-image:packageBin", "Create a native executable of the CLI client"),
  UsefulTask("e", "benchmarks/jmh:run", "Run the benchmarks")
)

logoColor := scala.Console.MAGENTA
```

You can embed any other information in the logo, such as the project version with normal Scala string interpolation like:

```scala
logo := s"Some Logo ${version.value}\nScala ${scalaVersion.value}"
```

You can also change the default colors like so:

- `logoColor := scala.Console.RED`
- `aliasColor := scala.Console.CYAN`
- `commandColor := scala.Console.YELLOW`
- `descriptionColor := scala.Console.GREEN`

### Declaring `UsefulTask`s without aliases

If you declare a `UsefulTask` without an alias, then it is presented with a `>`. This is very useful if you are making users aware of existing aliases, rather than needing to establish new ones. For example:

```scala
UsefulTask("", "myCommandAlias", "Call the 'myCommandAlias' alias")
```

Will be rendered as:

```
>  myCommandAlias - Call the 'myCommandAlias' alias
```

### Logo

If you want to generate a fancy logo for the `logo` field, you can use one of the following tools:

- [embroidery](https://github.com/wi101/embroidery)
- [TAAG](http://patorjk.com/software/taag)
