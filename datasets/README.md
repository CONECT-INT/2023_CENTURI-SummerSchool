# Dataset 1: Neural signals from macaque motor cortex during a center-out reaching task

*Original paper*: Hatsopoulos, Joshi, and O’Leary. 2004. “Decoding Continuous and Discrete Motor Behaviors Using Motor and Premotor Cortical Ensembles.” *Journal of Neurophysiology* 92 (2): 1165–74.  [doi:10.1152/jn.01245.2003](https://journals.physiology.org/doi/full/10.1152/jn.01245.2003)


<img src="./dataset1_reaching-task/centerout-task.png" height="200" />      <img src="./dataset1_reaching-task/trajectories.png" height="200" />


*Task*: monkeys were trained to move a cursor toward one of 8 possible
targets, starting from a central target. The targets were organized in 8
different directions (0, 45, 90, …, 315°)

*Data format*: The matrix *R* contains the firing rate of 143 neurons
recorded during 158 trials. The vector *direction* specifies the reach
direction for each of the 158 trials. Direction 1 corresponds to a
rightward reach (0 degrees), direction 2 to 45 degrees, direction 3 to
90 degrees (straight up), etc, with direction 8 corresponding to 315
degrees.

*Goal*: we will build a simple neural decoder to read out the intended
movement of the monkey and control a robotic arm.

To get you started:

1)  Can you tell which direction the monkey is moving simply by looking
    at one neuron? Let’s pick the first neuron of the *R* matrix. Plot
    the mean firing rate of this neuron for all 8 directions separately.
    Given that spiking activity is noisy, how would you quantify the
    uncertainty in the firing rate? (Standard deviation? Standard error?
    Bootstrapping?)

1)  Can you better predict the direction if you use two neurons instead
    of one? Let’s pick neuron 1 and 2 of the *R* matrix. Using a 2D
    scatter plot, plot the firing rate of neuron 2 as a function of
    neuron 1, and color each trial according to the reach direction.
    What do you notice?

1)  Now let’s include ALL neurons. Technically, you would need a
    143-dimensional plot to visualize the data, which is not possible.
    So instead, we will use principal component analysis (PCA) to reduce
    the data to lower dimensions (tip: there is one important step to
    apply to the data before applying PCA, what is it?). Compute the
    amount of the total variance you can explain using 1, 2 or 3
    dimensions (i.e., scree plot)
    
1) In the previous step, you applied PCA to explain variance across and within condition. However, we might be more interested to look at the dimensions that explain differences across conditions only. Redo PCA now on the trial-averaged activity (that is, on a matrix of size # neuron x # condition), then project the single-trial data onto the top 2 PCs. What do you notice?     

1)  Let’s visualize the original data in the plane spanned by the top 2
    PCs. How do you interpret the unit vector describing this plane? Do
    some neurons contribute more than others to these dimensions? ​​Plot
    the projection of firing rates onto this 2D plane, and color the
    points according to the reach direction.

1)  Suppose we want to control a robotic hand that copies the monkey’s
    hand movements. Design a decoder (a linear system) that receives 143
    inputs (the firing rate of the 143 neurons) and generates 2 outputs
    that control the x (left-right) and y (up-down) motors of the
    robotic hand. How cool is that?
 
1)  One extra thing: you may have noticed that neurons tend to be active for nearby targets, showing a bell-shaped curve when plotting activity versus  direction. This phenomenon is referred to as “directional tuning”, and was first observed in the 80s by Georgopoulos (1982). To quantify this tuning, we will fit a Von Mises function (equivalent of a Gaussian on a circle) to each neuron individually; see Amirikian & Georgopoulos (2000) if needed. Plot the distribution of the Von Mises parameters (width and mean) across the population. 
    
*Resources*

- Understanding PCA: http://alexhwilliams.info/itsneuronalblog/2016/03/27/pca/
- Original Georgopoulos paper: https://www.jneurosci.org/content/2/11/1527
- Modeling paper for directional tuning: https://pubmed.ncbi.nlm.nih.gov/10678534/

# Dataset 2: Neural signals from macaque dorsomedial cortex during a time interval task

*Original paper*: Meirhaeghe, Nicolas, Hansem Sohn, and Mehrdad
Jazayeri. 2021. “A Precise and Adaptive Neural Mechanism for Predictive
Temporal Processing in the Frontal Cortex.” *Neuron* 109 (18):
2995–3011.e5.

*Task*: monkeys were trained to measure various time intervals and
reproduce them by making saccadic eye movements

Coming soon...
