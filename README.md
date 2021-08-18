go-testmark
===========

Wanna write test fixtures and example data in a language-agnostic way?

Wanna be able to display those fixtures and examples in the middle of the docs you're already writing?

Wanna be confident that your docs are also actually telling the truth, because your tests are executing directly on the example data in the docs?

Are those docs already in markdown, and all you need is some glue code to read it (and maybe even patch it) programmatically?

Welcome to `testmark`.  You might like it.


What is the testmark format?
----------------------------

Testmark is a very simple, and language-agnostic, format.
It's also a subset of markdown.
Your markdown documents can also be testmark documents!

Testmark data is contained in a markdown codeblock -- you know, the things that start and end with triple-backticks.

### testmark format by example

You see markdown code blocks all the time.  They render like this:

[testmark]:# (this-is-testmark-btw)
```json
{"these things": "you know?",
 "syntax highlighed, typically": "etc, etc"}
```

That right there?  That was also testmark fixture data.

Check out the "raw" form of this page, if you're looking a rendered form.
There's a specially structured "comment" in markdown which is tagging that markdown code block,
marking it as testmark data, and giving it a name.

The comment looks like this:

```
[testmark]:# (the-data-name-goes-here)
```

... and then the triple-backticks go right after that.
Data follows until the next line starting with triple-backticks, as is usual in markdown.

That's it.

Check out the [testdata/example.md](testdata/example.md) file for another example.
Be sure to mind the [raw form of the file](https://raw.githubusercontent.com/warpfork/go-testmark/master/testdata/example.md) too.

### the purpose of testmark

Formats for test fixtures and example data are extremely useful.
Some kind of language-agnostic format is critically important any time you're working on a project that involves codebases in more than one language, or any kind of networked interoperability.

Yet, picking a format (and getting people to agree on it) is hard.  And then getting it in your documentation is hard.

And then *maintaining* the fixtures and your documentation is hard, because you're typically stuck between two choices that are both bad:
either you can put some very ugly fixture formats in your documentation (and eventually realize that most users won't read them anyway, because of the eyeburn);
or you can maintain the fixtures and documentation separately,
while manually putting very pretty examples in the middle of your documentation (but then fail to make them load-bearing, so eventually coderot strikes,
and now your documentation misleads users, and adoption drops and frustration rises, and oh dear).

**Testmark is meant to solve all of these things.**

- Because testmark is "just markdown", you can easily use it together with other things that are already markdown.  That means including in documentation, websites, etc.
- Because you can intersperse markdown and prose with the code blocks, you can make good, readable, living and verifiable documentation, directly intertwined with your test data.
  It's great for commenting; it's great for docs.  People will actually read this!  And you have full control over the formatting and presentation.  Annotate things however you like.
- Because testmark is "just markdown", you can probably conclusively leap past a lot of bikeshedding conversations about test fixture data formats.
  Markdown isn't great, but good heavens is it useful, and ubiquitous.  Your colleagues will probably agree.
- Because testmark is "just markdown", you get all the other tools that work on markdown, for free.  For example, that tasty, tasty syntax highlighting.
  There's no smoother way to getting pretty example data on a website and getting it directly used in tests at the same time.
  (No fancy website or publishing tool pipeline needed, either.  You can just use github readme files -- just like this one!)
- Because more than one code block can be in a file, and you can tag them with names, you can cram many fixtures in one file.  (Or not.  Up to you.)
- Because it's machine-parsable, we can have tools and libraries that programmatically update the data blocks, too.
  And because testmark interacts with the markdown format in a very deterministic and well-bounded way, your markdown prose stays exactly where you put it, too.
  Easy fixture maintenance and automation, _and_ good human readability?  Yes, we _can_ have both.

tl;dr: deduplicate the work of spec fixtures and docs, both saving time, and getting more confident in the results, simultaneously.


This is go-testmark
-------------------

This is a golang library that implements a parser, a patcher, and a writer for the testmark format.

It works in the simplest way possible, while ignoring the actual markdown content as completely as possible.
(Simple is good.  And it turns out it's possible to parse testmark data out, and even patch the testmark data blocks, without a complete markdown parser.)

When you've parsed a testmark file, you can iterate over all the data hunks in it, and see their names, or look up them up by name.

When using the patch operation, the markdown you wrote will be maintained by the operation; only the testmark data blocks change.
(No markdown gets reformated; nothing tries to normalize anything.  Whatever you write is safe.
Use whatever other markdown extensions you like; we're not gonna error if there's something fancy we didn't expect.  It's chill.)

You should be able to look at the godoc for about five seconds and figure it out.  There's not much to it.

Check out the [`patch_test.go`](patch_test.go) file for an example of what updating a testmark file looks like with this library.


License
-------

SPDX-License-Identifier: Apache-2.0 OR MIT
