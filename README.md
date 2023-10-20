This is the third iteration of my project on wildfire prediction with machine learning.
The files are my Kaggle notebooks.

PROBLEM:
Here, I treat wildfire prediction as a binary classification task, classifying a future timestep for each pixel in a satellite image as either "yes fire" or "no fire." The degree of certainty can be interpreted as wildfire risk.

DATA:
I used the Seasfire Datacube introduced in the "Deep Learning for Global Wildfire Forecasting" paper here: https://s3.us-east-1.amazonaws.com/climate-change-ai/papers/neurips2022/52/paper.pdf
It contains global satellite data spanning 21 years (2001-2021) and has a spatial resolution of .25 deg x .25 deg and a temporal resolution of 8 days. While the data is of lower quality than larger and higher resolution datasets, it can be useful for rough tasks with lower resource costs.
Despite this low resolution, it can be a more accessible data format for wildfire risk prediction in smaller regions and can allow for the use of more variables input into the model.
For this specific project due to memory restrictions, I only used a 10 deg x 10 deg spatial region containing my home state of Califonia, which is known for a history of devastating wildfires, and I chose 29 variables covering both weather and geographical conditions.
Due to the infrequent and stochastic nature of wildfires, I roughly followed the sampling procedure used in "Wildfire Danger Prediction and Understanding With Deep Learning" https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2022GL099368 by sampling two negatives cases for every positive.

MODELS:
Traditional:
Random Forest: 
- Take in time-series of the past 64 timesteps to predict yes or no fire

Deep Learning:
"ConvTran" Transformer described in "Improving position encoding of transformers for multivariate time series classification" https://link.springer.com/article/10.1007/s10618-023-00948-2
- Implemented transformer with novel improvements specifically designed for time-series multivariate classification tasks
- Utilizes convolutions to aid in more efficient and effective embedding of the data
- Takes past 64 timesteps for one pixel at a time, no spatial context
Convolutional Transformer
- novel addition adding a convolutional layer to the input of the transformer to encode spatial context
- takes data from each neighboring pixel
Transformer utilized in the paper "Mesogeos: A multi-purpose dataset for data-driven wildfire modeling in the Mediterranean" https://arxiv.org/pdf/2306.05144.pdf
- basic transformer encoder that performed well in previous research

FINDINGS:
ConvTran performed the best, achieving an f1 score of 0.7042136921565159
ConvTran+spatial convolution was second, with an f1 of 0.6306645118473377
Mesogeos Transformer encoder got f1 of 0.6048925467730613
Random forest was worst with an f1 of 0.4747256177105379

These metrics demonstrate the potential that deep learning has for applications in the field of wildfire predicting. 
The ConvTran model specialized for time-series problems like this one achieved an impressive f1 score despite the low quality of the data.
The ConvTran with the added spatial convolution actually performed quite a bit worse, signaling that the data's resolution was not high enough for spatial context to provide any useful information. Further tests should be done on larger datasets.
The Transformer encoder that was used in the Mesogeos paper (and achieved an f1 of .780 on their data) performed third worse, showing the potential of the two newer models proposed above if applied to stronger data.
As expected, the traditional random forest model was outclassed and could not handle the complexity of this task as adequately as the other three deep learning models.

Overall, further studies should be conducted with novel algorithms with better data for this significant problem. It is also worth exploring more framings or approaches to this challenge (anomaly detection, danger ranking, etc). 

citations:
Foumani, N.M., Tan, C.W., Webb, G.I. et al. Improving position encoding of transformers for multivariate time series classification. Data Min Knowl Disc (2023). https://doi.org/10.1007/s10618-023-00948-2

Spyros Kondylatos, Ioannis Prapas, Gustau Camps-Valls, & Ioannis Papoutsis. (2023). 
Mesogeos: A multi-purpose dataset for data-driven wildfire modeling in the Mediterranean. 
Zenodo. https://doi.org/10.5281/zenodo.7473331

Alonso, Lazaro, Gans, Fabian, Karasante, Ilektra, Ahuja, Akanksha, Prapas, Ioannis, Kondylatos, Spyros, Papoutsis, Ioannis, Panagiotou, Eleannna, Michail, Dimitrios, Cremer, Felix, Weber, Ulrich, & Carvalhais, Nuno. (2022). 
SeasFire Cube: A Global Dataset for Seasonal Fire Modeling in the Earth System (0.0.1) [Data set]. 
Zenodo. https://doi.org/10.5281/zenodo.7108392
