# Notes About Common Lisp (From Practical Common Lisp)

## SetUp
Install SBCL and Quicklisp (package manager)

## Notes
- Strings, numbers are self-evaluating objects.
- LISP's version of null is `NIL`
- Functions are defined using `defun` e.g.
  ```lisp
  (defun hello-world ()
    (format t "hello, world"))
  ```
- The above code only defines the function to run the function it needs to be called like `(hello-world)`
- Its a convention to separate words (in function names, etc) using `-` rather than camelCase or `_`.
