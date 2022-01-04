# **Go Learning Notes**
<!-- vscode-markdown-toc -->
* 1. [0. Basics](#Basics)
	* 1.1. [**Imports**](#Imports)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

##  1. <a name='Basics'></a>Basics
###  1.1. <a name='Imports'></a>**Imports**
```go
package main

import (
  "fmt"
  "os"
)

func main() {
  if len(os.Args) != 2 {
    os.Exit(1)
  }
  fmt.Println("It's over", os.Args[1])
}
```