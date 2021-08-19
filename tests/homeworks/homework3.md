### Homework 3  

**Due date:** This homework is to be completed **before** the short course given on October 21. You need to make
sure the software has been properly installed and you have successfully completed the "homework 0" assignment. You should 
also read the [gnssrefl documentation provided on GitHub.](https://github.com/kristinemlarson/gnssrefl).

**Purpose:** Learn how to measure water level with gnssrefl using GNSS data 

**Station:**
We will be using station **ross**. It is operated by [NRCAN](https://www.nrcan.gc.ca). 
This [map](https://webapp.geod.nrcan.gc.ca/geod/data-donnees/cacs-scca.php?locale=en)
gives you an overview of GNSS stations operated by NRCAN. Use the plus sign on the map 
to look more closely at Lake Superior. Find **ross** and click on it (station M023004). 
If you scroll down, you will see a photo of the monument. 

NRCAN is operating what I would call a "legacy" GNSS instrument. This means it only tracks 
the original GPS signals that were designed in the 1970s. This means none of the 
outstanding enhanced GPS signals (L2C and L5) available since 2005 are provided. 
Furthermore, there are no signals from Glonass, Galileo, or Beidou. 
The bottom line is that you will be using only the L1 signal, which leaves you without ~15% of what 
would be available from a modern GNSS unit. Finally, the sample rate - 30 seconds - limits what 
kind of reflectometry you can do. For the purposes of this homework, it restricts the RH to values less 
than ~10 meters.

**Azimuth/Elevation Mask**

Next, let's get an idea of what this site looks like from a reflections viewpoint. 
Use the geoid tab on the [gnss-reflections webapp](https://gnss-reflections.org/geoid) to
get an idea of its surroundings. You can enter the station coordinates by hand if you know them, 
but since **ross** is part of a public archive known to geodesists, coordinates have been stored in the webapp.
Just type in **ross** for the station name. 
Make a note of the station latitude, longitude, and ellipsoidal height that is returned by the 
webapp because you will need it later.  Although the elevation above sea level of 
the site is ~186 meters, from the photo you know already this is not the value 
we will want to use for our reflections study. We will start with our common 
sense, look at the data, and iterate if necessary.

Use the [reflection zone section of the web app](https://gnss-reflections.org/rzones) to get an idea
of what reflection zones are possible for this site. You need to set a Reflector Height 
(RH) value and see what comes back. You don't want your reflection zones to cross 
a dock or the nearby boats, so you should also rerun it with different azimuth limits.  

Make a note of:

<UL>
<LI>RH
<LI>elevation values that give water coverage without interference from docks/boats
<LI>azimuth values that cover open water without interference
<LI>the DECIMAL latitude, longitude, and height (from the geoid webapp).
<LI>we can only use L1 GPS data at this site 
<LI>We can't estimate RH larger than 10 meters because of the sampling rate
</UL>

Now let's look at the **ross** data. We need to pick up a RINEX file and strip out the 
SNR data.  We use the <code>rinex2snr</code> for this purpose.  Use -h if you want to 
see the options for this module. We will throw caution to the winds and see if the defaults will work. 

* Make a SNR file for station **ross** in year 2020 and day of year 150. (to convert from ydoy to ymd and
vice versa, try the modules with those same name). If you know where the data are stored, you can specify
teh archive. In this case they are available from both sopac and nrcan.

* Run **quickLook** for the same date. You will see two graphical representations of the data. The first is 
periodograms for the four geographic quadrants (northwest, northeast, and so on). You are looking for nice clean (and 
colorful) peaks. Color means they have passed Quality Control (QC). The second plot summarizes the 
RH retrievals and how the QC metrics look compared to the defaults. In this case the x-axis is azimuth in degrees.

From these plots, how does the correct *RH* value compare with the one you assumed earlier when you 
were trying out the webapp?  How about the azimuths?  Go back to the reflection zone webapp and 
make sure you are happy with your azimuth and elevation angle selections.

* We need to save our analysis strategy using **make_json_input**. At a minimum you need to know 
the latitude, longitude, and height for the station:

<code>make_json_input ross LAT  LONG  HEIGHT </code> 

Make sure you read the <code>gnssrefl</code> documentation
(or <code>make_json_input -h</code>) to see how to set the elevation angles and RH 
restrictions (you might as well restrict RH above 2 meters 
because those lower RH values are clutter and not related to the water).
Since we only want to use the L1 data, you should use the <code>-l1 True</code> flag.
You will need to hand edit the azimuths. Since satellites rise and set in different quadrants, you want
to cut up your azimuth range in ~60 degree chunks.  So if you wanted to use 90-270, you should say 90-180
and 180-270. You can use smaller chunks, but I generally do not use less than 45 degree azimuth chunks.

*Run **gnssir** for the same date. This module is meant for *routine analysis* and thus there are not a lot
of bells and whistles. Check that something is actually created (the screen output will tell you where it is).

The output of **gnssir** tells you the vertical distance between the GPS antenna and the lake. That is not 
super exciting, so it is a little more intetesting to see if it changes, which means we have to analyze a bit more data.

[Ross Tide Gauge Data](https://www.isdm-gdsi.gc.ca/isdm-gdsi/twl-mne/inventory-inventaire/sd-ds-eng.asp?no=10220&user=isdm-gdsi&region=CA)