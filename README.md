# ppq

ppq is a tool to search through your perl code. 

## Basic usage

Run the query against the perl source files under current working directory.

   ppq ${query}

## Query by example

The query language of `ppq` is also perl, but the syntatical elements has different meaning the
what's in a perl program. For example, here's how you recall all invocations of 'foo()' function:

    ppq 'foo()'

... and here's how to recall all invocations of `foo()` method,

    ppq '...->foo()'

THe `...` operator is used here as a placeholder to match whatever in the corpus. Natuarlly it
matches the actual use of `...` operator in the corpus too.

## Queries

perl tokens and operators are used to find themself in the code, in the given order. The following
queries finds those expressions that have the meaning of themself. Basically, imagine that `ppq`
trim the whitespaces and normalize those delimiting operators and only comparse the meaningful
elements.

    $bar + 42
    $bar . "42"
    42.0 * $bar

The order of tokens are meaningful, so `$bar + 42` and `42 + $bar` are 2 different quries that
recalls differently.

    foo() && $bar || @bar
    foo() && bar() + 42
    foo() && bar() || baz()

## Recall all uses of certain hash key, given the variable name

    ppq '$h{foo}'
    ppq '$h->{foo}'

## Recall all uses of certain hash key

    ppq '...->{foo}'
    ppq '...{foo}'

## Queries to recall subroutine definitions

## Queries to recall subroutine calls.

Subroutine calls means both "function invocation" and "method invocation". Here's the query to recall
all function invocations of a function named `foo`:

    foo()

The parens is required just for the sake of making it easier. This query recall ALL invocations,
reguardless the number of arguments.

Here's the query to recall method invocations of a method named foo:

    ...->foo()

The `...` operator here does not mean a literal `...` in the code, but rather it has a special
meaning -- whatever.

For example, the query above recall all the following expressions in the code:

    $o->foo()
    $o->{p}->foo()
    $o->[q]->foo()
    $o->r()->foo()
    42->foo()
    (do { 42 })->foo()

## Queries to recall string-mutating expressions

To recall string literals, query with a simple string expression:

    "foo"

This recall all string literlas that is "foo" in the code. Meaning, these:

    q/foo/
    'foo'
    "foo"
    q<foo>
    qq<foo>

This list is not the complete list, since the delimiter of `q` and `qq` operator can be arbeitrary.

    "40 $bar 42" 

    "40 $bar 42" 




