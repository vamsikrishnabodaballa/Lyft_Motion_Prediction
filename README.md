# Lyft_Motion_Prediction


Autonomous vehicles (AVs) are expected to dramatically redefine the future of transportation. However, there are still significant engineering challenges to be solved before one can fully realize the benefits of self-driving cars. One such challenge is building models that reliably predict the movement of traffic agents around the AV, such as cars, cyclists, and pedestrians.

The ridesharing company Lyft started Level 5 to take on the self-driving challenge and build a full self-driving system. Their previous competition tasked participants with identifying 3D objects, an important step prior to detecting their movement. Now, they are challenging you to predict the motion of these traffic agents.

Our aim is to build motion prediction models for self-driving vehicles. You'll have access to the largest Prediction Dataset ever released to train and test your models. Your knowledge of machine learning will then be required to predict how cars, cyclists,and pedestrians move in the AV's environment.

Lyft’s mission is to improve people’s lives with the world’s best transportation. They believe in a future where self-driving cars make transportation safer, environment-friendly and more accessible for everyone. Their goal is to accelerate development across the industry by sharing data with researchers. As a result of your participation, you can have a hand in propelling the industry forward and helping people around the world benefit from self-driving cars sooner.

## Data Description

The Lyft Motion Prediction for Autonomous Vehicles competition is fairly unique, data-wise. In it, a very large amount of data is provided, which can be used in many different ways. Reading the data is also complex - please refer to Lyft's L5Kit module and sample notebooks to properly load the data and use it for training. Further Kaggle-specific sample notebooks will follow shortly.

Note also that this competition requires that submissions be made from kernels, and that internet must be turned off in your submission kernels. For your convenience, Lyft's l5kit module is provided via a utility script called kaggle_l5kit. Just attach it to your kernel, and the latest version of l5kit and all dependencies will be available.

The data is packaged in .zarr files. These are loaded using the zarr Python module, and are also loaded natively by l5kit. Each .zarr file contains a set of:

- **scenes:** driving episodes acquired from a given vehicle.
- **frames:** snapshots in time of the pose of the vehicle.
- **agents:** a generic entity captured by the vehicle's sensors. Note that only 4 of the 17 possible agent label_probabilities are present in this dataset.
- **agents_mask:** a mask that (for train and validation) masks out objects that aren't useful for training. In test, the mask (provided in files as mask.npz) masks out any test object for which predictions are NOT required.
- **traffic_light_faces:** traffic light information.

## What am I predicting?

We are predicting the motion of the objects in a given scene. For test, you will have 99 frames of objects moving around will be asked to predict their location in the next 50.

## Files

- **aerial_map** - an aerial map used when rasterisation is performed with mode "py_satellite"
- **semantic_map** - a high definition semantic map used when rasterisation is performed with mode "py_semantic"
- **sample.zarr** - a small sample set, designed for exploration
- **train.zarr** - the training set, in .zarr format
- **validate.zarr** - a validation set (roughly the size of train)
- **test.csv** - the test set, in .zarr format
- **mask.npz** - a boolean mask for the test set. All and only the agents included in the mask should be submitted
- **sample_submission.csv** - two sample submissions, one in multi-mode format, the other in single-mode

The goal of this competition is to predict the trajectories of other traffic participants. You can employ uni-modal models yielding a single prediction per sample, or multi-modal ones generating multiple hypotheses (up to 3) - further described by a confidence vector.

Due to the high amount of multi-modality and ambiguity in traffic scenes, the used evaluation metric to score this competition is tailored to account for multiple predictions.
The goal of this competition is to predict the trajectories of other traffic participants. You can employ uni-modal models yielding a single prediction per sample, or multi-modal ones generating multiple hypotheses (up to 3) - further described by a confidence vector.

We calculate the negative log-likelihood of the ground truth data given the multi-modal predictions. Let us take a closer look at this.
