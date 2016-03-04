# Threading macros in clojure, the -> and ->> thingy

A few days into learning clojure, I thought it would be a good idea to look at some actual clojure projects in github. I was feeling all confident and what not - you know getting used to lispy way of writing things. The puspose of going through some code, was to get a gist of what was going on in the code, if not understanding it fully. I guess you already know where this is going don't you ? Yep, I find myself reading through code and i find these two, `->` and `->>` (and a lot more of *scary* stuff) staring at me, and I had no clue what to make of it. I guess there would at least be a few of you guys who felt the same. Hence the blogpost.

So first things first, what are they called ? Apparently, the threading macros. `->` is the thread first and `->>` thread last macros. What do these do ? Well they are just macros that add syntactical sugar to your code. It makes reading/writing code easier. Meh! Just that ? -you ask. Lets see.

The syntax goes something like this : [`(-> x & forms)`](http://clojuredocs.org/clojure.core/-%3E) and [`(->> x & forms)`](http://clojuredocs.org/clojure.core/-%3E%3E). The following examples might help you understand it.

Let say you want to do this (divide 2 by 1 then subtract 3 then add 4 and multiply with 5). How would you write it in clojure?

```clojure
user>(* (+ (- (/ 2 1) 3) 4 )5)
15
```
Boy! It can get difficult to read when you have a bunch of these strung together.

Now lets see how we write it with `->`

```clojure
user>(-> 2 (/ 1) (- 3) (+ 4) (* 5))
15
```
Woh! This is a lot simpler to read (at least for me)!

So what happens here is the thread first macro just takes the 2 and then pass it as the first argument to the next funtion and then the result of that as the first argument to the next and so on.

Thread last does something similar, insted of passing it as the first argument it would pass it as the second argument. So if you where to apply the `->>` to the previous example you would get

```clojure
user>(->> 2 (/ 1) (- 3) (+ 4) (* 5))
65/2
```

which is
```clojure
user>(* 5 (+ 4 (- 3 (/ 1 2))))
65/2
```

These help you a lot when having to write code with a lot of functions (wrting almost any clojure?). The best part of this is when dealing with collections and java interops (doing java stuff in clojure). It just makes the code look a lot cleaner.

Lets take another example. Here we some java code [(To create a docx using doc4j lib)](http://blog.iprofs.nl/2012/09/06/creating-word-documents-with-docx4j/), for which we would like to write a java interop in clojure
```java
WordprocessingMLPackage wordMLPackage = WordprocessingMLPackage.createPackage();
wordMLPackage.getMainDocumentPart().addParagraphOfText("Hello Word!");
wordMLPackage.save(new java.io.File("src/main/files/HelloWord1.docx"));
```
Clojure without threading operator
```clojure
(let [pkg (WordprocessingMLPackage/createPackage)]
  (.addParagraphOfText (.getMainDocumentPart pkg) "Hello World!")
  (.save pkg (file docx-file)))
```
Clojure with threading operator
```clojure
(let [pkg (WordprocessingMLPackage/createPackage)]
  (-> pkg (.getMainDocumentPart) (.addParagraphOfText "Hello World!"))
  (-> pkg (.save (file docx-file))))
```

Oh ! And these are very helpful getting stuff out of your nested maps.
```clojure
user>(def x {:a {:b {:c {:d {:e "foobar"}}}}})
user>(->> x :a :b :c :d :e)
"foobar"
user>(-> x :a :b :c :d :e)
"foobar"
```
instead of you know
```clojure
user>(((((x :a) :b) :c) :d) :e)
"foobar"
```
Now you tell me. Would you rather use threading or not.
