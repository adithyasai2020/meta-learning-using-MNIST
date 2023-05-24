# meta-learning-using-MNIST
Meta learning is a learning method which aims to learn new features using limited number of samples by using knowledge of known features.
In this Project I first made a CNN network and trained it on MNIST dataset(pretty standard stuff). Then I made another CNN network and trained it only on images of numbers (0-6). The model has not seen any sample of 7, 8 and 9. Its one -hot vector output is of shape (7,). Now this model is introduced to the classes [7, 8, 9]. The constraint here is that we have only limited samples for training. Thus here the concept of meta learning comes in handy.

I took the model trained on 0-6 and dropped the final flatten and dense layers and used the resulting model as a feature extractor.
I defined "Tasks" and each task has 2 sets of samples - support set(3 samples per class[7, 8, 9]) and querry set(2 samples per class).
A set of tasks is called an episode and during training, iteration over all episodes is considered as an epoch.

Meta Model architecture : feature extractor of previous model + new classifier layers

Training phase - 
the meta model is trained over each task separately. For each task, a copy of the meta model is made, support set is passed into this copy, loss is calculated and gradients w.r.t. this copy is calculated and updated. Then querry set is passed into this new copied model and this new loss is added to a variable called "outer_loss", which adds all querry set losses over the episode. Then the gradients over the original meta model is calculated w.r.t. this outer_loss and the weights are updates.


Results : The neta model has precision and recall of around **93%**  This is pretty phenomenal result compared to the fact that the classes (0-6) are trained over around **1000 samples per class** and the classes [7, 8, 9] are trained on only **5 samples per class**.
