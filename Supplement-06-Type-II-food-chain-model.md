# A food chain model with type-II functional response
The following code simulates dynamics of a continuous-time food chain model, where the consumers follow type-II functional response interaction dynamics. 
We derived this model in Lecture 15 - *Coarse-graining*.

Make sure you install the required libraries before running the code. At the end, you should see a plot of resource, herbivore, and top predator time-series.

```r
## Load the following libraries (install each of these if you've never used them before)
library(deSolve)  ## This is for solving ODEs
library(reshape2) ## This is for converting data between formats for plotting
library(ggplot2)  ## This is for making pretty plots

## Define the food chain model according to R/deSolve syntax
foodChainModel = function(time, state, parameters) {
  ## Extract the state variables
  R = state['R'] ## Resources
  H = state['H'] ## Herbivores
  P = state['P'] ## Predators
  ## Extract the model parameters
  a = parameters['a'] ## Herbivory rate by herbivores, H
  b = parameters['b'] ## Predation rate by predators, P
  f = parameters['f'] ## Transfer efficiency, how many units of H must be eaten to make a unit of P
  m = parameters['m'] ## Natural death rate of predators, P
  ## Define the equations __HERE__
  dR = R*(1 - R) - R*H / (1 + R)
  dH = a*R*H / (1 + R) - H - b*H*P / (1 + H)
  dP = f*b*H*P / (1 + H) - m*P
  # Return the derivatives as a list
  return(list(c(dR, dH, dP)))
}

## Define the initial conditions and model parameters
initialState = c('R' = 1e-1, 'H' = 1e-2, 'P' = 1e-3)
parameters   = c('a' = 5, 'b' = 3, 'f' = 0.5, 'm' = 0.6)

## Define the time span to simulate
timeSpan = seq(0, 250, by = 0.01)

## Pass these to the ODE solver from deSolve
results = as.data.frame(ode(y = initialState, times = timeSpan, func = foodChainModel, parms = parameters, method = 'lsoda'))

## Convert my results to long format for plotting
resultsClean = melt(results, id.vars = 'time')

## Plot the results with ggplot, make the time series of each species a different color
ggplot(data = resultsClean, aes(x = time, y = value, group = variable, colour = variable)) +
  theme_classic() +
  xlab('Time') +
  ylab('Abundance') +
  geom_path(size = 0.65) +
  scale_colour_brewer(palette = 'Dark2', name = NULL)
```
