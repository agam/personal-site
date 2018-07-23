{:title "up and running with scala"
:layout :post
 :tags ["old-post"]}



The "Getting started" section of the "Functional Programming with Scala" course mentions using Eclipse to build the first 'hello world' kind of project, but if you'd rather not get out of your Emacs comfort zone, there is a different way to do the same (credits to numerous online sources).



* Install `sbt` (`sudo apt-get install sbt`) and `scala` (download [from here](http://www.scala-lang.org/download/))



* Install the `ensime` plugin for `sbt`: Create a directory `~/.sbt/0.13/plugins`, and create `plugins.sbt`



```scala

resolvers += "Sonatype releases" at "https://oss.sonatype.org/content/repositories/releases"

addSbtPlugin("org.ensime" % "ensime-sbt-cmd" % "0.1.2")

```



* Install the `scala-mode2` Emacs: if you have Emacs24, this is as straightforward as running `M-x package-list-packages` and finding it)



* Install `ensime`: [Download](https://github.com/ensime/ensime-server/downloads) the latest `.tar.gz`, and install it somewhere under `.emacs.d`, then modify your `.emacs` to load the appropriate paths:



```

(add-to-list 'load-path "~/.emacs.d/ensime/elisp")

(require 'ensime)

(add-hook 'scala-mode-hook 'ensime-scala-mode-hook)

```



* Create a new directory: `mkdir helloworld; cd helloworld`



* Initialize the project: Create `build.sbt` with the following



```scala

name := "hello_world"

version := "1.0"

```

* Set up `ensime`: `sbt ensime generate`



* Open a file (say) `Main.scala`



* Start `ensime` in Emacs: `M-x ensime` (say 'yes' to various prompts). You should see something like "ENSIME ready. May the _ be with you." in the minibuffer.



* Add something in the file, e.g. (copying from the tutorial)



```scala

package greeter



object Hello extends App {

  println("Hello World")

}

```



* Hit `C-c C-b v`, this brings up an `sbt` console _inside Emacs_ (similar to how Slime works with Lisp)



* Type `run` in the console, you should see something like



```

> run

[info] Compiling 1 Scala source to /home/agam/Scala/hello-world/target/scala-2.10/classes...

[info] Running greeter.Hello 

Hello World

[success] Total time: 2 s, completed Jul 30, 2014 5:03:35 PM

```



... and that's it!


