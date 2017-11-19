---
layout: post
title: Regex search containing one or more values in one line
date: 2017-11-19 20:42
excerpt: "RegEx: a developer's swiss knife for searching contents in text files"
tags: [regex, search, group]
comments: true
---
# RegEx search containing one or more values in one line

I love regular expressions - though I would call myself a beginner... but hey I'm willing to learn ;).

There are probably better and more detailed sources than Wikipedia. But it is a good start:

- [Regular Expressions](https://en.wikipedia.org/wiki/Regular_expression)
- [German, Regulärer Ausdruck](https://de.wikipedia.org/wiki/Regulärer_Ausdruck)

In case you understand German you should definitely have also a look there. Both, the English and German Wikipedia page, are in parts complementary to each other.

Example: search for lines containing 'href, # and onclick' or 'onclick, href, and #'
```
# source: http://stackoverflow.com/a/4389683
^(?=.*(?:href.*#+.*onclick|onclick.*href.*#+)).*$
# search for lines containing 'href, # and onclick' or 'onclick, href, and #', but does not contain 'return'
^(?=.*(?:href.*#+.*onclick|onclick.*href.*#+))(?!.*(?:return)).*$
```

