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

*Original paper*: Meirhaeghe, Sohn, and Jazayeri. 2021. “A Precise and Adaptive Neural Mechanism for Predictive
Temporal Processing in the Frontal Cortex.” *Neuron* 109 (18):
2995–3011.e5.

<img width="453" alt="image" src="https://github.com/CONECT-INT/2023_CENTURI-SummerSchool/assets/25228402/fc761eaf-bd42-4782-a53b-41c2de5c5c97">

*Task*: Two monkeys were trained to perform a time interval reproduction task known as ‘‘Ready-Set-Go’’ (see Figure above). The task requires animals to measure a sample time interval (ts) between two visual flashes (‘‘Ready’’ followed by ‘‘Set’’) and produce a matching interval (tp) after Set by initiating a saccade (‘‘Go’’) toward a visual target presented left or right of the fixation point. Animals performed this task in two conditions (Figure 1A, top right). When the fixation spot was red, ts was sampled from a ‘‘Short’’ uniform distribution (480–800 ms); when the fixation spot was blue, ts was sampled from a ‘‘Long’’ distribution (800–1200 ms). The distributions were alternated in short blocks of trials (5–20 trials), and intervals within each distribution were randomly sampled. The amount of reward monkeys received at the end of each trial decreased linearly with the magnitude of the relative error ((tp_ts)/ts).

*Data format*: dataset2 contains 2 data structures, called monkeyG and monkeyH. Each structure contains 7 fields:
- spikes is a 3-D tensor [TxNxK] containing the spiking activity of N neurons, across K trials and T times points. Spikes are binned in a 1-ms window (why do you think 1-ms?) and include data between Ready and Set. Data were NaN-padded along the time dimension to match the duration of the longest interval of the Long distribution (1200 ms). Also note that trials in which a particular neuron was inactive (i.e., before its first detected spike and after its last detected spike) are marked as NaNs.
- ts is a vector [Kx1] of sample intervals.
- tp is a vector [Kx1] of produced intervals.
- id_eye is a vector [Kx1] indicating if the animal responded using an eye (1) or hand (0) movement.
- id_short is a vector [Kx1] indicating if the sample interval was sampled from the Short (1) or Long (0) distribution.
- id_left is a vector [Kx1] indicating if the peripheral target was located left (1) or right (0) of the fixation point.
- id_neuron is a vector [Nx2] containing the ID of each neuron (first column) and whether it is a well-isolated single unit (1) or a multi-unit (0).

*Goal*: we will neural activity during the measurement epoch of the task to study how temporal expectations (Short vs Long) impact neural dynamics.

To get you started:

1.	A good way to start analyzing dynamic neural activity is to visualize it… Make a summary plot with a subplot for each neuron showing the trial-averaged activity of the neuron as a function of time. Use 20-ms non-overlapping windows to bin the spikes. Additionally, you can smooth the data using a standard Gaussian kernel (SD=40 ms); use the smoother.m helper function if needed, or refer to https://matthew-brett.github.io/teaching/smoothing_intro.html#smoothing-with-the-kernel. Plot the Short and Long conditions on the same plot in different colors; and add circles at the different times of the Set. Use the bootstrap method to compute confidence intervals. What do you notice when comparing the dynamics in the Short vs Long condition? 
2.	Now zooming out of individual neurons, let’s take a look at the dynamics of the population as a whole. Based on what you learned with dataset 1, you will reduce the dimensions of the data to visualize the dynamics. Use the helper function PCAneuralData.m to get you started. At the end of this step, you should have plotted the “neural trajectories” associated with each condition (Short and Long) in the same 3-D plot. If you use PCA, do you think it is the best method to explain variance in the time domain? Plot dots along the trajectory to show consecutive states separated by 20-ms; what do you notice in terms of speed along the two trajectories? 
3.	Now that you have visualized the high-D data in low-D to get some intuitions, let’s be more quantitative. How could you quantify the speed differences you observed along the trajectories? Perform this calculation both in the 3-D PC space and in the full space (including all neurons). Is there a difference?
4.	Moving away from trial-averages, let’s analyze finer features of the neural activity by focusing on individual spikes. Plot the Inter-Spike Interval distribution and coefficient of variations for every neuron. Is there a difference between the Short and Long condition? 
5.	Now we will see if we can identify spiking patterns (or motifs) in the population of neurons. Start by looking for systematic delays between neurons by building the cross-correlogram for every pair of neurons.  

The rest is coming soon...
