@def author = "Qling Alter"
@def hasmath = false              <!-- mostly there's no maths on pages -->  

@def mintoclevel = 2              <!-- TOCS only for level h2 and higher -->
@def maxtoclevel = 3              <!-- TOCS only up to level 3 included -->

@def hasplotly = false
@def prepath = "Dynamic-Programming-Patterns"
@def generate_rss = true
@def website_title = "Qling's Blog"
@def website_descr = "Patterns for Dynamic Programming Questions"
@def website_url   = "https://rfhklwt.github.io/Dynamic-Programming-Patterns/"
<!--
Useful HTML snippets
* \blurb{...} for a blurb at the top of a page
* \refblank{...} for a link with target blank
* \lineskip forces skipping of a line somewhere
-->

\newcommand{\blurb}[1]{
    ~~~
    <span style="font-size:24px;font-weight:300;">!#1</span>
    ~~~
}
\newcommand{\refblank}[2]{
    ~~~
    <a href="!#2" target="_blank" rel="noopener noreferrer">#1</a> 
    ~~~
}
\newcommand{\lineskip}{@@blank@@}
\newcommand{\skipline}{\lineskip}

\newcommand{\esc}[2]{
    ```julia:esc__!#1
    #hideall
    using Markdown
    println("\`\`\`\`\`plaintext $(Markdown.htmlesc(raw"""#2""")) \`\`\`\`\`")
    ```
    \textoutput{esc__!#1}
}

\newcommand{\esch}[2]{
    ```julia:esc__!#1
    #hideall
    using Markdown
    println("\`\`\`\`\`html $(Markdown.htmlesc(raw"""#2""")) \`\`\`\`\`")
    ```
    \textoutput{esc__!#1}
}

\newcommand{\span}[2]{~~~<span style="display:inline-block;!#1">~~~!#2~~~</span>~~~}

\newcommand{\goto}[1]{~~~<a href="!#1" id="goto"><span id="check">&check;</span><span id="arrow"><b>&rarr;</b></span></a>~~~}

\newcommand{\smindent}[1]{\span{width:45px;text-align:right;color:slategray;}{#1}}
\newcommand{\smnote}[1]{\style{font-size:85%;line-height:0em;}{#1}}

<!-- Python code blocks -->
\newcommand{\pycode}[2]{
    ```julia:!#1
    #hideall
    using PyCall
    lines = replace("""!#2""", r"(^|\n)([^\n]+)\n?$" => s"\1res = \2")
    py"""
    $$lines
    """
    println(py"res")
    ```
    ```python
    #2
    ```
    resulting in:
    \codeoutput{!#1}
}

<!-- Julia code blocks -->
\newcommand{\jlcode}[2]{
    ```julia:!#1
    #2
    ```
    resulting in:
    \codeoutput{!#1}
}

\newcommand{\info}[1]{@@admonition @@admonition-title Info @@ @@content #1 @@ @@}

<!-- Hint -->
\newcommand{\hint}[1]{
    ~~~
    <div class="admonition hint">
        <div class="admonition-title">Hint</div>
        <div class="admonition-content">~~~!#1~~~</div>
    </div>
    ~~~
}

<!-- Question -->
\newcommand{\ques}[1]{
    ~~~
    <div class="admonition question">
        <div class="admonition-title">🙋 Question</div>
        <div class="admonition-content">~~~!#1~~~</div>
    </div>
    ~~~
}

<!-- Complexity -->
\newcommand{\comp}[1]{
    ~~~
    <div class="admonition complexity">
        <div class="admonition-title">👉 Complexity</div>
        <div class="admonition-content">~~~!#1~~~</div>
    </div>
    ~~~
}

<!-- Note -->
\newcommand{\note}[1]{
    ~~~
    <div class="admonition note">
        <div class="admonition-title">⚠ Note</div>
        <div class="admonition-content">~~~!#1~~~</div>
    </div>
    ~~~
}

<!-- Warning -->
\newcommand{\warn}[1]{
    ~~~
    <div class="admonition warning">
        <div class="admonition-title">Warning</div>
        <div class="admonition-content">~~~!#1~~~</div>
    </div>
    ~~~
}

