## Eluvio-Scene-Segmentation-Challenge
# Methodolgy 
* The dataset is divided into 4 sub-datasets in order to use transfer learning to train the model. The first 3 sub-datasets are used to train and validate the model while the 4th one is used for testing. The first 3 sub-datsets contain shot features for 17 movies each and the 4th one contains shot features for 13 movies in order to achieve a train-validation-test split of 65-15-20.
* This method uses a sliding window approach to generate feature embeddings that are used to represent the boundary after the central shot. 
* To implement the sliding window approach the ends of the movie were padded with zeros and the position of the central shot was determined using this formula int((window-1)/2). Where in the size of the window must be an odd intager greater than 1. 
* Four seperate convolutional networks with the same architecture are trained on each of the features seperately. The main benefit of this is more efficient use of computational resources. 
* The learned embeddings from the output of the convolutional layers are then merged together and used as an input for a dense network which outputs the predictions. 
* Since there is an imbalance between the true scene boundaries and shot boundaries a Focal loss function with sigmoid activation and an SGD optimizer is used. Furthermore, this loss function obtained the best results when compared with binary cross-entropy loss. 
# Challenges 
One of the main challenges of this approach is that it is compuatationally very expensive. Hence, the model is trained using transfer learning by first training the model on sub-dataset 1, saving the weights and these weights are updated by training the model on sub-dataset 2. 
# Results
Two metrics are used to evaluate the performance of this method. Mean Average Precision and Mean Maximu IoU. 
This method achieved a MAp score of 0.152 and Miou of 0.332. 
# Alternative Methods
The optimization problem could be approached as a grouping problem. Once the shot feature vectors are obtained they could be used to find the pair wise distance between shots thus essentially creating a distance matrix for the shots. The scenes in this distance matrix will form a diagonal block-like structure where each block corresponds to a scene. The idea behind this structure forming is that shots within the same scene will have a low distance value as compared to those from different scenes. Finally, a distance based cost function can be optimized to split the shots into non intersecting groups where in these non-intersecting groups represent a scene. The idea has been introduced in this paper https://dl.acm.org/doi/10.1145/3206025.3206055 by Daniel Rotman et al. 
