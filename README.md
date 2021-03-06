# MMCE
This is an example of the Maximum Mean Calibration Error on 20 Newsgroups dataset with a global pooling CNN model, as described in the paper **Trainable Calibration Measures for Neural Networks from Kernel Mean Embeddings, ICML 2018**. The code is yet in its preliminary version and will be updated with other datasets, unit tests, comments and extensive documentation in near future. 

An example application of MMCE on 20 Newsgroups document classification is provided as of now. To run MMCE, follow the procedure given below:

In order to use MMCE for training, run:
```bash
python 20ng_mmce.py --mmce_coeff=8.0 --batch_size=batch_size > outfile.txt
```
In order to compute the ECE numbers, Brier score and test-NLL or to visualize the reliability plots, we then run the ```get_calibration.py``` script.
```bash
python get_calibration.py --file_name=outfile.txt --T=1.0 --N=10 --num_classes=20 --visualize=True
```
The flag ```T``` is used to control the temperature at which calibration is to be measured. ```N``` is used to control the number of bins used to compute ECE and ```num_classes``` should be set to the number of possible output classes for the dataset. If ```visualize``` is set to True, a matplotlib based reliability diagram should be generated.

The code makes use of Glove embeddings, which need to be downloaded and kept. We expect that the glove embeddings should be in the directory ```glove.6B``` relative to the src file, and the dataset should be located in the ```20newsgroup``` directory inside the base directory. The code for pre-processing the documents is borrowed from the Keras Pre-trained word embedding tutorial at https://blog.keras.io/using-pre-trained-word-embeddings-in-a-keras-model.html. 

Given is a script **```run_mmce.sh```**, which contains code for running MMCE training, with a default ```batch size=128``` and ```mmce_coeff=8.0``` and then generate the ECE, Brier Score and test-NLL numbers. Also, it contains the experiment to run MMCE training for different values of ```mmce_coeff``` and then generates results on these metrics. In order to use the script, just run:
```bash
bash run_mmce.sh
```
In order to look at the number of points predicted with confidence more than 99% for MMCE and Baseline, please use the following command:
```bash
python get_insights.py --file_name=<MMCE file name> --baseline_file_name=<Baseline File Name> --T=<Temperature for Baseline>
```
Usually the temperature for 20 Newsgroups baseline lies in the range of 0.3-0.4 (as found out by minimizing NLL with respect to only T). For MMCE, the temperature lies around 0.85.

## Dependencies
Tensorflow (>1.4), Keras (for preprocessing text files), Numpy, Python

## Results
We expect the ECE numbers to lie in the range of 6-7%, Brier score in the range of 0.34-0.38 and test NLL around 0.94-0.98 for the finetuned value of ```mmce_coeff=8.0 or 9.0```.  Baseline ECE for this code is expected to be around 16-18%, Brier score close to 0.40-0.42 and test NLL close to 1.35-1.5. For MMCE, we expect it to reduce the number of points with confidence >0.99 as compared to the baseline but would significantly beat Baseline+T. In a sample run, we obtained 42% of points with >0.99 confidence with MMCE, 16% with Baseline+T and 62% with the Baseline. 

## Disclaimer
In case of any queries or suggestions please contact Aviral Kumar at aviralkumar2907@gmail.com. 
