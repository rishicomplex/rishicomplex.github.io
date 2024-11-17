---
layout: post
title:  "AlphaProof's Greatest Hits"
date:   2024-11-17 00:00:00 -0800
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$']],
    processEscapes: true
  }
});
</script>

Here I'll try to explain the coolest ideas in each of [AlphaProof](https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/)'s IMO 2024 solutions. AlphaProof produces proofs in [Lean](https://leanprover.github.io/), and each Lean proof is composed of a series of tactics. So I'll pick out the tactics that correspond to these ideas in the proofs for problems 1, 2 and 4.

<!--more-->

If you're not familiar with the problems already, I recommend reading [Evan Chen's solution notes](https://web.evanchen.cc/exams/IMO-2024-notes.pdf), or watching these [videos](https://youtube.com/playlist?list=PLSa4NIW1yxdg7xoEL2x8wGzci0t8h51nN&si=-GB9q9hreFBD7eOI) that give an intuition for some of the human solutions. The full AlphaProof solutions are available [here](https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/imo-2024-solutions/index.html) - I will only include snippets of the proofs in this post.

* TOC
{:toc}

## Problem 1

### Problem
Determine all real numbers $\alpha$ such that, for every positive integer $n$, the integer

$$
\lfloor \alpha \rfloor + \lfloor 2\alpha \rfloor + \dots +\lfloor n\alpha \rfloor
$$

is a multiple of $n$. (Note that $\lfloor z \rfloor$ denotes the greatest integer less than or equal to $z$. For example, $\lfloor -\pi \rfloor = -4$ and $\lfloor 2 \rfloor = \lfloor 2.9 \rfloor = 2$.)

### Solution

AlphaProof's full solution is [here](https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/imo-2024-solutions/P1/index.html).

The answer turns out to be the set of even integers. The hard part of this proof is to show that no $\alpha$ except the even integers can satisfy the given property. AlphaProof does this in an interesting (if convoluted) way. It first sets up an integer $\ell$ such that $2\ell = \lfloor\alpha\rfloor + \lfloor2\alpha\rfloor$. This is possible by substituting $n=2$ into the given property.

{% highlight lean %}
existsλx L=>(L 2 two_pos).rec λl Y=>?_
{% endhighlight %}

Further, it claims (and goes on to prove) that for all natural numbers $n$, 

$$\lfloor (n + 1)\:\alpha \rfloor = \lfloor \alpha \rfloor + 2n\:(\ell - \lfloor \alpha \rfloor) \tag{1} \label{eq:claim}$$

{% highlight lean %}
suffices: ∀ (n : ℕ),⌊(n+1)*x⌋ =⌊ x⌋+2 * ↑ (n : ℕ) * (l-(⌊(x)⌋))
{% endhighlight %}

From this, it is able to show that $\alpha = 2\,(\ell-\lfloor\alpha\rfloor)$.

{% highlight lean %}
use(l-⌊x⌋)*2
{% endhighlight %}

which has to be an even integer (since it's 2 multiplied by an integer).

The way in which it proves these things involves some rather elaborate simplifications. But setting up the claim in \eqref{eq:claim} is the impressive bit that makes the rest of the proof work. To my eyes, the motivation for this claim is rather unintuitive, and the fact that it all works out is almost magical.

## Problem 2

### Problem

Find all positive integer pairs $(a,b)$ such that there exist positive integers $g$ and $N$ where

$$
\gcd\,(a^n + b, b^n + a) = g
$$

holds for all integers $n \geq N$.


### Solution

AlphaProof's full solution is [here](https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/imo-2024-solutions/P2/index.html).

AlphaProof correctly proposes that $(1, 1)$ is the only solution. To show that no other solution can work, it asks us to consider the number $ab + 1$. It claims that $ab + 1$ must divide $g$.

{% highlight lean %}
have:b.1+b.2∣Y:=?_
{% endhighlight %}

Note that in its infinite wisdom, AlphaProof decides to rename pairs $(a, b)$ to $b$, so that it must reference the elements as $b.1$ and $b.2$. It has also chosen, for reasons best known to itself, to rename the variable $g$ to $Y$.

After proving this, it uses this fact to show that $ab + 1 \leq b + 1$ and $ab + 1 \leq a + 1$ which requires $a = b = 1$.

The fact that $ab + 1$ is coprime to $a$ and $b$ makes it so that we can apply [Euler's theorem](https://en.wikipedia.org/wiki/Euler%27s_theorem), ie 

$$
  a^{\varphi(ab+1)} \equiv 1 \pmod{ab+1}
$$

$$
  b^{\varphi(ab+1)} \equiv 1 \pmod{ab+1}
$$

which is used several times in the proof. This strategy closely follows human proofs to this problem. The choice to consider $ab + 1$ is the clever idea that sets up the proof.

## Problem 6

### Problem
Let $\mathbb{Q}$ be the set of rational numbers. A function $f: \mathbb{Q} \to \mathbb{Q}$ is called *aquaesulian* if the following property holds: for every $x,y \in \mathbb{Q}$,

$$
f(x+f(y)) = f(x) + y \quad \text{or} \quad f(f(x)+y) = x + f(y).
$$

Show that there exists an integer $c$ such that for any aquaesulian function $f$ there are at most $c$ different rational numbers of the form $f(r) + f(-r)$ for some rational number $r$, and find the smallest possible value of $c$.

### Solution

AlphaProof's full solution is [here](https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/imo-2024-solutions/P6/index.html).

AlphaProof finds the answer $c=2$. It breaks the proof into two parts. First, it shows that $c \leq 2$, by showing that $f(r) + f(-r)$ can either be $0$ or some singular other value. This part of the proof is quite elaborate, and makes clever use of the given aquaesulian properties.

Once this is done, $c$ can be either $1$ or $2$. To show that $c=2$, AlphaProof proposes an aquaesulian function $f$ that takes on two different values for  $f(r) + f(-r)$.

$$
f(x) = -x + 2⌈x⌉
$$

{% highlight lean %}
specialize V $ λ N=>-N+2 *Int.ceil N
{% endhighlight %}

It then shows that $f(-1) + f(1) = 0$ and $f(1/2) = f(-1/2) = 2$, which gives us the two distinct values we need.

{% highlight lean %}
use Finset.one_lt_card.2$ by exists@0,V.1.mem_toFinset.2 (by exists-1),2,V.1.mem_toFinset.2 (by exists 1/2)
{% endhighlight %}

This is a remarkable construction! And it's pretty hard to find. Only 5/509 participants solved P6, and notably Tim Gowers gave it a bit of a shot while judging this solution and didn't find a construction that gave two distinct values.

## Postscript

At an IMO afterparty, some of us from the AlphaProof team had fun with signing the best tactic from each proof.

<img src="/assets/p1_tactic.jpg" alt="p1 tactic" style="width: 300px;"/>
<img src="/assets/p2_tactic.jpg" alt="p2 tactic" style="width: 300px;"/>
<img src="/assets/p6_tactic.jpg" alt="p6 tactic" style="width: 300px;"/>

I discussed the P6 solution in some more detail on the [No Priors podcast](https://youtu.be/uX6ceY1vcUg?si=d9LY1QvdQ-yeppPK&t=1845).

Thanks to the [Lean experts](https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/#:~:text=Oliver%20Nash%2C%20Bhavik%20Mehta%2C%20Paul%20Lezeau%2C%20Salvatore%20Mercuri%2C%20Lawrence%20Wu%2C%20Calle%20Soenne%2C%20Thomas%20Murrills%2C%20Luigi%20Massacci%20and%20Andrew%20Yang%20advised%20and%20contributed%20as%20Lean%20experts) who translated the cryptic Lean proofs into English, and Bhavik and Paul in particular for suggesting impressive lines in the proofs.