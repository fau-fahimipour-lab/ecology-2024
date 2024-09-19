# Assignment 2
You may use any software to create your document (e.g., Microsoft Word, Google Docs, LaTeX, Markdown, etc.), but submit your assignment as a PDF. Refer to Assignment 1 if you need a reminder for how to run R code in your web browser. Most questions can be answered correctly in 1, 2, or 3 sentences.

## Starting with discrete-time
Over the course of the first unit, we have focused our discussion on individuals mostly. How do individuals behave? Evolve? Acquire energy? Respond to the environment? In the terms we have discussed, success in nature is a combination of being able to survive long enough to pass your genetic material to the next generation, and to ensure that the next generation has what they need to survive themselves. And as we have learned, organisms do this in a variety of ways... they might for instance invest a lot of energy into a few offspring, or they might invest a little energy into a lot of offspring. They can scatter their reproductive events over the course of their lives, or they can wait until the end of their lives to invest in reproduction.

Now let’s take this a step further, and examine how these different strategies influence not an individual, but a *collective of individuals*, which we call a **population**. A population is a collection of individuals that interact and reproduce with each other more-so than they might do with other nearby populations. We will eventually discuss *metapopulations*, where multiple populations are linked together in space by the movement of organisms, but for now we will focus our efforts on a single population of a single species.

How do populations change through time? Without the complexities of immigration and emigration, we simply note that populations increase by reproduction (+ Births) and decrease by mortality (- Deaths). Let's start by investigating these processes in a *discrete-time model* (using the language that we discussed in class). So we can write

$N(t) = N(t - 1) + Births - Deaths = N(t - 1) + B - D$

where $N(t)$ is the number of individuals in my population at time $t$, $N(t - 1)$ is the number of individuals in my population one time step in the past, $B$ is the total number of births that occur within the time-step $(t + 1) − t$ and $D$ is the total number of deaths in that same timestep. As we will explore, the size of the timestep matters. Take 1000 humans... if our timestep is one hour, very few (likely zero) of those individuals will have reproduced or died within that span. If our timestep is one year, it is more likely that some will have given birth, and some will have died. We can generalize the size of the timestep by $\Delta_t$ (this is just some number that we can choose, say 10 minutes or 1 year or whatever you like) to note that in some time window, births and deaths occur:

$N(t + \Delta_t) = N(t) + (B − D) \Delta_t$

This means something slightly different. We are saying that $B$ and $D$ represent the total births and total deaths *per unit time*. So if $B$ and $D$ represent total births and total deaths per day, then the size of our time window $\Delta_t$ will increase or decrease the total number of births and deaths with respect to the size of the window. If $\Delta_t$ is 30 days, then the total number of births and deaths represent those expected within a month given the daily rates of $B$ and $D$.

Now the question is: should we always expect the same number of births and deaths for a given time window regardless of population size? I hope you say **no**, because that’s the correct answer. Imagine our current population is 10... or imagine it is 10000000. We would expect a different total number of births and deaths, and we would expect this to change as a function of (in other words, depend on) the population size. In this way we can describe the total number of births and deaths as being **density-dependent**. A simple assumption would be that births and deaths increase linearly with the size of the population. So $B = b N(t)$ and $D = d N(t)$. Notice that these are both straight lines with a *y*-intercept of zero (makes sense... if there are no individuals in the population there are no births or deaths!). Here, $b$ is the per capita *birth rate*, and $d$ is the per capita *death rate*. We can then use some basic algebra to rewrite

$N(t + \Delta_t) = N(t) + (b N(t) − d N(t)) \Delta_t$

$N(t + \Delta_t) = N(t) + (b − d) N(t) \Delta_t$

$N(t + \Delta_t) = N(t) + r_d N(t) \Delta_t$

where we have defined a new parameter $r_d = b − d$, which is the average per capita growth rate of the population. The subscript $d$ reminds us that this is a growth rate specifically for populations that change over *discrete* rather than *continuous* intervals. Before we move on, let’s examine the current equation for population growth

```r
discrete.growth = function(N0, tmax, Deltat, b, d) {
    pop = numeric(tmax)
    pop[1] = N0
    for (t in 2:(tmax)){
        N = pop[t-1]
        NewN = N + Deltat * (b - d) * N
        pop[t] = NewN
    }
    plot(pop, xlab = 'Time Steps', ylab = 'Population Size', type = 'b', pch = 16, col = "#000000")
}
discrete.growth(N0 = 5, tmax = 20, Deltat = 1, b = 0.8, d = 0.7)
```

### Question 1.1
Experiment with the above parameter values by changing them in the last line of the above code block. For example, if you want to change $b$ to 99 and $d$ to 44 (you don't, just saying for example) you would modify the last line to read `discrete.growth(N0 = 5, tmax = 20, Deltat = 1, b = 99, d = 44)` and then re-run the entire code block. What happens when you change $b$ to be greater, less-than, or equal to $d$? Describe in words what is happening in each scenario.

### Question 1.2
What is the shape of the population over time when we expand the amount of time over which we observe the population? *e.g.* try `tmax = 100`.

### Question 1.3
Compare results for small versus large time windows $\Delta_t$ by changing `Deltat`. Show me an example of a large time window (say, something around `Deltat = 10`) by providing the figure output.

## On to continuous-time
As we discussed in class last Friday, if we make the time window infinitesimally small (*i.e.* $\Delta_t \rightarrow 0$, which mathematically speaking means something like "imagine delta-t gets very close to zero"), we can write

$\frac{dN}{dt} = r N$

A few things to note here... first is that in class we used the notation $\dot{N} = ...$ which is exactly the mathematical shorthand for writing $\frac{dN}{dt}$. Next is that the right-hand-side (**RHS**) of the equation looks a lot like the discrete-time version, at least the part that reads $r_d N(t)$. However, we now are using $r$ instead of $r_d$ to denote the population growth rate. This is because our interpretation of these parameters is a bit different. Here $r$ is the instantaneous rate of growth of the population. To put this equation into words, we would say: the change in the population ($dN$) over time ($dt$) is equal to $r N$, where $r$ is the instantaneous rate of growth and $N$ is the size of the population.

You can use calculus to solve this equation. In fact, we did this together in class. You don't need to know the details of the calculus, they are in the lecture slides if you'd like to understand them. But, what you do need to know is that we can use calculus to solve the above equation for the population size at any point in time N(t). In class, we saw that the final solution to this model is

$N(t) = N_0 e^{r t}$

### Question 2.1
Considering this solution, sketch your expectation for what $N(t)$ as a function of time $t$ should look like according to the above equation (*i.e.* draw what you think a curve would look like with time on the *x*-axis and $N(t)$ on the $y$-axis). Do you think the continuous-time model looks similar to or different than the discrete-time version?

### Question 2.2
We want to make a prediction for the following: imagine you are running a salmon fishery, and need to breed your current salmon stock to 10-times their current numbers to fulfill an order. How long would it take for your population of salmon to increase by a factor of 10 if you start with 500 individuals and their growth rate $r = 0.0032$ per year. How many years would it take for the population to reach 10× the starting size? *Hint*: try plugging these values into the above solution and solve for $t$.

## Carrying capacity
In the world of ecology, understanding population dynamics is crucial. One of the fundamental concepts we explore is the idea of carrying capacity and logistic growth. Imagine a species that colonizes a new habitat. Initially, it experiences exponential population growth, where resources and the environment are not limiting factors. However, as time progresses, the population grows, and individuals start consuming resources and occupying space. This begins to influence the rate of population growth. In mathematical terms, we can think of this as the per capita birth and death rates changing as the environment becomes crowded with individuals.

Before, we defined total births ($B$) and deaths ($D$) as density-dependent, meaning they were linear functions of the population size ($N$), with slopes given by constant per-capita birth ($b$) and death ($d$) rates. However, as the population size increases, we realize that these per-capita rates are not constant. In reality, births decrease with increasing population size, and deaths increase with population size due to things like limiting resources, crowding, disease, and predation. In equations, this change can be represented as:

$b' = b − a N$

$d' = d + c N$

Here, $b'$ represents the new per-capita birth rate, and $d'$ represents the new per-capita death rate. Importantly, when the population size ($N$) is close to zero, $b'$ is approximately equal to $b$, and $d'$ is approximately equal to $d$.

We have defined the intrinsic rate of growth ($r$) as $r = b − d$. Now, with the density-dependent rates, we can define a new density-dependent intrinsic growth rate, $r'$, as $r' = b' − d'$.

Let's incorporate this into the equation for continuous-time population growth:

$\frac{dN}{dt} = r' N (1 - \frac{a + c}{b - d} N)$

$\frac{dN}{dt} = r' N (1 - \frac{N}{K})$

Where we've introduced a new parameter called $K$, which is the carrying capacity, which is defined as $K = \frac{b − d}{a + c}$.

This equation captures the essence of logistic growth, where population growth slows down as it approaches the carrying capacity ($K$). It illustrates how birth rates decrease and death rates increase as the population size approaches its environmental limits, demonstrating the intricate relationship between a species and its habitat. While we've introduced some mathematical expressions, remember that this is primarily an exploration of ecological concepts rather than a math-intensive course.

In our exploration of population dynamics and the concept of carrying capacity, we want to understand how the population size, denoted as N(t), changes over time, represented as t. To achieve this, we'll employ a powerful tool called a differential equation solver. Now, you might wonder, what exactly is a differential equation solver? In simple terms, it's a numerical method that allows us to observe the behavior of a system described by differential equations without the need to explicitly solve for the function N(t). In more advanced mathematics, we could solve these equations using techniques like integration by parts, but that can get quite complex, and it's not the focus of this course.

Instead, we turn to a differential equation solver as a practical and efficient way to gain insights into how populations grow and interact with their environment over time. This solver helps us track changes in N(t) as time progresses, providing a dynamic view of ecological processes. So, when we use the differential equation solver, we are essentially asking it to simulate the behavior of our ecological model and provide us with a numerical solution. This way, we can see how population dynamics unfold without delving too deeply into the complex mathematical machinery that lies beneath it. It's a valuable tool for understanding the real-world implications of the ecological concepts we're discussing in this course. We can do this using the code below:

```r
library(deSolve)
pop.logistic = function(r, K, tmax, N0){
    yini = c(N = N0)
    fmap = function(t, y, parms){
    with(as.list(y), {
        dN = r * N * (1 - (N / K))
        list(dN)
    })}
    times = seq(from = 0, to = tmax, by = 0.1) 
    out   = ode(y = yini, times = times, func = fmap, parms = NULL)
    plot(out, lwd = 2, xlab = 'Time', ylab = 'Population N(t)')
}
pop.logistic(r = 0.01, K = 30, tmax = 1000, N0 = 1)
```

### Question 3.1
What is the shape of the population over time? Try entering a large `N0` as the starting population. What do you observe? Does the **long-term** dynamics change?

### Question 3.2
What is the role in `r`? How does altering `r` change what happens to the population in the long term?

### Question 3.3
What is the role in `K`? How does altering `K` change what happens to the population in the long term?
