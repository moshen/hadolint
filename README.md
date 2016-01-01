# Dockerfile Linter written in Haskell [![Build Status](https://travis-ci.org/lukasmartinelli/hadolint.svg)](https://travis-ci.org/lukasmartinelli/hadolint)


A smarter Dockerfile that helps you build [best practice Docker images](https://docs.docker.com/engine/articles/dockerfile_best-practices/).
The linter is parsing the Dockerfile into an AST and performs rules on top of the AST. Haskell is the ideal language for writing a Dockerfile linter because it makes parsing so easy and allows integrating [Shellcheck](https://github.com/koalaman/shellcheck) at a later stage.

## Get Started

Try it out online: http://hadolint.lukasmartinelli.ch/

The linter can be transpiled to JavaScript and is available online so you can copy paste your Dockerfile
into the code editor.

## Checks

List of implemented checks. Take a look into `Analyzer.hs` to find the implementation of the rules.

| Check                | Description
|----------------------|-----------------------------------
| **NoLatestTag**      | Using latest is prone to errors if the image will ever update. Pin the version explicitely to a release tag.
| **NoSudo**           | Do not use `sudo` as it leas to unpredictable behavior. Use a tool like `gosu` to enforce root.
| **NoUpgrade**        | Do not use `apt-get upgrade` or `dist-upgrade`.
| **NoCd**             | Use WORKDIR to switch to a directory
| **InvalidCmd**       | For some bash commands it makes no sense running them in a Docker container like `ssh`, `vim`, `shutdown`, `service`, `ps`, `free`, `top`, `kill`, `mount`, `ifconfig`
| **WgetOrCurl**       | Either use Wget or Curl but not both
| **HasMaintainer**    | Specify a maintainer of the Dockerfile
| **AbsoluteWorkdir**  | Use absolute WORKDIR

## Develop

Create a new sandbox.

```
cabal init
```

The easiest way to try out the parser is using the REPL.

```
cabal repl
```

In the REPL you can load the parser code with `:l Parser.hs` and use `parseString` or `parseFile` to get a quick look at the AST.

```
parseString "FROM debian:jessie"
```


### Parsing

The Dockerfile is parsed using [Parsec](https://wiki.haskell.org/Parsec) and is using the lexer `Lexer.hs` and parser `Parser.hs`.

Parser is nearly complete. There are still some problems with newlines and escape characters though.

### AST

Dockerfile syntax is is fully described in the [Dockerfile reference](http://docs.docker.com/engine/reference/builder/).
Just take a look at `Syntax.hs` to see the AST definition.
