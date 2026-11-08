---
tags:
- math
MOC: Sciences
---
[[_0000 Home|Home]] | [[_0004 Sciences MOC|Back to Sciences MOC]] | [[SC Mathematics index|Back to index]]

![[Pasted image 20261101232635.png]]
A quick-reference guide, with examples, on how to add math notation to Markdown documents.

The scope of mathematical notation included in this cheat sheet is drawn from the [Math Notation Cheat Sheet](https://www.flickr.com/photos/95869671@N08/40544016221/in/dateposted-public/) poster, created by [Dominic Walliman](https://dominicwalliman.com/) , included here with permission. The associated YouTube video, which is excellent, is [The Map of Mathematics](https://youtu.be/OmJ-4B-mS-Y) .

## Including Math Notation in Markdown

There are two ways to include math notation in Markdown. First, `inline`, which means that the notation is included in the paragraph or sentence, with the flow of text.

The second is as separate `code blocks`, so that the notation is shown in it’s own paragraph.

Inline math notation is wrapped in single-dollar signs. For example, for `the square of "x"`, just type `$x^2$`, which is then formatted as $x^2$. This is LaTeX\LaTeX notation.

Alternatively, `code blocks` of LaTeX\LaTeX begin and end with two dollar signs, wrapped inside triple backticks. For example…

```latex
$$
\displaystyle\sum_{k=3}^5 k^2=3^2 + 4^2 + 5^2 =50
$$
```

The above is rendered as:

$$
\displaystyle\sum_{k=3}^5 k^2=3^2 + 4^2 + 5^2 =50
$$

## LaTeX Cheat Sheet

> Tip: These tables are wide, so you may need to scroll horizontally to see all the columns, or rotate your phone to landscape.

### Arithmetic

| Notation                         | Example                                                           | Inline                                                               | Code Block                        |
| -------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------- | --------------------------------- |
| Addition                         | a+ba+b                                                            | `$a+b$`                                                              | `$$`  <br>`a+b`  <br>`$$`         |
| Subtraction                      | a−ba-b                                                            | `$a-b$`                                                              | `$$`  <br>`a-b`  <br>`$$`         |
| Various Forms of Multiplication  | a×ba \times b  <br>a∗ba \ast b  <br>a⋅ba \cdot b                  | `$a \times b$`  <br>`$a \ast b$`  <br>`$a \cdot b$`                  | `$$`  <br>`a \cdot b`  <br>`$$`   |
| Various Forms of Division        | a ⁣:ba \colon b  <br>a/ba / b  <br>a÷ba \div b  <br>ab\frac{a}{b} | `$a \colon b$`  <br>`$a / b$`  <br>`$a \div b$`  <br>`$\frac{a}{b}$` | `$$`  <br>`a \div b`  <br>`$$`    |
| Remainder / Modulo               | 5mod  2=15 \mod 2 = 1                                             | `$5\mod 2=1$`                                                        | `$$`  <br>`5\mod 2=1`  <br>`$$`   |
| Negative Value                   | −a-a                                                              | `$-a$`                                                               | `$$`  <br>`-a`  <br>`$$`          |
| Plus or Minus, Minus or Plus     | ±a\pm a  <br>∓a\mp a                                              | `$\pm a$`  <br>`$\mp a$`                                             | `$$`  <br>`\pm a`  <br>`$$`       |
| Squared, Cubed, nth-Power        | a2a^2  <br>a3a^3  <br>ana^n                                       | `$a^2$`  <br>`$a^3$`  <br>`$a^n$`                                    | `$$`  <br>`a^3`  <br>`$$`         |
| Square Root, Cube Root, nth-Root | a\sqrt{a}  <br>a3\sqrt[3]{a}  <br>an\sqrt[n]{a}                   | `$\sqrt{a}$`  <br>`$\sqrt[3]{a}$`  <br>`$\sqrt[n]{a}$`               | `$$`  <br>`\sqrt[3]{a}`  <br>`$$` |

### Equality

| Notation                  | Example                                | Inline                      | Code Block                                    |
| ------------------------- | -------------------------------------- | --------------------------- | --------------------------------------------- |
| Equals                    | 3=1+23=1+2                             | `$3=1+2$`                   | `$$`  <br>`3=1+2`  <br>`$$`                   |
| Not Equals                | 3≠43 \neq 4                            | `$3\neq4$`                  | `$$`  <br>`3\neq4`  <br>`$$`                  |
| Identical / Equivalent To | a≡ba \equiv b                          | `$a \equiv b$`              | `$$`  <br>`a \equiv b`  <br>`$$`              |
| Proportional To           | a∝ba \propto b                         | `$a \propto b$`             | `$$`  <br>`a \propto b`  <br>`$$`             |
| Approximately Equal To    | sin⁡(0.01)≈0.01\sin(0.01) \approx 0.01 | `$\sin(0.01) \approx 0.01$` | `$$`  <br>`\sin(0.01) \approx 0.01`  <br>`$$` |
### Comparison

| Notation                                                    | Example                      | Inline                         | Code Block                     |
| ----------------------------------------------------------- | ---------------------------- | ------------------------------ | ------------------------------ |
| a Less Than b  <br>a Greater Than b                         | a<ba < b  <br>a>ba > b       | `$a<b$`  <br>`$a>b$`           | `$$`  <br>`a<b`  <br>`$$`      |
| a Less Than or Equal To b  <br>a Greater Than or Equal To b | a≤ba \leq b  <br>a≥ba \geq b | `$a \leq b$`  <br>`$a \geq b$` | `$$`  <br>`a \leq b`  <br>`$$` |
| a Much Smaller Than b  <br>a Much Larger Than b             | a≪ba \ll b  <br>a≫ba \gg b   | `$a \ll b$`  <br>`$a \gg b$`   | `$$`  <br>`a \ll b`  <br>`$$`  |
### Algebra

| Notation               | Example                                                                              | Inline                                                               | Code Block                                                                          |
| ---------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Factorial              | 5!=5×4×3×2×15 ! = 5 \times 4 \times 3 \times 2 \times 1                              | `$5!=5 \times 4 \times 3 \times 2 \times 1$`                         | `$$`  <br>`5!=5 \times 4 \times 3 \times 2 \times 1`  <br>`$$`                      |
| Absolute Value         | ∣−5∣=5\| -5 \| = 5                                                                   | `$\|-5\|=5$`                                                         | `$$`  <br>`\|-5\|=5`  <br>`$$`                                                      |
| Function Of            | f(x)=2x2f(x) = 2x^2                                                                  | `$f(x)=2x^2$`                                                        | `$$`  <br>`f(x)=2x^2`  <br>`$$`                                                     |
| Change or Difference   | Δx=x1−x0\Delta x = x_1 - x_0                                                         | `$\Delta x = x_1 - x_0$`                                             | `$$`  <br>`\Delta x = x_1 - x_0`  <br>`$$`                                          |
| Pi                     | π=3.14159…\pi = 3.14159…                                                             | `$\pi = 3.14159...$`                                                 | `$$`  <br>`\pi`  <br>`$$`                                                           |
| Euler’s Constant       | e=2.71828…e = 2.71828…                                                               | `$e = 2.71828...$`                                                   | `$$`  <br>`e = 2.71828...`  <br>`$$`                                                |
| Sum                    | ∑k=35k2=32+42+52=50\displaystyle\sum_{k=3}^5 k^2 = 3^2 + 4^2 + 5^2 = 50              | `$\displaystyle\sum_{k=3}^5 k^2=3^2 + 4^2 + 5^2 =50$`                | `$$`  <br>`\displaystyle\sum_{k=3}^5 k^2=3^2 + 4^2 + 5^2 =50`  <br>`$$`             |
| Series Product         | ∏x=24x2=22×32×42=576\displaystyle\prod_{x=2}^4 x^2 = 2^2 \times 3^2 \times 4^2 = 576 | `$\displaystyle\prod_{x=2}^4 x^2 = 2^2 \times 3^2 \times 4^2 = 576$` | `$$`  <br>`\displaystyle\sum_{k=2}^4 k^2=2^2 \times 3^2 \times 4^2 = 576`  <br>`$$` |
| Brackets & Parentheses | […][\ldots] (…)(\ldots)                                                              | `$[\ldots] (\ldots)$`                                                | `$$`  <br>`[\ldots] (\ldots)`  <br>`$$`                                             |
### Angles

| Notation                 | Example                          | Inline                     | Code Block                                   |
| ------------------------ | -------------------------------- | -------------------------- | -------------------------------------------- |
| Angle                    | ∠\angle                          | `$\angle$`                 | `$$`  <br>`\angle`  <br>`$$`                 |
| Degree, Arc Min, Arc Sec | 30°45′30′′30\degree45\rq30\rq\rq | `$30\degree45\rq30\rq\rq$` | `$$`  <br>`30\degree45\rq30\rq\rq`  <br>`$$` |
| Radians                  | 360°=2πrad360\degree = 2\pi rad  | `$360\degree = 2\pi rad$`  | `$$`  <br>`360\degree = 2\pi rad`  <br>`$$`  |
### Probability & Statistics

| Notation                       | Example                                        | Inline                                     | Code Block                                                   |
| ------------------------------ | ---------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| Probability of Event A         | P(A)P(A) or Pr⁡(A)\Pr(A)                       | `$P(A)$ or $\Pr(A)$`                       | `$$`  <br>`P(A)`  <br>`$$`                                   |
| Intersection Prob. of A & B    | P(A∩B)P(A \cap B)                              | `$P(A \cap B)$`                            | `$$`  <br>`P(A \ca pB)`  <br>`$$`                            |
| Union Prob. of A or B          | P(A∪B)P(A \cup B)                              | `$P(A \cup B)$`                            | `$$`  <br>`P(A \cup B)`  <br>`$$`                            |
| Conditional Prob. of A Given B | P(A∣B)P(A \| B)                                | `$P(A\|B)$`                                | `$$`  <br>`P(A\|B)`  <br>`$$`                                |
| Median                         | x~\tilde{x}                                    | `$\tilde{x}$`                              | `$$`  <br>`\tilde{x}`  <br>`$$`                              |
| Population Mean                | μ,x‾,⟨x⟩\mu , \overline{x} , \langle x \rangle | `$\mu , \overline{x} , \langle x \rangle$` | `$$`  <br>`\mu , \overline{x} , \langle x \rangle`  <br>`$$` |
| Standard Deviation             | σ\sigma                                        | `$\sigma$`                                 | `$$`  <br>`\sigma`  <br>`$$`                                 |
| Varience                       | σ2\sigma^2                                     | `$\sigma^2$`                               | `$$`  <br>`\sigma^2`  <br>`$$`                               |
### Linear Algebra
#### Linear Algebra: Vectors

| Notation      | Example                                                                      | Inline                                                              | Code Block                                                                                          |
| ------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Vectors       | vv‾v⃗\mathbf{v} \overline{v} \vec{v}                                         | `$\mathbf{v}\overline{v}\vec{v}$`                                   | `$$`  <br>`\mathbf{v} \overline{v} \vec{v}`  <br>`$$`                                               |
| Row Vector    | v=(123)v = \begin{pmatrix} 1 & 2 & 3 \end{pmatrix}                           | `$v=\begin{pmatrix}1&2&3\end{pmatrix}$`                             | `$$`  <br>`v = \begin{pmatrix}`  <br>`1 & 2 & 3`  <br>`\end{pmatrix}`  <br>`$$`                     |
| Column Vector | w=(456)w = \begin{pmatrix} 4 \cr 5 \cr 6 \cr \end{pmatrix}                   | `$w=\begin{pmatrix}4\cr5\cr6\cr\end{pmatrix}$`                      | `$$`  <br>`w=\begin{pmatrix}`  <br>`4 \cr`  <br>`5 \cr`  <br>`6 \cr`  <br>`\end{pmatrix}`  <br>`$$` |
| Dot Product   | v⋅w\mathbf{v} \cdot \mathbf{w}  <br>(v,w)(v,w)  <br><v∣w>\left< v\|w \right> | `$\mathbf{v} \cdot \mathbf{w}$<br>$(v,w)$<br>$\left<v \| w\right>$` | `$$`  <br>`\mathbf{v}\cdot\mathbf{w}`  <br>`(v,w)`  <br>`\left<v\|w \right>`  <br>`$$`              |
| Cross Product | v×wv \times w                                                                | `$v \times w$`                                                      | `$$`  <br>`v \times w`  <br>`$$`                                                                    |
| Length of v   | ∣v∣\|v\|                                                                     | `$\|v\|$`                                                           | `$$`  <br>`\|v\|`  <br>`$$`                                                                         |
| Norm of v     | ∣∣v∣∣\|v\|                                                                   | `$\|v\|$`                                                           | `$$`  <br>`\|v\|`  <br>`$$`                                                                         |
#### Linear Algebra: Matrices

| Notation                                     | Example                                                           | Inline                                          | Code Block                                                                                                 |
| -------------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Matrix, 2 By 3                               | A=[123456]A=\begin{bmatrix} 1 & 2 & 3 \cr 4 & 5 & 6 \end{bmatrix} | `$A=\begin{bmatrix}1&2&3\cr4&5&6\end{bmatrix}$` | `$$`  <br>`A=`  <br>`\begin{bmatrix}`  <br>`1 & 2 & 3 \cr`  <br>`4 & 5 & 6`  <br>`\end{bmatrix}`  <br>`$$` |
| Product                                      | A⋅BA \cdot B                                                      | `$A \cdot B$`                                   | `$$`  <br>`A \cdot B`  <br>`$$`                                                                            |
| Hadamard Product                             | A∘BA \circ B                                                      | `$A \circ B$`                                   | `$$`  <br>`A \circ B`  <br>`$$`                                                                            |
| Kronecker Product                            | A⊗BA \otimes B                                                    | `$A \otimes B$`                                 | `$$`  <br>`A \otimes B`  <br>`$$`                                                                          |
| Transposed Matrix                            | ATA^T                                                             | `$A^T$`                                         | `$$`  <br>`A^T`  <br>`$$`                                                                                  |
| Hermitian Matrix or  <br>Conjugate Transpose | A†A^\dag  <br>A∗A^\ast                                            | `$A^\dag$`  <br>`$A^\ast$`                      | `$$`  <br>`A^\dag`  <br>`A^\ast`  <br>`$$`                                                                 |
| Inverse Matrix                               | A−1A^{-1}                                                         | `$A^{-1}$`                                      | `$$`  <br>`A^{-1}`  <br>`$$`                                                                               |
| Determinant                                  | ∣A∣\|A\|                                                          | `$\|A\|$`                                       | `$$`  <br>`\|A\|`  <br>`$$`                                                                                |
| Norm                                         | ∣∣A∣∣\|A\|                                                        | `$\|A\|$`                                       | `$$`  <br>`\|A\|`  <br>`$$`                                                                                |
### Calculus

| Notation                                              | Example                                 | Inline                            | Code Block                                          |
| ----------------------------------------------------- | --------------------------------------- | --------------------------------- | --------------------------------------------------- |
| Example Function:  <br>y=x24y = \frac{x^2}{4}         | y=x24y = \frac{x^2}{4}                  | `$y = \frac{x^2}{4}$`             | `$$`  <br>`y = \frac{x^2}{4}`  <br>`$$`             |
| Integration  <br>(Limits: 1 to 4)                     | A=∫14x2xdxA = \int_1^4 \frac{x^2}{x} dx | `$A = \int_1^4 \frac{x^2}{x} dx$` | `$$`  <br>`A = \int_1^4 \frac{x^2}{x} dx`  <br>`$$` |
| Differentiation                                       |                                         |                                   |                                                     |
| First Derivative  <br>With Respect To xx              | dfdx\frac{df}{dx}                       | `$\frac{df}{dx}$`                 | `$$`  <br>`\frac{df}{dx}`  <br>`$$`                 |
| Partial Derivative  <br>With Respect To xx            | ∂f∂x\frac{\partial f}{\partial x}       | `$\frac{\partial f}{\partial x}$` | `$$`  <br>`\frac{\partial f}{\partial x}`  <br>`$$` |
| First and Second Derivative  <br>of Function          | f′f\rq  <br>f′′f\rq\rq                  | `$f\rq$`  <br>`$f\rq\rq$`         | `$$`  <br>`f\rq`  <br>`f\rq\rq`  <br>`$$`           |
| First and Second Derivative  <br>With Respect To Time | f˙\dot f  <br>f¨\ddot f                 | `$\dot f$`  <br>`$\ddot f$`       | `$$`  <br>`\dot f`  <br>`\ddot f`  <br>`$$`         |
### Complex Numbers

| Notation                         | Example                                | Inline                                       | Block                                                        |
| -------------------------------- | -------------------------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| Imaginary Unit ii                | $z=3+2i$                               | `$z=3+2i$`                                   | `$$`  <br>`z=3+2i`  <br>`$$`                                 |
| Real Part Of Complex Number      | $\Re(z)=3$<br>$\operatorname{Re}(z)=3$ | `$\Re(z)=3$`  <br>`$\operatorname{Re}(z)=3$` | `$$`  <br>`\Re(z)=3`  <br>`\operatorname{Re}(z)=3`  <br>`$$` |
| Imaginary Part Of Complex Number | $\Im(z)=2$<br>$\operatorname{Im}(z)=2$ | `$\Im(z)=2$`  <br>`$\operatorname{Im}(z)=2$` | `$$`  <br>`\Im(z)=2`  <br>`\operatorname{Im}(z)=2`  <br>`$$` |
| Complex Conjugate                | $\bar{z}=z^*=3-2i$                     | `$\bar{z}=z^*=3-2i$`                         | `$$`  <br>`\bar{z}=z^*=3-2i`  <br>`$$`                       |
### Greek Alphabet

| Letter  | Lower | Inline       | Upper | Inline       |
| ------- | ----- | ------------ | ----- | ------------ |
| Alpha   | α     | `$\alpha$`   | A     | `$\Alpha$`   |
| Beta    | β     | `$\beta$`    | B     | `$\Beta$`    |
| Gamma   | γ     | `$\gamma$`   | Γ     | `$\Gamma$`   |
| Delta   | δ     | `$\delta$`   | Δ     | `$\Delta$`   |
| Epsilon | ϵ     | `$\epsilon$` | E     | `$\Epsilon$` |
| Zeta    | ζ     | `$\zeta$`    | Z     | `$\Zeta$`    |
| Eta     | η     | `$\eta$`     | H     | `$\Eta$`     |
| Theta   | θ     | `$\theta$`   | Θ     | `$\Theta$`   |
| Iota    | ι     | `$\iota$`    | I     | `$\Iota$`    |
| Kappa   | κ     | `$\kappa$`   | K     | `$\Kappa$`   |
| Lambda  | λ     | `$\lambda$`  | Λ     | `$\Lambda$`  |
| Mu      | μ     | `$\mu$`      | M     | `$\Mu$`      |
| Nu      | ν     | `$\nu$`      | N     | `$\Nu$`      |
| Xi      | ξ     | `$\xi$`      | Ξ     | `$\Xi$`      |
| Omicron | ο     | `$\omicron$` | O     | `$\Omicron$` |
| Pi      | π     | `$\pi$`      | Π     | `$\Pi$`      |
| Rho     | ρ     | `$\rho$`     | P     | `$\Rho$`     |
| Sigma   | σ     | `$\sigma$`   | Σ     | `$\Sigma$`   |
| Tau     | τ     | `$\tau$`     | T     | `$\Tau$`     |
| Upsilon | υ     | `$\upsilon$` | Υ     | `$\Upsilon$` |
| Phi     | ϕ     | `$\phi$`     | Φ     | `$\Phi$`     |
| Chi     | χ     | `$\chi$`     | X     | `$\Chi$`     |
| Psi     | ψ     | `$\psi$`     | Ψ     | `$\Psi$`     |
| Omega   | ω     | `$\omega$`   | Ω     | `$\Omega$`   |
