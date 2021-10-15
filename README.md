<!--#region:intro-->
# Regular Expression Extended Mode and Comments for ECMAScript

<!--#endregion:intro-->

<!--#region:status-->
## Status

**Stage:** 0  
**Champion:** Ron Buckton ([@rbuckton](https://github.com/rbuckton))  

_For detailed status of this proposal see [TODO](#todo), below._  
<!--#endregion:status-->

<!--#region:authors-->
## Authors

* Ron Buckton ([@rbuckton](https://github.com/rbuckton))  
<!--#endregion:authors-->

<!--#region:motivations-->
# Motivations

From https://github.com/rbuckton/proposal-regexp-features:
> ECMAScript regular expressions have slowly improved over the years to adopt new 
> functionality commonly present in other languages, including:
> 
> - Unicode Support
> - Named Capture Groups
> - Match Indices
> 
> However, a large majority of other languages and libraries have a common set of 
> features that ECMAScript regular expressions currently lack.
> Some of these features improve performance in degenerative cases such as backtracking 
> in complex patterns. Some of these features introduce
> new tools for developers to write more powerful regular expressions.
> 
> As a result, ECMAScript developers wishing to leverage these capabilities are left with 
> few options, relying on native bindings to third-party
> libraries in environments such as NodeJS, or server-side evaluation.
> 
> There are numerous applications for extending the ECMAScript regular expression feature 
> set, including:
> 
> - In-browser support for TextMate grammars for web based editors/IDEs.
> - Improved performance for expressions through possessive quantifiers and backtracking 
>   control.
> - RegExp-based parsers that can support balanced brackets/parens.
> - Documenting complex patterns *in the pattern itself*.
> - Improved readability through the use of multi-line patterns and insignificant 
>   whitespace.

> NOTE: See https://github.com/rbuckton/proposal-regexp-features for an overview of
> how this proposal fits into other possible future features for Regular Expressions.

The RegExp Extended mode is a feature commonly supported amongst multiple regular expression engines that makes it possible
to write regular expressions that are easier to read and understand through the introduction of insignificant white space
and comments.

<!--#endregion:motivations-->

<!--#region:prior-art-->
# Prior Art 

* Perl: [Comments](https://rbuckton.github.io/regexp-features/engines/perl.html#feature-comments), 
  [Line Comments](https://rbuckton.github.io/regexp-features/engines/perl.html#feature-line-comments)  
* PCRE: [Comments](https://rbuckton.github.io/regexp-features/engines/pcre.html#feature-comments), 
  [Line Comments](https://rbuckton.github.io/regexp-features/engines/pcre.html#feature-line-comments)  
* Boost.Regex: [Comments](https://rbuckton.github.io/regexp-features/engines/boost.regex.html#feature-comments)  
* .NET: [Comments](https://rbuckton.github.io/regexp-features/engines/dotnet.html#feature-comments), 
  [Line Comments](https://rbuckton.github.io/regexp-features/engines/dotnet.html#feature-line-comments)  
* Oniguruma: [Comments](https://rbuckton.github.io/regexp-features/engines/oniguruma.html#feature-comments)  
* Hyperscan: [Comments](https://rbuckton.github.io/regexp-features/engines/hyperscan.html#feature-comments)  
* ICU: [Comments](https://rbuckton.github.io/regexp-features/engines/icu.html#feature-comments), 
  [Line Comments](https://rbuckton.github.io/regexp-features/engines/icu.html#feature-line-comments)  
* Glib/GRegex: [Comments](https://rbuckton.github.io/regexp-features/engines/glib-gregex.html#feature-comments), 
  [Line Comments](https://rbuckton.github.io/regexp-features/engines/glib-gregex.html#feature-line-comments)  

See https://rbuckton.github.io/regexp-features/features/comments.html and
https://rbuckton.github.io/regexp-features/features/line-comments.html for additional information.
<!--#endregion:prior-art-->

<!--#region:syntax-->
# Syntax

## Flags

### Extended mode (`x`)

**Prior Art:** Perl, PCRE, Boost.Regex, .NET, Oniguruma, Hyperscan, ICU, Glib/GRegex ([feature comparison](https://rbuckton.github.io/regexp-features/features/flags.html))

The extended mode (`x`) flag treats unescaped whitespace characters as insignificant, allowing for multi-line regular expressions. It also enables [Line Comments](#line-comments).

> NOTE: The `x`-mode flag can be used inside of a [Modifier](#modifiers)

> NOTE: While the `x`-mode flag can be used in a _RegularExpressionLiteral_, it does not permit the use of _LineTerminator_ in _RegularExpressonLiteral_. For multi-line
> regular expressions you would need to use the `RegExp` constructor.

> NOTE: Perl's original `x`-mode treated whitespace as insignificant anywhere within a pattern *except* for within character classes. Perl v5.26 introduced the `xx` flag which
> also ignores non-escaped SPACE and TAB characters. Should we chose to adopt the `x`-mode flag, we could opt to treat it as Perl's `xx` mode at the outset.

## Comments

**Prior Art:** Perl, PCRE, Boost.Regex, .NET, Oniguruma, Hyperscan, ICU, Glib/GRegex ([feature comparison](https://rbuckton.github.io/regexp-features/features/comments.html))

A comment is a sequence of characters that is ignored by pattern matching and can be used to document a pattern.

- `(?#comment)` &mdash; The entire expression is removed from the pattern. The text of *comment* may not contain other `(` or `)` characters.

> NOTE: This has no conflicts with existing syntax, as ECMAScript currently produces an error for this syntax in both `u` and non-`u` modes.

## Line Comments

**Prior Art:** Perl, PCRE, .NET, ICU, Glib/GRegex ([feature comparison](https://rbuckton.github.io/regexp-features/features/line-comments.html))

A Line Comment is a sequence of characters starting with `#` and ending with `\n` (or the end of the pattern) that is ignored by pattern matching and can be used to document a pattern.

- `# comment` &mdash; A line comment in a multi-line RegExp

> NOTE: Requires the `x`-mode [flag](#extended-mode-x).

> NOTE: Inside of `x`-mode, the `#` character must be escaped (using `\#`) outside of a character class.

> NOTE: Not supported in `x` mode in a Regular Expression Literal

<!--#endregion:syntax-->

<!--#region:semantics-->
<!-- # Semantics -->


<!--#endregion:semantics-->

<!--#region:examples-->
# Examples

## Insignificant Whitespace

```js
const re = /(foo) (bar) (baz)/x;
re.test("foobarbaz"); // true
```

## Comments

```js
const re = /foo(?#comment)bar/;
re.test("foobar"); // true
```

## Line Comments

```js
const re = new RegExp(String.raw`
    # match ASCII alpha-numerics
    [a-zA-Z0-9]
`, "x");
```


<!--#endregion:examples-->

<!--#region:api-->

# API

## Flags

### Extended mode (`x`)

#### API

- `RegExp.prototype.extended` (Boolean) &mdash; Indicates the `x`-mode flag is set.

<!--#endregion:api-->

<!--#region:grammar-->
<!-- # Grammar

```grammarkdown
``` -->
<!--#endregion:grammar-->

<!--#region:references-->
<!-- # References

> TODO: Provide links to other specifications, etc.

* [Title](url)   -->
<!--#endregion:references-->

<!--#region:todo-->
# TODO

The following is a high-level list of tasks to progress through each stage of the [TC39 proposal process](https://tc39.github.io/process-document/):

### Stage 1 Entrance Criteria

* [x] Identified a "[champion][Champion]" who will advance the addition.  
* [x] [Prose][Prose] outlining the problem or need and the general shape of a solution.  
* [x] Illustrative [examples][Examples] of usage.  
* [ ] ~~High-level [API][API].~~  

### Stage 2 Entrance Criteria

* [ ] [Initial specification text][Specification].  
* [ ] [Transpiler support][Transpiler] (_Optional_).  

### Stage 3 Entrance Criteria

* [ ] [Complete specification text][Specification].  
* [ ] Designated reviewers have [signed off][Stage3ReviewerSignOff] on the current spec text.  
* [ ] The ECMAScript editor has [signed off][Stage3EditorSignOff] on the current spec text.  

### Stage 4 Entrance Criteria

* [ ] [Test262](https://github.com/tc39/test262) acceptance tests have been written for mainline usage scenarios and [merged][Test262PullRequest].  
* [ ] Two compatible implementations which pass the acceptance tests: [\[1\]][Implementation1], [\[2\]][Implementation2].  
* [ ] A [pull request][Ecma262PullRequest] has been sent to tc39/ecma262 with the integrated spec text.  
* [ ] The ECMAScript editor has signed off on the [pull request][Ecma262PullRequest].  
<!--#endregion:todo-->

<!-- The following links are used throughout the README: -->

[Process]: https://tc39.es/process-document/
[Proposals]: https://github.com/tc39/proposals/
[Grammarkdown]: http://github.com/rbuckton/grammarkdown#readme
[Champion]: #status
[Prose]: #motivations
[Examples]: #examples
[API]: #api
[Specification]: https://rbuckton.github.io/proposal-regexp-modifiers

[Transpiler]: #todo
[Stage3ReviewerSignOff]: #todo
[Stage3EditorSignOff]: #todo
[Test262PullRequest]: #todo
[Implementation1]: #todo
[Implementation2]: #todo
[Ecma262PullRequest]: #todo
