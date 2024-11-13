# Island biogeography in the real world

In 1969, Daniel Simberloff and E.O. Wilson decided to test a theory that Robert MacArthur and Wilson had formulated a few years before: The Theory of Island Biogeography. 
We discussed these in detail in class, but here is the gist: [_i_] Islands represent unique opportunities to test ecological theories, 
[_ii_] they are as close to a controlled natural experiment as we will ever get,
[_iii_] islands are limited in area and resources, so the number of species that live on islands should also be limited, and
[_iv_] such limitations on species richness should be set by the size and distance of the island to the mainland (where species will be migrating from).

## Question 1

**1.1.** How should the immigration rate change with island size or distance?

**1.2.** How should the extinction rate change with island size or distance?

# The experiment
The experiment Simberloff devised is both simple and a bit nutty. 
He selected a number of mangrove islands off the coast of Florida of varying sizes and distances from the mainland, which hosted diverse arthopod communities. 
He surrounded each mangrove island with a metal frame, covered it with fumigation tarps, and gassed the arthropods into oblivion. 
Following these localized "mass extinction" events, Simberloff and colleagues observed the recolonization of the mangrove islands, documenting which species arrived and when. 
We will now examine these data below using the R programming language 
(_note_: the following code will spit out some warnings, but everything is ok, you may ignore these and look at the plot below). 
Run the following code snippet on your personal computer, or on [https://rdrr.io/snippets/](https://rdrr.io/snippets/).

```r
## Load in the dataset collected by Simberloff (nicely compiled as a package!)
library(island)
library(RColorBrewer)
data = simberloff
pal  = sample(brewer.pal(8, 'Dark2'))
## Prepare the dataset
island1 = apply(data[[1]][,2:16],2,sum)
time1 = as.numeric(names(data[[1]][,2:16])); time1[1] = 0
island2 = apply(data[[2]][,2:18],2,sum)
time2 = as.numeric(names(data[[2]][,2:18])); time2[1] = 0
island3 = apply(data[[3]][,2:17],2,sum)
time3 = as.numeric(names(data[[3]][,2:17])); time3[1] = 0
island4 = apply(data[[4]][,2:19],2,sum)
time4 = as.numeric(names(data[[4]][,2:19])); time4[1] = 0
island5 = apply(data[[5]][,2:17],2,sum)
time5 = as.numeric(names(data[[5]][,2:17])); time5[1] = 0
island6 = apply(data[[6]][,2:16],2,sum)
time6 = as.numeric(names(data[[6]][,2:16])); time6[1] = 0
## Let's plot these diversity curves for islands 1-6
plot(time1,island1,type='l',col=pal[1],lwd=2,xlim=c(0,550),ylim=c(0,50),xlab='Time', ylab='Island species richness')
lines(time2,island2,col=pal[2],lwd=2)
lines(time3,island3,col=pal[3],lwd=2)
lines(time4,island4,col=pal[4],lwd=2)
lines(time5,island5,col=pal[5],lwd=2)
lines(time6,island6,col=pal[6],lwd=2)
legend(400,20,legend=c(1,2,3,4,5,6),col=pal,pch=16)
## Output some statistics
pre_extinction = head(island1,1)
post_recovery = tail(island1,1)
ratio = post_recovery/pre_extinction
print(paste('ratio =',ratio))
```

## Question 2

**2.1.** What happens to the island communities following Simberloff’s fumigation treatment (the datapoint at time t = 0 is species richness before fumigation)? 
Provide your data figure and use it to justify your response.

**2.2.** What do you think about the ethics of fumigating mangrove islands full of arthropods to examine foundational concepts in ecology? 
Is there a less impactful way to go about investigating these questions?

**2.3.** Simberloff's experiment demonstrated species recolonization rates in a controlled, isolated setting. 
Now, consider how this theory might apply to "islands" of habitat in fragmented **terrestrial** landscapes on the mainland.
How might the principles of island biogeography inform our conservation strategies for fragmented ecosystems?
In your answer discuss how concepts like immigration, extinction, and habitat size might change in fragmented landscapes, and consider human impacts.
