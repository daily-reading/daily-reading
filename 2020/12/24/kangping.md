# Test your tests

## 1. Intro
Tests need to be robust. If tests are flaky/non-deterministic, you risk wasting engineering time budget on problems that do not exist. 

In practice though, most tests are flaky, we just need to understand how flaky they are. The author proposed a measurement for flakiness of the tests, called the probabilistic flakiness score (PFS). 

PFS is language independent, easy to calculate and easy to understand. 

## 2. PFS
### 2.1 overview
The author is very vague here, but in general you run a test multiple times, and get a sample distribution of the success rate, after some code changes are made, you again run the same test multiple times, and get a new distribution, after which you can easily estimate if those two distribution are different or if the test has lower success rate after the code change. 

### 2.2 what affects the result of a test
1. code changes
2. state of the world (especially in integration-tests.)
3. rubbish bin (loads of external factors, e.g. network failure, randomness, etc.) This is what we want to measure.

### 2.3 PFS estimation
Define two variables: 
1. Probability of bad ($p_b$)
2. Probability of failure in good state (PFS) ($p_f$)

This is equivalent to type-I and type-II error. 

We now assume that state of the world do not change during the multiple run of tests. 

Due to Bayes rule, we have 


`
$P(p_b, p_f| \text{test results}) = \frac{P(\text{test results}| p_b,p_f)P(p_b,p_f)}{P(\text{test results})}$
`

What we care here is the posterior distribution `P(p_b, p_f| \text{test results})`, the left-hand side of the equation. Now we look at the right-hand side, `$P(\text{test results}| p_b,p_f)$` is trivial to know, base on the binomial trial model, `$P(p_b,p_f)$` is the prior distribution. We do not need to care about the normalizer `$P(\text{test results})$` as in most bayesian model. (We just need the integral of LHS to be 1. )

Now we know the posterior distribution, we can also derive the marginal of the posterior distribution. i.e.

`$P(p_b| \text{test results})$`
and 
`$P(p_f| \text{test results})$`

We want posterior `$P(p_f| \text{test results})$` to be spiky/narrow, which means the estimation error of flakiness is small, otherwise, we just need to run more tests. 

 