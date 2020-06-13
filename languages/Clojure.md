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
> Good Clojurists use def to set up a program initially, and only change those definitions with careful thought.

### Conditionals
clojure provides `if`, `if-not`, `when` and `when-not` (in case we only have one branch to be evaluated), and also `if-let` and `when-let` in case we want to store the value of the predicate
```Clojure
user=> (when-let [x (+ 1 2 3 4)]
         (str x))
"10"
user=> (when-let [x (first [])]
         (str x))
nil
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
- `def` can be used to mutate a var and declare a function as seen an the last example of the previous section but there is a syntax sugar for defining functions `(defn half [number] (/ number 2))` which is equivalent to `(def half (fn [number] (/ number 2)))`
- For functions that take no arg you can leave the vector empty `defn funfunfunc [] (...)`
- To handle multiple arities in our functions we can define a list of args and bodies much like function clauses in Elixir but in the case of Clojure they are the part of the same function definition
```clojure
user=> (defn half
         ([]  1/2)
         ([x] (/ x 2)))
```
- For variadic arguments we can use `&` to slurp the rest of the args as a list and `""` can be used to write docstrings for functions
```clojure
user=> (defn vargs
	     "Takes two mandatory arguments x and y and the rest are variadic"
         [x y & more-args]
         {:x    x
          :y    y
          :more more-args})
#'user/vargs
user=> (vargs 1 2 3 4 5)
{:x 1, :y 2, :more (3 4 5)}
```
- `doc` function can be used to look up the docstring of a function and `meta` for function's metadata `(meta #'inc)` and `source` to look at the source code of functions except for special forms like `let`, `def`.
- to know whether a symbol points to a function we can use `(fn? let)` which will give us false and `(fn? inc)` which returns true.

### Recursion

```clojure
(defn inc-more [nums]
  (if (first nums)
    (cons (inc (first nums))
          (inc-more (rest nums)))
    ; base case or the recurrence relation
    (list)))
user=> (inc-more [1 2 3 4])
(2 3 4 5)
```

- A more generalized form of this would be the `map` function that can apply any kind of transformation on a collection of values.
- `map` can also be used as a zip kind of function to stitch together multiple collections
```clojure
user=> (map (fn [index element] (str index ". " element))
            ; since this is an infinite sequence
            (iterate inc 0)
            ; map will stop at the length of this samller collection
            ["erlang" "ruby" "haskell"])
("0. erlang" "1. ruby" "2. haskell")
```
- Clojure has an `iterate` and, `repeatedly` `repeat` function for building sequences 
```clojure
user=> (take 10 (iterate inc 0))
(0 1 2 3 4 5 6 7 8 9)
user=> (take 10 (repeat :hi))
(:hi :hi :hi :hi :hi :hi :hi :hi :hi :hi)
user=> (repeat 3 :echo)
(:echo :echo :echo)
user=> (take 3 (repeatedly rand))
(0.44442397843046755 0.33668691162169784 0.18244875487846746)
```
- Note: `rand` is an impure function since it gives a different result each time.
- `cycle` allows us to infinitely repeat a collection
```clojure
user=> (take 10 (cycle [1 2 3]))
(1 2 3 1 2 3 1 2 3 1)
```
- clojure also has a `range` function
```clojure
; first and third args are optional
; an reversed order of UB and LB returns an empty list instead of an error
user=> (range 0 100 5)
(0 5 10 15 20 25 30 35 40 45 50 55 60 65 70 75 80 85 90 95)
```

- A few more useful functions that operate on sequences.
```clojure

user=> (map-indexed (fn [index element] (str index ". " element))
                    ["erlang" "ruby" "haskell"])
("0. erlang" "1. ruby" "2. haskell")
user=> (concat [1 2 3] [:a :b :c] [4 5 6])
(1 2 3 :a :b :c 4 5 6)
user=> (interleave [:a :b :c] [1 2 3])
(:a 1 :b 2 :c 3)
user=> (interpose :and [1 2 3 4])
(1 :and 2 :and 3 :and 4)
user=> (reverse [1 2 3])
(3 2 1)
user=> (reverse "woolf")
(\f \l \o \o \w)
user=> (apply str (reverse "woolf"))
"floow"
; break strings into a sequence
user=> (seq "sato")
user=> (shuffle [1 2 3 4])
[3 1 2 4]
(\s \a \t \o)
```
- functions for subsequences
```clojure
user=> (take 3 (range 10))
(0 1 2)
user=> (drop 3 (range 10))
(3 4 5 6 7 8 9)
user=> (take-last 3 (range 10))
(7 8 9)
user=> (drop-last 3 (range 10))
(0 1 2 3 4 5 6)
user=> (take-while pos? [3 2 1 0 -1 -2 10])
(3 2 1)
user=> (drop-while pos? [3 2 1 0 -1 -2 10])
(0 -1 -2 10)

; for cutting sequences
(split-at 4 (range 10))
[(0 1 2 3) (4 5 6 7 8 9)]
user=> (split-with number? [1 2 3 :mark 4 5 6 :mark 7])
[(1 2 3) (:mark 4 5 6 :mark 7)]

user=> (filter pos? [1 5 -4 -7 3 0])
(1 5 3)

; `filter`'s complement
user=> (remove string? [1 "turing" :apple])
(1 :apple)


user=> (partition 2 [:cats 5 :bats 27 :crocodiles 0])
((:cats 5) (:bats 27) (:crocodiles 0))
(user=> (partition-by neg? [1 2 3 2 1 -1 -2 -3 -2 -1 1 2])
((1 2 3 2 1) (-1 -2 -3 -2 -1) (1 2))
```
- functions to collapse sequences
```clojure
user=> (frequencies [:meow :mrrrow :meow :meow])
{:meow 3, :mrrrow 1}


user=> (pprint (group-by :first [{:first "Li"    :last "Zhou"}
                                 {:first "Sarah" :last "Lee"}
                                 {:first "Sarah" :last "Dunn"}
                                 {:first "Li"    :last "O'Toole"}]))
{"Li"    [{:last "Zhou", :first "Li"}   {:last "O'Toole", :first "Li"}],
 "Sarah" [{:last "Lee", :first "Sarah"} {:last "Dunn", :first "Sarah"}]}
```
- when it comes to collapsing sequences `reduce` is the counterpart to map
```clojure

user=> (reduce + [1 2 3 4])
10

; reductions can show us intermediate steps of reduce
user=> (reductions + [1 2 3 4])
(1 3 6 10)

; reduce can also be used with an initial/default value
; this example starts with an empty set
user=> (reduce conj #{} [:a :b :b :b :a :a])
#{:a :b}

; Reducing elements into a collection has its own name
user=> (into {} [[:a 2] [:b 3]])
{:a 2, :b 3}
; reverse order
user=> (into (list) [1 2 3 4])
(4 3 2 1)

; in order 
user=> (reduce conj [] [1 2 3 4 5])
(reduce conj [] [1 2 3 4 5])
[1 2 3 4 5]
```

- `map` can be written in the form of `reduce`
```clojure
(defn my-map [f coll]
  (reduce (fn [output element]
            (conj output (f element)))
          []
          coll))
user=> (my-map inc [1 2 3 4])
[2 3 4 5]
```
- clojure has lazy functions (most of the sequence functions are lazy) which return unreliazed sequences
```clojure
user=> (def infseq (map inc (iterate inc 0)))
#'user/infseq
user=> (realized? infseq)
false
```

## Quotes
- A single quote can be used in Clojure to return an expression without evaluating it `'(list 1 2)` would return `(list 1 2)`
- We can also use backticks (called syntax quote operator) for this along with unquote (`~`) and unquote-splice (`~@`) operators.
```Clojure
; ~ substitutes the value but the expressin isn't evaluated
user=> (let [x 2] `(inc x))
(clojure.core/inc user/x)
user=> (let [x 2] `(inc ~x))
(clojure.core/inc 2)

; ~@ turns lists to multiple expressions
user=> `(foo ~[1 2 3])
(user/foo [1 2 3])
user=> `(foo ~@[1 2 3])
(user/foo 1 2 3)
```
## Macros
- `defmacro` can be used to define macros and macroexpand with a `'` to the macro call to see the expanded code after the macro rewrites it.
- Since macros generate code we might need to rename the symbols in the generated code for that we can use `gensym` which gives us new variable names in our macros
```Clojure
scratch.core=> (gensym "or")
or1615
```
- gensyms can also be generated by suffixing a `#` to a particular symbol
```Clojure
user=> `(let [x# 2] x#)
(clojure.core/let [x__339__auto__ 2] x__339__auto__)
```
- We can also override the local variables by not generating a gensym using `~'or` but this leads to an unhygenic macro
```Clojure
user=> (source or)
(defmacro or
  "Evaluates exprs one at a time, from left to right. If a form
  returns a logical true value, or returns that value and doesn't
  evaluate any of the other expressions, otherwise it returns the
  value of the last expression. (or) returns nil."
  {:added "1.0"}
  ([] nil)
  ([x] x)
  ([x & next]
      `(let [or# ~x]
         (if or# or# (or ~@next)))))

user=> (pprint (clojure.walk/macroexpand-all
                 '(or (mossy? stone) (cool? stone) (wet? stone))))
(let*
 [or__3943__auto__ (mossy? stone)]
 (if
  or__3943__auto__
  or__3943__auto__
  (let*
   [or__3943__auto__ (cool? stone)]
   (if or__3943__auto__ or__3943__auto__ (wet? stone)))))
```