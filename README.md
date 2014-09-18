Debugging in R slides for Computing Club
=============

2014-09-18

#### Intro

This set of (slides)[http://hebing.github.io/computingClub/index.html] was created based on [Chapter Exceptions and debugging](http://adv-r.had.co.nz/Exceptions-Debugging.html) from [Advanced R](http://adv-r.had.co.nz/) (by Hadley Wickham).

This set of (slides)[http://hebing.github.io/computingClub/index.html] was presented in Computing Club 09-18-2014.

#### Some notes on the creation process

1. In R,

```r
library(devtools)
library(slidify)
author("computingClub")

# edit the index.Rmd file

slidify("index.Rmd")
```

2. Publish to Github
Since I use Windows, I did not use the default way `publish(user = "USER", repo = "REPO", host = 'github')` for SSH connection.

I followed instructions here: [Rpubs Adding Slidify to GitHub](http://rpubs.com/thoughtfulbloke/25103).

After creating a new repository and pushing the Slidify files onto it, there are a few more steps:

`git branch gh-pages`
`git push origin gh-pages`

3. Wait for 10 mins and check: http://USERNAME.github.io/REPONAME/HTMLFILENAME
