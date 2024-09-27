# Assignment 3

You can use any software to create your document (like Microsoft Word, Google Docs, LaTeX, or Markdown), but be sure to submit your assignment as a PDF. If you need a reminder on how to run R code in your web browser, refer back to Assignment 1. Most of the questions can be answered in 1 to 3 sentences.

## The Lotka-Volterra Predation System

In class, we discussed how the Lotka-Volterra model helps us understand the relationship between predators and prey. Since we're dealing with two species, the model consists of two equations—one for the prey and one for the predator. These equations are different because predators and prey interact in different ways. There are four key processes we need to account for:

1. The prey species, $N$, grows if there are no predators.
2. Predators cause mortality in the prey population.
3. The predator species, $P$, grows by consuming the prey.
4. The predators die off at a constant rate when prey is scarce.

We can model how the population sizes of both the prey ($N$) and predator ($P$) change over time using a **dynamical system**. In these types of models, variables like population size change over time according to specific rules, which we represent with parameters. The two equations for our model are:

$$
\frac{dN}{dt} = r N - a N P
$$

$$
\frac{dP}{dt} = b a N P - m P
$$

Here, $\frac{dN}{dt}$ means "the change in the number of prey over time," and $\frac{dP}{dt}$ means "the change in the number of predators over time." 

### Question 1.1
What happens to the prey population if the predators go extinct? What shape will the curve of prey population numbers in time be, if no predators are present?

### Question 1.2
If the prey goes extinct, what does the equation for the predator population look like? What shape will its time series curve be? How is this different from the prey equation in the previous question?

## What do the parameters mean?

Next, let’s go over the different parameters in these equations. A **parameter** is simply a number that doesn’t change over time. For example, in the prey equation, the parameter $r$ is the growth rate of the prey population. It has units of 1/**time**, meaning it describes how quickly the prey population changes over time.

The parameter $a$ is new. We can figure out its units by looking at how the variables and parameters fit together. Since we’re working with units like individuals and time, the rule is that both sides of the equation must have the same units. For example, $N$ (prey population) and $P$ (predator population) are both measured in **individuals**. But when we multiply them together ($N \times P$), the units become **individuals**$^{2}$.

From this, we can infer that $a$ must have the units of 1/(**time** $\times$ **individuals**) in order for the equation to make sense. The parameter $a$ represents how efficient the predator is at capturing prey. A higher $a$ means the predator catches prey more quickly, reducing the prey population faster. Now let's look at the term $-aNP$. If there are more predators, or if they are more efficient (high $a$), the prey population declines faster.

The parameter $b$ is the **conversion efficiency**. It represents how much of the prey's energy is turned into new predator growth. For example, if a predator converts 10% of the prey it eats into new biomass, $b = 0.1$. This explains why the term $-aNP$ reduces the prey population while $+baNP$ adds to the predator population. However, since $b$ is usually much less than 1, not all of the prey’s biomass is converted into predator biomass (some energy is lost as heat).

### Question 2.1
Let’s say the prey is a plant species and the predator is an herbivore. Would $b$ be higher or lower compared to when a carnivore is eating an herbivore? Why?

## Dynamics of the Lotka-Volterra model

Now that you understand the parameters, we’ll use the same methods we used for the Lotka-Volterra competition system to study the dynamics of the predator-prey system. To do this, we will:

1. Set $\frac{d}{dt}$ equal to 0 to find the steady states (these are the **isoclines** of the system). One isocline will be for the prey population, and the other for the predator population.
2. Find the x- and y-intercepts of these isoclines to plot them.
3. Determine the flow direction in the system by solving for the regions where prey and predator populations are increasing or decreasing.
4. Combine these to predict how the population densities of $N$ and $P$ will change over time and what the long-term outcome will be.

Refer to your lecture notes or Chapter 12 of the textbook for a review of these steps if necessary.

Here’s the code to simulate the Lotka-Volterra predator-prey system. The plot shows the isoclines for the predator (red) and prey (blue), along with the trajectory of the populations over time (green). A smaller plot in the top right shows the same dynamics as a time series.

```r
library(RColorBrewer)
library(deSolve)
pal = brewer.pal(3, 'Set1')
## Define the Rates function
pred.flow = function(r, a, b, m, Pstart, Nstart, tmax){
    yini  = c(P = Pstart,N = Nstart)
    fmap  = function (t, y, parms) {
    with(as.list(y), {
        dP = b*a*y[2]*y[1] - m*y[1]
        dN = r*y[2] - a*y[2]*y[1]
        list(c(dP, dN))
    })}
    times = seq(from = 0, to = tmax, by = 0.1) 
    out   = ode(y = yini, times = times, func = fmap, parms = NULL)
    Ptraj = out[,2];
    Ntraj = out[,3];
    timeline = out[,1];
    Pend = tail(Ptraj, n = 1)
    Nend = tail(Ntraj, n = 1)
    maxtraj = max(c(Ptraj,Ntraj));
    Psize = maxtraj*1.2;
    Nsize = maxtraj*1.2;
    p_isocline = m/(b*a)
    n_isocline = r/a
    par(fig = c(0,1,0,1))
    plot(rep(p_isocline,100),seq(-10,Psize,length.out=100),type='l',lwd=2,col=pal[1],xlim=c(0,500),ylim = c(0,150),xlab='N population',ylab='P population',lty=2)
    lines(seq(-10,Nsize,length.out=100),rep(n_isocline,100),lwd=2,col=pal[2],lty=2)
    points(Nstart,Pstart,pch=16,cex=2,col=pal[3])
    lines(Ntraj,Ptraj,col=pal[3],lwd=2)
    points(Nend,Pend,pch=8,cex=2,col=pal[3])
    legend(0,maxtraj*1.2,c('P isocline','N isocline'),pch=16,col=pal)
    par(fig = c(0.5,1, 0.5, 1), new = T)
    plot(timeline,Ptraj,type='l',col=pal[1],xlab='Time',ylab='Pop. size',lwd=2,ylim=c(0,maxtraj))
    lines(timeline,Ntraj,type='l',col=pal[2],lwd=2)}
## Plug in parameter values and run HERE
pred.flow(r = 0.5, a = 0.01, b = 0.1, m = 0.1, Pstart = 70, Nstart = 100, tmax = 500)
```

### Question 3.1

Suppose strange environmental conditions cause the predator's conversion efficiency to fall by half. How would this change the predator-prey cycle compared to the default conditions? Describe what happens to the population dynamics in words.

### Question 3.2

In the Lotka-Volterra model, the predator's growth depends on both the prey population and the conversion efficiency bb. Suppose we increase the predator's per capita mortality rate mm instead of changing the conversion efficiency. How does this affect the dynamics of the predator-prey cycle? Compare the outcome to the default parameter values and describe how both predator and prey populations are impacted over time.
