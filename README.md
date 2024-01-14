# Pragma

_A structured way of embedding source analyzer metadata in Hack._

## How to use

```HACK
// You must use use statements in exactly this form.
// `use namespace` or use statements with an `as` are not allowed.
use type HTL\Pragma\Pragmas;
use function HTL\Pragma\pragma;

// Apply an effect to an entire file.
<<file: Pragmas(vec['SourceAnalyzer', 'strict_mode=1'])>>

// Apply an effect to a class.
<<Pragmas(vec['PerfAnalyzer', 'hot_code=0'], vec['Ownership', 'maintainer=You'])>>
final class Example implements Printable {
  public function print(): string {
    // Apply an effect to the next line.
    pragma('CodingStandards', 'refused_bequest=ignore');
    throw new Exception();
  }
}
```

For details on the strings passed to `pragma(...)` and `Pragmas(vec[...])`,
read the documentation of the source analyzer.

## For implementers

This package contains one class and one function:

- The `pragma` function, also known as the `pragma` directive.
- The `Pragmas` class, also known as the `Pragmas` attribute.

They are general replacements for structured comments.

```HACK
// @lint-ignore[unused-pure-expression]
1 + 1;
```

`@lint-ignore` isn't a normal comment from the perspective of some tool.
It suppresses a lint error from being raised for an unused pure expression.

I subscribe wholehartedly to the following quote from the [CppCoreGuidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines):

```
Compilers don’t read comments ... and neither do many programmers (consistently).
```

If over half of the comments in a project are to silence some tool,
be it the Hack typechecker or a linter, programmers will be even more inclined
to skip reading the comments. It will instill fear for removing useless comments,
since some tool might rely on them.

The idea behind the `pragma(...)` directive is to eliminate these comments.
The following code block expersses the same intent.

```HACK
pragma('LinterFramework', 'ignore:unused-pure-expression');
1 + 1;
```

If you wish to accept `pragma(...)` directives and `<<Pragmas(vec[...])>>`
attributes in your own source analyzers, read [pragma.hack](./src/pragma.hack)
for the details for this informal standard.
