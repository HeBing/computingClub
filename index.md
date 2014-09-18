---
title       : Debugging in R
subtitle    : A Short Introduction to debugging and condition handling in R
author      : presented by Bing He
job         : Journal Club, 2014-09-18
logo        : bloomberg_shield.png
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}

---
## Let's debug!

![width](./fig/debug2.png)

--- .segue .dark

## What is the most handy tool for debugging in nearly all programming languages?


--- .segue

## `print()`

---

## Content

+ __Interactive debugging tools__: 


```r
traceback(); browser()
recover()
dump.frames(); debugger()
```

+ __Handling expected errors automatically (Condition Handling)__:


```r
stop(); warning(); message()
try(); tryCatch()
```

+ __Defensive programming__

--- 

## `traceback()`
`traceback()` lists the sequence of calls that lead to the error.


```r
# exceptions-example.R
f <- function(a) g(a)
g <- function(b) h(b)
h <- function(c) i(c)
i <- function(d) "a" + d
```



```r
source("exceptions-example.R")
f(10)
# Error in "a" + d : non-numeric argument to binary operator
traceback()
# 4: i(c) at exceptions-example.R#3
# 3: h(b) at exceptions-example.R#2
# 2: g(a) at exceptions-example.R#1
# 1: f(10)
```

Remember to read the call stack from bottom to top!

---

## `browser()`

`browser()` steps through the execution of an R function. 


```r
browser()
Browse[1]>
Browse[1]> ls()
## [1] "f" "g" "h" "i"
Browse[1]> print(f)
## function(a) g(a)
```

* Setting up a breakpoint in Rstudio is equivalent to inserting an `browser()`.

---

## `browser()` continued

![width](./fig/Rstudio-debug-toolbar.png)


```r
browser()
Browse[1]> n # s, f, c, Q
```

Special commands in `browser()` mode: 
* Next, `n`: executes the next line of code.
* Step into, or `s`: step into that function if next line is a function.
* Finish, or `f`: finishes execution of the current loop.
* Continue, `c`: leaves interactive debugging and continues regular execution of the function.
* Stop, `Q`: stops debugging

---

## Use `browser()` in clever ways

* Set up global option: call `browser()` whenever an error occurs:


```r
options(error = browser)
options(error = NULL)
```

* Combine with `if` to set up conditional breakpoints:


```r
if(i = 3) browser()
```

* You can nest multiple `browser()`!


```r
browser()
Browse[1]> browser()
Browse[2]> browser()
Browse[3]> 
```

---

## recover()

`recover()` allows to select from call stacks and call `browser()`


```r
myFit <- lm(y ~ x, data = xy, weights = w)
## Error in lm.wfit(x, y, w, offset = offset, ...) :
##        missing or negative weights not allowed
recover()
## Enter a frame number, or 0 to exit
##    1:lm(y ~ x, data = xy, weights = w)
##    2:lm.wfit(x, y, w, offset = offset, ...)
Selection: 2
## Called from: eval(expr, envir, enclos)
Browse[1]> ls()
## [1] "method" "n"      "ny"     "offset" "tol"    "w"
## [7] "x"      "y"
Browse[1]> w
## [1] -0.5013844  1.3112515  0.2939348 -0.8983705 -0.1538642
## [6] -0.9772989  0.7888790 -0.1919154 -0.3026882
Browse[1]> c # continue
```

---

## Interactive debugging of batch R code

Use `dump.frames()` and `debugger()`:


```r
# In batch R process ----
dump_and_quit <- function() {
  # Save debugging info to file last.dump.rda
  dump.frames(to.file = TRUE)
  # Quit R with error status
  q(status = 1)
}
options(error = dump_and_quit)

# In a later interactive session ----
load("last.dump.rda")
debugger()
```

`debugger()` enters an interactive debugger with the same interface as `recover()`.

---
## Condition Handling

Condition handlers: handling expected errors automatically.

* Expected errors
  * example: when you're fitting many models to different datasets, such as bootstrap replicates. Sometimes the model might fail to fit and throw an error, but you don't want to stop everything. 

* Common conditions: errors, warnings, and messages. 

* Common condition handlers (i.e., R functions): 


```r
stop("errors!"); warning("warnings!"); message("messages!")
```

---

## `try()`

`try()` allows execution to continue even after an error has occurred: 


```r
elements <- list(letters[1:5], 1:5)
print( lapply(elements, log) )
## Error in FUN(X[[1L]], ...) : 
##   non-numeric argument to mathematical function
print( lapply(elements, function(x) try(log(x))) )
## [[1]]
## [1] "Error in log(x) : non-numeric argument to mathematical function\n"
## attr(,"class")
## [1] "try-error"
## attr(,"condition")
## <simpleError in log(x): non-numeric argument to mathematical function>
## 
## [[2]]
## [1] 0.0000000 0.6931472 1.0986123 1.3862944 1.6094379
```

---

## tryCatch()

THe function `tryCatch()` allows you to use user-defined handlers (i.e. functions) when different conditions occur.


```r
tryCatch(mycode,
  error = function(c) { "your action when error occurs" },
  warning = function(c) { "your action when warning occurs" },
  message = function(c) { "your action when message occurs" }
)
```

---

## Defensive programming

<q> Fail fast: as soon as something wrong is discovered, signal an error. (Hadley Wickham) </q> 


---

## Defensive programming 

For example, a good practice is to check function inputs: 
* `stopifnot()`, 
* the `assertthat` package, 
* or simple `if` statements and `stop()`.

---

## Reference

[1] The content of these slides is from [Advanced R](http://adv-r.had.co.nz/) by Hadley Wickham

[2] R help

--- .segue .dark

Thank you!
-----
