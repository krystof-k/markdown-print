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

And you should see a newly created PDF file in `output` folder.

*If you get `pdflatex not found` error, this commands should fix it:*

```sh
# Append correct path to .bash_profile
echo "export PATH="$PATH:/Library/TeX/texbin"" >> ~/.bash_profile
# Update shell 
$ source ~/.bash_profile
```

## Let's start writing

### Citations & references

Citations with [pandoc-citeproc](https://github.com/jgm/pandoc-citeproc) are a piece of cake. Just collect all sources at one place and then reference them during writing.

#### Collection

Sources collection is placed in `content/references/references.yaml` file. It uses the CSL YAML format. See Citation Style Language see [specification](http://docs.citationstyles.org/en/stable/specification.html), especially useful is [list of types](http://docs.citationstyles.org/en/stable/specification.html#appendix-iii-types) and [variables](http://docs.citationstyles.org/en/stable/specification.html#appendix-iv-variables).

YAML is a human-readable data serialization language, so it's easy to write the collection by hand. Otherwise it is based on [CSL JSON](https://github.com/citation-style-language/schema) format.

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
  issued:
  - year: 2011
  title: Thinking, Fast and Slow
  publisher: Farrar, Straus and Giroux
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
  issued:
  - year: 2011
  title: Thinking, Fast and Slow
  publisher: Farrar, Straus and Giroux
---
```

#### Citations

And now the easier part. See [Pandoc User's Guide](http://pandoc.org/MANUAL.html#citations).
