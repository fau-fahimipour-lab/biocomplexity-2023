# Building a discrete-time dynamical systems model

Please refer to the lecture 10 notes whenever necessary. To begin, we will write a function that updates our model one time step

```r
## Start with our updating function
updateDiscreteModel = function(x, a){
  x_t = a * x
  x_t
}
```

Next we need a function that initializes the model and loops through time and records our observations

```r
## This function will handle our 'initialize' and 'observe' steps
simulateModel = function(x0, timeSteps){
  ## Start by putting the initial conditions as my first entry
  observe = x0
  ## Loop through timeSteps and update variable
  for(t in timeSteps){
    ## My new x at the next time step
    new_x  = updateDiscreteModel(x = observe[t], a = 1.1)
    ## Append this new observation to my running time series list
    observe = c(observe, new_x)
  }
  observe
}
```

Now to actually run this code, we can evaluate

```r
## Simulate one run and plot it
times  = 0:30
result = simulateModel(x0 = 1, timeSteps = times)
plot(result ~ times, cex = 0.5, type = 'b')
```

You should have a nice time-series of our simulation.

We can extend our model to the case of logistic growth by changing the `updateDiscreteModel` function like:

```r
## A more complicated updating function
updateDiscreteModel = function(x, r, k){
  x_t = x + r * x * (1 - (x / k))
  x_t
}
```

We already have the pieces we need to run this, just need to pass a couple of new arguments to our update function

```r
## This will be our initialize and observe steps
simulateLogistic = function(x0, timeSteps){
  ## Start by putting the initial conditions as my first entry
  observe = x0
  ## Loop through timeSteps and update variable
  for(t in timeSteps){
    ## My new x at the next time step
    new_x  = updateDiscreteModel(x = observe[t], r = 0.5, k = 10)
    ## Append this new observation to my running time series list
    observe = c(observe, new_x)
  }
  observe
}

## Simulate it!
times  = 0:30
result = simulateLogistic(x0 = 1, timeSteps = times)
plot(result ~ times, cex = 0.5, type = 'b')
```
