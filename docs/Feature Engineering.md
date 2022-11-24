This page contain notes on different mobile sensing features which can be extracted and added as Sampling Packages to CARP Mobile Sensing. This work as a notebook.

The following features are discussed:

* Mobility
* Context
* Sleep
* Social Activity
* Physical Activity 
* Sound Environment


## Mobility Features

Raw GPS data is not very useful in determining the mobility of people. Rather higher level mobility features are relevant. In [1] there is a [nice outline](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4667797/) of how to calculate the following core features.

**Feature** | **Description**
------------|------------
Number of Clusters | This feature represents the total number of clusters found by the clustering algorithm.
Location Variance | This feature measures the variability of a participant’s location data from stationary states. LV was computed as the natural logarithm of the sum of the statistical variances of the latitude and the longitude components of the location data.
Entropy | Entropy measures the variability of the time the participant spent at the location clusters.
Normalized Entropy | We define normalized entropy by dividing the cluster entropy by its maximum value, which is the logarithm of the total number of clusters.
Home Stay | This feature measures the percentage of time the participant has been at the cluster that represents home. We define home cluster as the cluster, which is mostly visited during the time period between 12am and 6am.
Transition Time | Transition Time measures the percentage of time the participant has been in the transition state.
Total Distance | This feature measures the total distance the participant has traveled in the transition state.
Circadian Movement | This feature measures to what extent the changes in a participant’s location follow a 24-hour, or circadian, rhythm. To calculate circadian movement, we obtained the distribution of the periodicity of the stationary location data and then calculated the percentage of it that falls in the 24±0.5 hour periodicity.

In [4] they state; "We use the following features based on location clusters. They are similar to those in [1] except that the clustering is based on **DBSCAN** instead of K-means."


## Context Features

[3] suggests the following [context features](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5052463/) for Behavioral Activation studies.

**Feature** | **Description**
------------|------------
General Activity | To assess the general activity levels, the standard deviation of the three-dimensional (3D) acceleration norm was computed 
Walking Time | To approximate the walking time, for every time window of 2 minutes, we made use of the standard deviation of the 3D acceleration norm (1) again.
Time at Home | To measure the time a subject stays at home, an approach by Rekimoto et al [31] for WiFi-based location logging was adapted. Every 15 minutes, the WiFi basic service set identifier of hotspots in the surrounding were scanned. Based on a rule-based approach, MOSS tried to learn a subject’s home by comparing WiFi fingerprints stored during the first 3 consecutive nights. 
Phone Usage | This feature measured the total time subjects were using their mobile phone depicted by the time the smartphone was unlocked following Saeb et al [17]. The time spent with the MOSS app was excluded.
Geographic Movement | Every 15 minutes, coordinates of the current location of the subject were captured. From these coordinates, the maximum and the *total distance traveled* were calculated using techniques for geographical distance calculation. Additionally, the *location variance* was calculated from the latitudes and longitudes 
Number of Unique WiFi Fingerprints | In addition to GPS information as a proxy for geographic movement, every 15 minutes, WiFi fingerprints of the surrounding were scanned.
Number of Text Messages | This feature kept track of the incoming and outgoing text messages together with a count of different unique contacts the messages were sent to and received from. 
Number of Calls | This feature kept track of the number of incoming and outgoing calls a subject made together with a count of different individuals 
Number of Calendar Events | This feature kept track of the number of calendar events stored. It distinguished between events taking place in the morning, afternoon, and evening time

## Sleep

Build a sleep sensor from the phone taking into account several sensor modalities (e.g. movement), time, and phone activity. See the [SensibleSleep](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0169901) project [7].


## Social Activity

Like the Activity Recognition API, create a "Social Activity Recognition" API which can detect different types of social activity, like;
* alone
* with one person
* in a small group
* talking to someone (either on the phone or in general)
* ...

Haven't seen this in the literature, but a search is needed.


## Physical Activity

The current Activity Recognition API provides activity "event" like "Walking", "Sitting", etc. I would be relevant to have a higher-level information on the level of physical activity over e.g. an hour, and maybe compared to some baseline. Hence, build a "personalised activity meter" which (e.g. on a scale 0--100) reports on a persons physical activity level. This could be relevant also to the MET level from e.g. an ECG device / Movisens?

## Sound Environment

Detect different social context based on sound analysis:
* quite / alone
* talking w. a person
* in a small group / meeting
* in a large crowd
* in a busy street
* at a concert

The [SoundSense](http://www.cs.cornell.edu/~tanzeem/pubs/mobisys09_soundsense.pdf) project did this [6]. See also the [StudentLife AWARE plugin](https://github.com/denzilferreira/com.aware.plugin.studentlife.audio_final).

## Features from Raw Data


This is a list of inspiration for feature extraction methods from the raw data [2]:

raw | features
----|---------
sensors (accelerometer, gyroscope, ...) | avg, S.D., max, min, diff
activity | count, ratio for each activity
screen status | avg, S.D., max, min, count on & off
location | max distance from home, total distance, max radius of daily trace, count of places visited, location entropy
location | *Jaccard Dice Index* of visiting points compared to 1 day/week ago
screen on | *Bhattacharyya coefficient* of on-distribution difference of total on-count & on-duration against his/her average usage & 1d/1w ago
application | *Bhattacharyya coefficient*, difference of total count & duration for each application against his/her average usage & 1d/1w ago

## References
1. Saeb, Sohrab et al. “[The Relationship between Clinical, Momentary, and Sensor-based Assessment of Depression](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4667797/)” International Conference on Pervasive Computing Technologies for Healthcare : [proceedings]. International Conference on Pervasive Computing Technologies for Healthcare vol. 2015 (2015): 10.4108/icst.pervasivehealth.2015.259034.
2. Luca Canzian and Mirco Musolesi. 2015. Trajectories of depression: unobtrusive monitoring of depressive states by means of smartphone mobility traces analysis. In Proceedings of the 2015 ACM international joint conference on pervasive and ubiquitous computing. ACM, 1293– 1304.
3. Wahle F, Kowatsch T, Fleisch E, Rufer M, Weidt S. [Mobile Sensing and Support for People With Depression: A Pilot Trial in the Wild](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5052463/). JMIR Mhealth Uhealth. 2016;4(3):e111. Published 2016 Sep 21. doi:10.2196/mhealth.5960
4. Farhan AA, Yue C, Morillo R, et al. [Behavior vs. introspection: refining prediction of clinical depression via smartphone sensing data](https://ieeexplore.ieee.org/abstract/document/7764553). In: Wireless Health. ; 2016:30-37. [[pdf](http://www.engr.uconn.edu/~jil14036/paper/Farhan-behavior-2016.pdf)]
5. Flora Or, John Torous, Juliana Simms, Jukka-Pekka Onnela. Smartphone Applications for Monitoring Mood Disorders: A Systematic Review. [[pdf](https://github.com/cph-cachet/carp.documentation/blob/master/papers/Smartphone-Applications-for-Monitoring-Mood-Disorders-A-Systematic-Review.pdf)]
6. Lu, H., Pan, W., Lane, N. D., Choudhury, T., & Campbell, A. T. (2009, June). [SoundSense: scalable sound sensing for people-centric applications on mobile phones](https://dl.acm.org/citation.cfm?id=1555834). In Proceedings of the 7th international conference on Mobile systems, applications, and services (pp. 165-178). ACM. [pdf](http://www.cs.cornell.edu/~tanzeem/pubs/mobisys09_soundsense.pdf)
7. Cuttone, A., Bækgaard, P., Sekara, V., Jonsson, H., Larsen, J. E., & Lehmann, S. (2017). [Sensiblesleep: A bayesian model for learning sleep patterns from smartphone events](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0169901). PloS one, 12(1), e0169901.
