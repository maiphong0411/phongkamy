

## 🧠 Learning Note: Backpropagation

### What is Backpropagation?

Backpropagation is an algorithm used to **propagate errors backward** through a neural network to compute gradients with respect to model parameters.
In deep learning, we often cannot derive an exact function to describe our data, so we train a model to learn an **approximate function**. Backpropagation helps update parameters efficiently based on how much they contribute to the error.

---

### Numerical Gradient (Derivative Definition)

To understand gradients, imagine slightly changing a variable and seeing how the output changes:

1. Set a small value ( h = 0.0001 )
2. Compute ( f(x) )
3. Compute ( f(x + h) )
4. Estimate the derivative: ( (f(x + h) - f(x)) / h )

This method gives an intuitive **numerical gradient**, but backpropagation uses the **chain rule** to compute exact gradients efficiently.

---

### Manual Example

Let ( Y = a + b \times c ).
We can compute partial derivatives:

* ( \frac{\partial Y}{\partial a} = 1 )
* ( \frac{\partial Y}{\partial b} = c )
* ( \frac{\partial Y}{\partial c} = b )

Try setting values for ( a, b, c ) and verify your results numerically.

---

### Interview Questions

**Q: Why do we need to accumulate gradients for shared variables?**
Because a variable can influence the output through multiple paths. If we don’t accumulate, later gradients overwrite earlier ones.
Example:
If ( b = a + a ), the true gradient ( \frac{db}{da} = 2 ), but without accumulation we’d only get ( 1.0 ).

**Q: Why do we need to reset gradients before each backward pass?**
Since gradients accumulate by default, we must clear them (e.g., `optimizer.zero_grad()`) before the next backpropagation step, or the new gradients will add to the old ones.

---