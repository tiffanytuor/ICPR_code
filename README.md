## Overcoming Noisy and Irrelevant Data in Federated Learning  
Tensorflow implementation of our paper : Overcoming Noisy and Irrelevant Data in Federated Learning (ICPR)

#### Prerequisites:
- Python 3.6
- Tensorflow 1.15.2
- Scikit_learn 0.24.1
- scipy 1.2.0
- xlrd 1.2.0
- numpy 1.16.4



### Structure of the project
The following structure is expected in the main directory:
```
./configs                   : Settings for each experiment
./datasets                  : Folders with dataset files and functions related to dataset
./datasets/daset_files      : Folders where dataset files need to be saved
./models                    : Model architectures
./results                   : Results are saved in here , this folder will be automatically created
./utils                     : Helper functions
/fl_training.py             : Scripts realted to federated training
/main.py                    : Main script
```


- To run the experiment use `/main.py --config <name> --approach <approach> --samples_per_dataset <samples per dataset>, --pc_validation <pc>, --sim  <sim>` where `name` is the name of the config file corresponding to the experiment [ `config_mnist_fashion`],
where `approach` is [`0`,`1`,`2`,`3`], with `0` for our proposed approach, `1` for 'ideal' baseline (i.e. without noise),  `2` 'random' baseline (i.e. filtered data is selected randomly), option `3` 'naive' baseline (i.e. focus dataset and noise are trained together), 
where `samples per dataset` is the number of data samples per dataset, where `pc` is percentage of the target/focus dataset to use as benchmark (i.e. validation) and `sim` is the number of runs to average the results
(Note: for baseline 1-3 `pc`  can be set to any value, as it doesn't matter for the final results)

###  Examples:


##### Our approoch 

```shell
python3 main.py --config config_mnist_fashion --approach 0  --samples_per_dataset 3000 --pc_validation 0.05 --sim 1 
````

##### Ideal Baseline

```shell
python3 main.py --config config_mnist_fashion --approach 1  --samples_per_dataset 3000 --pc_validation 0.05 --sim 1 

````
##### Random baseline

```shell

python3 main.py --config config_mnist_fashion --approach 2  --samples_per_dataset 3000 --pc_validation 0.05 --sim 1 

````

##### Naive Baseline 

```shell
python3 main.py --config config_mnist_fashion --approach 3  --samples_per_dataset 3000 --pc_validation 0.05 --sim 1 

````


### Experiment parameters:
In the config files, located in the configs folder,  one can specify: 



#####  Target/noise scenario 
- dataset_list : Each scenario is described as a list, where the first dataset of the list is the target/focus dataset and the remaining dataset(s) is the noise
 For example: 
  - dataset_list= [dataset_mnist,dataset_fashion]
  
  
##### Training (i.e. both validation and federated)
- n_category (i.e. number of classes in the target/focus dataset)
- model_name (i.e. specify architecture of the model for example : 'ModelCNNMnist')
- percentage_batch (i.e. size of the mini-batch, determines as a percentage of the total number of data)
- step_size


##### Specific to Federated Training
 
- total_iterations (i.e. number of iterations for federated training )
- n_nodes (i.e. number of nodes)


##### Folder
- dataset_file_path (i.e. Folder for dataset)
- results_file_path (i.e. Folder for results )

### Results Folder
The results folder contains different files:

#### Intermediary results :

- dico-order.csv : ranking of the samples by " importance" order, with their respective loss function values evaluate on the benchmark/validation model
- dico-validation.csv : loss function values obtained on the benchmark/validation dataset
- loss-accu-validation.csv : log of the training of the validation dataset

#### Final results : 
- loss-acc.csv :  final results with the following format :
   iterations, approach, loss_train_focus, loss_train_noise, accuracy_test_focus, accuracy_test_noise,  accuracy_test_validation

where :
- loss_train_focus is the loss value obtained on the focus/target dataset during training
- loss_train_noise is the loss value obtained on the noise dataset during training
- accuracy_test_focus is the testing accuracy obtained on the focus/target dataset 
- accuracy_test_noise is the testing accuracy obtained on the noise dataset 
- accuracy_test_validation is the testing accuracy obtained when training only on the benchmark/validation dataset (Note: only available for approach 0 )








