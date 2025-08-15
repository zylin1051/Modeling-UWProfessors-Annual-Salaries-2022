# Modeling-UWProfessors-Annual-Salaries-2022

## Data Set Description

University of Waterloo Salary Disclosure - 2022

Disclosed by University of Waterloo:
<https://uwaterloo.ca/about/accountability/salary-disclosure-2022>

The data set contains the annual salary in excess of \$100,000 and the
taxable benefit of the academic staff at the University of Waterloo in
2022. I have removed the surname and the given name of all the members
when importing the data into R since these are not relevant to this
assignment. This data set can be treated as a sample to estimate the
distribution of annual salaries of university professors in 2022 in
Ontario, Canada. The professors here refer to those with position titles
"assistant professor", "associate professor", and "professor".
"Lecturer" is not included in the analysis.

## Frequentist Approach

Since the annual salaries in the data set all exceed 100000, we decided
to use the following transformation $$y_i=f(x_i)=x_i/100000\;,$$ where
$x_i$ is the annual salary so that very value is resealed to a number
between 1 and 5.

The histogram of the transformed salary seems to resemble the shape of a
gamma distribution. After guessing several times, a gamma distribution
$(\alpha,\beta)=(11,20)$ seems to work okay for now.

The log-likelihood function of $\Gamma(\alpha,\beta)$ is

$$\begin{aligned}
l(\alpha,\beta)=n\alpha\log(\beta)-n\log(\Gamma(\alpha))+(\alpha-1)\sum_{i=1}^n\log(y_i)-\beta\sum_{i=1}^ny_i.
\end{aligned}
$$

::: center
![image](./Question-1,-Assignment-2_files/figure-latex/unnamed-chunk-1-1.pdf)
:::

So, the MLE of $\alpha$ and $\beta$ should be somewhere in the region
enclosed by the blue dashed line above.\

### MLE of $\alpha$ and $\beta$

We use the $\text{optim()}$ function in R to find the MLE of $\alpha$
and $\beta$:

    ## [1] 19.91390 10.85772

::: center
![image](./Question-1,-Assignment-2_files/figure-latex/unnamed-chunk-2-1.pdf)
:::

Based on the observed data, we found that
$\left(\hat{\alpha},\hat{\beta}\right)=(19.91390,10.85772)$.

### Confidence Region and Interval of $\alpha$ and $\beta$

We can construct a $95\%$ confidence region for $\theta=(\alpha,\beta)$
using both the normality of MLE and the log-likelihood ratio statistic:

$$
\begin{aligned}
R(\alpha,\beta)=2\left[l\left(\widetilde{\alpha}, \widetilde{\beta}\right)-l\left(\alpha,\beta\right)\right]\rightarrow^\mathcal{D}\chi_{(2)}^2 \qquad;\qquad \left(\boldsymbol{\widetilde{\theta}}-\boldsymbol{\theta}\right)^T\mathcal{I}(\boldsymbol{\theta})\left(\boldsymbol{\widetilde{\theta}}-\boldsymbol{\theta}\right)\;&\rightarrow^\mathcal{D}\chi_{(2)}^2
\end{aligned}
$$

::: center
![image](./Question-1,-Assignment-2_files/figure-latex/unnamed-chunk-3-1.pdf)
:::

::: center
![image](./Question-1,-Assignment-2_files/figure-latex/unnamed-chunk-3-2.pdf)
:::

Interestingly, the two statistics result in confidence regions that
"point in different directions".

We can find the marginal confidence interval for $alpha$ and $\beta$
using the normality of MLE:

$$
\widetilde{\boldsymbol{\theta}}_i\pm Z_{1-\alpha/2}\sqrt{\left[ \mathcal{I}( \widetilde{\boldsymbol{\theta}}^{-1}\right]_{ii}}
$$

    ## [1] "Marginal 95% confidence interval for alpha (normality of MLE): [18.20739,21.62042]"

    ## [1] "Marginal 95% confidence interval for beta (normality of MLE): [9.91547,11.79997]"

We saw that a marginal $95\%$ confidence interval for $\alpha$ is
$[18.20739,21.62042]$ and a marginal $95\%$ confidence interval for
$\beta$ is $[9.91547,11.79997]$ based on the normality of MLE.

Based on the contour plot above, if we instead construct the marginal
confidence interval based on the log-likelihood ratio statistic, it
should give a very similar result.

## Conclusion from the Frequentist Approach

We saw that the distribution of professors' annual salary per 100K seems
to follow a gamma distribution. The maximum likelihood estimates of the
parameters are

$$\left(\hat{\alpha},\hat{\beta}\right)=(19.91390,10.85772)$$ 

with  confidence intervals $\alpha\in[18.20739,21.62042]$ and
$\beta\in[9.91547,11.79997]$.

## [Potential Issue:]{.underline} {#section-3}

The salary data disclosed by the University of Waterloo only contains
staff with annual salaries exceeding \$100,000. There is no information
on those with an annual salary of less than \$100,000. This could
potentially overestimate the distribution of annual salaries in 2022 of
university professors in Ontario, Canada and disturb the validity of our
assumption of a gamma parametric model.
