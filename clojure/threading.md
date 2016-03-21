# Threading macros in clojure, the -> and ->> thingy

A few days into learning clojure, I thought it would be a good idea to look at some actual clojure projects in github. I was feeling all confident and what not - you know getting used to lispy way of writing things. The puspose of going through some code, was to get a gist of what was going on in the code, if not understanding it fully. I guess you already know where this is going don't you ? Yep, I find myself reading through code and I find these two, `->` and `->>` (and a lot more *scary* stuff) staring at me, and I had no clue what to make of it. I guess there would at least be a few of you guys who felt the same.

Apparently, they are called threading macros. `->` is the thread first and `->>` thread last macros, and they are syntactical sugar to your code. It makes reading/writing code easier. "Meh! Just that?" you ask. Let's see.

The syntax goes something like this : [`(-> x & forms)`](http://clojuredocs.org/clojure.core/-%3E) and [`(->> x & forms)`](http://clojuredocs.org/clojure.core/-%3E%3E). The following examples might help you understand it.

Let say you want to do this (divide 2 by 1 then subtract 3 then add 4 and multiply with 5). How would you write it in clojure?

## ->
```clojure
user=> (* (+ (- (/ 2 1) 3) 4 )5)
15
```
Boy! It can get difficult to read when you have a bunch of these strung together.

Now lets see how we write it with `->`

```clojure
user=> (-> 2
	   (/ 1)
	   (- 3)
	   (+ 4)
	   (* 5))
15
```
Woh! This is a lot simpler to read (at least for me)! So what happens here is the thread first macro just takes the 2 and then pass it as the first argument to the next funtion and then the result of that as the first argument to the next and so on.

## ->>
Thread last does something similar, insted of passing it as the first argument it would pass it as the last argument. So if you where to apply the `->>` to the previous example you would get
```clojure
user=> (->> 2
	    (/ 1)
	    (- 3)
	    (+ 4)
	    (* 5))
65/2
```
which is
```clojure
user=> (* 5 (+ 4 (- 3 (/ 1 2))))
65/2
```

## Objects and collections
My favorite use of the threading macros has been when I have used them with java/clojure data structures. It makes handeling them a lot easier.

### Collections
The thread-last macro `->>` is very usefull in dealing with collections. Where you have to transofrm them or apply functions to them, which is what you might be doing in a lot of your clojure code.
For exmaple, if you have this collection:
```clojure
(def x {:document
	{:paragraph
	 {:text ["This is the first line"
	 	 "This is the second line"
		 "This is the third line"]}}})
```
Say you want to add a new '\n' at the end of each line and then print them together as a single string. How would you do this? Well its easy, you just get the text and then apply map and reduce to it and then print. Let's write it shall we?
```clojure
(println (reduce str (map #(str % "\n") (:text (:paragraph (:document x))))))
```
Now lets take a look at this if we decide to write it using thread last macro
```clojure
(->> x
     :document :paragraph :text
     (map #(str % "\n"))
     (reduce str)
     println)
```
It's a lot more cleaner, and you don't have to keep matching the parenthesis to actually figure out what is happening. This works even better when you want to do a lot more transformation on the collections.

While at it, we can make use of this neat function `get-in` that helps you get values from deep inside a map, which is somewhat better to use at times. The advantage of useing `get-in` over the therading would be that it helps you supply a `not-found` value, the would be returned if the key you are looking for is not there in the collection. Pretty neat huh? Let's try that.
```clojure
(->> (get-in x [:document :paragraph :text] ["No text found"])
     (map #(str % "\n"))
     (reduce str)
     println)
```


### Objects
Now if you are working with java interop and you aren't using the thread-first macro, then this might change your mind.
Let's take this example, where you have a java object and you apply a series of methods on the Java object or Java objects returned on applying these methods. This is how you would be doing it.
```clojure
(.add (.getContent (.getBody (.getJaxbelement (.getMaindocumentpart (Wordprocessingmlpackage/createPackage)))) paragraph)
```
Now with thread first this becomes
```clojure
(-> (WordprocessingMLPackage/createPackage)
    .getMainDocumentPart
    .getJaxbElement
    .getBody
    .getContent
    (.add paragraph)
```
Which is way more easier to read, and write. It is aligned with the original java representation, which aids in better understanding of the code. It feels less clunky than the previous case where you could get lost in all those parenthesis.

Since we are at it, let's talk about another function: `doto`. This is very helpful when you have to apply multiple funtions on a single java object. We didn't use it in the previous example because, each of the functions were returing a different object.

Consider you have a table-border object and you want to set border to it. This is how you would be writing with thread the `doto` funtion.
```clojure
(defn set-table-border
  [table-border border]
  (doto table-border
    (.setBottom border)
    (.setTop border)
    (.setRight border)
    (.setLeft border)
    (.setInsideH border)
    (.setInsideV border)))
```
You could use the threading operator or even write it in a single line, but it would be messy.

Reading clojure code, you will realise that a lot of peps out there use threading macro and are right in doing so, as code readability is very important. Use them when ever you can, but keep in mind why you are using them. It's not just the `how` that is important, `why` matters too.
