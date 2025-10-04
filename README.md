# Trigger Word Detection

1) Data Syntesis of Trigger Word Detection problem
- Audio clips of people saying positive words ('activate') and negative words (words other than 'activate') were collected.
- 10 sec audio clips of background noises were collected.
- There were two 10 secs background audio clips. 0-4 audio clips of 'activate' and 0-2 audio clips of negative words were randomly inserted into each of the background audio clips.
- The resultant audio clip (0-4 audio clips of 'activate' and 0-2 audio clips overlaid over background noise) is passed through spectogram to get an array of numbers. This is the input of model.
- The timestep when the person completes saying 'activate' was labelled as 1. The next 49 consecutive timesteps were also labelled as 1 accounting for the uncertainty in the exact time step the word finishes. This also helps to balance the dataset and improves the model's ability to learn from both positive and negative examples. These are the ground truth labels.
- A training set of 32 examples were created in a similar manner.
2) Development of Model
- Implemented a 1D convolutional layer with 196 filters, a kernel size of 15, and a stride of 4. The total timesteps were decreased from 5511 to 1375 to increase the computational efficieny of our RNN model.
- Incorporated batch normalization after the convolutional layer to maintain consistency in distribution of input being fed into the GRU blocks. This way, the weights of the GRU block can learn efficiently.
- Applied ReLU activation to introduce non-linearity, enabling the model to learn complex representations from the audio data.
- Utilized dropout regularization with a rate of 0.8 post-activation to prevent overfitting.
- Implemented dual-layered Gated Recurrent Units (GRUs) with 128 units each.
- Implemented additional dropout and batch normalization layers between GRU layers to further regularize the model and maintain consistent internal state representations.
- Applied dropout, batch normalization and again dropout on the output produced by the second GRU layer.
- Finalized the model with a time-distributed dense layer using sigmoid activation to provide a probability score at each timestep, indicating the likelihood of the trigger phrase's occurrence.
- Trained the model with 'binary_crossentropy' loss and Adam's optimization as the optimizing algorithm.
3) Insertion of Chime
- A logic was implemented to overlay a chime sound in the audio clip at the instant when the person completes saying the trigger word 'activate'.
