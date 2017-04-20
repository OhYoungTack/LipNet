#LipNet: End-to-End Sentence-level Lipreading
Keras implementation of the method described in the paper 'LipNet: End-to-End Sentence-level Lipreading' by Yannis M. Assael, Brendan Shillingford, Shimon Whiteson, and Nando de Freitas (https://arxiv.org/abs/1611.01599).

![LipNet performing prediction (subtitle alignment only for visualization)](assets/lipreading.gif)

##Dependencies
* Keras 2.0+
* Tensorflow 1.0+ / Theano 0.9+
* PIP (for package installation)

Plus several other libraries listed on `setup.py`

##Usage
To use the model, first you need to clone the repository:
```
git clone https://github.com/rizkiarm/LipNet
```
Then you can install the package:
```
cd LipNet/
pip install -e .
```
**Note:** if you don't want to use CUDA, you need to edit the ``setup.py`` and change ``tensorflow-gpu`` to ``tensorflow``

You're done!

Here is some ideas on what you can do next:
* Modify the package and make some improvements to it.
* Train the model using predefined training scenarios.
* Make your own training scenarios.
* Use pre-trained weight to do lipreading.
* Go crazy and experiment on other dataset! by changing some hyperparameters or modify the model.

##Dataset
This model uses GRID corpus (http://spandh.dcs.shef.ac.uk/gridcorpus/)

##Training
There are five different training scenarios that are (going to be) available:

###Prerequisites
1. Download all video (normal) and align from the GRID Corpus website.
2. Extracts all the videos and aligns.
3. Create ``datasets`` folder on each training scenario folder.
4. Create ``align`` folder inside the ``datasets`` folder.
5. All current ``train.py`` expect the videos to be in the form of 100x50px mouthcrop image frames.
You can change this by adding ``vtype = "face"`` and ``face_predictor_path`` (which can be found in ``evaluation/models``) in the instantiation of ``Generator`` inside the ``train.py``
6. The other way would be to extract the mouthcrop image using (very crude) bash scripts inside ``scripts`` folder.
7. Create symlink from each ``training/*/datasets/align`` to your align folder.
8. You can change the training parameters by modifying ``train.py`` inside its respective scenarios.

###Random split
Create symlink from ``training/random_split/datasets/video`` to your video dataset folder (which contains ``s*`` directory).

Train the model using the following command:
```
./train random_split [GPUs (optional)]
```

**Note:** You can change the validation split value by modifying the ``val_split`` argument inside the ``train.py``.
###Unseen speakers
Create the following folder:
* ``training/unseen_speakers/datasets/train``
* ``training/unseen_speakers/datasets/val``

Then, create symlink from ``training/unseen_speakers/datasets/[train|val]/s*`` to your selection of ``s*`` inside of the video dataset folder.

The paper used ``s1``, ``s2``, ``s20``, and ``s22`` for evaluation and the remainder for training.

Train the model using the following command:
```
./train unseen_speakers [GPUs (optional)]
```
###Unseen speakers with curriculum learning
The same way you do unseen speakers.

**Note:** You can change the curriculum by modifying the ``curriculum_rules`` method inside the ``train.py``

```
./train unseen_speakers_curriculum [GPUs (optional)]
```
###Planned
* Overlapped speakers
* Overlapped speakers with curriculum learning

##Evaluation
To evaluate and visualize the trained model on a single video / image frames, you can execute the following command:
```
./predict [path to weight] [path to video]
```
**Example:**
```
./predict evaluation/models/weights04.h5 evaluation/samples/id2_vcd_swwp2s.mpg
```
##Work in Progress
This is a work in progress. Errors are to be expected.
If you found some errors in terms of implementation please report them by submitting issue(s) or making PR(s). Thanks!

**Some todos:**
- [ ] Use Stanford-CTC beam search
- [ ] Integrate language model for beam search
- [ ] RGB normalization over the dataset.
- [ ] Validate CTC implementation in training.
- [ ] Proper documentation
- [ ] Unit tests
- [ ] (Maybe) better curriculum learning.
- [ ] (Maybe) some proper scripts to do dataset stuff.

##License
MIT License