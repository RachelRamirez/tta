## Overview 

The TTA python notebook is an introduction to Test-Time-Augmentation I forked from somewhere else.  
The NB-Chap8_04 is a notebook created by TensorChiefs for their book Bayesian Neural Networks, that compared Non-Bayesian CNN with Bayesian CNN (with MC Dropout) and Bayesian CNN (Variational Inference).  They created the experiment with CIFAR-10 that removed one class (horses) during training, and then reintroduced it at test time to see how well the Bayesian networks identified a "novel" class.  

My contribution was adding TTA code to the NB_chap8_04 notebook to include a comparison with a fourth-technique-TTA.  

Each of the techniques compare using 50-looks of an image, to come up with a spread of predictions.  When you have a lot of predictions for each image, you can use the probabilities (logits) of the predictions to do more comparisons on measures like Entropy.  

*Overall Results*: TTA appears to do 2nd Best, with MC at the best.  Here's one test run:


| 	| Non-Bayesian	| VI	| MC | 	TTA |
|---|---|---|----|--- | 
|test acc on known labels |	0.649444|	0.6870|	0.706889|	0.6910|
|test acc on all labels	| 0.584500	|0.6183|	0.636200	|0.6219|

Because I loaded pre-trained weights from the three models, I expect the results to be similar every time I rerun, but the Test Time Augmentation will be different each time because it is not seeded. Training VI and MC take a lot of time so it's valuable to have these checkpoints already established for the two and then be able to add another comparison.  

 

"Hypothesis": I wonder if the test time augmentation techniques is making the more confident predictions more confident and the less confident predictions less confident so that if you are already "sure" the picture is a cat, looking at it 50 different ways will still be pretty "sure", but if you are not "sure" what you are looking at, looking at it 50 more times will give you a spread of a lot of different things so that you overall "sure" score goes down.  This will lead to more entropy in the final prediction for unknown-classes (OOD), and the incorrect/misclassified known predictions.    If this is the case, it seems like TTA could be a waste of time, you could just adjust the threshold on what is confident enough to be a classification.  


Potential steps to improve this Colab notebook is to introduce Model-Ensembling.

Future research steps is to see if there is a way to use a DOE Screening Design to find the "optimal" test time augmentations to increase accuracy on this model, and other other models.






