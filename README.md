# R with (x) => x + 1

Modified R grammar file that adds anonymous functions like this:

```r
f <- (x) => x + 1
f(2)
# 3
```

## Install

1. Define the R version:

    ```
    export R_VERSION=3.6.3
    ```

    This is the R version I based the modifications on, so it might not work with other versions.
2. Follow the 'Download and extract R' instructions from https://docs.rstudio.com/resources/install-r-source/
3. Replace the downloaded version of `src/main/gram.y` with `src/main/gram.y` from this repo
4. Generate a new parser file:

    ```
    bison -d src/main/gram.y -o src/main/gram.c
    ```

    You should see this output:

    ```
    src/main/gram.y: warning: 84 shift/reduce conflicts [-Wconflicts-sr]
    ```
5. Follow the 'Build and install R' instructions from https://docs.rstudio.com/resources/install-r-source/

    When I did this, I added a couple of lines to the `./configure` command:

    ```
    ./configure \
        --prefix=/opt/R/${R_VERSION} \
        --enable-memory-profiling \
        --enable-R-shlib \
        --with-blas \
        --with-lapack \
        --with-pcre1 \
        --without-recommended-packages
    ```

## Usage

Run:

```
/opt/R/${R_VERSION}/bin/R
```

Play:

```r
f <- (x) => x + 1
g <- (x, y) => x + y
h <- (x = 1) => x + 1
i <- () => 1
f(2)
# 3
g(2, 3)
# 5
h()
# 2
i()
# 1
```

Check that things haven't broken too much:

```r
# Normal functions
j <- function(x) x + 1
j(2)
# 3
# Normal expressions in parentheses
(1)
# 1
# Normal assignment in parentheses
(y = 5)
y
# 5
```

## Issues

The R grammar is complex and there's a fair chance that these modifications have broken something. Feel free to raise an issue if you find any errors.
