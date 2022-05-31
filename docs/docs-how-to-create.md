---
layout: default
title: DOCS - How to Create
nav_order: 100
---

# [](#header-1)How to Create and Publish a Document

- The document must be created as a Markdown file (MD).
  You can use for intance MarkText as a editor
  

```
[](#header-1) should be use as the "header 1"" tag (place it before the text)
```

- The pictures to include in the MD file should be in a separate directory (preferably with the same name as the MD file).

- The head of the MD file must include the following 
  {: .label .label-green }

```
---
layout: default
title: <title of the document>
nav_order: <sequence number>
---
```

- Copy both the document and the directory with the pictures into "docs" directory in the server
- It should be visible on the site after refresh

------------------

[ Cisco ](http://www.cisco.com){: .btn }

------------------

```
── 404.html
├── CODE_OF_CONDUCT.md
├── Dockerfile
├── Gemfile
├── Gemfile.lock
├── LICENSE.txt
├── README.md
├── Rakefile
├── _config.yml
├── _includes
│   ├── css
│   │   ├── custom.scss.liquid
│   │   └── just-the-docs.scss.liquid
│   ├── fix_linenos.html
│   ├── footer_custom.html
│   ├── head.html
│   ├── head_custom.html
│   ├── header_custom.html
│   ├── js
│   │   └── custom.js
│   ├── nav.html
│   ├── title.html
│   └── vendor
│       └── anchor_headings.html
├── _layouts
│   ├── about.html
│   ├── default.html
│   ├── home.html
│   ├── page.html
│   ├── post.html
│   ├── table_wrappers.html
│   └── vendor
│       └── compress.html
├── _sass
│   ├── base.scss
│   ├── buttons.scss
│   ├── code.scss
│   ├── color_schemes
│   │   ├── dark.scss
│   │   └── light.scss
│   ├── content.scss
│   ├── custom
│   │   └── custom.scss
│   ├── labels.scss
│   ├── layout.scss
│   ├── modules.scss
│   ├── navigation.scss
│   ├── print.scss
│   ├── search.scss
│   ├── support
│   │   ├── _functions.scss
│   │   ├── _variables.scss
│   │   ├── mixins
│   │   │   ├── _buttons.scss
│   │   │   ├── _layout.scss
│   │   │   ├── _typography.scss
│   │   │   └── mixins.scss
│   │   └── support.scss
│   ├── tables.scss
│   ├── typography.scss
│   ├── utilities
│   │   ├── _colors.scss
│   │   ├── _layout.scss
│   │   ├── _lists.scss
│   │   ├── _spacing.scss
│   │   ├── _typography.scss
│   │   └── utilities.scss
│   └── vendor
│       └── normalize.scss
│           ├── README.md
│           └── normalize.scss
├── _site
│   ├── 404.html
│   ├── CODE_OF_CONDUCT.md
│   ├── Dockerfile
│   ├── assets
│   │   ├── css
│   │   │   ├── just-the-docs-dark.css
│   │   │   ├── just-the-docs-dark.css.map
│   │   │   ├── just-the-docs-default.css
│   │   │   ├── just-the-docs-default.css.map
│   │   │   ├── just-the-docs-light.css
│   │   │   └── just-the-docs-light.css.map
│   │   ├── images
│   │   │   ├── just-the-docs.png
│   │   │   ├── large-image.jpg
│   │   │   ├── search.svg
│   │   │   └── small-image.jpg
│   │   └── js
│   │       ├── just-the-docs.js
│   │       ├── search-data.json
│   │       └── vendor
│   │           └── lunr.min.js
│   ├── docker-compose.yml
│   ├── docs
│   │   ├── configuration
│   │   │   └── index.html
│   │   ├── customization
│   │   │   └── index.html
│   │   ├── index-test
│   │   │   └── index.html
│   │   ├── navigation-structure
│   │   │   └── index.html
│   │   ├── search
│   │   │   └── index.html
│   │   ├── ui-components
│   │   │   ├── buttons
│   │   │   │   └── index.html
│   │   │   ├── code
│   │   │   │   ├── index.html
│   │   │   │   └── line-numbers
│   │   │   │       └── index.html
│   │   │   ├── labels
│   │   │   │   └── index.html
│   │   │   ├── lists
│   │   │   │   └── index.html
│   │   │   ├── tables
│   │   │   │   └── index.html
│   │   │   └── typography
│   │   │       └── index.html
│   │   ├── ui-components.html
│   │   ├── utilities
│   │   │   ├── color
│   │   │   │   └── index.html
│   │   │   ├── layout
│   │   │   │   └── index.html
│   │   │   ├── responsive-modifiers
│   │   │   │   └── index.html
│   │   │   └── typography
│   │   │       └── index.html
│   │   └── utilities.html
│   ├── favicon.ico
│   └── index.html
├── assets
│   ├── css
│   │   ├── just-the-docs-dark.scss
│   │   ├── just-the-docs-default.scss
│   │   └── just-the-docs-light.scss
│   ├── images
│   │   ├── just-the-docs.png
│   │   ├── large-image.jpg
│   │   ├── search.svg
│   │   └── small-image.jpg
│   └── js
│       ├── just-the-docs.js
│       ├── vendor
│       │   └── lunr.min.js
│       └── zzzz-search-data.json
├── bin
│   └── just-the-docs
├── docker-compose.yml
├── docs
│   ├── configuration.md
│   ├── customization.md
│   ├── index-test.md
│   ├── navigation-structure.md
│   ├── search.md
│   ├── tests
│   │   ├── index.md
│   │   ├── navigation
│   │   │   ├── disambiguation
│   │   │   │   ├── a.md
│   │   │   │   ├── b.md
│   │   │   │   ├── ca.md
│   │   │   │   ├── cb.md
│   │   │   │   ├── dca.md
│   │   │   │   ├── dcb.md
│   │   │   │   └── index.md
│   │   │   ├── exclusion
│   │   │   │   ├── 0.md
│   │   │   │   ├── 00.md
│   │   │   │   ├── 000.md
│   │   │   │   ├── 001.md
│   │   │   │   ├── 01.md
│   │   │   │   ├── 010.md
│   │   │   │   ├── 011.md
│   │   │   │   ├── 1.md
│   │   │   │   ├── 10.md
│   │   │   │   ├── 100.md
│   │   │   │   ├── 101.md
│   │   │   │   ├── 11.md
│   │   │   │   ├── 110.md
│   │   │   │   ├── 111.md
│   │   │   │   ├── excluded.md
│   │   │   │   ├── index.md
│   │   │   │   └── untitled.md
│   │   │   ├── index.md
│   │   │   └── order
│   │   │       ├── default
│   │   │       │   ├── 10.md
│   │   │       │   ├── 2.md
│   │   │       │   ├── a.md
│   │   │       │   ├── aa-lower.md
│   │   │       │   ├── aa.md
│   │   │       │   └── index.md
│   │   │       ├── floats
│   │   │       │   ├── -1.1.md
│   │   │       │   ├── 0.0.md
│   │   │       │   ├── 10.0.md
│   │   │       │   ├── 2.2222.md
│   │   │       │   └── index.md
│   │   │       ├── index.md
│   │   │       ├── integers
│   │   │       │   ├── -1.md
│   │   │       │   ├── 0.md
│   │   │       │   ├── 10.md
│   │   │       │   ├── 2.md
│   │   │       │   └── index.md
│   │   │       ├── mixture
│   │   │       │   ├── -1.1.md
│   │   │       │   ├── -1.md
│   │   │       │   ├── 0.0.md
│   │   │       │   ├── 0.md
│   │   │       │   ├── 10.0.md
│   │   │       │   ├── 10.md
│   │   │       │   ├── 2.2222.md
│   │   │       │   ├── 2.md
│   │   │       │   ├── a.md
│   │   │       │   ├── aa-lower.md
│   │   │       │   ├── aa.md
│   │   │       │   └── index.md
│   │   │       ├── order.md
│   │   │       └── strings
│   │   │           ├── 10.md
│   │   │           ├── 2.md
│   │   │           ├── a.md
│   │   │           ├── aa-lower.md
│   │   │           ├── aa.md
│   │   │           └── index.md
│   │   └── styling
│   │       ├── dl.md
│   │       ├── index.md
│   │       ├── ol.md
│   │       └── ul.md
│   ├── ui-components
│   │   ├── buttons.md
│   │   ├── code.md
│   │   ├── labels.md
│   │   ├── line-nos.md
│   │   ├── lists.md
│   │   ├── tables.md
│   │   ├── typography.md
│   │   └── ui-components.md
│   └── utilities
│       ├── color.md
│       ├── layout.md
│       ├── responsive-modifiers.md
│       ├── typography.md
│       └── utilities.md
├── favicon.ico
├── index.md
├── just-the-docs.gemspec
├── lib
│   └── tasks
│       └── search.rake
├── package-lock.json
├── package.json
└── script
    └── build

62 directories, 197 files
```
