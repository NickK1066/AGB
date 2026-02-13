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
<img width="1232" height="653" alt="Screenshot 2026-02-12 at 07 31 39" src="https://github.com/user-attachments/assets/d4393ee5-2793-4516-8998-8473aa6d1d6e" />
The displayed contours can be filled or line which is useful.

The code also creates a png for front and rear plates that is scaled to 1pixel/mm for easy printing, here being used on a European Spruce wedge for the soundboard:
![IMG_5390](https://github.com/user-attachments/assets/61c73d9a-c825-4d33-b5d8-7a92e5ec7062)

The code also generates a rear arch - this is traditionally different in that it dome centre point is symetrically centre. Note you will need to mirror the contour if you have a non-symetrical outline, here being used on a Maple wedge (actually an old cello wedge):
![IMG_5391](https://github.com/user-attachments/assets/a79551fb-f714-474b-bb10-0b417afcb578)


**instructions for software use**

The code is written in Google Colab notebook using Gemini-only originally but later hand refactored and rationalied so it's easiest to simply run it in there:

1. Got to Google Colab: https://colab.research.google.com

2. Upload and open the notebook (.ipnyb)

3. Upload the GOR.csv or your own outline and run all:
<img width="783" height="400" alt="image" src="https://github.com/user-attachments/assets/75a83b52-de71-442e-9777-4ad6153bb84a" />

I've also included the inside 'GOR.csv' which is a real life non-symetrical outline from my current build.

This is an example of an earlier iteration being tested:
![jpeg](https://github.com/user-attachments/assets/55f4da7f-6bc7-42bc-8fe3-b2947e740ce8)

Warning: Use at your own risk. I cannot and will not be held accountable for the use or intended use of this software.
Infact it's worth treble checking the orientation of the plates to ensure when you've printed them out they're actually the right way up for the front and back.

**instructions as part of a guitar build**

*Overview Steps*

1. Create an inside edge outline
The software will need an outline shape to work from. This is deliberate as many builds aren't quite perfect. This is time consuming but if you can make a 'perfect' guitar mould and outline template then you can simply reuse it.
It took the best part of a morning and about 100 measurements.
I've already built a mould, bent the ribs (side wood), glued in the neck and tail blocks, then bent and bonded in eight linings (if you attempt this - premade kerfling is much less of a headache!) 

* I used a piece of wallpaper backing paper on a flat surface, and then pencil stencilled the inside lining edge around the whole inside of the guitar. Mark the centre line points from the body onto the paper and then rule a line across the outline so we have a centre line of the guitar to work from. It's important that this is right as we will use that as the centreline for the entire guitar.

* Mark the front edge of the lining on the front of the paper (you will need to rule a line through the neck/tail blocks). This will be you zero point on the centre line. X=0.

* Mark 10mm intervals along the centre line. Note that the tail lining line may not be on an interval but we'll be using that too. This is our guitar top "spine" (Y=0 line).
 
* for each point from, X=0, and each interval, through to the tail lining point, use a 90deg angle (make the edges long for better accuracy (or failing that, scale up the 3,4,5 triangle to get a perpendicular line), to draw a line across the guitar transversally.

* Record the first point Y=0, X=0, then going anti-clockwise around the outline, record the points in sequence. This is important because the interpolation resampling assumes a sequence of points going anti-clockwise. You will then get to the tail, and continue recording below the centre line Y=0, so the Y coordinates will now be negative on the way back and the X coordinates will progress back to the start.

The outline file has an ascending Order, X and Y values in an Order, X, Y column table, I just used Apple Numbers spreadsheet to record the numbers, then exported as CSV (see the GOR.csv as an example).
Originally when I did this for the GOR.csv, I did the two halves seperately so the X points didn't line up, so the code is built to cope with this and resample and interpolate to 5mm intervals. You can add more intervals if you wish (or if you have code generating it geometrically). However 10mm worked fine for my non-perfect guitar body build which just so happens is a worse case scenario.

2. Edit the parameters

The code has parameters, documented, for controlling the height of the guitar dome, where that dome is (in terms of bridge point). You could also adjust the mid point on the rear with a minor coode change should you want the rear centre of the dome moved back towards the tail. All these measurements are refenced from the X=0 point from your outline.
You will also perhaps want to look at the thickness parameter which governs the thickness of the wood as a constant thickness across the dome. It'll be up to you to tune/tailor the thickness to suit your individual piece of wood's unique characteristics and any bracing. I've put 5mm as a default.

To adjust the head/tail block you will need to adjust the plot commands in the plotting code plot_contour_map() function. The oddity here is that the X coords are all in a list within square brackets then all the Y coords, the col is simply a colour used which if a filled contour is generated will be white, but for line contours for the file etc is black - you don't need to change that.
For example, this code will create a line from (100,41) to (100,-41): plt.plot([100,100],[41,-41], col )

3. Generate the top and back by uploading the CSV outline file and Run All

Follow the instructions previously given. ALWAYS keep the master copy of your CSV on your own local drive as the instance of colab will be deleted when an inactivity timer occurs (this is colab and not my code). This also affects the generated outline files on the colab instance - so download those when you're happy before the colab instance gets deleted.

The files generated are a 1 pixel per mm sized and sized to the dimensions of your outline automatically. The bridge point is marked as a dot, along with the neck, tail and fretboard that you've adjusted with the plot code.

4. Check the orientation, adjust the flip_normal to True/False as needed.

This is important to ensure that your "up" is facing the right way. If in doubt, sit with the body look at the plate conour against it facing the correct way. A quick way of doing this is print a simple smaller than 1:1 A4 paper sheet, and hold the sheet against the body. The printed contour side should be facing out as this presents the 'up' of the dome. Do that with both the front and rear.

If your outline is not perfectly symmetrical (mine is asymmetrical remember) then you MUST ensure that the outline matches the outline of the guitar, as just flipping the paper will look like it doesn't fit right. Use the "flip_normal=" parameter to change this. It will flip the X axis in the image and regenerate - you can then recheck. With the same asymetrical pattern it should appear that the print of the contours is now on the opposite side of the paper.  For my build the front needed flipping but the rear did not. 

5. Check the outlines are what you'd expect.

Check the colour bars on the notebook image, checking the hieghts of of the dome and the recurve points are what you've calculated previously. If not have a look at the parameters and work out why. Remember that the engineering curve will have 1/2 thickness above and bellow, and the recurve depth will push up the height. Lastly there's padding that will increase height but you will remove that before fitting the plate. The additional thickness helps prevent dings/dents and strengthens against tearout.

5. Print a full sized 1:1

So how you do this will vary on your computer and software.

I used an odd technique that uses the natural mechansim that spreadsheets will break up large spreadsheets onto multiple pages to print. I use this to print the large 1:1 image in sections onto a A4 laser printer.

a) Set the ruler measurement in the spread sheet settings to centimeters rather than points.
b) Import the 1mm/pixel contour image into the sheet as an image object.
c) Set the location of the object as (0,0) on the sheet
d) Set the size of the object to the size of your object. For me that's a 515mm wide, but I enter 51.5cm, leaving the ratio of the image locked so the Y then automatically updates to 41.9cm (or 419mm).
e) I then go to print, and then adjust the print settings:
f) landscape paper orientation.
g) 100% sale not scale to fit.
h) 0cm margin for the upper and left margin, but 1cm for the lower and right margin - this creates a blank tab you can use to help selotape the paper together with.
i) turn off/set the header and footer to 0mm.
j) set to print all sheets.
k) Hit Print... et Voila! you have a 1:1 sized print of your contour and outline.
l) match up the edges and stick them together - either prittstick or celotape (I normally cellotape both sides of the paper join).
m) I then cut off the excess paper at the outline. This makes it easier to see your work and if it fits.

6. Check your outline and your body.

Check that the sizing fits your outline, it should sit just inside the lining. If it does not, stop an review your outline, the generation parameters and the steps to printing it. It's important that the edge sits on the lining to support the arch otherwise the top may sink over time as the wood bends with the bridge downforce. Also check that the outline sits in the right orientation as discussed before so the dome faces the right way.

7. Git Building (do you like the github pun?) 

At this point you have many options but it depends on your wood and build approach. I will leave that to you.

I will be cutting my wedge down to near flat dimensions of the bridge point. Then transferring the contours to the wood and using a drill & router around the copied pencil contour lines. Todo this I have added a 30mm edge around the outside of the outline on the wood. That edge will then support the router bars during the routing around the outlines. I'll route down to near depth before switching to wood scrapers to smooth and approach the final depth, measuring with a 0.0005" graduated engineering gauge.

The inside I'll use a duplicate flipped contour map but I will mark out the depths of the contours so that the thickness is 6mm. This means that the routing only starts in the inner contours, then finish with scrapers and planes during the tuning process. I plan to use chladni patterns and modes to refine to create clear lines. I can use the same 0.0005" 1" travel gauge to ensure an even thickness.

I have a test rig for downforce, which will allow me to check top depression with the calculated bridge downforce using the same 0.0005" gauge to detect the movement of the top. I will then brace and tune the braces using a spectrum analysis tool.

The rear I will then do precisely the same process (the dome hieght is 15mm), but I will tune for a number of steps higher in pitch to avoid woof notes.

8. Enjoy!


**Why?**

I've spent the last 30 years developing solutions and operating services - from developer through to Chief. With the emergence of AI, I've had to do AI at Oxford SBS to build on my Software Engineering degree and experience and also statistics. To really understand the pit falls of engineering with AI, it helps in having handson experience solving real world problems to empathise with engineers and teams. It pushes me to use more python too.
The code here isn't quite as simple as it seems given the realworld outline aren't uniform or symmetrical. As a result the input data needs resampling before shaping curves. It uses numerical iterative bisectional searching to solve x=a*t-b*sin(t), but with varying asymmetrical curves this requires different a and b radii for each curve.
