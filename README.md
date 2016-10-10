# Heatmap
How to make your own heatmap using your Strava activities

## Pre-Requisites: 
* A Strava Account containing all the activities/runs you want on your heatmap.
* Using an Ubunutu Operating System (should also work with *ubunutu flavours/deriatives such as Lubuntu, Kubunutu & Xubuntu).
* Sudo/Root access/password


## Step 1: <br/> Download Data from Strava
Login to Strava then go "My Account" -> "Settings" -> scroll down to "Download your data".<br/>
Select "Download all your activites". An email will be sent to you with a link to download all your activities (this may take sometime)<br/>
Once you recieve the email, download the zip folder to your computer.

## Step 2: <br/>Create Mapbox Account
Go to www.mapbox.com and create a (free) account for yourself.<br/>
Mapbox has a free tier which is good enough for personal/light usage (50,000 views/month).

## Step 3: <br/> Installing OGR2OGR
Add the Ubuntu GIS repository to your system.
```bash
sudo add-apt-repository ppa:ubuntugis/ppa && sudo apt-get update
```
When prompted for confirmation press the ENTER key. Once completed enter:
```bash
sudo apt-get install gdal-bin
```
When prompted type 'y' and press enter to confirm installation.<br/>
You will now have ogr2ogr installed. To verify try: 'ogrinfo' in the terminal.<br/>
If it returns 'Usage: ogrinfo [--help-general] ....' then ogr2ogr has been successfully installed.<br/>

## Step 4: <br/> Locate the downloaded activities.zip folder.
Navigate to the zip folder that you downloaded from the email (generally named activities.zip).<br/>
This will generally download to the Downloads directory (~/Downloads).<br/>
Once you have located the zipped folder unzip/extract the contents of activities.zip to a new folder named activities.<br/>

## Step 5: <br/> Navigate to the activities folder.
In the terminal navigate to unzipped activities folder. <br/>
If the folder is located in the Downloads directory per Step 4, just enter the following into the terminal:
```
cd ~/Downloads/activities
```
## Step 6: <br/> Create a Shapefile containing your activities.
In the terminal enter:
```bash
ogr2ogr runs <first-track.gpx>
```
_Replace \<first-track.gpx> with the oldest/earliest file in the activities folder._
An example would be 
```bash
ogr2ogr runs 20160101-130000-Run.gpx
```
## Step 7: <br/> Bash Script to append the files.
In the the terminal type:
```bash
nano ogr2ogr.bash
```
Hit enter and the shell/terminal will change into the nano text editor.<br/>
Type the following into the editor:
 ```bash
 #!/bin/bash
directory=~/Downloads/activities/
for file in $( ls $directory )
do
	ogr2ogr -skipfailures -append runs $directory$file
done
```
Press ctrl+x, type y to save and then press enter to confirm.

## Step 8: <br/> Execute the bash script.
In the terminal type:
```bash
chmod 700 ogr2ogr.bash
```
to ensure that you have full privledges to execute the script.<br/>
Then type:
```bash
sh ogr2ogr.bash 
```
to execute the script.<br/>
It will then take a few minutes to execute the script. It may throw some warnings but it should be ok.<br/>
Once it has completed running the script there will be a 'runs' folder inside the activites folder.<br/>

## Step 9: <br/>Zip the required Shapefile folders.
Navigate into the 'runs' folder.<br/>
Then create a new folder named 'heatmap' .<br/>
Copy the files tracks.dbf, tracks.prj, tracks.shp & tracks.shx into the heatmap folder.<br/>
Once you have copied them into the folder, then compress heatmap into heatmap.zip

## Step 10: <br/>Upload the folder.
In your browser go the mapbox.com, login with the credentials you made earlier and navigate to [Mapbox Studio url: mapbox.com/studio] (http://www.mapbox.com/studio).<br/>
Under the Tilesets block in the center, click 'New tileset'.<br/>
Then click 'Select a file' and navigate to the heatmap.zip folder (which should be located in Downloads/activities/runs/ if your folder structure has been kept the same as the guide).<br/>
Select 'heatmap.zip' and click the Open button.<br/>
Then click the 'Upload' button and wait for it to upload (this can take a few minutes).<br/>

## Setp 11: <br/> Base Map Style
Once it has finished uploading, click on 'Styles' on the side bar.<br/>
Then click the 'New Style' button.<br/>
And then select the base map style you would like. For this guide we will select 'Satellite'. Then click 'Create'.<br/>

## Step 12 <br/> Adding your heatmap/tileset to the base map/style
Return to the Tilesets page (on the side bar click the four squares icon).<br/>
Then click on the name of the heatmap.zip file you uploaded.<br/>
They (Mapbox) add some additional letters/numbers to the name, so it might be something like heatmap-c6i6s2 or similar.<br/>
Then click the 'Add to style' button' and select the Style you had generated earlier (which would be Satellite in our case).<br/>
You will then be taken to the editor area.<br/>
A 'New Layer' side panel should be open ith your heatmap file/tileset as it's source. Just click 'Create Layer'.<br/>
This will overlay your heatmap tileset over the base style (which in our case is a satellite map).<br/>

## Set 13: <br/> Personalizing your heatmap
You will want to change some of the styling away from the defaults.<br/>
On the left panel select the heatmap tileset layer, which will show an aditional layer. <br/>
For this guide we will change the following: Color, Opacity, Width and Blur. With all four the best option will vary on personal preference, but for this guide I will make some suggestions).<br/>
+ Color \- click on the input field next to the Color and select the color you want for your heatmap (you can enter either Hex, RGB, HSV values or use the color picker). We will enter the Hex value of #0066ff.<br/>
+ Opacity \- With Opacity you can enter a value between 0 and 1 (you can increment it in decimal values such as 0.25, 0.33, 0.5, etc). Play around with these as your decision will vary based on what base map you are using and the color of the lines. We will enter an opacity of 0.5.<br/>
+ Width \- The default width for the lines is 1px. For this guide we will set it to 2px.<br/>
+ Blur \- The Blur option really helps give the map a more heatmap feel. As with the Opacity option you will need to play around with it to find what looks best for you. For this guide we will give Blur the value of 1px.<br/>
+ _All these options also have the ability to change them based on the zoom level, which you can play around with later._<br/>
Once you have made the changes, click "Publish" in the top of the centeral panel. Click "Publish" to confirm.	<br/>
Then click "Preview & Use" and then click "Preview" on the right.<br/>
Your heatmap is finished! :)<br/><br/>

You can go back to the styles again at anytime and change them as feel.



