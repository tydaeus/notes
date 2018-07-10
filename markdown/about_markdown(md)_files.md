# About Markdown
I like to write documentation in the markdown (.md) format. This document can help you get started with using markdown.

## What is Markdown?
Markdown is a simple markup language that can be easily transpiled to html.

## Why Markdown?
* Files are stored and can be read in plaintext
    - Code snippets/examples do not get mangled like they do by Word
    - No need to install/run an editor/viewer more advanced than Notepad
    - Version control systems (e.g. SVN, git) work better with plaintext
    - Read/Edit within IDE or text editor
* Files can be processed into html
    - More advanced formatting than plaintext
    - Can view within web browser with plugin (no download and open required)
    - Pretty code formatting available for snippets/examples
    - Hyperlinks!
* Commonly Used Format
    - Github
    - Stack Overflow

## Reading Markdown Documents
It is possible, but slightly difficult, to read Markdown as plaintext. However, a Markdown viewer can make it much easier to read Markdown. Some Markdown viewing option(s) are listed below.

### Chrome's Markdown Viewer extension
1. Get the Markdown Viewer extension for Chrome https://chrome.google.com/webstore/detail/markdown-viewer/ckkdlimhmcjmikdlpkmbgfkaikojcbjk?hl=en-US
2. Navigate to Chrome settings->Extensions
3. Ensure Markdown Viewer is enabled
4. Check the box that says "Allow access to file URLs". This setting will allow you to use the plugin to display local files by dragging them into Chrome or entering their path in the url bar.
5. Click the "m" icon for the Markdown Viewer extension, then the "Advanced Options" button
6. Under "Allowed Origins", add any web domains you will want to have the plugin apply to, such as `https://svn`. This will allow you to use the plugin to format files viewed from the web.

## Converting Markdown to HTML
Many Markdown converters exist that will convert markdown to html for easier reading.

### markdown npm package
The `markdown` npm package provides nodejs functionality for converting Markdown to html, and can also provide a command-line utility to perform the conversion.

To install the command-line utility: `npm install -g markdown`.

To use the command-line utility: `md2html MyMarkdown.md > MyHtml.html`.

## Writing Markdown Documents
You can learn markdown syntax from http://daringfireball.net/projects/markdown/syntax.
Markdown can be written in plaintext, but it is easier if you use a markdown-aware editor. Some markdown-aware editing option(s) are listed below.

### Markdown Language Config in Notepad++
Here's one plugin for markdown config in Notepad++:

1. Get the latest version of [Notepad++](https://notepad-plus-plus.org/)
2. Install Notepad++
3. Download the [markdown_npp_zenburn](https://github.com/kylefarris/markdown_npp_zenburn) zip from github
4. From inside the extracted `markdown_npp_zenburn-master` folder default_theme, copy the `userDefineLang.xml` into your Notepad++ folder
5. Launch Notepad++
6. Click Language->Define Your Language->Import, navigate to your Notepad++ folder and select the `userDefineLang.xml` file
7. A prompt should say "Import Successful"
8. While still in the User Defined Language prompt, click User language: dropdown, select markdown, Save As, enter the name "markdown", click Ok.
9. To select markdown as the language you are using, click Language->markdown, check the bottom left to see if it worked, "User Define File - markdown"
10. Congrats! Notepad++ now knows how to display markdown for editing!

## Known Issues
* Different sites and different plugins use different Markdown renderers, which have slight variations on how Markdown gets translated to HTML
