# Quoted Printable for Crystal

The `QuotedPrintable` module provides for the encoding (`#encode`) and decoding (`#decode`, `#decode_string`) of binary data using a base64 representation.

## Installation

Add this to your application's `shard.yml`:

```yaml
dependencies:
  quoted_printable:
    github: arcage/crystal-quotedprintable
```

## Usage

```crystal
require "quoted_printable"

data = <<-HTM
<html lang=\"ja\">
<head>
  <title>日本語タイトル</title>
</head>
<body>
  <h1>見出し</h1>
  <p>本文<p>
</body>
</html>
HTM

encoded = QuotedPrintable.encode(data)
# => <html lang=3D"ja">
#    <head>
#      <title>=E6=97=A5=E6=9C=AC=E8=AA=9E=E3=82=BF=E3=82=A4=E3=83=88=E3=83=AB</t=
#    itle>
#    </head>
#    <body>
#      <h1>=E8=A6=8B=E5=87=BA=E3=81=97</h1>
#      <p>=E6=9C=AC=E6=96=87<p>
#    </body>
#    </html>

decoded = QuotedPrintable.decode_string(encoded)
#=> <html lang="ja">
#    <head>
#      <title>日本語タイトル</title>
#    </head>
#    <body>
#      <h1>見出し</h1>
#      <p>本文<p>
#    </body>
#    </html>
```

An argument of the `#encode` can be `String` or `Enumerable(UInt8)`(eg. `Bytes`, `Array(UInt8)`, `Tuple(UInt8)`) .

When the `String` object is given, it will be encoded as a text. That means it will be encoded with literal representation, and all line breaks in it will be represented as a CRLF sequence.

When the `Enumerable(UInt8)` object is given, all bytes wil be encoded as it is.

In case of the encoding as a text, even if original data was written with LF line breaks, all line breaks in decoded data will be CRLF. To avoid this, you can specify the line break code with `line_break:` argument of `#decode_string` method.

```crystal
# decode with LF line breaks
decoded = QuotedPrintable.decode_string(encoded, line_break: "\n")
```

To decode non-utf-8 text, you can specify the `encoding:` and `invalid:` arguments, same as those of the some methods of `String`.

```crystal
# decode from Shift JIS text
decoded = QuotedPrintable.decode_string(encoded, encoding: "Shift_JIS", invalid: :skip)
```

## Contributors

- [ʕ·ᴥ·ʔAKJ](https://github.com/arcage) - creator, maintainer
