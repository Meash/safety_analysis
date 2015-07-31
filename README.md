#Exercises for the class *Safety Analysis*
######University of Augsburg

Martin Schrimpf, Alexander Dittrich and Philipp Koytek

##About this repository
To control railway crossings, the Deutsche Bahn is interested in a novel approach which uses wireless communication between the train and the crossing to secure the passing of the train.

We search for possible hazards (i.e. train on unsecured crossing) and their causes using techniques such as the Fault Tree Analysis and the FMEA.

Ultimately, we model the behaviour of the system in program graphs which are then transformed to a NuSMV specification.
We extend the specification and add safety checks in LTL or CTL which are then model-checked by NuSMV.
We also perform a DCCA and find all minimally critical sets of failures.


##Solutions for the exercises
The solutions for tasks 1 and 2 from the first sheet (*Failures* and *Fault Tree Analysis*) and for task 3 from the second sheet (*FMEA*) can be found in the repository's root folder.

The program graphs and NuSMV specifications can be found in a VisualStudio project which is located in the root directory.
Simply clone this repository and open the VisualStudio solution.

**Note**: you need an *F# 2.0 runtime* as well as the *Safety* plugin provided by the University of Augsburg to open the program graphs and transform them to a NuSMV module specification.
Obviousy, you also need the symbolic model-checker *NuSMV*.