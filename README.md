# Zaznavanje pljučnega raka na podlagi CT slik


# Luna16
**Disclaimer**: We are not the host of the data.
Please make sure to read the requirements and usage policies of the data and **give credit to the authors of the dataset**!

Please read the information from the homepage carefully and follow the rules and instructions provided by the original authors when using the data.
- Homepage: https://luna16.grand-challenge.org/Home/

## Setup
0. Follow the installation instructions of nnDetection and create a data directory name `Task016_Luna`.
1. Follow the instructions and usage policies to download the data and place all the subsets into `Task016_Luna / raw`
2. Run `python prepare.py` in `projects / Task016_Luna / scripts` of the nnDetection repository.

The data is now converted to the correct format and the instructions from the nnDetection README can be used to train the networks.

Notes:
- since Luna is a 10 Fold cross validation, all 10 folds need to be run
- all runs should be run with the `--sweep` option and consolidation should be performed via the `--no_model -c copy` since we are not planning to predict a separate test set.

## Evaluation
1. Run `python prepare_eval_cpm.py [model_name]` to convert the predictions to the Luna format.
Note: The script needs access to the raw_splitted images.
2. Download and run the luna evaluation script.


Docker
The easiest way to get started with nnDetection is the provided is to build a Docker Container with the provided Dockerfile.

Please install docker and nvidia-docker2 before continuing.

All projects which are based on nnDetection assume that the base image was built with the following tagging scheme nnDetection:[version]. To build a container (nnDetection Version 0.1) run the following command from the base directory:

docker build -t nndetection:0.1 --build-arg env_det_num_threads=6 --build-arg env_det_verbose=1 .
(--build-arg env_det_num_threads=6 and --build-arg env_det_verbose=1 are optional and are used to overwrite the provided default parameters)

The docker container expects data and models in its own /opt/data and /opt/models directories respectively. The directories need to be mounted via docker -v. For simplicity and speed, the ENV variables det_data and det_models can be set in the host system to point to the desired directories. To run:

docker run --gpus all -v ${det_data}:/opt/data -v ${det_models}:/opt/models -it --shm-size=24gb nndetection:0.1 /bin/bash
Warning: When running a training inside the container it is necessary to increase the shared memory (via --shm-size).
