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

