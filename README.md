# Sun Path Diagram for any given location or latitude on earth

## Executive Summary

A Sun Path Diagram can help building environment designers gather information about how the sun will impact the building site throughout the year. It is used to determine the day of the year and hour of day when shading will take place at a location of interest. Below you can see a figure showing the sun path on three days of the year in the northern hemisphere. Going from right to left, three paths are shown, one being for the summer solstice, the next one for equinoxes and the last one for winter solstice. A Sun Path Diagram represents four parameters related to the position of the sun over a particular location and they are: hour, day, azimuth and altitude. 

![](./sun_path_expl.gif)
*[Visualisation of a 3D Sun Path Diagram](http://friskykcollyer.blogspot.co.uk/2010/06/sun−path.html) \[Accessed 20 April 2016\]*

This program is designed to automatically draw a Sun Path Diagram for a location of interest, by inputting the location (the latitude of which is determined using the Google Maps API) or by inputting the latitude of the location of interest.

## Method

#### Calculating hour angles (H)
Hour angle is the angle the Earth needs to rotate to bring the meridian to noon. We need to calculate this for every hour in the day, in order to be able to calculate the altitude of the sun at every hour of the day.
The formula used to calculate the Hour angle is: $H = 15\times(T-12)$, where $T$ is the time in a 24 hour format.

#### Calculating declinations (D)
Declination is the angle of the sun's rays to the equatorial plane and it is positive in the summer. Declinations for 3-5 days in a year need to be calculated in order to be able build a sunpath diagram. In this program declinations for 5 days of the year have been calculated, for completness. Two of the days are summer and winter solstices and the rest are days evenly spaced between the solstices, including either of the equinoxes (declination for both equinoxes is the same: 0). The formula used to calculate the Declination is: 

$$D=23.4\times\sin\times\Bigg(\frac{360\times(284+N)}{365}\Bigg) $$
$N$ - is the day number in a year (example: 1st of February is 32)

#### Calculating altitudes ($\gamma$)
When we have all the required values for hour angles and declinations from the above formulas, we can start calculating the altitudes of the sun during the day, on each of the five days chosen. The formula used to calculate the altitude is the following:

$$\sin(\gamma)=\cos(D)\times\cos(L)\times\cos(H)+\sin(D)\times\sin(L)$$
which rearranged is:

$$\gamma = \arcsin(\cos(D)\times\cos(L)\times\cos(H)+\sin(D)\times\sin(L))$$

$\gamma$ - Altitude

$D$ - Declination

$L$ - Latitude

$H$ - Hour angle

Five lists of altitudes of the sun are created, with values of altitude at every hour in a 24 hour period, of each one of the five days. Those lists are then later used to plot the altitudes on the Sunpath Diagram.

#### Calculating azimuths (z) for each hour of each one of the five days chosen.
The formula used to calculate the azimuths for each given altitude of the sun is displayed below:

$$\cos(z) = \frac{\sin(D)\times\cos(L)-\cos(D)\times\sin(L)\times\cos(H)}{\cos(\gamma)}$$
which rearranged is:

$$z=\arccos\bigg(\frac{\sin(D)\times\cos(L)-\cos(D)\times\sin(L)\times\cos(H)}{\cos(\gamma)}\bigg)$$

This formula calculates azimuth clockwise or anticlockwise from north up to 180 degrees. However for the purpose of this program azimuths needed to go up to 360 degrees to be able to plot them, so in order to calculate the azimuth clockwise from north the azimuth calulated has been subtracted from 360. Then lists of azimuths had to be built of lenght 25, with each value of azimuth in the lists matching the corresponding value under the same index in the lists of altitudes. The code below works through this process and works for both positive and negative latitudes.

## Comments on the programming code
All code has been written in Python 2.7. In the code I have used matplotlib basemap toolkit, which is a library for plotting 2D data on maps in Python. Alghogh there was no need to printed data on maps, basemap toolkit proved to be usefuk to help transform coordinates to a different map projection. Then Matplotlib was used to plot the rest of the required data. The installation for the basemap toolkit can be found by clicking on this link: http://matplotlib.org/basemap/users/download.html \[Accessed 20 April 2016\].

Also Google Geodata API was used to determine the latitude of the location typed in by the user. The code takes the search string and constructs a URL as an encoded parameter and then uses urllib to retrieve the text from the Google geocoding API. The data received depends on the parameters sent and the geographical data stored in Google’s servers. When JSON data is retreived, it is parsed with the json library and a few checks are done, to make sure that data received is usable format and then the latitude coordinate is extracted for the purpose of the program.

The programming code asks the user for a location or a latitude, then calculates lists of altitudes and azimuths throughout the year. Once all the lists are complete, all the points are plotted on the Sun Path Diagram.
