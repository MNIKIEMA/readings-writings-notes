## Base
Let :
- $d_v, d_k, n, T \in \mathcal{R}$
## Scaled Dot Product Attention
For $X \in \mathcal{R}^{T\times d}, Q, K  \in \mathcal{R}^{T\times d_k}, \text{and}  V \in \mathcal{R}^{T\times d_v}$ the attention is defined as follows:
$$Attention(Q, K, V) = \text{Softmax}(\frac{QK^{T}}{\sqrt{d_k}})V$$
## Self Attention
Self attention is defined as follows:
$$ Q = XW^{Q}$$
$$K = XW^{K}$$
$$V = XW^{V}$$
## Multi Head Attention
A multi head attention is a concatenation of many attention heads:
$$MHA(n) = [\text{Attention}(Q_1, K_1, V_1), ..., \text{Attention}(Q_n, K_n, V_n)]W^{O}$$
```typst
#import "@preview/fletcher:0.5.0" as fletcher: diagram, node, edge


#import fletcher.shapes: house, hexagon
#set page(width: auto, height: auto, margin: 5mm, fill: white)
#set text(font: "New Computer Modern")

#let blob(pos, label, tint: white, ..args) = node(
	pos, align(center, label),
	width: 28mm,
	fill: tint.lighten(60%),
	stroke: 1pt + tint.darken(20%),
	corner-radius: 5pt,
	..args,
)

#diagram(
	spacing: 8pt,
	cell-size: (8mm, 10mm),
	edge-stroke: 1pt,
	edge-corner-radius: 5pt,
	mark-scale: 70%,

	blob((0,1), [Add & Norm], tint: yellow, shape: hexagon),
	edge(),
	blob((0,2), [Multi-Head\ Attention], tint: orange),
	blob((0,4), [Input], shape: house.with(angle: 30deg),
		width: auto, tint: red),

	for x in (-.3, -.1, +.1, +.3) {
		edge((0,2.8), (x,2.8), (x,2), "-|>")
	},
	edge((0,2.8), (0,4)),

	edge((0,3), "l,uu,r", "--|>"),
	edge((0,1), (0, 0.35), "r", (1,3), "r,u", "-|>"),
	edge((1,2), "d,rr,uu,l", "--|>"),

	blob((2,0), [Softmax], tint: green),
	edge("<|-"),
	blob((2,1), [Add & Norm], tint: yellow, shape: hexagon),
	edge(),
	blob((2,2), [Feed\ Forward], tint: blue),
)
```

## The Encoder Layer

```typst
#import "@preview/fletcher:0.5.1" as fletcher: diagram, node, edge

#import "@preview/cetz:0.3.0": canvas, draw
#import fletcher.shapes: house, hexagon
#import fletcher.shapes: diamond
#import draw: line, content, rect, on-layer

#set page(width: auto, height: auto, margin: 8pt)


#canvas({
  rect(
    (-4, -5.5),
    (4, 5),
    frame: "rect",
    stroke: (dash: "dashed"),
    fill: none,
    width: 22em,
    height: 12em,
    name: "enclosure",
    radius: 5pt,
  )
  // Decoder enclosure  
  rect(
    (6, -5.5),
    (13, 9.5),
    stroke: (dash: "dashed", thickness: 1.5pt),
    fill: none,
    name: "decoder-box",
    width: 22em,
    height: 12em,
    radius: 5pt,
  )

  on-layer(
    2,
    content(
      "enclosure.south",
      text(weight: "bold", size: 1.2em)[Transformer Encoder],
      frame: "rect",
      stroke: .5pt,
      fill: white,
      padding: 3pt,
    ),
  )

  let box-style = (
    frame: "rect",
    stroke: .5pt,
    fill: rgb("#DCDCDC"),
    padding: 8pt,
    width: 18em,
  )

  let arrow-style = (
    mark: (end: "stealth", fill: black, scale: .5),
    stroke: 1pt,
  )

  content(
    (0, -4.7),
    [Input Tokens → Embedding],
    ..box-style,
    fill: rgb("#f0c674").lighten(40%),
    name: "input-embedding",
  )
  
  content(
    (rel: (0, 2.0), to: "input-embedding.south"),
    [Add Positional Encoding],
    ..box-style,
    fill: rgb("#f0c674").lighten(60%),
    name: "positional-encoding",
  )

  content(
    (rel: (0, 2.0), to: "positional-encoding.south"),
    [Multi-Head Self-Attention],
    ..box-style,
    fill: rgb("#54aef8").lighten(40%),
    name: "self-attention",
  )

  content(
    (rel: (0, 2.0), to: "self-attention.south"),
    [Add & Layer Norm],
    ..box-style,
    fill: rgb("#f8cecc"),
    name: "add-norm-1",
  )

  content(
    (rel: (0, 2.0), to: "add-norm-1.south"),
    [Feed-Forward Network (FFN)],
    ..box-style,
    fill: rgb("#c6aa0c").lighten(50%),
    name: "ffn",
  )

  content(
    (rel: (0, 2.0), to: "ffn.south"),
    [Add & Layer Norm],
    ..box-style,
    fill: rgb("#f8cecc"),
    name: "add-norm-2",
  )

  // content(
    //(rel: (0, 2.0), to: "add-norm-2.south"),
    //[Sofmax],
    //..box-style,
    //fill: rgb("#b5bd68").lighten(40%),
    //name: "output",
  //)

  // Decoder

  content((9.5, -4.8), [Inputs], ..box-style, name: "dec-inputs")
  content(
    (rel: (0, 1.5), to: "dec-inputs"),
    [Embedding +  PE],
    ..box-style,
    fill: rgb("#b5bd68").lighten(40%),
    name: "dec-out",
  )
  content(
    (rel: (0, 1.5), to: "dec-out"),
    [Masked-MHA],
    ..box-style,
    fill: rgb("#54aef8").lighten(40%),
    name: "mha1-out",
  )

  content(
    (rel: (0, 1.5), to: "mha1-out"),
    [Add & Layer Norm],
    ..box-style,
    fill: rgb("#f8cecc"),
    name: "add-norm-dec-1",
  )

  content(
    (rel: (0, 1.5), to: "add-norm-dec-1"),
    [Cross Attention],
    ..box-style,
    fill: rgb("#54aef8").lighten(40%),
    name: "cross-attn-out",
  )

  content(
    (rel: (0, 1.5), to: "cross-attn-out"),
    [Add & Layer Norm],
    ..box-style,
    name: "add-norm-dec-2",
  )

  content(
    (rel: (0, 1.5), to: "add-norm-dec-2"),
    [FFN],
    ..box-style,
    name: "ffn-dec-1",
  )

  content(
    (rel: (0, 1.5), to: "ffn-dec-1"),
    [Add & Layer Norm],
    ..box-style,
    fill: rgb("#f8cecc"),
    name: "add-norm-dec-3",
  )

  content(
    (rel: (0, 1.5), to: "add-norm-dec-3"),
    [Linear],
    ..box-style,
    fill: rgb("#E8F4FD"),
    name: "dec-linear",
  )

  content(
    (rel: (0, 1.5), to: "dec-linear"),
    [Sofmax],
    ..box-style,
    fill: rgb("#b5bd68").lighten(40%),
    name: "final-out",
  )


  // Draw arrows
  line("input-embedding", "positional-encoding", ..arrow-style)
  line("positional-encoding", "self-attention", ..arrow-style)
  line("self-attention", "add-norm-1", ..arrow-style)
  line("ffn", "add-norm-2", ..arrow-style)
  line("add-norm-1", "ffn", ..arrow-style)
  // line("add-norm-2", "output", ..arrow-style)

  // Decoder Arrow

  line("dec-inputs", "dec-out", ..arrow-style)
  line("dec-out", "mha1-out", ..arrow-style)
  line("mha1-out", "add-norm-dec-1", ..arrow-style)
  line("add-norm-dec-1", "cross-attn-out", ..arrow-style)
  line("cross-attn-out", "add-norm-dec-2", ..arrow-style)
  line("add-norm-dec-2", "ffn-dec-1", ..arrow-style)
  line( "ffn-dec-1", "add-norm-dec-3", ..arrow-style)
  line( "add-norm-dec-3", "dec-linear",  ..arrow-style)
  line( "dec-linear", "final-out",   ..arrow-style)
  // line( "add-norm-2", "cross-attn-out",   ..arrow-style)
  line("add-norm-2.north", (0, 6))
  line((0, 6), (4.3, 6))
  line((4.3, 6), (4.3, 1.2))
  line((4.3, 1.2), "cross-attn-out", ..arrow-style)

})

```

$X = Embedding(X) + PE(X)$
$X = X + MHA(X)$
$X = LayerNorm(X)$
$X = FFN(X)$
$X = LayerNorm(X)$

## The Decoder Layer
$X = Embedding(X) + PE(X)$
$X = MHA(X) + X$
$X = LayerNorm(X)$
$X = MHA(X) + X$
$X = LayerNorm(X)$
$X = X + FFN(X)$
$X = Linear(X)$
$X = Softmax(X)$
