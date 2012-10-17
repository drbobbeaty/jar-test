# JRuby 1.7.0 and Warble Minimal Test

## Introduction

I wanted to make a simple test case for the JRuby guys to work with when
looking at a problem we've had in processing the `Dir.glob()` method and
using _relative_ directories. This is that test case.

## Building

Simply bundle everything up:
```bash
$ bundle install
```
and you're ready to run it as a script.

If you want to package it up as a jar, then simply:
```bash
$ warble
```
and the jar will be created.

## Running as a Script

This should work _regardless_ of the JDK or platform as it's just using
stock JRuby 1.7.0:
```bash
$ bin/myapp
Howdy!
got: bin/../config
config files: ["bin/../config/admin"]
contents: Line 1
Line 2
Line 3
====================================
uniform way: .
config files: ["./config/admin"]
contents: Line 1
Line 2
Line 3
```
all looks good.

## Running as an Executable Jar

This is where it gets dodgey:
```bash
$ java -jar jar-test.jar
Howdy!
got: file:/Users/bob/jar-test/jar-test.jar!/jar-test/bin/../config
config files: []
contents: 
====================================
uniform way: file:/Users/bob/jar-test/jar-test.jar!/jar-test
config files: ["file:/Users/bob/jar-test/jar-test.jar!/jar-test/config/admin"]
contents: Line 1
Line 2
Line 3
```
At this point the problem presents itself. It's not the JDK, it's the
relative file path manipulation in the jar and it's use in the `Dir.glob()`
method. It's not properly moving _up_ a node in the directory tree - in
this case, in the jar, when it hits the `/../`.
