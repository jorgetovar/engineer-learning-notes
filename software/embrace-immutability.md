# Embrace immutability (Programming & Infrastructure)

## Mutability 🚨

If you are a programmer, and you have some experience with languages such as Java, Python and Go are likely you are aware of the **benefits and drawbacks of mutability.**

The problem with mutability are mainly:

- **Increasing Complexity**: which function changes the value? why this property is null? 
- **Concurrency**: threads modifying the same variable 

With immutable data structures is possible to share data without any concerns between threads and functions.

Even in the infrastructure world, we used to deploy the servers, and continually update and modify in place them, as a result, it was very difficult to keep track of all the changes and create new environments from scratch.

**Immutable infrastructure** is another infrastructure paradigm in which servers are never modified. If something needs to be updated new servers would be built and provisioned to replace the old ones.
 
In the book Effective Java, the author advocates using the final keyword in methods, and classes. Why?

> The final keyword is a non-access modifier used for classes, attributes, and methods, which makes them non-changeable (impossible to inherit or override).

> The final keyword is useful when you want a variable to always store the same value, like PI

In languages such as Java, Go and Python objects are mutable by default. This makes it much harder to reason about the problem at hand.

The same happens in the infrastructure world with configuration management tools such as Chef, Puppet, and Ansible that typically default to a mutable infrastructure paradigm.


## Immutability to the rescue ⛑

This idea is inspired by functional programming, where variables are immutable, so after you’ve set a variable to a value, you can never change the value. If you need to update something, you create a new variable. 

**Because values never change, it’s easier to reason about your code.**

[Code example (Go/Clojure)](https://github.com/jorgetovar/immutability-spike)

The good news is that is possible to overcome this problem! 😎

### Go Pointers
- Pass values instead of references 
```go
package main

import (
	"fmt"
	"math/rand"
	"strconv"
	"strings"
)

type Book struct {
	Author string
	Title  string
	Pages  int
}

func (book Book) GetPagesWithCode() string {
	var n = rand.Intn(200)
	book.Pages -= n
	return "The " + strings.ToUpper(book.Title) + " book has " + strconv.Itoa(book.Pages) + " pages"
}

func main() {

	var book = Book{Author: "Rich Hickey", Title: "Clojure for the Brave & True", Pages: 232}
	fmt.Printf("1) %+v\n", book)
	changeAuthor(&book, "Daniel Higginbotham")
	fmt.Printf("2) %+v\n", book)
	fmt.Println(book.GetPagesWithCode())
	fmt.Printf("3) %+v\n", book)

}

func changeAuthor(book *Book, author string) {
	book.Author = author
}

```

### Clojure
- Create functions that return new values 
```clojure
(ns main.core
  (:require [clojure.string :as s])
  (:gen-class)
  )

(defn get-pages-with-code [book]
  (str "The " (s/upper-case (:title book)) " book has " (:pages book) " pages")
  )

(defn change-author [book author]
  (assoc book :author author))


(defn -main
  [& args]
  (let [book {:author "Rich Hickey", :title "Clojure for the Brave & True", :pages 232}
        new-book (change-author book "Daniel Higginbotham")]
    (println "1)" book)
    (println "2)" new-book)
    (println "3)" (get-pages-with-code book))
    )
  )
```

### Terraform
- Deploy new servers instead of updating current ones
```terraform
resource "aws_instance" "george_server" {
  ami           = "ami-0fb653ca2d3203ac1"
  instance_type = "t2.micro"
}
```

```terraform
resource "aws_instance" "george_server" {
  ami           = "ami-0CCC3ca2d32CCC1"
  instance_type = "t2.micro"
}
```

Because servers never change, it’s a lot easier to reason about what’s deployed.

## Conclusion

Controlling which parts of your program can make changes to variables and objects will allow you to write more robust and predictable software.

**Embrace immutability 🔥**

```ruby
severity = :mild
error_message = "OH GOD! IT'S A DISASTER! WE'RE "
if severity == :mild
  error_message = error_message + "MILDLY INCONVENIENCED!"
else
  error_message = error_message + "DOOOOOOOMED!"
end
You might be tempted to do something similar in Clojure:
```

```clojure
(def severity :mild)
(def error-message "OH GOD! IT'S A DISASTER! WE'RE ")
(if (= severity :mild)
  (def error-message (str error-message "MILDLY INCONVENIENCED!"))
  (def error-message (str error-message "DOOOOOOOMED!")))
```

- https://www.braveclojure.com/do-things/
