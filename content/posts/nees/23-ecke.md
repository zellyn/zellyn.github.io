---
title: "23 ecke - Georg Nees"
subtitle: "More searching for seeds"
slug: "23-ecke"
date: 2024-07-08T21:00:00-04:00
tags: ['genart', 'plotter', 'nees']
---

Having [replicated]({{< relref "schotter-2.md" >}}#victory) Georg
Nees' iconic 1968/69 work, "Schotter", I turned my attention to his
"23 ecke" (literally "23 corners", usually referred to as "Polygons of
23 vertices"), an earlier work from 1964:

## Background

![An array of small, tangled shapes, fourteen across and twenty
high. The background is white, and each shape is drawn in pen by a
plotter. The shapes consist of 23 line segments, 22 of them either
vertical or horizontal, and one at some kind of
diagonal.](/img/2024/nees/23-ecke-rot19.png)

<center><small>

_23 ecke ("Polygon of 23 Vertices") --- Georg Nees, 1964<br>
(Version published in_ rot 19 _in 1965)_

</small></center>
<br>

From the 5<sup>th</sup> to the 19<sup>th</sup> of February, 1965, the
world's first exhibition of digital art was staged in the
Studiengalerie of the University of Stuttgart. Georg Nees displayed a
dozen or so drawings on the walls, and in the accompanying nineteenth
edition of Max Bense's booklet series _rot ("red")_, Nees provided
short descriptions in his usual clear style. _23 ecke_ was one of the
works displayed, and described in [_rot
19_](https://monoskop.org/images/b/b1/Rot_19_Computer-Grafik_1968.pdf).

My description of the 1965 show is drawn from Frieder Nake's short
paper, _Computer Art. A Personal Recollection.[^1]_ I recommend it: his
account of the reactions of other artists and musings on "artificial
art" (as Bense coined it) are fascinating.

[^1]: "Computer Art. A Personal Recollection, by Frieder Nake"
[doi:10.1145/1056224.1056234](https://doi.org/10.1145/1056224.1056234)
For easiest access, paste the DOI into Sci-Hub. If that interestes
you, check out his 2019 paper ["Georg Nees & Harold Cohen: Re:tracing
the origins of digital
media"](https://mediarep.org/server/api/core/bitstreams/f7017d56-9284-49d2-852a-9b9aa3885a30/content).

> The pure visual appearance of the drawings was not too exciting. One
> could easily ignore them, if one wanted to. The challenge really
> came from the principle that emerged. It was outrageous. It may have
> appeared as if conceptual art was left way behind but, at the same
> time, its idea gained a new kind of power. The schema, the
> structure, the class properties of entire sets of drawings were
> described and given to the machine for it to work out the
> details. The individual work became part only of a generative
> schema. Aesthetics was confronted by a new task, and the artist was
> threatened in his or her very existence. The _individual_
> realization on the wall no longer carried the essence of art. Art
> now had become the _class_ of works which the picture belonged
> to. That class consisted most likely of endless chains of
> variations.  --- Frieder Nake

Nake refers to "issue no. 19 of the now legendary _edition rot_", and
indeed, the booklet series--especially _rot 19_---seems to come up
often when reading about digital art. The _center of excellence
digital art_---comp**art**---[describes rot
19](http://dada.compart-bremen.de/item/publication/362):

> This little booklet of 14 pages is one of the first publications
> ever on computer art. It appeared at the occasion of the first
> exhibition of computer-generated algorithmic art world-wide: the
> famous show of a small set of graphic works by Georg Nees. The show
> was opened on February 4, 1965, and lasted until February 19, 1965,
> on the premises of the Studiengalerie of TH Stuttgart (now
> University of Stuttgart).

For more information on the _rot_ journal, ZKM has [an
exhibit](https://zkm.de/en/exhibition/2024/07/reading-rot-again).

## First attempt

At first I thought reproducing _23 ecke_ was going to be easy, using
the experience and resources gained reproducing _schotter_. In
sections
[3.2.2](https://archive.org/details/generative_computergraphik/generative_computergraphik__georg_nees__1969_thesis/page/188/mode/2up)
and
[3.2.3](https://archive.org/details/generative_computergraphik/generative_computergraphik__georg_nees__1969_thesis/page/194/mode/2up)
of his 1969 thesis, Nees lists the routine, "ELIRR" (ELementaren
IRRweg --- "elementary random path"[^2]):

[^2]: The most common
[translation](https://www.collinsdictionary.com/us/dictionary/german-english/irrweg)
of "irrweg" appears to be the figurative "wrong path"---as in being on
the wrong track, or leaving the straight and narrow---or a wandering
or tortuous path. I find the idea of a computer procedure designed to
be called repeatedly in a series, named _elementary wrong path_,
delightful.

```
1   'PROCEDURE'ELIRR.,
2   'BEGIN'
3   JA1.=P+JLI.,JE1.=P+JRE.,
4   JA2.=Q+JUN.,JE2.=Q+JOB.,
5   P1.=J1.,Q1.=J2.,
6   LEER(P1.Q1).,
7   'FOR'M.=1'STEP'1'UNTIL'N'DO'
8   'IF'T'EQUAL'0'THEN'
9   'BEGIN'
10  LINE(J1,Y).,LINE(X,J2)
11  'END'
12  'ELSE'LINE(J1,J2).,
13  'IF'T'EQUAL'0'THEN'
14  'BEGIN'
15  LINE(P1,Y).,LINE(X,Q1)
16  'END'
17  'ELSE'LINE(P1,Q1)
18  'END'ELIRR.,
```

> 3.2.2.
>
> Now that we have the multi-purpose procedure The elementary random
> walk SERIES, we standardize the figure-drawing procedure that has to
> replace the formal parameter FIGURE in SERIE. The parameterless
> procedure ELIRR draws a random walk in a frame rectangle whose
> corner point coordinates match the interval limits of the random
> generators J1 and J2. The random walk controlled by J1 and J2
> consists of diagonal or axis-parallel sections, depending on whether
> the integer and global size T agreed in line 5 of the declaration
> sequence (1/3.2.1) in ELIRR has the value 1 or 0.

We can transliterate this into ALGOL-ish Python:

```python
import drawsvg as draw

def draw_figures(QUER, HOCH, XMAL, YMAL, inset, N, S1, S2, diag=False,
                  stroke_width=0.3, svg_width=160, svg_height=200):
    JLI = inset / 2
    JRE = QUER - inset / 2
    JUN = inset / 2
    JOB = HOCH - inset / 2
    x = y = 0

    r1 = Random(S1)
    r2 = Random(S2)

    d = draw.Drawing(svg_width, svg_height, origin='center',
                     style="background-color:#eae6e2")
    g = draw.Group(transform="rotate(180)", stroke='#41403a',
                   stroke_width=f'{stroke_width}', fill='none',
                   stroke_linecap="round", stroke_linejoin="round")

    def serie(QUER, HOCH, XMAL, YMAL, FIGUR):
      P = -QUER * XMAL * 0.5
      Q = YANF = -HOCH * YMAL * 0.5

      for COUNTX in range(1, XMAL+1):
        Q = YANF
        for COUNTY in range(1, YMAL+1):
          p = draw.Path()
          FIGUR(P, Q, p)
          g.append(p)
          Q = Q + HOCH
        P = P + QUER

    def elirr(P, Q, p):
        JA1 = P + JLI
        JE1 = P + JRE
        JA2 = Q + JUN
        JE2 = Q + JOB
        P1 = r1.next(JA1, JE1)
        Q1 = r2.next(JA2, JE2)
        p.m(Q1,P1)
        X, Y = P1, Q1
        for M in range(1, N+1):
            X = r1.next(JA1, JE1)
            p.L(Y, X)
            Y = r2.next(JA2, JE2)
            p.L(Y,X)
        if diag:
            p.L(Q1,P1)
        else:
            p.L(Y,P1)
            X = P1
            p.L(Q1,X)

    serie(QUER, HOCH, XMAL, YMAL, elirr)

    d.append(g)
    return d

svg = draw_figures(20, 20, 8, 6, 0, 20, JS2, JS5, stroke_width=0.3)
svg.set_render_size(w=500)
```

The resulting images (reproduced using seeds [JS2 and
JS5](https://archive.org/details/generative_computergraphik/generative_computergraphik__georg_nees__1969_thesis/page/122/mode/1up)
from the thesis) look similar to those in "23 ecke", except that the
patterns here are a little more tangled, and lack the single diagonal
line:

![An array, six across and eight high, of larger shapes. Each consists
of 40 line segments, all either horizontal or vertical, and traces out
a tangled loop.](/img/2024/nees/bild-22.png) <center><small>

Bild 22 (Picture 22) _from Georg Nees' 1969 thesis_

</small></center>

<br/>

## Second attempt

My next step was to start trying to trace out the shapes from _23 ecke_, in an attempt to iterate through seeds and find matching shapes:

![Detail from an excalidraw image where I was trying to trace out
shapes from "23 ecke". There are some enlarged (and thus
blurry/pixelated) shapes with little arrows adjacent to each segment,
where I was trying to guess which order and direction they were drawn
in. Orange arrows sweep over from the enlarged shapes to their
position within "23 ecke". The shapes are numbered, and have strings
of letters like "wnesenwswneswnenws?nwnes" beside them, giving the
cardinal directions for line segment
directions.](/img/2024/nees/23-ecke-nsew.png)

This didn't work: the shapes were too blurry, and have too much
overlap, and despite spending a *lot* of time on it, I never did feel
sure that I had correctly deciphered and transcribed any of them. The
long search far and wide for ever higher resolution images of _23
ecke_ was useful though: I found many of the resources mentioned
in this post.

By this time, I had also noticed that the _23 ecke_ image is sometimes
shown with 14x19 shapes (as in _rot 19_), and sometimes 14x20, and
also that its orientation is unclear: from the thesis, it seems that
Nees' plotter drew on paper in what we would consider "landscape mode"
but the work is often shown oriented vertically, so I wasn't even sure
in which order the shapes were drawn.

The missing row was explained in a paper called ["A Vision without a
Sight: From Max Bense's Theory to the Dialectic of Programmed Images",
by Gaëtan Robillard and Prof. Alain
Lioret](http://www.generativeart.com/GA2019_web/22_Ga%C3%ABtan%20Robillard_Alain%20Lioret_paper_168x240.pdf),
from the [website of the XXII Generative Art Conference in
2019](https://generativeart.com/GA2019_web/Generative%20Art%202019%20index.htm):
it turns out that Max Bense had edited the original image Nees gave
him when printing _rot 19_, removing one row, and also Nees' name:

![Detail from a photograph of the marked-up printer's draft of "23
ecke" used in _rot 19_: Nees name beside the image is struck through,
and the last row if the image is x'ed out and struck through multiple
times.](/img/2024/nees/23-ecke-rot19-draft.png)<center><small>

Fig2. Georg Nees, 23-Edge: detailed view from editor’s draft, Rot n°
19, 1965. From the Walther-Bense Estate archive at the Center for Art
and Media Karlsruhe (ZKM)

</small></center>
<br>

I recommend reading that section of the paper, since M. Robillard's
comments and questions about Bense's editing of the image are
thought-provoking.

> ...it is remarkable that this simple gesture of framing witnesses
> Bense's relation to authorship and to the new category of programmed
> images. ...

I find it significant that Bense removed the human touch, and human
attribution, of Nees' name. It seems to fit his presentation of
"artificial art".

I have emailed the curators of [the Elisabeth Walther-Bense Archives
at ZKM](https://zkm.de/en/elisabeth-walther-bense) seeking to get a
full image of the printer's draft that I can post online, but haven't
heard back yet.

Luckily, searching at length for a higher-resolution image of _23
ecke_, I looked more closely at the [(20x14)
version](https://archive.org/details/generative_computergraphik/generative_computergraphik__georg_nees__2006_reprint/page/XII/mode/1up)
in Nees' preface to the 2006 reprint of his thesis, and noticed that
the figure was captioned, "Abb. 4: Georg Nees: Variationen von Figuren
in der statistischen Grafik. In: Grundlagenstudien aus Kybernetik und
Geisteswissenschaft 5 (1964), H. 3/4, S. 121-125."

Happily, that 1964 publication is [available at
archive.org](https://archive.org/details/grkg-1960-bd.-1-h-1-h-4/1964/GRKG-1964-Bd5-H3%264/page/123/mode/2up),
and even more happily, also [contains a description of the
algorithm](https://archive.org/details/grkg-1960-bd.-1-h-1-h-4/1964/GRKG-1964-Bd5-H3%264/page/122/mode/1up).

Note that rather than two separate random generators, one for the X
coordinates and one for the Y, this description uses just one. Perhaps
in 1964, Nees had not yet settled as firmly on the approach laid out
in his thesis. I now knew that I would probably need to try a few
different variations.

Having failed to find the seeds by attempting to correctly annotate
movements within shapes, I decided to focus instead on the
characteristics of the diagonal lines. By happenstance, I ran across
[Marimo](https://marimo.io/), a version of Python notebooks that
allows easy use of interactive widgets, and was able to whip up a
crude interface and manually classify as many of the 280 diagonals as
possible.

![Image of a crude UI created in Marimo, showing two images of the
same small sub-shape from _23 ecke_. To the right, they are annotated
"Comments for (17,2)" and "/ wide short up". Above, there are sliders
for picking horizontal and vertical sub-images. Below are buttons for
moving to the previous or next sub-shape, and for marking the diagonal
as "/", "\", as well as narrow, wide, short, tall, and up or
down.](/img/2024/nees/23-ecke-classify.png)

Even that wasn't quite enough. After failing to match the images with
any of the two billion available seeds, I started trying
variations. Apparently, in 1964 Nees was using random numbers modulo
2<sup>30</sup>, rather than 2<sup>31</sup>. Eventually, I had a hit!

## Success!

And with that, we have the original image! The seed is 314,748,365,
the modulo is 2<sup>30</sup>, and one random generator is used for
both X and Y coordinates. Otherwise the code is as described in ELIRR.
(My source code is available [on
github](https://github.com/zellyn/genart/tree/main/research/23-ecke),
which will even render the [jupyter
notebook](https://github.com/zellyn/genart/blob/main/research/23-ecke/23-ecke.ipynb)
nicely.)

![SVG recreation of 23 ecke, finally!](/img/2024/nees/23-ecke.svg)<center><small>

Recreation of _23 ecke_

</small></center>
<br>

## Final notes, a request, and #followfriday

### The real plots are tiny

One fascinating aspect of these images, especially _Schotter_, is just
how _small_ they are. Accustomed as I am to seeing artwork on my
computer screen, I assumed Schotter was a foot or two wide. In fact,
the area Georg Nees had available to him on his plotter was more like
28-30cm (about a foot) on the long edge. These pictures are tiny!

### An unexpected email

I was delighted and surprised to receive an email from one of Georg
Nees' sons! Apparently a colleague sent him a link to my blog posts on
_Schotter_. It was gratifying to hear that they were excited about the
reconstruction of his code and images.

### Any leads on Vera Molnár source code?

Next, I want to try reproducing early images of another computer
artwork pioneer, [Vera Molnár](http://www.veramolnar.com/). If anyone
has leads on source code for any of her artwork, (or lives close
enough to her estate to visit her journals) I'd love have a go at
modernizing the code and looking for seeds.

### Random artist shoutout: Murilo Polese

While looking for information on Vera Molnár, I ran across [the Vera
Molnár
page](https://murilopolese.github.io/RTP_SFPC_2021/research/W01_Vera_Molnar.html)
in the research section on the website for the [Recreating the
Past](https://sfpc.study/projects/rtp2021) course at the intriguing
[School for Poetic Computation](https://sfpc.study/) in New York City.

From there, I found the recent leader of that course, Murilo Polese,
an astonishing dynamo of creative activity. If you'd like to get
inspired by a current computer art practitioner as well as by the
early artists, you could certainly lose an afternoon exploring their
many projects:

* [murilopolese.com](http://www.murilopolese.com/about.html)
* [bananabanana.me gallery](http://gallery.bananabanana.me/)
* [bananabanana.me random toys and projects](http://www.bananabanana.me/)
* [amazing color programming toy on bananabanana.me](http://colorcode2.bananabanana.me/)
* [@murilove@sunbeam.city on Mastodon](https://sunbeam.city/@murilove)
  (fka [@murilopolese on
  twitter](https://twitter.com/murilopolese))

## Ciao!

That's it for now. Please get in touch ---
[@zellyn@hachyderm.io](https://hachyderm.io/@zellyn) --- with any
comments, requests, or just to chat.

---

* [Part one: Schotter: Introduction]({{< relref "schotter-1.md" >}})
* [Part two: Schotter: Investigation]({{< relref "schotter-2.md" >}})
* Part three: 23 ecke
