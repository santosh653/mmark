# Figures in mmark

A `<figure>` encapsulates source code (v3) or artwork (v2/v3).
We allow multiple artworks/sources in a figure, but there is
currently no syntax to allow for this. Although Scholary Markdown
uses the following, start a axt section, with the special name
`Figure:` and in that section include the figure you want.

A source code or artwork is wrapped in a figure if a caption
is given.

An image is wrapped in a figure is the optional title is used.

## Proposal

*   A Fenced Code Block will becomes a source code in v3 and artwork in v2.
    We can use the language to signal the type.

        ``` c
        printf("%s\n", "hello");
        ```

*   An Indented Code Block becomes artwork in v3 and artwork in v2. The only way
    to indicate the type is by using an IAL. So one has to use:

        {type="ascii-art"}
            +-----+
            | ART |
            +-----+

    v3 allows the usage of a src= attribute to link to external files with images.
    In this proposal we use the image syntax for that.

*   An image `![Alt text](/path/to/img.jpg "Optional title")`, will be converted
    to an artwork with a `src` attribute in v3. The extension of the included
    image file will be used to generate the `type` attribute (see the table below).

    If the "Optional title" is specified the generated artwork will be wrapped in a
    figure with name set to "Optional title"

    extension | type
    ----------|-----
      txt     | ascii-art
      hex     | hex-dump
      ...     | ....

    Creating an artwork with an anchor will become:

        {#fig:id}
        ![](/path/to/art.txt "Optional title")

    For v2 this presents difficulties as there is no way to display any of this. The v2
    renderer will output a warning and not output anything in this case.

*   Grouping artworks and code blocks into figures. Scholary markdown has a neat syntax
    for this. It uses a special section syntax and all images in that section become
    subfigures of a larger figure. Disadvantage of this syntax is that it can not be
    used in lists. Hence we propose a quote like solution, just as asides and notes,
    but for figures:

    Basic usage:

    F>  {type="ascii-art"}
    F>      +-----+
    F>      | ART |
    F>      +-----+
    F>  Figure: This caption is ignored in v3, but used in v2.
    F>
    F>  ``` c
    F>  printf("%s\n", "hello");
    F>  ```
    F>
    Figure: Caption you will see, for both figures in v3 (in v2 it's ignored).

    In v2 this is not supported so the above will result in two figures. Note that in v2
    the inner captions *are* used, but the other one will be discarded as there is no
    way to typeset it.