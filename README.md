# ML4SCI Genie Tasks
### The tasks can be accessed from [here](https://docs.google.com/document/d/142YpKV7fJ49zaBZkSBekbBzw43KD71No2K_Jd-n5Neo/edit) 

### Applying for: - **Specific Task 3 - "Learning the Latent Structure with Diffusion Models"**

#### DATASET - Data of Quark/Gluon jet events available [here](https://drive.google.com/file/d/1WO2K-SfU2dntGU4Bb3IYBp9Rh7rtTYEr/view?usp=sharing). The dataset consists of 3 channels (ECAL, HCAL and Tracks) each containing 125x125 images.

<details>
<summary>Sample Image channels</summary> 
  
![input image](https://github.com/pratyush-1/DeepFalcon/blob/main/assets/img.png)
</details>

<details>
<summary>Common Task 1</summary> 
  
### Common Task 1. [Auto-encoder of the quark/gluon events](https://github.com/pratyush-1/Genie/blob/main/autoencoder.ipynb)

* Please train a variational auto-encoder to learn the representation based on three image channels (ECAL, HCAL and Tracks) for the dataset. 

* Please show a side-by-side comparison of original and reconstructed events. 

### Variational Autoencoder reconstructions vs original image
![input image](https://github.com/pratyush-1/Genie/blob/main/assets/img.png)
![AE reconstruction](https://github.com/pratyush-1/Genie/blob/main/assets/ae.png)

### DISCUSSION - 
* As the data doesn't contain normal RGB channels and instead has different channels like ECAL,HCAL,Tracks ,data preprocessing needs to be chosen carefully

* Model architecture might not be too complex to extract the patterns in the underlying data

* Since Images are highly structured data, the pixels are arranged in a meaningful way. If the way pixels are arranged changes then we lose the meaning , hence here convolutions may not work as we aren't dealing with our normal RGB channels image data.

* Instead working with other type of data like graphs (aka Graph Neural Networks) would give better results by extracting features in the graphical representation of the given images.
</details>

<details>
<summary> Common Task 2</summary>
  
### Common Task 2. [Jets as graphs](https://github.com/pratyush-1/Genie/blob/main/gnn.ipynb) 

* Please choose a graph-based GNN model of your choice to classify (quark/gluon) jets. Proceed as follows:
  1. Convert the images into a point cloud dataset by only considering the non-zero pixels for every event.
  2. Cast the point cloud data into a graph representation by coming up with suitable representations for nodes and edges.
  3. Train your model on the obtained graph representations of the jet events.
* Discuss the resulting performance of the chosen architecture.
  
### RESULTS - 
| Model | Test Accuracy | Validation Accuracy | 
| :-------: | :----: | :----: | 
| GCN (k=10) | 0.6947 | 0.6950 | 
| GCN (k=5) | 0.6943 | 0.6885 | 
| GCN (k=2) |  0.6887 | 0.6865 | 
| GAT(k=10) | 0.6950 | 0.7060 | 
| GAT (k=5) | 0.6917 | 0.6980 | 
| GAT (k=2) |  0.6870 | 0.6955 | 
| SageConv (k=10) | 0.6990 | 0.7080 | 
| SageConv (k=5) | 0.6973 | 0.7025 | 
| SageConv (k=2) |  0.6930 | 0.7040| 
| GraphConv (k=10) | 0.7070 | 0.7170 | 
| GraphConv (k=5) | 0.6857 | 0.6930 | 
| GraphConv (k=2) |  0.6923 | 0.7070 | 


### DISCUSSION - 
* Accuracy difference between the different vaues of n_neighbors is small, indicating that the various architectures employed are not very sensitive and are robust to different values of n_neighbors

* Brief about architectures used - 
    1. GCN operates by aaggregating feature information from neighboring nodes in graph to update central nodes representation. It can only take node features as input.

    2. GAT introduces attention mechanisms to weigh importance of neighboring nodes when aggregating information. It can take both node features and edge features.

    3. SageConv operates by sampling and aggregating features from neigboring nodes. It incorporates pooling operations to aggregate information from neighboring nodes. It can only take node features as input.

    4. GraphConv aggregates information from neighboring nodes using weighted combination of node features.It can only take node features as input.

* All of them achieve an accuracy in range of 68-70%, an method to improve the accuracy could be to either deepen the current neural network architectures or apply networks that could model longer range dependencies and capture more complex patterns like Graph Transformer Networks or Graph Isomorphism networks.

</details> 


<details>
<summary>Specific Task 3</summary>
  
### Specific Task 2. [“Learning the Latent Structure with Diffusion Models"](https://github.com/pratyush-1/Genie/blob/main/diffusion.ipynb) 

* Use a Diffusion Network model to represent the events in task 1. Please show a side-by side comparison of the original and reconstructed events and appropriate evaluation metric of your choice that estimates the difference between the two.
  
### RESULTS - 
![forward diffusion](https://github.com/pratyush-1/Genie/blob/main/assets/forward_diff.png)
![reverse diffusion](https://github.com/pratyush-1/Genie/blob/main/assets/backward_diffusion.png)

![reconstruction on test img](https://github.com/pratyush-1/Genie/blob/main/assets/reconstruction.png)


## DISCUSSION
* Implemented both DDPM(Denoising Diffusion Probabilistic Model) and DDIM (Denoising Diffusion Implict Models), the reconstruction seemed to be bad when tried on test image, but on training data reconstruction from a random noise seemed to improve over period of time. Potential reason could be the number of samples taken are less.

* The choice of scheduler is very important, here I have used linear scheduler

* As the data doesn't contain normal RGB channels and instead has different channels like ECAL,HCAL,Tracks normal convolutions might not be a good choice as they are good in extracting features from normal structured images. If the pixels change then the image loses its meaning.

* A choice could be to implement diffusion in the graphs  https://arxiv.org/abs/1911.05485, after converting the data into a graphical representation.

</details> 
