# GARCH_volatility_modeling
Using GARCH models to forecast future volatility
GARCH stands for Generalised Autoregressive Conditional Heteroskedasticity and is used to measure time-varying volatility. This contrasts with most models, which assume homoskedasticity (fixed volatility). Its aim is to track volatility clusters, when big price movements happen often leads to a period of high volatility, and therefore it can be used to manage risk.

The simplest version of this model is GARCH(1,1), which looks at the last period's volatility and square return. We will create this model first then deal with its problems in a future project ie assymetric properties of the market's volatility, we will be using japanese yen exchange rates as our financial instrument we are testing, but can easily change the model to  fit most financial assets with a little extra work needed for derivatives and bonds.

Reading: Tim Bollerslev’s paper, “Generalised Autoregressive Conditional Heteroscedasticity,” Journal of Econometrics, 31 (1986): 307–27.

The fun part (math): 
Sigma[n]^2 = y*V[LT] + a*u[n-1]^2 + b*Sigma[n-1]^2, in other words the variance of today is determined by some proportion of its long term variance, the variance of the last period, and the percentage change in the market variable.

This model is such that the coefficients must add to 1 as todays variance must be explained by these 3 variables. 
For the rest of this, the first term will be represented as w, as variance is constant in the long run. Also, notice that this model can be made recursive, where the discount factor of b is applied to every successive term.

Working out the MLE of the coefficients,
First let us consider a familiar example of the normal distribution:
PI[1-m] (2*pi*v)^-1/2 * exp(-u[i]^2/2*v), ln is an increasing function, so will maintain maximum
SIGMA[1-m] -1/2*ln(2*pi*v) -u[i]^2/2*v, now we need to find the maximum point using FOC:
-mlnv i SIGMA[1-n] -u[i]^2/v => 1/m SIGMA u[i]^2

Now for some assumptions about the GARCH model: conditional ui and normal, so we can work out the MLE using a version of the above.

The log likelihood is SIGMA[1-m](-ln(v[i]) -u[i]^2/v[i]). We will use a package to maximise this iteratively.

Data needed: day and price. 
Step 1 Create column representing the proportional change in price (UI) ( S[i] - S[i-1] // S[i] )
Step 2 Create estimates for a,b, w. ie 0.1
Step 3 create a new column to calculate daily variance(vi) starting from day 3 as the square of u on day 2 (use the formula)
Step 4 plug these into the MLE

we want to make estimates that maximise the sum of the values in the last column.
