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
We falsely learned that integers ($\mathbb{Z}$) are a subset of rationals ($\mathbb{Q}$), that rationals are a subset of reals ($\mathbb{R}$), and that reals are a subset of complex numbers ($\mathbb{C}$). It's a great simplification, but it's an oversimplification that fails outright in some cases. In fact, sets are an amazing tool, but they have been overused in explaining some structures.

:warning:If you need to call this image wrong to understand what I'm saying, then call it wrong.

:note: <div style="color:red">**This is not an original image, please replace.**</div>
<div style="text-align:center"><img src="./images/venn_diagram_numbers.png"/></div>

## What We Want to Mean by Subset
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
| $\exists f:\mathbb{Z}\to\mathbb{Q}$ | there exists a function (I'm naming '$f$') with integer inputs and rational outputs

What that doesn't mean is that there is a function from *any* rational back to an integer.

However, we lose abilities as we change number systems.

## Comparing Behaviors in Number Systems
I want *behavior* here to be both *operations* (like addition and multiplication) and *relations* (like equality or less-than). Keen mathematicians may say that "operations are relations" and they would be correct, but programmers may not use the word that way. Don't worry, we'll get the programmers thinking like this in the end.

As an introduction, consider the question: "Does negation, as an operation, make sense for unsigned integers (naturals)?"

```csharp
uint a = 3;
uint b = -a; // CS0266
```

If you look at the message from CS0266, it says "`Cannot implicitly convert type 'long' to 'uint'. An explicit conversion exists (are you missing a cast?)`".  This message may not make sense at first, but what you are seeing is that C# first attempts to convert the second line to `uint b = -((long)a)`, something the compiler calls *widening*. However, `long` cannot fit inside a `uint`, and so it gives CS0266. Keep in mind, it does this (implicitly), because it thinks you understand that `uint` can't be negative.

Reminder that the inability to convert from `uint` $\to$ `long` is not *faithful*, and that terminology is referred to in the compiler as *narrowing*.

So, you may think then that all operations we can do on naturals ($\mathbb{N}$) we can also do on integers ($\mathbb{Z}$), but you'd be wrong. We cannot do factorial on integers, because there is no definition for factorial on any negative integer.

Going to rationals, we have an even weirder difference. Both naturals and integers allow us to evaluate $0^0$ to $1$. However, we have the problem that, in rationals, this is entirely undefined.

$$
\left( \frac{0}{p} \right) ^ \frac{0}{q}
$$

We have a defined meaning for $0^0$ until moving to rationals. This should also lead you to understand that integers are not just a subset of rationals. We have tricks to resolve this, but **we have to know when our tricks work**. This observation is the key to why we will start using types instead of just sets.

If you have not been through Calculus, you can skip this next statement, but there is a proof that:

$$
\lim_{t\to 0} t^t = 1
$$

However, more generally, this is undefined and undefinable without thoroughly looking inside $f$ and $g$, even if we know that $f(t) \to 0$ and that $g(t) \to 0$:

$$
\lim_{t\to 0} f(t)^{g(t)}
$$

Likewise, division is complicated between these 3 systems:
* In Integers, we have both Euclidean Division and Cyclic Group Division
* In Naturals, we have one division, in which Euclidean and Cyclic Group Division converge
* In Rationals, we also have one division, but it's Rational Division and instead of a quotient-remainder pair, it gives a numerator-denominator pair`

I'll add that, Euclidean Division isn't even well-defined and consistent across implementations (see [Wikipedia:Modulo](https://en.wikipedia.org/wiki/Modulo))

Now, go back to when you were in elementary algebra class and ask yourself, why did they change division on me without explanation?

In C#, we often just ignore the remainder during natural number and integer division, and we don't get rationals to work with, we just get our approximation to reals (float).

```csharp
uint a = 10;
uint b = 6;
uint c = a / b; // --> 1
uint d = a % b; // --> 4
uint e = Math.DivRem(a, b); // --> (1, 4)

// Just don't even ask with regard to (%) on int

float x = 10.0;
float y = 4.0;
float z = x / y; // --> 2.5

```

Finally, consider the Reals ($\mathbb{R}$) vs Complex Numbers ($\mathbb{C}$). We may have been taught that $\mathbb{R}\subset\mathbb{C}$, but we were also taught that we could just convert it like this:

$$
x \mapsto (x, 0)
$$

Such that $(x, 0)$ means the complex number defined by the real-component as $x$, and the imaginary component as $0$. Next question is, what do we lose by converting to Complex Numbers, since addition, subtraction, multiplication, division, exponentiation, etc. all seem to work, and we get more range out of logarithms?

We lose ordering. Imagine the following code:

```csharp
using System.Numerics;

var a = new Complex(2, 3);
var b = new Complex(4, -5);

if (a < b) {  // CS0019
    Console.WriteLine("What have I done?");
}

```

For values of `float` or even `double` the line `if (a < b)` would be not come into question, but since `a` and `b` are both `Complex`, there is no definition for `<` and we get CS0019 telling us that.

This brings us to the major questions of *Type Theory* that we intend to address more rigorously, when is a behavior "compatible"?

Additionally, since you are a programmer, you may also be thinking about bit operations. Our `uint` and `int` has operations like `|` and `&` that are well-defined in C#, but in `float` and `double` they are not. In fact, mathematically, we can create structure that formalizes this too. We have to ignore naturals and integers and instead create structures that I'll call BitNaturals and BitIntegers. However, mathematically, they are sound.

## We Are Interested in Relations that Remain

# There are Multiple Logic Systems (Logical Pluralism)
:warning: The following information is provided with considerable snark, and not to be taken literally.

The history of mathematics is littered with the corpses of the non-believers, but then also sometimes the corpses of the believers killed by non-believers. To early mathematicians, nearly 2500 years ago, all numbers were rational, with no exceptions. They believed that the number $\sqrt{2}$ was able to be expressed by $\frac{p}{q}$, except that they thought they hadn't yet found the fraction, and for most purposes, $\frac{17}{12}$ was close enough for government work (literally).

Someone named Hippasus discovered that it can't be. His name, Greek for "Horse Whisperer" maybe, I don't know, sounds like "hip" "pspspsps" so maybe whispering instead to a cat. With such a proof, they thought he was a witch and that he could breathe underwater, but he failed to do that when forced, just as he failed to find the numerator and denominator for the rational $\sqrt{2}$.

We may argue that $0$ was *discovered*, but that's disingenious and what was discovered instead was a **notation**, not $0$ itself, so I'll skip that and move on.

The next big worthwhile discussion that led to people saying, "nuh-uh, you can't do that" was Gerolamo Cardano that just said, hey let me pretend these *imaginary* numbers exist, and it will help me do math on polynomials. When I'm done, I'll always put them back so they are real. The problem was that they took on a life of their own, and mathematicians realized that they'd been duped by their own intuition. Their brains continued to say "nuh-uh", but the logic worked out.

But we weren't yet ready to realize what was happening.

In the 1800s, a man that was very gaussy (from peritonitis), discovered that paper doesn't have to lay flat. All through history, up until that point, parallel lines didn't have Tinder and therefore couldn't meet. Suddenly, the paper was curvy and parallel lines learned to swipe left and right. We realized there was flat geometry, spherical geometric, and hyperbolic geometry, and that's when things began to click for mathematicians.

"If we can define it consistently, then we can use it"

No longer did they care if "it exists", instead they cared only if they could make it consistent.

But as George Boole was finalizing boolean logic, he and Augustus DeMorgan were using it to understand how to put things in order. Boole realized that an implication is only the assertion that "true" is "more true" than "false" (i.e. $T > F$). Furthermore, the two of them realized that implication ($p \implies q$), such as saying that "when it rains (p), the ground gets wet (q)", is just an assertion that $p$ is at least as true as $q$ ($p \leq q$).

They studied examples where things are put in order, and what that meant. They realized a logical argument is just structuring statements into an order, one not unlike the strange tree-order your generations of your family get put in.

They weren't ready yet for fuzzy logic, but they were thinking of 3-valued logic, where something was unknown. Others were thinking about modal logics, which were necessary and possible. Still others Russelled sets into an order and realized that sets themselves share Boole's operators.

At this time, not everyone shared the belief that consistent definitions make for a structure that could be used. A German man from Mississippi, Kronecker, had the "show me" attitude that he called **constructivism**. He believed in only one infinity: the infinity of the natural numbers, making him the first **finitist**.

Then there was the Church that had developed the holy language of Lambda. Church went Turing with this language and it began to Kleene the Heytingists. From then on, the Heytingists were lost in their constructivism, and opened a Pandora's Box for computation. Structures and spaces formed from the rifts, donuts and coffee cups became indistinguishable, and coconuts went nuts.

All of this is to say that, no longer is there just *logic*. There's Deontic Logic, Alethic Logic, Fuzzy Logic, Paraconsistent Logic, Probabilities, Sets, Types, and Topoi, and the prophets tell us that we shall know them by their categories. These people founded a pluralistic society of logic systems.

Ultimately, our computers, like us, became ultrafinitists. Our software became constructivists. It's not that they don't believe in the *Law of the Excluded Middle*, it's that they don't not believe in it.


# Equivalence
Substitutability