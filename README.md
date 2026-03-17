# Scalingo Clojure Buildpack

This is the [Scalingo buildpack](https://doc.scalingo.com/platform/deployment/buildpacks/intro)
for apps that use [Leiningen](https://leiningen.org/) as their build tool. It's
primarily used to build [Clojure](https://clojure.org/) applications.

If you're using a different JVM build tool, use the appropriate buildpack:
* [Java buildpack](https://github.com/Scalingo/java-buildpack) for [Maven](https://maven.apache.org/) projects
* [Gradle buildpack](https://github.com/Scalingo/gradle-buildpack) for [Gradle](https://gradle.org/) projects
* [Scala buildpack](https://github.com/Scalingo/scala-buildpack) for [sbt](https://www.scala-sbt.org/) projects

## Table of Contents

- [Supported Leiningen Versions](#supported-leiningen-versions)
- [Application Requirements](#application-requirements)
- [Configuration](#configuration)
  - [OpenJDK Version](#openjdk-version)
  - [Leiningen Version](#leiningen-version)
  - [Buildpack Configuration](#buildpack-configuration)
- [Documentation](#documentation)


## Supported Leiningen Versions

This buildpack officially supports Leiningen `2.x`. Leiningen `1.x` is no
longer supported

## Application Requirements

Your app requires a `project.clj` file in the root directory with
`:min-lein-version "2.0.0"` or higher. It's recommended to also configure
`:uberjar-name` in your `project.clj`.

The buildpack will detect your app as Clojure if it has a `project.clj` file in
the root. If you use the [clojure-maven-plugin](https://github.com/talios/clojure-maven-plugin),
[the standard Java buildpack](http://github.com/Scalingo/java-buildpack) should
work instead.


## Configuration

### OpenJDK Version

Specify an OpenJDK version by creating a `system.properties` file in the root
of your project directory and setting the `java.runtime.version` property. See
the [Java Support article](https://doc.scalingo.com/languages/java/start#availability)
for available versions and configuration instructions.

### Leiningen Version

The buildpack uses Leiningen `2.12.0` by default for projects that specify
`:min-lein-version "2.0.0"` or higher in their `project.clj`.

To use a specific Leiningen version, you can include a `bin/lein` script in
your repository. The buildpack will detect and use this script instead of the
default Leiningen installation.

### Buildpack Configuration

Configure the buildpack by setting environment variables:

| Environment Variable   | Description | Default |
| ---------------------- | ----------- | ------- |
| `LEIN_BUILD_TASK`      | Leiningen task to execute | `uberjar` (if `:uberjar-name` is set) or `with-profile production compile :all` |
| `LEIN_INCLUDE_IN_SLUG` | Include Leiningen in the slug for runtime use | `no` |
| `CLOJURE_CLI_VERSION`  | Clojure CLI tools version | `1.12.4.1597` |

You can also override the default build behavior by including a `bin/build`
script in your repository. The buildpack will execute this script instead of
the default build command.

## Documentation

For more information about using Clojure on Scalingo, see [our documentation](https://doc.scalingo.com/languages/clojure/start).
