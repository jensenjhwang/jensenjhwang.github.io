---
layout: post
title:  "Understanding Diffusion Models"
date:   2025-07-14 01:31:40 -0700
categories: jekyll update
---
## Introduction

Diffusion models are novel generative architecures that power breakthroughs such as Stable Diffusion and DALL-E2. They are more stable than Generative Adversarial Networks (GANs) in training, albeit with the drawback of longer inference times.

Diffusion models are motivated by the following physical process. Suppose you wish to model the temperature distribution of your room $\tau_0(\bm{x})$ (e.g. your desk might be warmer due to the tea you brewed). Even though this initial distribution is complex, nature tends to smooth this distribution over time towards a simple equilibrium distribution $\tau_{\infty}(\bm{x})$. The insight of diffusion models is that even though the reconstruction process of $\tau_T(\bm{x}) \to \tau_0(\bm{x})$ might be difficult, we can train a neural network $\theta$ to approximate the much easier infinitesimal process $\tau_t(\bm{x}) \to \tau_{t - \eps}(\bm{x})$ (e.g. if we know the locations and velocities of all air particles at the current instance, we have a decent guess of their locations and velocities at the previous instance). This neural network can then be applied many times, starting from $\tau_{\infty}(\bm{x})$ which plays the role of a prior distribution, to iteratively reconstruct the original distribution. Here, note that the forward process is inherently lossy so an exact reconstruction is impossible.

## Forward Process

We will focus on discrete time steps $t$. To attain a memoryless forward process, diffusion models perturb an input $\bm{x}_t$ according to the following diffusion equation starting from $\bm{x}_0 \sim \mathcal{D}$ where $\mathcal{D}$ is the distribution to model.

$$
\bm{x}_{t} = \sqrt{\alpha_t} \bm{x}_{t-1} + \sqrt{1 - \alpha_t} \bm{\eps}_t \quad \bm{\eps}_t \sim \mathcal{N}(0, I). \label{diffeq}
$$

It is easy to see that

$$
\bm{x}_t = \sqrt{ \tilde{\alpha}_t } \bm{x}_0 + \sqrt{1 - \tilde{\alpha}_t } \cdot \teps_t \quad \teps_t \sim \mathcal{N}(0, I) \quad , \label{forwardprocess}
$$

where $\tilde{\alpha}\_t = \prod\_{s=1}^t \alpha_s$. The mean is obvious and a quick way to derive the variance is to notice that $1)$ the variance doesn't depend on $\bm{x}\_0$ if $\bm{x}\_0$ is deterministic and $2\)$ in the case $\bm{x}_0 \sim \mathcal{N}(0, I)$, $\bm{x}_t$ has identity variance for all $t$ so the variance in the current situation must be $I$ minus the contribution of the variance of $x_0$ when $x_0 \sim \mathcal{N}(0, I)$  (i.e. $1 - \prod_{s=1}^t \alpha_s I$)!

Typically, the noise scale is chosen to increase over time $\alpha_1 > ... > \alpha_t$. In the literature, the convention $\beta_t = 1 - \alpha_t$ is also used to denote the variance of the noise added at step $t$.

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
