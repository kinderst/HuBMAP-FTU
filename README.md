# HuBMAP + HPA - Hacking the Human Body

Kaggle Competition Link: [https://www.kaggle.com/competitions/hubmap-organ-segmentation](https://www.kaggle.com/competitions/hubmap-organ-segmentation)

> In this competition, you’ll identify and segment functional tissue units (FTUs) across five human organs. You'll build your model using a dataset of tissue section images, with the best submissions segmenting FTUs as accurately as possible.

To see what this looks like, here are two samples and their associated masks in black and white:

![Sample A](/img/tissue-a.PNG "Sample A")

![Mask A](/img/mask-a.PNG "Mask A")

<br>

![Sample C](/img/tissue-c.PNG "Sample C")

![Mask C](/img/mask-c.PNG "Mask C")

## The Approach

This problem can be formulated as a binary [semantic segmentation](https://www.mathworks.com/solutions/image-video-processing/semantic-segmentation.html) task. [Recent advancements](https://arxiv.org/abs/1802.02611) by Google's DeepLab team have proven to be very effective at the semantic segmentation task. The idea was to apply the model architecture, "DeepLabv3+" to these medical images and see how well it works.

## Model Architecture

From Google's blog post [here](https://ai.googleblog.com/2018/03/semantic-image-segmentation-with.html) we can see at a high level the architecture for the model.

![Sample](/img/model-arch.PNG "Model Architecture")

A closer look at the specific architecture is in the script and Keras implementation [here](https://keras.io/examples/vision/deeplabv3_plus/)

Our initial guess should be that this architecture should work well on the images above, as the atrious convolution will allow a wider field of view looking at the regions, which I suspect could help see the more general FTU pattern. Otherwise, I don't see any glaring issues with this architecure for this approach

## Results

The way this problem is scored is based on the [Sorenson-Dice Coefficient](https://en.wikipedia.org/wiki/S%C3%B8rensen%E2%80%93Dice_coefficient) . This is essentially a measure of how much the pixels of your predicted class align with the pixels of the ground truth class. A score of 1 means perfect alignment (you perfectly segmented the mask), and a score of 0 means you didn't get any pixels correct.

The results from this training showed that on the validation dataset, a dice coefficient of .788 was scored, thus showing very significant overlap in the predicted mask versus the ground truth. Further potential improvements to the model are listed at the bottom of the notebook script. The results from 6 randomly selected images from the validation set (i.e. that the model has never technically seen before) are shown below.

![Results A](/img/all-a.PNG "Results A")

![Results B](/img/all-b.PNG "Results B")

![Results C](/img/all-c.PNG "Results C")

![Results D](/img/all-d.PNG "Results D")

![Results E](/img/all-e.PNG "Results E")

![Results F](/img/all-f.PNG "Results F")

Since this was a Kaggle competition, the implied test data was evaluated by the website since they do not want to release the test data, and unfortunately this model did not score as high on the leaderboard as I would have liked. This is because one major focus of this Kaggle competition was the ability to segment these FTUs as they come in from different imaging sources, while the training data was from just one source. Generally CNN models can have difficulties when dealing with different images sources, because the different sources can have variations in contrast, brightness, or other numerous sources of noise which can throw of the patterns that the CNN learned from training on a particular source. There has been a lot of research on how to mess with CNNs by applying noise in particular ways to the input image, and this is essentially the phenomenon when we get medical images from different sources. So unfortunately that was a difficulty, but otherwise I was very satisfied with the .788 dice score on the validation dataset, considering the winning score was 0.83562, so approximately just 0.05 better, but of course they are far, far more generalizable to different input images.