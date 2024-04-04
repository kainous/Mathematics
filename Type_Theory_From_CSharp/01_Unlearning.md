# Definitions
## Entity
An *entity* 



| Term | Everyday Speech | Programming | Philosophy | Mathematics |
|--|--|--|--|--|
| Entity | Something individual, distinct from its properties, attributes, and relations |
| Container | 

| Function |
| 



# Numbers and Sets
We falsely learned that integers ($\mathbb{Z}$) are a subset of rationals ($\mathbb{Q}$), that rationals are a subset of reals ($\mathbb{R}$), and that reals are a subset of complex numbers ($\mathbb{C}$). It's a great simplification, but it's an oversimplification that fails outright in some cases.

:warning:If you need to call this image wrong to understand what I'm saying, then call it wrong.

<div style="text-align:center"><img src="./images/venn_diagram_numbers.png"/></div>

Since we are going to learn about *Type Theory* and about types more generally, then you should start understanding natural numbers ($\mathbb{N}$), integers, rationals, reals, and any number system, not as being subsets of each other, but as objects that allow us to convert from one-to-another in manner which is faithful (loses no information) in one direction only.

If I were to claim that there is a way to convert from integers ($\mathbb{Z}$) to rational numbers ($\mathbb{Q}$), such that any integer '$n:\mathbb{Z}$' (meaning an value  named 'n' which is of the type of integers) can be made a rational by making the denominator 1 (i.e. $\frac{n}{1}$), then what I'm really saying is that there is exists a function with an input of type integers, and an output of type rationals, for any integer:

$$
f:\mathbb{Z}\to\mathbb{Q}
$$

Where the implementation of this **convertion function** is:

$$
f(n) = \frac{n}{1}
$$

In this, I've given the type of $f$ (its *declaration*) by its input and output (i.e. $\mathbb{Z}\to\mathbb{Q}$), and I've given it an implementation (its *defintiion*) by how it should behave. Without proof, I've asserted that this function $f(n)$ loses no information when it converts, and for now, I'll leave that as an intuition for the concept behind what it means to be *faithful*.

From now on, since we will be working with types, instead of with sets, I'd like any usage of the symbol for subset to take on the meaning of the existence of some faithful function from the input to the output, one that is not necessarily faithful in the opposite direction.

$$
\mathbb{Z}\subset\mathbb{Q}\implies\exists f:\mathbb{Z}\to\mathbb{Q}
$$

Reading this off by breaking it down:

|Expression|Meaning|
|--|--|
| $\mathbb{Z}\subset\mathbb{Q}$ | Integers ($\mathbb{Z}$), being a subset of rationals ($\mathbb{Q}$)
| $\implies$ | "implies that" or in this particular case "can be read that"
| $\exists f:\mathbb{Z}\to\mathbb{Q}$ | there exists a function with integer inputs and rational outputs

What that doesn't mean is that there is a function from *any* rational back to an integer.