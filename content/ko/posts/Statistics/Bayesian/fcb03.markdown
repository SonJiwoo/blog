---
collapsible: false
date: "2021-01-07T10:08:56+09:00"
description: One-parameter Models
draft: false
title: Conjugacy
weight: 3
---

## Chapter 03. <br> One-parameter Models
본 포스팅은 **First Course in Bayesian Statistical Methods**를 참고하였다.

### Binomial Model
Prior: `$\theta \text{ ~ } Beta(a,b)$`
Likelihood: `$Y|\theta \text{ ~ } Binomial(n, \theta) $`
Posterior: `$\theta|y \text{ ~ } Beta(a+y, b+n-y) $` <br>
a: prior 성공횟수, b: prior 실패횟수, `$\omega$`=a+b: concentration <br>
`$E[\theta|y] = \frac{a+y}{a+b+n} = \frac{n}{a+b+n}\times\frac{y}{n} + \frac{a+b}{a+b+n}\times\frac{a}{a+b}$` where `$\frac{y}{n}$` = sample mean, `$\frac{a}{a+b}$` = prior expectation <br>
Posterior Predictive  
`$n^* = 1$`일 때 : `$\tilde{Y}|y \text{ ~ } Ber(\frac{a+y}{a+b+n})$`
`$n^* \geq 2$`일 때 : `$p(\tilde{Y}=y^*|y) = \binom{n^*}{y^*}\frac{B(a+y+y^*, b+n+n^*-y-y^*)}{B(a+y, b+n-y)}$` where `$B(\alpha, \beta) = \frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)} $`

### Poisson Model
Prior: `$\theta \text{ ~ } Gamma(a,b) $`
Likelihood: `$Y_1, ..., Y_n \text{ ~ iid. } Poisson(\theta)$`
Posterior: `$\theta|y_1, ..., y_n \text{ ~ } Gamma(a+\sum_{i=1}^{n}{y_i}, b+n) $` <br>  
a: sum of counts from b prior observations, b: number of prior observations <br>
`$E[\theta|y_1, ..., y_n] = \frac{a+\sum y_i}{b+n} = \frac{b}{b+n}\frac{a}{b} + \frac{n}{b+n}\frac{\sum y_i}{n}$` <br>
Posterior Predictive: `$\tilde{Y}=y^*|y_1, ..., y_n \text{ ~ } NB(a+\sum y_i+y^*, \frac{b+n}{b+n+1}) $`
단, 여기서 `$Negative Binomial$`은 성공이 아닌 실패횟수를 세는 분포 형태이다. 자세한 내용은 [확률분포 포스팅](/posts/statistics/statistics/probability_distribution/)에서 확인하자.

### Exponential Family
exponential family(지수족)의 pdf 또는 pmf는 다음과 같은 형식으로 표현될 수 있어야 한다.  
`$ p(y_i|\phi) = h(y)c(\phi)exp\big[\phi K(y)\big]$`  
exponential family 자체에 대해서 보다 자세한 것은 [해당 포스팅](/posts/statistics/statistics/exponential_family/)을 참고하자.

Prior
`$$\begin{align}
p(\phi) &= k(n_0, t_0)c(\phi)^n_0e^{n_0t_0\phi} \\
&\propto c(\phi)^n_0e^{n_0t_0\phi}
\end{align}$$`

Likelihood
`$$L(\phi|y_1,...,y_n) \propto c(\phi)^n exp(\phi \sum_{i=1}^{n}K(y_i))$$`

Posterior
`$$\begin{align}
p(\phi|y) &\propto p(\phi)f(y|\phi) \\
&\propto c(\phi)^{n_0}e^{n_0t_0\phi} \cdot c(\phi)^n exp(\phi \sum_{i=1}^{n}K(y_i)) \\
&\propto c(\phi)^{n_0}exp\big[n_0t_0\phi + \phi \sum_{i=1}^{n}K(y_i) \big] \\
&\propto c(\phi)^{n_0}exp\big[ \phi \big( n_0t_0 + n\frac{\sum_{i=1}^{n}K(y_i)}{n} \big)\big]
\end{align}$$`
여기서 `$n_0$`와 `$t_0$`은 각각 **prior sample size**와 **prior guess** of `$K(Y)$`를 뜻한다.


{{<expand "참고: FCB 책 표현">}}
Prior: `$p(\theta) \propto g(\theta)^\eta \ exp(\phi(\theta)^T \ \nu)$`  
Likelihood: `$p(y|\theta) = \prod_{i=1}^{N} f(y_i) \ g(\theta)^N \ exp(\phi(\theta)^T \ \sum_{i=1}^{N}s(y_i))$` where `$\sum_{i=1}^{N}s(y_i))$` is sufficient statistics `$t(y)$`  
Posterior: `$p(\theta|y) \propto g(\theta)^{\eta+N} \ exp(\phi(\theta)^T \ (\nu + t(y)) $`  
{{</expand>}}

### Conjugate Prior
prior와 posterior의 확률분포형태가 같을 수 있도록 prior을 설정하면 이를 conjugate prior라고 한다.  
위의 예시 외에도 Normal model 등이 있는데, 이들에 대해서는 다음에 이어서 살펴보도록 하겠다.
다양한 예시들은 [위키백과](https://en.wikipedia.org/wiki/Conjugate_prior)에 자세히 나와있으니 궁금한 사람들은 추가적으로 살펴보아도 좋겠다.

### 주의사항
사후확률분포가 차이가 많이 나는 것과 사후예측치가 차이가 많이 나는 것의 차이를 알아두어야 한다. 즉, {`${\theta_1 > \theta_2}$`}와 {`$\tilde{Y_1} > \tilde{Y_2}$`}는 다르다.
> Strong evidence of a difference between two populations does not mean that the difference itself is large.

### Conclusion
<p style='text-align: center'> Conjugacy를 잘 알아두자. </p> 

---
<br> 
<p style='text-align: center; color:gray'> 혹시 궁금한 점이나 잘못된 내용이 있다면, 댓글로 알려주시면 적극 반영하도록 하겠습니다. </p>

<br>
<br>
