# random_search_optimization

### Badges:

[![Python](https://img.shields.io/badge/Python-3.8-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://www.tensorflow.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-violet.svg)](https://opensource.org/licenses/Apache-2.0)

<br>

### Motivation
While I was training a CNN, I had an idea in my mind where we update the weights randomly, without gradient descent and loss function, until we achieve a good solution or combination of these weights. I did not know what this algorithm was while I was implementing it.

Random search is a family of metaheuristic algorithms that rely on sampling random solutions from the search space and keeping the best solution found so far. I tried implementing this idea on the MNIST digit dataset by creating a simple neural network from scratch with 3 classic layers (hidden layer has 64 neurons). I researched on optimization algorithms that resemble the one I implemented. I didn't find an exact resemble, however, I found Particle Swarm Optimization (PSO) to be the closest one to what I did, albeit still far from what I did.

<br>

### Table of Contents

- [Algorithm](https://github.com/jl-csar/random_search_optimization/edit/main/README.md#algorithm)
- [Results](https://github.com/jl-csar/random_search_optimization/edit/main/README.md#results)
- [Further Developments and Ideas](https://github.com/jl-csar/random_search_optimization/edit/main/README.md#further-developments-and-ideas)
- [License](https://github.com/jl-csar/random_search_optimization/edit/main/README.md#license)

<br>

### Algorithm
Here is how I implemented a random search. Initially, I have a random set of parameters that is standardized between -1 and 1. The parameters should be updated by adding very small numbers to them. Say I have around 50,000 parameters, we want to update each parameter by adding very small random number to them (e.g., for the first parameter I add -0.04 to it; for the second parameter, I add 0.01; and so on.) These very small numbers are randomly generated, and is only within the range of [-0.05, 0.05].

For the first random search (or iteration), I have a random set of parameters that is between -1 and 1; this is the baseline parameters. Calculate the accuracy using this random parameters and this serves as our baseline accuracy. For the second random search, the network's parameters will also be updated by adding random small numbers within that range. Then, calculate the accuracy. If the accuracy is lower than the first random search's accuracy, then keep updating the parameters of the baseline parameters. If we get higher accuracy than the baseline accuracy, we use that higher accuracy as our baseline accuracy, and also the parameters that we got from that higher accuracy will now be used as our baseline parameters. If we get higher accuracy, we then proceed to the next epoch, and we start again another random search (or iteration) using the baseline accuracy and baseline parameters.

<br>

### Results
For the MNIST digit dataset, the model is better than a model randomly predicting the 10 digit classes. A random model should have only 10% percent accuracy on both training and test datasets. The model I trained gained about 62% percent training and test accuracy (or about 6/10). When I got to 60% accuracy, the model started to plateau. I already stopped training at 62% because, although it was still increasing, it was painfully slow. I only showed here up to 50% accuracy as it would take me time to reach 60%.

Most interestingly, the model did not overfit ever (see picture below). The test accuracy is always higher than the training accuracy by a very small margin. This is because random search introduces noise to searching the optimal parameters which acts as a regularizer, just like how a dropout regularization would do.

This does not involve any gradient descent which is one of the reasons why one epoch is done for roughly two seconds only. Plus, no loss function is calculated which reduces computation time.

In earlier epochs, the accuracy increases fast. However, in later epochs, the accuracy, while still increasing, increases very slowly. At 62%, it was only increasing by about 0.002%

![Image](https://github.com/jl-csar/random_search_optimization/blob/main/training_results/accuracy%20graph.png)

<br>

### Further Developments and Ideas

1. When I was training a CNN model for Cassava Leaf Disease Classification (check my repo), I thought of using this algorithm as a regularizing effect. If the validation and training accuracies are more than 3% apart for 3 straight consecutive epochs, I added random updates to selected parameters. The random updates to the baseline parameters only stops when it produces an accuracy greater than the baseline accuracy and when the training and validation accuracies are equal to or less than 3% apart. It gave a regularizing effect for every random update / iteration. However, I could not see its effect in the long run because tensorflow calculates the gradient and loss for every random update / iteration which is a big computation overhead. Each random update / iteration would take around 58 seconds. This is why I implemented it on this MNIST digit dataset without extensively using external deep learning frameworks.

2. The random updates in the MNIST digit classification were done for each parameter. However, one could choose which parameters should be updated. In the case of number 1 above, I randomly chose some parameters (about 40%) of all the trainable parameters. The choice of parameters for every random update / iteration is always different and randomly chosen.

3. The model is plateauing around 62% accuracy and is most probably stuck in a local optima. To solve this, I devised another method which is to let the model explore the search space which I call "branching out". When the model is not finding a better set of parameters than the baseline parameter (the trunk) for a certain number of iterations, the parameters of that iteration becomes the baseline parameters, and another round of iteration happens as usual, and so on, until it creates a branch. If it doesn't find a better set of parameters for that branch, we go back to the original set of parameters (the trunk), and start another branch. This loops until we find an optimal solution. Imagine this is as a tree (or the roots of a tree) with many branches (or roots) that search for solutions within its vicinity.

4. One could also make the range of random update dynamic. In the later epochs of training, I found that minimizing the range leads to faster search of better parameters. However, as mentioned earlier, the increase in accuracy is very small.

<br>

### License

This project is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).

The Apache License 2.0 is a permissive open-source license that allows you to use, modify, and distribute the software for any purpose, including commercial purposes, as long as you satisfy the conditions of the license.

Copyright 2024 Julius Ceasar Dumaslan. All rights reserved.
