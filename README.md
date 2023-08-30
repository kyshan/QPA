# Quantum-Inspired Parallel Annealing for Combinatorial Optimization
This repository consists of the source code for the paper `Efficient combinatorial optimization by quantum-inspired parallel annealing in analogue memristor crossbar`, which is currently in archival status. This repository contains the code for implementing a quantum-inspired parallel annealing method for solving combinatorial optimization problems. 
## Code 

The code provided in this repository is an implementation of the quantum-inspired parallel annealing method for solving combinatorial optimization problems. It includes the following files:
- `QPA_code_demo.ipynb`: This main Jupyter notebook file contains the code that runs the essential part of the QPA algorithm.
- `adjMat.npy`: An example Max-Cut problem in the form of an adjacency matrix. This file is loaded by the main script.
- `README.md`: This file, providing an overview of the code and instructions for usage.

## Usage
To run the code, follow these steps:
1. Install `Jupyter Notebook` and set up your `Python3` runtime environment.
2. Make sure you have the following dependencies installed:
   - `numPy`
   - `matplotlib`
3. Download and unzip the code repository.
4. Open Jupyter Notebook and navigate to the extracted code repository directory.
5. Open the `QPA_code_demo.ipynb` file.
6. Execute the code cells in order to see the QPA algorithm in action.

## Algorithm
The algorithm implemented in this repository is a quantum-inspired parallel annealing method designed to solve combinatorial optimization problems efficiently utilizing memristor crossbar arrays.
```python
# load data, Initialize parameters
...

# run QPA
for ii in np.arange(numIterations):
    delHising = np.dot(-coulpingMatrix, spinVector)     # can be implemented experimentally by memristor crossbar array
    delHinit = xVector
    delHsystem = delHising + lamda[ii]*delHinit
    momentum = (1-adpRate)*momentum - adpRate*delHsystem    # update momentum
    momentum = np.clip(momentum,-1,1)   # clip momentum
    xVector += momentum     # update classical "superposition"
    xVector = np.clip(xVector,-1,1 )    # clip classical "superposition"
    spinVector = np.sign(xVector)
    for tt in np.arange(numTrials):  
        isingHamil[tt] = - 0.5*np.dot(spinVector[:,tt].T, (coulpingMatrix @ spinVector[:,tt]))      # calculate Ising Hamiltonian
    isingHamilHistory[ii] = isingHamil      # energy tracking

```
The algorithm involves the following key components:

- Initialization: This step involves loading data, choosing parameters, and initializing the memristor crossbar array with problem-specific constraints.

- Quantum-Inspired Annealing: This phase is the main computation loop of the algorithm. It performs multiple iterations denoted by numIterations and includes the steps for quantum-inspired annealing.


## Results

To evaluate the performance of the Ising Hamiltonian simulation, we conducted multiple trials and analyzed the results. The following steps were taken:

1. Simulating Ising Hamiltonian history: We performed `numTrials` trials of the simulation. Each trial consists of generating a random Ising Hamiltonian history matrix, `isingHamilHistory`, with dimensions (1000, numTrials). 

2. Plotting Ising Hamiltonian history: We visualized the Ising Hamiltonian history by plotting the values for each trial and the mean value across all trials. The following code was used:

```python
 for tt in np.arange(numTrials):
     plt.plot(isingHamilHistory[:, tt], color='palegreen')
 plt.plot(np.mean(isingHamilHistory, axis=1), color='darkgreen')
 plt.ylim(-200, 200)
 
 plt.show()
```
3. Computing success probability: We compared the final Ising Hamiltonian values with a known optimal solution, -188, to determine the success probability. The success probability is calculated as the ratio of trials where the final Ising Hamiltonian value matches the optimal solution to the total number of trials. The following code was used:
```python
successProbability = np.sum(isingHamilHistory[-1, :] == -188) / numTrials
print(successProbability)
```