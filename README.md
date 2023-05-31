# Multimodal Human Trajectory Prediction
## Motion Indeterminacy Diffusion (MID) on the Nuscene Dataset
This repository presents our work on implementing the pedestrian trajectory prediction model, Motion Indeterminacy Diffusion (MID), with the aim of training and evaluating it on the [Nuscenes dataset](https://www.nuscenes.org). Our implementation is based on the original code from [MID Github Repository](https://github.com/Gutianpei/MID/tree/main).

The [Nuscenes dataset](https://www.nuscenes.org) poses unique challenges compared to the original ETH dataset that the MID model was designed to work with. As such, one of the primary components of our project involved adapting the preprocessing steps to suit the characteristics of the Nuscene dataset.

We undertook a rigorous process of fine-tuning the MID model's hyperparameters to optimize its performance for the Nuscene dataset. This proved to be a complex task due to the intricacies of the MID model and the peculiarities of the data.

To ensure an accurate and fair assessment of the MID model's performance, we developed an equivalent evaluation method. Our goal was to enable an unbiased comparison with other models, specifically Trajectron++.

Our evaluation, performed across various prediction horizons, resulted in valuable insights into the performance of the MID model and highlighted areas where we could refine and improve our implementation.

While the Trajectron++ model consistently outperformed the MID model, we believe this provides us with an opportunity to further investigate and understand the underlying factors. These findings not only underline the robustness and adaptability of Trajectron++ but also underscore the potential areas of improvement for our application of the MID model.

This project serves as a valuable reference for those looking to apply and compare different pedestrian trajectory prediction models on the Nuscene dataset. It also highlights the nuances and considerations required when applying these models to datasets they were not originally designed for.

For a more in-depth understanding of our work, we recommend going through the code and associated documentation available in this repository. Contributions, questions, and feedback are most welcome.

## To process, train and evaluate the model you can run the run_mid.ipynb notebook
Choose to proportion of the Nuscenes dataset you want to work with in process_data_nuscenes.py (10% if you don't change).
Make sure the eval_mode is set to False in your own config file in /configs to train, and True to evaluate.
Choose the prediction horizon you want in /utils/trajectron_hypers.py.

### Data setup

#### Pedestrian Datasets

If you want to use the MID model on Pedestrian Datasets (ETH-UCY), please follow the original [MID Github Repository](https://github.com/Gutianpei/MID/tree/main).

#### NuScenes Dataset

Download the [Nuscenes dataset](https://www.nuscenes.org) (this requires signing up on [their website](https://www.nuscenes.org)). Note that the full dataset is very large, so if you only wish to test out the codebase and model then you can just download the nuScenes "mini" dataset which only requires around 4 GB of space. Extract the downloaded zip file's contents and place them in the "/v1.0/v1.0-trainval" directory (use only the metadata file). Then, download the latest map expansion pack and copy the contents of the extracted maps folder into the "v1.0/maps" folder. Finally, process them into a data format that our model can work with, by running the following :
Install requirements :
```
pip install -r requirements.txt
```
Process Nuscenes data :
```
python drive/MyDrive/DLAV-2023/MID/process_data_nuscenes.py --data=./v1.0 --version="v1.0-trainval" --output_path=drive/MyDrive/DLAV-2023/MID/processed_data_noise
```

For more explanation, follow the directives on the [Nuscenes website](https://www.nuscenes.org).

### Model Training
You can adjust parameters in config file as you like and change the network architecture of the diffusion model in models/diffusion.py
Make sure the eval_mode is set to False in your own config file in /configs.
Run the following commande :
```
python drive/MyDrive/DLAV-2023/MID/main.py --config drive/MyDrive/DLAV-2023/MID/configs/nuscenes.yaml --dataset "nuscenes "
```
Logs and checkpoints will be automatically saved.

### Model evaluation
Make sure you have a trained model.
Make sure the eval_mode is set to True in your own config file in /configs.
Choose the prediction horizon you want in /utils/trajectron_hypers.py.
Run the following command :
```
python drive/MyDrive/DLAV-2023/MID/main.py --config drive/MyDrive/DLAV-2023/MID/configs/nuscenes.yaml --dataset "nuscenes "
```
