# Clojure

These notes were made while going through [Clojure from the ground up series](https://aphyr.com/posts/301-clojure-from-the-ground-up-welcome)

- `nil` is the most basic value in Clojure. It represents emptiness, nothing-doing, not-a-thing. The absence of information.
- true, false, and nil form the three poles of the Lisp logical system.
- an expression in any dialect of a Lisp is a list `(inc 0)`
- the order of evaluation of lists in from L-R and innermost lists get evaluated first
```clojure
(+ 1 (- 5 2) (+ 3 4))
(+ 1 3       (+ 3 4))
(+ 1 3       7)
11
```
- to return the expression itself add a quote before its name `'inc` or `(+ 2 2)`
- Clojure has a strongly typed, dynamic type system.

```clojure
user=> (= 3 3.0)
false
user=> (== 3 3.0)
true
```

- `str` can be used to turn things into a string or for concatanetation
```clojure
user=> (str true)
"true"

user=> (str "meow " 3 " times")
"meow 3 times"

```
- regexes using `re-find` and `re-matches`
```clojure
user=> (re-find #"cat" "mystic cat mouse")
"cat"
user=> (re-find #"cat" "only dogs here")
nil
```
- only `nil` and `false` are false in clojure

```clojure
user=> (boolean false)
false
user=> (boolean nil)
false
```
- `and` returns the first negative(false) or the last value if all are positive value and `or` returns the first positive(true) value

```clojure
user=> (and true false true)
false
user=> (and true true true)
true
user=> (and 1 2 3)
3

user=> (or false 2 3)
2
user=> (or false nil)
nil
```

- Symbols: The job of symbols is to refer to things, to point to other values. When evaluating a program, symbols are looked up and replaced by their corresponding values
```clojure
user=> (class 'str)
clojure.lang.Symbol
user=> (= str clojure.core/str)
true
user=> (name 'clojure.core/str)
"str"
```
- Keywords are like strings in that they’re made up of text, but are specifically intended for use as labels or identifiers.
```clojure
user=> (type :cat)
clojure.lang.Keyword
```
- Keywords can also be used as verbs to look up specific values in other data types
- Lists can be constructed using `list`
```clojure
user=> (list 1 2 3)
(1 2 3)
```

- elements can be conjoined to a list

```clojure
user=> (conj '(1 2 3) 4)
(4 1 2 3)
```

- and get different elements of a list

```clojure
user=> (first (list 1 2 3))
1
user=> (second (list 1 2 3))
2
user=> (nth (list 1 2 3) 2)
3
```
- Vectors are an alternative to lists when you have to deal with large amount of elements and arbitrary access.
```clojure
user=> [1 2 3]
[1 2 3]
user=> (type [1 2 3])
clojure.lang.PersistentVector
user=> (vector 1 2 3)
[1 2 3]
user=> (vec (list 1 2 3))
[1 2 3]
user=> (conj [1 2 3] 4)
[1 2 3 4]
user=> (rest [1 2 3])
(2 3)
user=> (next [1 2 3])
(2 3)
user=> (rest [1])
()
user=> (next [1])
nil
user=> (last [1 2 3])
3
user=> (count [1 2 3])
3
```
- We can have direct index lookups in vectors
```clojure
user=> ([:a :b :c] 1)
:b
```
- Finally, note that vectors and lists containing the same elements are considered equal in Clojure
- Sets
```clojure
user=> #{:a :b :c}
#{:a :c :b}
user=> (conj #{:a :b :c} :d)
#{:a :c :b :d}
user=> (conj #{:a :b :c} :a)
#{:a :c :b}
user=> (disj #{"hornet" "hummingbird"} "hummingbird")
#{"hornet"}
user=> (contains? #{1 2 3} 3)
true
```
- Sets themselves can be used as a verb and perform lookpups
```clojure
user=> (#{1 2 3} 3)
3
user=> (#{1 2 3} 4)
nil
```
- Maps
```clojure
user=> {:name "mittens" :weight 9 :color "black"}
{:weight 9, :name "mittens", :color "black"}
user=> (get {"cat" "meow" "dog" "woof"} "cat")
"meow"
user=> (get {:a 1 :b 2} :c)
nil
```
- `get` can take a default value too and maps themselves can be used as verbs
```clojure
user=> (get {:glinda :good} :wicked :not-here)
:not-here
user=> ({"amlodipine" 12 "ibuprofen" 50} "ibuprofen")
50
```
- in case of keyword keys the syntax for lookup is a bit different
```clojure
user=> (:raccoon {:weasel "queen" :raccoon "king"})
"king"
```
- add/update in a map
```clojure
user=> (assoc {:bolts 1088} :camshafts 3)
{:camshafts 3 :bolts 1088}
user=> (assoc {:camshafts 3} :camshafts 2)
{:camshafts 2}
```
- new map creating using `assoc`
```clojure
user=> (assoc nil 5 2)
{5 2}
```
- remove values and merge maps
```clojure
user=> (dissoc {:potatoes 5 :mushrooms 2} :mushrooms)
{:potatoes 5}
user=> (merge {:a 1 :b 2} {:b 3 :c 4})
{:c 4, :a 1, :b 3}
```

- for binding values to names we can use `let` expression which takes a vector of bindings alternating between symbols and their values. Bindings are also evaluated in order
and bindings declared once in the vector can be reused in the next bindings.
```clojure
user=> (let [cats 3
             legs (* 4 cats)]
         (str legs " legs all together"))
"12 legs all together"

```
- let bindings are limited to the scope of the let expression
```clojure
user=> (let [+ -] (+ 2 3))
-1
```
- let bindings are immutable for mutable bindings we use `def` to define them and they are of var type
```clojure
user=> (def cats 5)
#'user/cats
user=> (type #'user/cats)
clojure.lang.Var
```
- functions/verbs in the clojure core library have two layers of indirection, for e.g. the symbol `inc` points to a var `clojure.core/inc` which in turn points to the function defined in the core `#object[clojure.core$inc 0x12e5be11 "clojure.core$inc@12e5be11"]`
```clojure
user=> 'inc
inc ; the symbol
user=> (resolve 'inc)
#'clojure.core/inc ; the var
user=> (eval 'inc)
#<core$inc clojure.core$inc@16bc0b3c> ; the value
```

> This way you can change what inc points to
```clojure
scratch.core=> (def inc (fn [x] (* x 2)))
WARNING: inc already refers to: #'clojure.core/inc in namespace: scratch.core, being replaced by: #'scratch.core/inc
#'scratch.core/inc
scratch.core=> (inc 2)
4
```

### Functions
```clojure
user=> ((fn [x] (+ x 1)) 2)
3
user=> (let [burrito #(list "beans" % "cheese")]
         (burrito "carnitas"))
("beans" "carnitas" "cheese")
```

- functions can also be written with a shorthand `#(+ %1 %2)` which translates to `(fn [x y] (+ x y))` where %1 and %2 refer to the first and second arg in case of a lone arg we can simply use `%`
