## Debugging in R slides for Computing Club

__2014-09-18__

#### Introduction

This set of [slides](http://hebing.github.io/computingClub/index.html) was created based on [Chapter Exceptions and debugging](http://adv-r.had.co.nz/Exceptions-Debugging.html) from [Advanced R](http://adv-r.had.co.nz/) (by Hadley Wickham).

This set of [slides](http://hebing.github.io/computingClub/index.html) was presented in Computing Club 09-18-2014.

#### Some notes on Slidify

I used Slidify to creat this set of slides.

* In R,

```r
library(devtools)
library(slidify)
author("computingClub")

# edit the index.Rmd file

slidify("index.Rmd")
```
Ref: [Get started with Slidify] (http://slidify.org/start.html)

* Publish to Github

Since I use Windows, I did not use the default way `publish(user = "USER", repo = "REPO", host = 'github')` for SSH connection.

After creating a new repository and pushing the Slidify files onto it, there are a few more steps:

```r
git branch gh-pages
git push origin gh-pages
```

Ref: [Rpubs Adding Slidify to GitHub](http://rpubs.com/thoughtfulbloke/25103).

* Wait for 10 mins and check: http://USERNAME.github.io/REPONAME/HTMLFILENAME
