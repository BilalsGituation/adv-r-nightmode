# Rewriting R code in C++ {#rcpp}



## Introduction

Sometimes R code just isn't fast enough. You've used profiling to figure out where your bottlenecks are, and you've done everything you can in R, but your code still isn't fast enough. In this chapter you'll learn how to improve performance by rewriting key functions in C++. This magic comes by way of the [Rcpp](http://www.rcpp.org/) package [@Rcpp] (with key contributions by Doug Bates, John Chambers, and JJ Allaire). 

Rcpp makes it very simple to connect C++ to R. While it is _possible_ to write C or Fortran code for use in R, it will be painful by comparison. Rcpp provides a clean, approachable API that lets you write high-performance code, insulated from R's complex C API. \index{Rcpp} \index{C++}

Typical bottlenecks that C++ can address include:

* Loops that can't be easily vectorised because subsequent iterations depend 
  on previous ones.

* Recursive functions, or problems which involve calling functions millions of 
  times. The overhead of calling a function in C++ is much lower than in R.

* Problems that require advanced data structures and algorithms that R doesn't 
  provide. Through the standard template library (STL), C++ has efficient 
  implementations of many important data structures, from ordered maps to 
  double-ended queues.

The aim of this chapter is to discuss only those aspects of C++ and Rcpp that are absolutely necessary to help you eliminate bottlenecks in your code. We won't spend much time on advanced features like object-oriented programming or templates because the focus is on writing small, self-contained functions, not big programs. A working knowledge of C++ is helpful, but not essential. Many good tutorials and references are freely available, including <http://www.learncpp.com/> and <https://en.cppreference.com/w/cpp>. For more advanced topics, the _Effective C++_ series by Scott Meyers is a popular choice. 

### Outline {-}

* Section \@ref(rcpp-intro) teaches you how to write C++ by 
  converting simple R functions to their C++ equivalents. You'll learn how 
  C++ differs from R, and what the key scalar, vector, and matrix classes
  are called.

* Section \@ref(sourceCpp) shows you how to use `sourceCpp()` to load
  a C++ file from disk in the same way you use `source()` to load a file of
  R code. 

* Section \@ref(rcpp-classes) discusses how to modify
  attributes from Rcpp, and mentions some of the other important classes.

* Section \@ref(rcpp-na) teaches you how to work with R's missing values 
  in C++.

* Section \@ref(stl) shows you how to use some of the most important data 
  structures and algorithms from the standard template library, or STL, 
  built-in to C++.

* Section \@ref(rcpp-case-studies) shows two real case studies where 
  Rcpp was used to get considerable performance improvements.

* Section \@ref(rcpp-package) teaches you how to add C++ code
  to a package.

* Section \@ref(rcpp-more) concludes the chapter with pointers to 
  more resources to help you learn Rcpp and C++.

### Prerequisites {-}

We'll use [Rcpp](http://www.rcpp.org/) to call C++ from R:


```r
library(Rcpp)
```

You'll also need a working C++ compiler. To get it:

* On Windows, install [Rtools](http://cran.r-project.org/bin/windows/Rtools/).
* On Mac, install Xcode from the app store.
* On Linux, `sudo apt-get install r-base-dev` or similar.

## Getting started with C++ {#rcpp-intro}

`cppFunction()` allows you to write C++ functions in R: \indexc{cppFunction()}



















































































