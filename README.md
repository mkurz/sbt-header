# sbt-header #

[![Join the chat at https://gitter.im/sbt/sbt-header](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/sbt/sbt-header?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

sbt-header is an [sbt](http://www.scala-sbt.org) plugin for creating or updating file headers, e.g. copyright headers.

## Requirements

- Java 7 or higher
- sbt 0.13.8 or higher

## Configuration

In order to add the sbt-header plugin to your build, add the following line to `project/plugins.sbt`:

``` scala
addSbtPlugin("de.heikoseeberger" % "sbt-header" % "1.4.0")
```

If your build uses an auto plugin for common settings, make sure to add `HeaderPlugin` to `requires`:

``` scala
import de.heikoseeberger.sbtheader.HeaderPlugin

object Build extends AutoPlugin {
  override def requires = ... && HeaderPlugin
  ...
}
```

You have to define which source or resource files should be considered by sbt-header and if so, how the headers should look like. sbt-header uses a mapping from file extension to header pattern and text for that purpose, specified with the `headers` setting. Here's an example:

``` scala
import de.heikoseeberger.sbtheader.HeaderPattern

headers := Map(
  "scala" -> (
    HeaderPattern.cStyleBlockComment,
    """|/*
       | * Copyright 2015 Heiko Seeberger
       | *
       | * Licensed under the Apache License, Version 2.0 (the "License");
       | * you may not use this file except in compliance with the License.
       | * You may obtain a copy of the License at
       | *
       | *    http://www.apache.org/licenses/LICENSE-2.0
       | *
       | * Unless required by applicable law or agreed to in writing, software
       | * distributed under the License is distributed on an "AS IS" BASIS,
       | * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
       | * See the License for the specific language governing permissions and
       | * limitations under the License.
       | */
       |
       |""".stripMargin
  )
)
```

The Apache 2.0 license has been pre-canned in [Apache2_0](https://github.com/sbt/sbt-header/blob/master/src/main/scala/de/heikoseeberger/sbtheader/license/Apache2_0.scala) and can be added as follows:

``` scala
import de.heikoseeberger.sbtheader.license.Apache2_0

headers := Map(
  "scala" -> Apache2_0("2015", "Heiko Seeberger")
)
```

Notice that for the header pattern you have to provide a `Regex` which extracts the header and the body for a given file, i.e. one with two capturing groups. `HeaderPattern` defines two widely used patterns:
- `cStyleBlockComment`, e.g. for Scala and Java
- `hashLineComment`, e.g. for Bash, Pyhon and HOCON

By the way, first lines starting with shebang are not touched by sbt-header.

By default sbt-header takes `Compile` and `Test` configurations into account. If you need more, just add them:

``` scala
HeaderPlugin.settingsFor(It, MultiJvm)
```

## Usage

In order to create or update file headers, execute the `createHeaders` task:

```
> createHeaders
[info] Headers created for 2 files:
[info]   /Users/heiko/projects/sbt-header/sbt-header-test/test.scala
[info]   /Users/heiko/projects/sbt-header/sbt-header-test/test2.scala
```

### Automation

If you want to automate header creation/update on compile, enable the `AutomateHeaderPlugin`:

``` scala
lazy val myProject = project
  .in(file("."))
  .enablePlugins(AutomateHeaderPlugin)
```

By default automation takes `Compile` and `Test` configurations into account. If you need more, just add them:

``` scala
AutomateHeaderPlugin.automateFor(It, MultiJvm)
```

## Contribution policy ##

Contributions via GitHub pull requests are gladly accepted from their original author. Along with any pull requests, please state that the contribution is your original work and that you license the work to the project under the project's open source license. Whether or not you state this explicitly, by submitting any copyrighted material via pull request, email, or other means you agree to license the material under the project's open source license and warrant that you have the legal authority to do so.

## License ##

This code is open source software licensed under the [Apache 2.0 License]("http://www.apache.org/licenses/LICENSE-2.0.html").
