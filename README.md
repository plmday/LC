#Lambda Calculators#

This repository provides reference implementaitons using Haskell of the *pure*
lambda-calculus in three styles, namely __Higher-Order Abstract Syntax__
(**HOAS**), __De-Bruijn Indices__ (**DBI**), __Indexed Names and Named
Indices__ (**INNI**).  Each implements *8* normalization strategies.

Currently, a REPL is provided for the INNI-style implementation.  To try it,
enter the `INNI` directory and compile the `Main.hs` file using GHC.

```
$ cd INNI
$ ghc -o repl Main.hs
```

Run the `repl`,

```
$ ./repl
```

and you will see the following prompt:

```
LC>
```

Below is an example interactive session:

```
LC> (<- (<- (-> m n f x (<- m f (<- n f x))) (-> f x (<- f x))) (-> f x (<- f (<- f x))))
(-> f x (<- f (<- f (<- f x))))
```

The syntax for lambda-expressions is a little different.  It can be easily
translated into the more traditional syntax.  For example,

```
(<- (-> x y x) y z)
```

will translate to

```
(\x.\y.x) y z
```

So `<-` means application, and `->` means abstraction.  Note that the last
sub-expression in an abstraction is the body of the function.  So the last `x`
in `(-> x y x)` is the funciton body.

The input expression in the above interactive session corresponds to

```
(\m.\n.\f.\x.m f (n f x)) (\f.\x.f x) (\f.\x.f (f x))
```

which means adding the Church-numeral one and two.  As expected, the
Church-numeral three was returned, because by default the REPL uses the
__normal-order__ (*non*) normalization strategy.  Other strategies, including
__call-by-name__ (*bnn*), __applicative-order__ (*aon*), __call-by-value__
(*bvn*), __hybrid-applicative-order__ (*han*), __head-spine__ (*hsn*),
__head__ (*hdn*), and __hybrid-normal-order__ (*hnn*) can be chosen on the fly
by the built-in `set` command of the REPL.

```
LC> set bnn
```

After issuing this command in the REPL, the call-by-name normalization
strategy is chosen.  Retyping in the above expression will not result the
Church-numeral three because call-by-name does not normalize under lambda.

```
LC> (<- (<- (-> m n f x (<- m f (<- n f x))) (-> f x (<- f x))) (-> f x (<- f (<- f x))))
(-> f x (<- (-> f x (<- f x)) f (<- (-> f x (<- f (<- f x))) f x)))
```

