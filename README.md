# Maya

[![Build Status](https://travis-ci.org/if1live/maya.svg?branch=master)](https://travis-ci.org/if1live/maya)
[![Coverage Status](https://coveralls.io/repos/github/if1live/maya/badge.svg?branch=master)](https://coveralls.io/github/if1live/maya?branch=master)

![maya](https://raw.githubusercontent.com/if1live/maya/master/document/maya.jpg)

Markdown preprocessor for static site generator.

## Feature
### Generate markdown file from markdown template file.
There are many static site generator exists.
Static site generator requires some metadata. (For example, title, slug, category, tags,...)
There is no standard markdown syntax for metadata.
So, every static site generate make their own syntax to express metadata.

For example, [pelican](http://blog.getpelican.com/) use this markdown.

```
Title: My super title
Date: 2010-12-03 10:20
Modified: 2010-12-05 19:30
Category: Python
Tags: pelican, publishing
Slug: my-super-post
Authors: Alexis Metaireau, Conan Doyle
Summary: Short version for index and feeds

This is the content of my super blog post.
```

[Hugo](https://gohugo.io/) use this markdown.

```
+++
date = "2015-01-08T08:36:54-07:00"
draft = true
title = "about"

+++

## A headline

Some Content
```

If syntax to express metadata exists, we can migrate from pelican to hugo easily.
(or migrate from A-static-site-generator to B-static-site-generator)

### Replace code and command line output
Embedding code into markdown is bothering task. Maya read source and embed it into markdown document.
Embedding command line output into markdown is bothering task. Maya execute command and embed result into makrdown document.


## Install

```bash
go install github.com/if1live/maya
```

## Usage

### Step1. Prepare markdown-like file and other file.

**demo.md**

```md
title: this is title
subtitle: this is subtitle
tags: python, demo
author: if1live
slug: sample-article

## write article

attach text file.

~~~maya:view
file=demo.py
lang=python
~~~

attach text file with line number.

~~~maya:view
file=demo.py
start_line=0
end_line=1
lang=python
~~~

print stdout/stderr as markdown code format.

~~~maya:execute
cmd=python demo.py
~~~

print stdout/stderr as markdown blockquote format.

~~~maya:execute
cmd=python demo.py
format=blockquote
~~~
```

**demo.py**
demo.py is used in ``demo.md``.

```python
def sample():
    print("hello, world")
sample()
```

## Step 2. Build document

```bash
maya -mode=pelican -file=demo.md
```

-----

title: this is title
subtitle: this is subtitle
tags: python, demo
author: if1live
slug: sample-article

## write article

attach text file.

```python
def sample():
    print("hello, world")
sample()
```

attach text file with line number.

```python
def sample():
```

print stdout/stderr as markdown code format.

```bash
hello, world
```

print stdout/stderr as markdown blockquote format.

> hello, world
>
>


-----

Output is markdown syntax, but it is hard to embed markdown document into another document. so, I use blockquote instead of code syntax.

## Is it Useful?

This `README.md` is generated from `README.tpl.md`.
Embedded code and output are generated by maya.

## Syntax

ignore backslash.

### Metadata

```
+++
title: this-is-title
subtitle: this-is-subtitle
<key>: <value>
+++
```

### Embed file

```
\~~~maya:view
file=demo.py
lang=python
start_line=1
end_line=2
format=blockquote
\~~~
```

| key | desc | required? |
|-------|------|-----------|
| file | file to attach | required |
| lang | language. if not exist, use extension |  optional |
| start_line | starting line to begin reading include file | optional |
| end_line | last line from include file to display | optional |
| format | blockquote/code/bold | optional |


### Embed command output

```
\~~~maya:execute
cmd=maya -mode=pelican -flie=demo.md
format=blockquote
attach_cmd=true
\~~~
```

| key | desc | required? |
|-------|------|-----------|
| cmd | command to execute | required |
| format | blockquote/code/bold |  optional |
| attach_cmd | attach cmd or not (if value exist, attach cmd) | optional |

### Embed youtube

example: https://www.youtube.com/watch?v=ESCv5qDuQIA

```
\~~~maya:youtube
video_id=ESCv5qDuQIA
width=480
height=320
\~~~
```


| key | desc | required? |
|-----|------|-----------|
| video_id | video id | required |
| width | width | optional |
| height | height | optional |

### Embed Gist

example: https://gist.github.com/if1live/b23494b9e42ae89e6f28#file-factorial-sh

```
\~~~maya:gist
id=b23494b9e42ae89e6f28
file=factorial.sh
\~~~
```

| key | desc | required? |
|-----|------|-----------|
| id | gist id | required |
| file | filename | required |
