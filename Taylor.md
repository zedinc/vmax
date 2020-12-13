Background/Preamble
---

It can be shown that the motion with respect to time of a vehicle which travels in a straight line, subject to aerodynamic drag, is governed by the following ODE:

$$m\ddot{x}+c\dot{x}^2-F=0$$

The thing that makes this tricky to solve analytically is the non-linear drag component, which is a function of the square of the velocity ($\dot{x}^2$).

At this stage, it makes sense to break something out from the numerical methods toolbox.

---

Hello, Mr. Taylor
---

One possible way to overcome the absence of an analytical solution is to approximate the first and second-order derivatives *à la* Taylor series expansion:

    $$\dot{x} \cong \frac{x_{i+1}-x_{i-1}}{2h} - \mathcal{O}(h^2)$$

    $$\ddot{x} \cong \frac{x_{i+1}-2x_{i}+x_{i-1}}{h^2} + \mathcal{O}(h^2)$$

where $h$ is the (constant time) difference between time instances $i-1$, $i$ and $i+1$

This is what the equation looks like after substituting these terms in:

    $$m\left( \frac{x_{i+1}-2x_{i}+x_{i-1}}{h^2} \right) + c\left( \frac{x_{i+1}-x_{i-1}}{2h} \right)\left( \frac{x_{i+1}-x_{i-1}}{2h} \right) - F = 0$$

---

My goal with this equation is to predict the position of the next time instance, $x_{i+1}$, since I will have the values for $x_{i}$ and $x_{i-1}$ already available. $x_0$ and $x_1$ can be estimated.

After some rearrangement, this jungle of a quadratic equation results:

    $$ \alpha \cdot x^2_{i+1} + \beta \cdot x_{i+1} + \gamma = 0$$

where
    $$\alpha = \frac{c}{4h^2}$$
    $$\beta = \frac{m}{h^2} - \frac{2cx_{i-1}}{4h^2} $$
    $$\gamma = \frac{m}{h^2} x_{i-1} + \frac{c}{4h^2} x^2_{i-1} -\frac{2m}{h^2} x_{i} - F $$

*Yuck.* Let's clean that up some more by utilizing a common denominator, $4h^2$.

    $$ c \cdot x^2_{i+1} + ( 4m - 2cx_{i-1} ) \cdot x_{i+1} + x_{i-1}(4m+cx_{i-1})-8mx_{i}-4Fh^2 = 0$$

*A bit better*.

I'm not too concerned about encountering imaginary roots (where $\beta^2 < 4\alpha\gamma$), since $x_{i+1} > x_{i}$ will always be true, $m >>> c$ and $F>0$.

I'm also expecting that one of the roots will be positive, the other negative. Since $x > 0$ will always be true, I expect that

    $$ x_{i+1} = \frac{-\beta + \sqrt{\beta^2-4\alpha\gamma}}{2\alpha} $$
