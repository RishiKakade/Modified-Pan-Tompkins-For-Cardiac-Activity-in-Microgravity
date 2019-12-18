# Modified-Pan-Tompkins-For-Cardiac-Activity-in-Microgravity
Modifications to Pan-Tompkins Algorithm for QRS detection, compensating for left ventricular atrophy as seen in 3-lead ECG signals 

## What does microgravity do to cardiac structure? 

- Lengthened Ventricular Depolarization >> Unchanged QRS sequence but increased QT interval >> wider T-wave

- Left Ventricular Atrophy >> Higher amplitude T-wave

- [proof](https://en.wikipedia.org/wiki/Cardiac_rhythm_problems_during_space_flight#cite_ref-Myerburg_et_al_1989_(1)_1-0), [more proof](http://www.cinc.org/2018/preprints/358_CinCFinalPDF.pdf), [more more proof](https://www.researchgate.net/publication/235387666_Microgravity_effects_on_ventricular_response_to_heart_rate_changes), [also rats](https://ieeexplore.ieee.org/document/8743889)

The effects on the left ventricle necessitate changes in the algorithm to better distinguish R peaks from T peaks.

This algorithm modifies [this](https://github.com/rafaelmmoreira/PanTompkinsQRS) implementation of the Pan-Tompkins QRS detection algorithm. 

![Flowchart](https://github.com/RishiKakade/Modified-Pan-Tompkins-For-Cardiac-Activity-in-Microgravity/blob/master/flowchart.PNG)

## Modifications

**1. Remove DC component correction**

**2. New difference function on lowpass filter**

The cascaded band pass described in the 1985 [paper](https://sci-hub.tw/10.1109/tbme.1985.325532) removes 5-12Hz, albeit 5-15 was intended. Transfer function determined using Filter Builder in Matlab. New passband is 7-12Hz.

Difference Function:

![Difference Equation 1](https://github.com/RishiKakade/Modified-Pan-Tompkins-For-Cardiac-Activity-in-Microgravity/blob/master/diff1.PNG)

**3. 5-Point Stencil Differentiation**

Replaced the numerical differentiation using the central difference method (I guess a 3-point stencil??) with a slight modification to the 5-point used in the original. The 5-point is marginally less efficient (still in constant time) but more accurate. I also recalculated  the difference equation and realized a few coefficients were different. I compared the amplitude response of both and they seem to be linear between 0-30Hz. I decided to implement mine because of the work I put into realizing that Taylor expansions from Calc II are actually useful, and in the interest of learning something.

Difference Function:

![Difference Equation 1](https://github.com/RishiKakade/Modified-Pan-Tompkins-For-Cardiac-Activity-in-Microgravity/blob/master/diff2.PNG)

Its even possible to calculate error, although it would be exceedingly difficult to implement, requiring very high ordered derivatives to be computed:

![Error](https://github.com/RishiKakade/Modified-Pan-Tompkins-For-Cardiac-Activity-in-Microgravity/blob/master/error.PNG)
