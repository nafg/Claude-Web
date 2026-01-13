# Minimal Mill Scala Project

A minimal Scala project using the Mill build tool.

## Project Structure

- `build.mill` - Mill build configuration
- `hello/src/` - Scala source files
- `Hello.scala` - Simple "Hello World" application

## Requirements

- Java 11 or higher
- [Mill](https://mill-build.org) build tool

## Building and Running

```bash
# Compile the project
./mill hello.compile

# Run the application
./mill hello.run

# Run tests (when added)
./mill hello.test
```

## Project Configuration

- Scala version: 2.13.12
- Mill version: 1.0.6 (specified in .mill-version)

## About Mill

Mill is a fast, simple build tool for Scala and Java projects, offering:
- 3-6x faster dev workflows than other JVM build tools
- Simpler configuration than Maven or Gradle
- Built-in caching and parallelization
