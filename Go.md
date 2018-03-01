# My notes on the GO programming language
Learning Go

## Packaging
The root directory of a Go package/application is called a workspace and is divided into the following parts:

- **src** – This directory contains source files organized as packages. The application is written in this directory.
- **pkg** – This directory contains Go package objects.
- **bin** – This directory contains executable programs.

### A few important points about packages
- Source files are organized into system directories called packages, which enable code reusability across the Go applications.
- The naming convention for a Go package is to use the name of the system directory where we are putting our Go source files. Within a single folder, the package name will be same for the all source files which belong to that directory.
- We develop our Go programs in the $GOPATH directory, where we organize source code files into directories as packages.
- All identifiers will be exported to other packages if the first letter of the identifier name starts with an uppercase letter.
- The functions and types will not be exported to other packages if we start with a lowercase letter for the identifier name.

#### Package main
Only used when we want to write executable scripts and not a shared library/package.
Include `package main` in the scripts to make them executable (kind of like shebang)
```
package main

import (
  "fmt"
  "buffio"
  )

func main(){
  // do some stuff
}
```

- Here this script is an executable and function `main` acts as the entry point for the Go compiler while executing the script.
- The keyword `import` is used for importing a package into another package.
- When you import packages, the Go compiler will look on the locations specified by the environment variable GOROOT (standard library package) and GOPATH (third party packages).

#### Init function
While writing Go packages we can provide an init function that executes some initialization logic into the package at the time of execution.

In some cases where one wants to use only the `init` function of the package are not using the package identifier while importing, Go compiler will show an error.
In such a case, use a blank identifier ( _ ) as the package alias name, so the compiler ignores the error of not using the package identifier, but will still invoke the init function.

```
package main

import (
        _ "mywebapp/libs/mongodb/db"
	"fmt"
	"log"
)

func main() {
  //implementation here
}
```
