# markdown ❤️ print

*Inspired by Dan Prince's [Academic Pandoc](https://github.com/danprince/academic-pandoc).*

## Getting ready

##### macOS

Everything can be easily installed via [Homebrew](http://brew.sh) package manager on macOS. Command below should install Command Line Tools from Apple as well (you'll need it later too; if not, run `xcode-select —install`  to invoke a pop-up).

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Then install [Pandoc](http://pandoc.org):

```sh
brew install pandoc pandoc-citeproc
```

You'll also need LaTeX. Pandoc recommends installing [BasicTeX](http://www.tug.org/mactex/morepackages.html), since a full [MacTeX](https://tug.org/mactex) installation takes more than a gigabyte of disk space. You can either [download the installer](http://tug.org/cgi-bin/mactex-download/BasicTeX.pkg) or install it by [Homebrew-Cask](https://github.com/caskroom/homebrew-cask) (which you have already installed with Homebrew):

```sh
brew cask install basictex
```

*If you need any additional packages later, you can install them by `tlmgr` tool. For example:*

```sh
sudo tlmgr update --self
sudo tlmgr install collection-fontsrecommended
```

> **Not ready yet** 😢
>
> And if you want to enjoy comfortable styling by CSS, you'll need also [Prince](http://www.princexml.com) – a tool which can convert HTML  documents to PDF. It's free for non-commercial use. You have to [download the archive](http://www.princexml.com/download), unpack it and run this command in the folder:
>
> ```sh
> sudo ./install.sh
> ```
>
> See [official installation guide](http://www.princexml.com/doc/installing/#macos) for more details.

And now, let's try if everything works fine. Run:

```sh
./scripts/build
```

And you should see a newly created PDF file in the `output` folder.

*If you get `pdflatex not found` error, these commands should fix it:*

```sh
# Append correct path to .bash_profile
echo "export PATH="$PATH:/Library/TeX/texbin"" >> ~/.bash_profile
# Update shell 
source ~/.bash_profile
```

## Let's start writing

- [Citations & references](#citations-references)
- [Footnotes](#footnotes)
- [Figures](#figures)
- [Tables](#tables)
- [Appendices](#appendices)

### Citations & references

Citations with [pandoc-citeproc](https://github.com/jgm/pandoc-citeproc) are a piece of cake. Just collect all sources at one place and then reference them during writing.

#### Preparing the collections

Sources collections are placed in `content/references` folder. You can add as many files as you want.

You can use a lot of formats (see [list](https://github.com/jgm/pandoc-citeproc/blob/master/man/pandoc-citeproc.1.md#filter-mode)), most notably BibTeX, which can be downloaded at many services like [Google Scholar](https://scholar.google.com)/[Books](https://books.google.com). 

If you want to manage the collection manually, the most convenient option is to use CSL YAML format (don't forget to add `.yaml` extension to your file). CSL means *Citation Style Language*, see [specification](http://docs.citationstyles.org/en/stable/specification.html), especially useful is [list of types](http://docs.citationstyles.org/en/stable/specification.html#appendix-iii-types) and [variables](http://docs.citationstyles.org/en/stable/specification.html#appendix-iv-variables). YAML is a human-readable data serialization language, so it's easy to write the collection by hand. Otherwise it is based on [CSL JSON](https://github.com/citation-style-language/schema) format.

The file has to start with:

```yaml
---
references:
```

and again end with:

```yaml
---
```

Between these lines, there's a space for your sources. Let's try *Thinking, Fast and Slow by Daniel Kahneman* book:

```yaml
- id: thinking-fast-and-slow
  type: book
  author:
  - family: Kahneman
    given: Daniel 
  title: Thinking, Fast and Slow
  edition: 1
  publisher: Farrar, Straus and Giroux
  publisher-place: New York
  issued:
  - year: 2011
  ISBN: 978-0374275631
```

Nothing difficult, right? It's important to keep the indentation precise. As `id` you can use any string – you'll reference it later in the text. So the whole file may look like this:

```yaml
---
references:
- id: thinking-fast-and-slow
  type: book
  author:
  - family: Kahneman
    given: Daniel 
  title: Thinking, Fast and Slow
  edition: 1
  publisher: Farrar, Straus and Giroux
  publisher-place: New York
  issued:
  - year: 2011
  ISBN: 978-0374275631
---
```

#### In-text citations

And now the easier part. See [Pandoc User's Guide](http://pandoc.org/MANUAL.html#citations).

### Footnotes

Inserting footnotes is totally simple. The first and simplier approach allows you to write footnotes directly in text:

```markdown
This is a text with footnote.^[…and this the inline footnote itself.]
```

If you prefer to separate the footnotes, you can reference the footnote by an identifier. Identifier can be anything (except it cannot contain spaces) and it has to be unique throughout the document – footnotes will be numbered sequentially automatically.

```markdown
You can use also footnote identifiers, which separates the footnote content.[^footnote]

[^footnote]: It's a good thing that in that case, you can use multiple paragraphs within the footnote.

    Just don't forget to indent other paragraphs by four spaces.
```

### Figures

Inserting figures – or photos/images/illustrations/schemes/whatever-you-want-to-call-it – works in the same way as inserting images in Markdown. Just be sure the image is in a separate paragraph.

```markdown
![Cat picture](https://placekitten.com/400/200)
```

Figure is then automatically annotated and added to *List of figures*. If you don't want *List of figures*, simply delete the `0-03-list-of-figures.md` file.

But because you'll probably want to reference the figures in the text, the syntax gets a bit more complicated:

```markdown
![One more cat picture \label{fig:figure}](https://placekitten.com/400/200){#fig:figure}

See the cat in [\autoref{fig:figure}](#fig:figure).
```

Where `figure` presents unique identifier of the certain figure.

As you can see, it's possible to link images directly from the internet, which is definitely not recommended. This is how you can insert local file, even PDF. Figures are saved in `content/figures` folder.

```markdown
![Local figure as PDF](./content/figures/figure.pdf)
```

If you want to insert an image not as an figure, simply add backslash at the end:

```markdown
![Another cat picture, but not a figure](https://placekitten.com/400/200)\
```

### Tables

Inserting tables is a bit more complicated, especially if you want to use advanced formatting (in that case see [Pandoc manual](http://pandoc.org/MANUAL.html#tables)). But not that much:

```markdown
Table: This is a table caption, you can even use a citation [@thinking-fast-and-slow]

First column  | Centered column | Right aligned column
------------- |:---------------:| -------------------:
First row     | Second cell     | 1234
Second row    | Centered cell   | 4567
3rd row       | …another cell   | 89
```

If you want to reference a table, it works in a similar way as with figures:

```markdown
This is a reference to [\autoref{tab:table}](#tab:table).

Table: [This is a table without header, but with a reference]{#tab:table} \label{tab:table}

-----------    ----------------
Another way    to insert tables
This time      without header
-----------    ----------------
```

Tables are again automatically listed in *List of tables*, if you don't want that, simply delete the `0-04-list-of-tables.md` file.

### Appendices

You can add appendices. It's important to name the files beginning with `3`. You can add as many appendices as you want, all will be numbered by letter. To change the appendices name (from *Appendix* to something else), edit `0-00-metadata-and-settings.yaml` file.