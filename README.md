# AGB - Archtop Guitar Builder
Archtop Guitar builder - contour map generation for luthiers carving

**what is it**

When Luthiers create an archtop guitar, they need a shaped profile to create the cuved top of the guitar front and the rear

The curve in use is a Curtate Cycloid:
<img width="1036" height="320" alt="Screenshot 2026-02-10 at 14 32 37" src="https://github.com/user-attachments/assets/c7c65110-dbaa-492b-bce0-c13a8f89c440" />
Source: https://www.researchgate.net/figure/Curtate-common-and-prolate-trochoids-The-common-trochoid-is-also-called-a-cycloid_fig2_361278698
More info on cycloids can be found here: https://tomrocksmaths.com/wp-content/uploads/2021/05/teddy-rocks-maths-1-1.pdf

As a profile when used for a guitar this looks like this:
<img width="1436" height="703" alt="Screenshot 2026-02-04 at 00 12 57" src="https://github.com/user-attachments/assets/41cebd50-37aa-4d27-9c9d-21a1773802e0" />
The lowest point has to sit on the linings to spread load of the curve. There's more to this in that there's a trade off between structural strength and frequency response.
For example - we can see the modes and chladni patterns that we will be looking, as a luthier, to refine by carving the thickness of the wood and shaping the bracing structs bonded to the underside.

The result of the tool is a height above the wood base, that allows for the thickness and prevents the recuve dipping below the joint with the sides of guitar, this needs to be done along the guitar (the spine) and across the guitar (transverse). These curves are not symetrical to cope with the bridge point along the spine and the measured outline. To illustrate:
 <img width="790" height="812" alt="plot" src="https://github.com/user-attachments/assets/b44d4098-d284-423f-b62e-f1285c09a815" />

The resulting plot is the colour coded into contours and that can then be printed out to allow the top to be carved. Here's a 0.5 and 1mm step used for carving: 
<img width="976" height="798" alt="low-res-approx-1mm steps" src="https://github.com/user-attachments/assets/86724b0b-a9fa-4ead-9fbc-2e4dd7921754" />

The same but with 0.1mm steps showing more resolution and the smoothness:
<img width="985" height="798" alt="hi-res-new" src="https://github.com/user-attachments/assets/9211afc1-92d9-4255-9981-eb2aaa474524" />

The code also generates a rear arch - this is traditionally different in that it dome centre point is symetrically centre. Note you will need to mirror the contour if you have a non-symetrical outline.

**instructions for use**

The code is written in Google Colab notebook using Gemini-only originally but later hand refactored and rationalied so it's easiest to simply run it in there:

1. Got to Google Colab: https://colab.research.google.com

2. Upload and open the notebook (.ipnyb)

3. Upload the GOR.csv or your own outline and run all:
<img width="783" height="400" alt="image" src="https://github.com/user-attachments/assets/75a83b52-de71-442e-9777-4ad6153bb84a" />

I've also included the inside 'GOR.csv' which is a real life non-symetrical outline from my current build.

This is an example of an earlier iteration being tested:
![jpeg](https://github.com/user-attachments/assets/55f4da7f-6bc7-42bc-8fe3-b2947e740ce8)

Warning: Use at your own risk. I cannot and will not be held accountable for the use or intended use of this software.

**Why?**

I've spent the last 30 years developing solutions and operating services - from developer through to Chief. With the emergence of AI, I've had to do AI at Oxford SBS to build on my Software Engineering degree and experience and also statistics. To really understand the pit falls of engineering with AI, it helps in having handson experience solving real world problems to empathise with engineers and teams. It pushes me to use more python too.
The code here isn't quite as simple as it seems given the realworld outline aren't uniform or symmetrical. As a result the input data needs resampling before shaping curves. It uses numerical iterative bisectional searching to solve x=a*t-b*sin(t), but with varying asymmetrical curves this requires different a and b radii for each curve.
