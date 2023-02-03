# Project2-LinearAlgebra
PSI Numerical Methods (2023) course Project 2.
The solutions for the problems are in the file `Project2-SercanHusnugil.ipynb` file, written in Julia v1.8.4.
This notebook also includes the reasoning of the code and some comments on the results.
Note that, if the figures do not show upp when you run all the notebook at once, please turn back and evaluate the cells belonging to that section/question from the start of the section in order not to mix variables from different sections.

Our main aim is to get familiarized with linear algebraic manipulation in Julia using least squares approximation as an example.

Overall, the functions in the notebook are aimed to approximate `sin(x)` with a polynomial expansion and test the accuracy of the method. To obtain the answers to the project questions, just go to the notebook and run the cells in the presented order. Note that, `WGLMakie` and `BenchmarkTools` packages might need to be manually added before running the notebook.

## Question 1

For question 1, the function `least_squares(pmax, npoints, x, fstar, xx)` is used to approximate `fstar(x) = sin(x)` as a sum of polynomials up to power `pmax`.  Basically, the function is approximated via expanding: $f^*(x) = {\sum_n}^p c_n x^n$ 
With the parameters `pmax = 5`, `npoints = 1000` one should recover the result:

![question1](https://user-images.githubusercontent.com/122399037/216506834-2593ba17-4b28-44d2-8cdf-b6b301a0a3bd.png)

As it can be seen from the plot, the approximated values via least squares method match well with the target function $f^*(x)=\sin(x)$ for the interval $[0,\pi/2]$.

## Question 2

In question 2, we measure the root mean square (RMS) error using the function `errorl2(y, ystar)` defined in the code. The function takes the result of `least_squares(...)` and calculates RMS value as: $\text{RMS} = \sqrt{\frac{1}{n}{\sum_{i}}^{n}(f(x_i) - f^*(x_i))^2}$

We run this function for different maximum polynomial orders (i.e., different `pmax` in `least_squares(...)` ) and obtain the plot:

![question2](https://user-images.githubusercontent.com/122399037/216507044-854e092f-7b69-46f1-942a-2e7fe56392f4.png)

According to the plot where the $y-$axis values rescaled logarithmically, we can achieve RMS values of order $10^{-10}$ when we go to the polynomial order 10. Above that order, the error value does not decrease but instead fluctuates. This might be due to numerical errors (e.g., floating point errors) introduced along the calculation.

## Question 3

For question 3, we modify the function in question 1 to only take odd-ordered polynomials into account since `sin(x)` is an odd function. As a result of this, we define a new function: `least_squares_odd(pmaxodd, npoints, x, fstar, xx)`. Note that, the maximum polynomial order, `pmaxodd` input paramater, **has to be an odd** number. We repeat the RMS calculation in question 2 and plot the RMS values versus the highest odd polynomial order:

![question3](https://user-images.githubusercontent.com/122399037/216507066-5f6e5b15-567b-4f22-bf7a-256b23b18ce2.png)

From the figure above, we can deduce that lower RMS values can be achieved using only the odd polynomials. Again, we see that increasing the order after some value does not improve the error in the code due to numerical errors.

We compare the cost of the two methods by measuring the execution times (using the same number of terms in the expansions) by the function `@btime` of `BenchmarkTools`. Comparing the execution times, one can say that the full `least_squares` function works faster than `least_squares_odd` when evaluated over the same number of terms. Note that the exacution time values measured by `@btime` can differ from machine to machine.

## Question 4

In question 4, we calculate the derivative of the polynomial expansion. We calculate the matrix `D` which defines the linear operator acting on the expansion coefficients of `fstar(x)` giving the expansion coefficients of `fstar'(x)`. The details of the analytical calculation is given in the notebook. Finally, we compare the resulting plot where we used matrix `D` to calculate the derivative of ${f^*}'(x)$ with the plot where we approximate `fstar'(x) = cos(x)` via  `least_squares()` function:

![question4](https://user-images.githubusercontent.com/122399037/216509820-e85f1884-5112-4e64-ac4b-6e9b3fcd77fc.png)

From the figure above, we see that using the linear derivative operator $D$ and approximating the function via least squares agree well on the interval we implemented the calculation.
