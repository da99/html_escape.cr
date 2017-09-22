da\_html.cr
============

DA\_HTML is a Crystal shard to escape/unescape HTML.

A much better shard: [https://github.com/kostya/myhtml](https://github.com/kostya/myhtml)
More info: [https://github.com/kostya/entities/pull/1#issuecomment-330849435](https://github.com/kostya/entities/pull/1#issuecomment-330849435)

If you are still curious about this shard:
==========================================

HTML entity decoding is done by the [kostya/myhtml](https://github.com/kostya/myhtml) shard.

Encoding characters into HTML entities is done by taking the codepoints and convert them
to hexadecimal HTML entities. I ported segments of [HTMLEntities by Paul Battley](https://github.com/threedaymonk/htmlentities)
for the encoding and most of the specs/tests.

This shard escapes all non-ASCII characters to hexadecimal only.
No named or decimal entities are used when escaping.  This shard is also useless for XML
entities.

I use this because Crystal's standard lib's [HTML](https://crystal-lang.org/api/master/HTML.html)
only escapes a few characters. I decide to play it extra safe
and escape all non-ASCII characters.

Notes:
=========

Further security info:
[OSWAP: Cross Site Prevention](https://goo.gl/Rka7pX)

List of hexadecimal entities with counterpart codepoints:
* [http://www.howtocreate.co.uk/sidehtmlentity.html](http://www.howtocreate.co.uk/sidehtmlentity.html)
* [https://dev.w3.org/html5/html-author/charref](https://dev.w3.org/html5/html-author/charref)
* Multibyte chars: [https://www.w3schools.com/charsets/ref_html_entities_v.asp](https://www.w3schools.com/charsets/ref_html_entities_v.asp)
* Convert chars: [https://r12a.github.io/apps/conversion/](https://r12a.github.io/apps/conversion/)

Searchable list of entities:
[http://www.fileformat.info/info/unicode/char/0000/index.htm](http://www.fileformat.info/info/unicode/char/0000/index.htm)

Usage:
=======

```crystal
  require "da_html"

  # Escape unsafe/non-ASCII codepoints using hexadecimal entities:
  raw = "<élan>"
  DA_HTML.escape(raw) # => "&#x3c;&#xe9;lan&#x3e;"

  # Unescaping:
  escaped = "&eacute;lan"
  DA_HTML.unescape_once(escaped) # => "élan"
  DA_HTML.unescape!(escaped) # => "élan"
```

## Licence

This code is free to use under the terms of the MIT licence. See the file
[LICENSE](https://github.com/da99/da_html.cr/blob/master/LICENSE) for more details.


## Useless Benchmarks:

NOTE: Encoding is twice as slow as using `HTML.escape`
from the Crystal standard library. The main reason is
because `DA_HTML` replaces control characters with spaces
and non-ASCII chars with hexadecimal HTML entities.

```
$ crystal run perf/benchmark.cr --release --no-debug
  # 100 iterations
  Encoding:   0.730000   0.040000   0.770000 (  0.828541)
  Decoding:   1.820000   0.010000   1.830000 (  1.891078)

$ neofetch
  CPU: AMD Athlon 5350 APU with Radeon R3 (4) @ 2.050GHz
  Memory: 1555MiB / 7934MiB
```
