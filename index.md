---
layout: index
type: project
title: reveal-md
---
# reveal-md

[reveal.js](http://lab.hakim.se/reveal-js/#/) on steroids! Get beautiful reveal.js presentations from your Markdown files.

## Installation

``` bash
npm install -g reveal-md
```

## Run

``` bash
reveal-md path/to/my/slides.md
```

This starts a local server and opens your Markdown file as a reveal.js presentation in the default browser.

## Quick demo

Get a quick preview with a few demo decks:

``` bash
reveal-md demo
```

## Markdown in reveal.js

The Markdown feature of reveal.js is awesome, and has an easy (and configurable) syntax to separate slides.
Use three dashes surrounded by two blank lines (`\n---\n`).
Example:

``` text
# Title

* Point 1
* Point 2

---

## Second slide

> Best quote ever.

Note: speaker notes FTW!

```

The separator syntax can be overriden (e.g. I like to use three blank lines).

## Speaker Notes

You can use the [speaker notes](https://github.com/hakimel/reveal.js#speaker-notes) feature by using a line starting with `Note:`.

## Usage

To open specific Markdown file as Reveal.js slideshow:

``` bash
reveal-md slides.md
```

You can also provide a url that resolves to a Markdown resource (over http(s)).

``` bash
reveal-md https://raw.github.com/webpro/reveal-md/master/demo/a.md
```

Show (recursive) directory listing of Markdown files:

``` bash
reveal-md dir/
```

Show directory listing of Markdown files in current directory:

``` bash
reveal-md
```

Override theme (default: `black`):

``` bash
reveal-md slides.md --theme solarized
```

Override reveal theme with a custom one:

``` bash
# you'll need a theme/my-custom.css file
reveal-md slides.md --theme my-custom
```

Inject custom scripts into the page:

``` bash
reveal-md slides.md --scripts script.js,another-script.js
```

Override reveal theme with a remote one (use rawgit.com because the url must allow cross-site access):
```
reveal-md slides.md --theme https://rawgit.com/puzzle/pitc-revealjs-theme/master/theme/puzzle.css
```

Override [highlight theme](https://github.com/isagalaev/highlight.js/tree/master/src/styles) (default: `zenburn`):

``` bash
reveal-md slides.md --highlight-theme github
```

Override slide separator (default: `\n---\n`):

``` bash
reveal-md slides.md --separator "^\n\n\n"
```

Override vertical/nested slide separator (default: `\n----\n`):

``` bash
reveal-md slides.md --vertical-separator "^\n\n"
```

Override port (default: `1948`):

``` bash
reveal-md slides.md --port 8888
```

Disable to automatically open your web browser:

``` bash
reveal-md slides.md --disable-auto-open
```

## Live-reload

Using `-w` option changes to markdown files will trigger the browser to
reload and thus display the changed presentation without the user having
to reload the browser.

## YAML Front matter

You can set markdown options and revealoptions specific to your pressentation in the .md file with YAML
front matter header Jekyll style.

```
---
title: Foobar
separator: <!--s-->
verticalSeparator: <!--v-->
theme: css/theme/solarized.css
revealOptions:
    transition: 'fade'
---
Foo

Note: test note

<!--s-->

# Bar

<!--v-->
```

## Print Support

*Requires phantomjs to be installed (preferably globally)*

This will create a PDF from the provided Markdown file and saved to the name passed into the `--print` parameter (e.g. slides.pdf):

``` bash
reveal-md slides.md --print slides.pdf
```

## Static website

This will produce a standalone version of the passed file in HTML including static scripts and stylesheets.
The files are saved to the directory passed to the `--static` parameter, or `./_static` if not provided:

```bash
reveal-md slides.md --static _site
```

## Options

You can define Reveal.js [options](https://github.com/hakimel/reveal.js#configuration) in a `reveal.json` file that you should put in the root directory of the Markdown files. They'll be picked up automatically. Example:

``` json
{
    "controls": true,
    "progress": true
}
```

## Custom slide attributes

You can use the [reveal.js slide attributes](https://github.com/hakimel/reveal.js#slide-attributes) functionality to add HTML attributes, e.g. custom backgrounds. Alternatively you could add an HTML `id` attribute to a specific slide and style it with your own CSS.

If you want yor second slide to have a png background:

``` text
# slide1

This slide has no background image.

---

<!-- .slide: data-background="./image1.png" -->
# slide2

This one does!
```

## Markdown preprocessor

`reveal-md` can be given a markdown preprocessor script via the `--preprocessor` (or
`-P`) option. This can be useful to implement custom tweaks on the document
format without having to dive into the guys of the Markdown parser.

For example, to have headers automatically create new slides, one could have
the script `preproc.js`:

```javascript
// headings trigger a new slide
// headings with a caret (e.g., '##^ foo`) trigger a new vertical slide
module.exports = (markdown, options) => {
  return markdown.split('\n').map((line, index) => {
    if(!/^#/.test(line) || index === 0) return line;
    const is_vertical = /#\^/.test(line);
    return (is_vertical ? '\n----\n\n' : '\n---\n\n') + line.replace('#^', '#');
  }).join('\n');
};
```

and use it like this

```bash
$ reveal-md -P preproc.js slides.md
```

## License

[MIT](http://webpro.mit-license.org)
