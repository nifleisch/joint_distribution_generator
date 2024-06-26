# Joint Distribution Generator

## Overview

In this project, we have implemented a generator capable of producing samples from a custom joint distribution defined by marginal distributions (using `scipy.stats`) and a correlation matrix. The core idea is to use a neural network to transform random uniform noise into the desired joint distribution. This neural network processes multiple uniform noise samples together and transforms them into a sample set of the approximated target distribution.

The loss function, computed based on a set of generated samples, consists of two components:
- Correlation Loss: The generated samples are used to compute their empirical correlation matrix. The loss is then calculated by taking the absolute difference between this empirical correlation matrix and the desired correlation matrix.

- Marginal Loss: For each marginal distribution, differentiable sorting (using the `torchsort` library) is employed to compute differentiable quantiles of the samples. The loss is then determined by computing the absolute difference between these empirical quantiles and the actual quantiles of the desired marginal distribution scaled.


## Installation

Follow these steps to set up the project environment and install the required libraries:

1. **Create a virtual environment**:
    ```bash
    python -m venv venv
    ```

2. **Activate the virtual environment**:
    - On Windows:
      ```bash
      venv\Scripts\activate
      ```
    - On macOS/Linux:
      ```bash
      source venv/bin/activate
      ```

3. **Install the required libraries**:
    ```bash
    pip install -r requirements.txt
    ```

## Usage

To train the model, follow these steps:

1. **Configure the marginals and the correlation matrix**: Edit the `train.py` script to set up the desired marginal distributions and the correlation matrix for the joint distribution.

2. **Run the training script**:
    ```bash
    python train.py
    ```

### Output

After the model is successfully trained, you can find the following outputs in the `/plots` folder:

- Plots showing the generated marginals plotted against the true marginal distributions.
- A comparison of the predefined and empirical correlation matrices.


## Results

The results of running train.py are shown below. The code ran for approximately 15 minutes on my MacBook Air. The plots indicate that this approach yields good approximations for the marginals while also maintaining the correlation. I believe there is still room for improvement through hyperparameter tuning, particularly for the neural network.

### Correlation Matrix

![Correlation Matrix](plots/correlation.png)

### Marginal Distributions

![Marginal Distributions](plots/marginals.png)

## TODO

- Many datasets also include categorical data, which is not currently supported. One potential solution is to use the Gumbel trick to generate differentiable samples of such distributions.
- It would be neat to anonymize a given dataset. This would involve first fitting distributions to the dataset, then using these distributions and the correlation to generate an anonymized version of the dataset.
---
