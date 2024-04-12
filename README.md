# random_parameter_update

# Motivation
I had an idea in my mind where we update the weights randomly until we achieve a good solution or combination of these weights. I tried implementing this idea on the MNIST digit dataset by creating a simple neural network from scratch with 3 classic layers (hidden layer has 64 neurons). The algorithm goes something like this: 


### Algorithm
Initially, i have a random set of parameters that is between -1 and 1. The parameters should be updated by adding very small numbers to them. Say i have around 50,000 parameters, we want to update each parameter by adding very small random number to them (for example, for the first parameter i will add -0.08 to it. for the second parameter, i will add 0.07, and so on.) These very small numbers are randomly generated, and is only within the range of [-0.05, 0.05]. For the first random search (or iteration), i have a random set of parameters that is between -1 and 1, this is the baseline parameters. Calculate the accuracy using this random parameters and this serves as our baseline accuracy. For the second random search, the network's parameters will be updated using the method i said earlier (by adding small numbers within that range), then calculate the accuracy. If the accuracy is lower than the first random search, then keep updating the parameters using that method. If we get higher accuracy than the baseline accuracy, we use that higher accuracy as our baseline accuracy, and also the parameters that we got from that higher accuracy will now be used as our baseline parameters. If we get higher accuracy, we then proceed to the next epoch just like what you coded and printed, and we start again another random search (or iteration) using the baseline accuracy and baseline parameters.


### Results
The model is better than a model randomly predicting the 10 classes. A random model should have only 1/10 percent accuracy on both training and validation dataset, but the model I trained using this inefficient algorithm gained 0.62 percent accuracy (or about 6/10). The validation accuracy is 1% lower than the training accuracy. The model did not overfit ever.

This didn't involve gradient descent, it used some kind of random search optimization algorithm. I researched on optimization algorithms that resemble the one I did. I didn't find an exact resemble, however, I found Particle Swarm Optimization (PSO) to be the closest one to what I did. I wasn't able to implement a PSO algorithm as it was having a bug, but I would continue it in the coming days, I guess.

I'm still looking forward to improving this kind of optimization using only random updates.
