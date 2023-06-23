# CRYPTANALYTIC EXTRACTION OF NEURAL NETWORK MODELS

This repository contains an implementation of the model extraction attack in our CRYPTO'20 paper

Cryptanalytic Extraction of Neural Network Models
https://arxiv.org/abs/2003.04884
Nicholas Carlini, Matthew Jagielski, Ilya Mironov


## INSTALLING

Install additional libraries that are needed for the python installation

```
sudo apt install libssl-dev zlib1g-dev build-essential libffi-dev libbz2-dev
```

First we need to make sure, we use a python version lower than `3.11`. This repo was testes succesfully with version `3.10.0`

```
python3.10 -m venv cme-venv
source cme-venv/bin/activate
pip install -r requirements.txt
```

Sometimes JaX (or, more correctly, XLA) puts up a fight during the install,
but if the above works then everything should run properly.

### Installing python3.10

1. Download python
    ``` bash
    cd /usr/src
    sudo wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
    ```
2. Extract
    ```
    sudo tar -xzf Python-3.10.0.tgz
    ```
3. Compile and install
    ```
    cd Python-3.10.0
    sudo ./configure --enable-optimizations
    sudo make altinstall
    ```
    `make altinstall` is used to prevent replacing the default python binary file /`usr/bin/python`


## EXTRACTING EXAMPLE MODELS

First, generate a model that we will extract by running

> python3 train_models.py 10-15-15-1 42

and then extract it with

> python3 extract.py 10-15-15-1 42

this should be quick to extract and then check the quality of this extraction with

> python3 check_solution_svd.py 10-15-15-1

or if you have MILP solver installed you can run

> python3 check_solution_milp.py 10-15-15-1

and then running the solver on /tmp/test.mod

By default, the code is set up so that it won't cheat and look at the weights of the
actual neural network we're extracting (and will throw ugly errors if we try).
Some logging looks better if we're allowed to cheat though (e.g., to catch errors
earlier in the process).

To enable this, set CHEATING=True in src/global_vars.py.


## EXTRACTING YOUR OWN MODELS

The code can currently extract only fully-connected neural networks.

To extract a model, save it as a numpy array in the format [weights, biases]. For
example, a 20-10-1 network could be saved to models/UID_20-10-1.npy
[[np.random.normal(size=(20,10)), np.random.normal(size=(10, 1))], [np.zeros((10,)), np.zeros((1,))]]

and then run

> python extract.py UID 20-10-1


## CITING THIS WORK

If you find this code useful you can cite

@article{carlini2020cryptanalytic,
  title={Cryptanalytic Extraction of Neural Network Models},
  author={Carlini, Nicholas and Jagielski, Matthew and Mironov, Ilya},
  booktitle={Annual International Cryptology Conference},
  year={2020}
}
