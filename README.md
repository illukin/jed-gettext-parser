Jed Gettext Parser
==================

JavaScript Gettext `.mo` file parsing for [Jed](https://github.com/slexaxton/Jed/).

## Introduction

[Gettext](https://www.gnu.org/software/gettext/) is an old translation standard with implementations in many languages. It's one that programmers and translators are likely to be familiar with.

[Jed](https://github.com/slexaxton/Jed/) provides a very nice interface for translation using Gettext in Javascript.

Jed doesn't supply Gettext translation file parsers, so this library can act as the bridge between Gettext binary files and Jed. Maybe in future it will also gain the ability to parse `.po` files.

*Note: Jed Gettext Parser is made to work with Jed, but is a third-party library. Please direct any support queries to this repository's issue tracker, and [the author](https://github.com/WrinklyNinja).*

## Requirements

Jed Gettext Parser uses some fairly recent Javascript features:

* [Typed Arrays](http://caniuse.com/#feat=typedarrays)
* The Encoding API (available in Firefox 20 and Chromium 38: a polyfill can be found [here](https://github.com/inexorabletash/text-encoding))

## Usage

Jed Gettext Parser can be loaded as a browser global or an AMD module.

### Browser Global

```
<script src="jedGettextParser.js"></script>
<script>
var moBuffer;  // An ArrayBuffer.
// Fill the moBuffer with the contents of a .mo file in whatever way you like.

// messages is an object holding locale data as expected by Jed.
var locale_data = jedGettextParser.mo.parse(moBuffer);

// Now load using Jed.
var i18n = new Jed({
    'locale_data': locale_data,
    'domain': 'messages'
});
</script>
```

### AMD Module

```
require(['jedGettextParser', 'jed'], function(jedGettextParser, Jed) {
    var moBuffer;  // An ArrayBuffer.
    // Fill the moBuffer with the contents of a .mo file in whatever way you like.

    // messages is an object holding locale data as expected by Jed.
    var locale_data = jedGettextParser.mo.parse(moBuffer);

    // Now load using Jed.
    var i18n = new Jed({
        'locale_data': locale_data,
        'domain': 'messages'
    });
});
```

## Usage

The library currently exposes only one function:

```
var data = jedGettextParser.mo.parse(buffer, options);
```

`data` is an object that can be used as the value of Jed's `locale_data` initialisation option. The `buffer` argument is an `ArrayBuffer` object that holds the contents of the `.mo` file to parse. The `options` argument is an object with the following members (default values given):

```
var options = {
    encoding: undefined,
    domain: 'messages'
}
```

If `options.encoding` is undefined, the encoding given in the `.mo` file will be used to interpret the string data. Otherwise, valid values are identical to those accepted by the [TextDecoder constructor](https://developer.mozilla.org/en-US/docs/Web/API/TextDecoder.TextDecoder#Parameters) for its first parameter.

## Motivation

There are two types of Gettext translation files: the `.po` files contain human-readable text that can be easily edited by translators, and the `.mo` files contain equivalent binary data. Some Gettext implementations use one, the other, or both.

There are several Javascript `.po` file parsers available, which can operate in browser or [Node.js](http://nodejs.org/) environments, but the only Javascript `.mo` file parsers that seem to exist are all based off [gettext-parser](https://github.com/andris9/gettext-parser) and so require a Node.js environment, and do not function in the browser.

While developing a [Chromium Embedded Framework](https://code.google.com/p/chromiumembedded)-based application ([LOOT](github.com/loot/loot)) which required localisation of strings in the C++ and the Javascript code, I decided that parsing the `.mo` localisation files in each language separately was the neatest and simplest way of achieving this. Since there didn't seem to be anything that already existed for the task, I wrote this little library.

I used [gettext-parser](https://github.com/andris9/gettext-parser) to cross-check my understanding of the Gettext mo file [spec](https://www.gnu.org/software/gettext/manual/html_node/MO-Files.html), and for some inspiration on this library's API, so many to Andris Reinman for that help.