# UAV mapping logistics

1. Objective definition
	- study area: mark study area on your phone map app
	- features of interest
	- mapping accuracy
	- other mapping requirements

2. Legal and regulatory consideration
	- check FAA airspace
	- UAV registration
	- permissions and authorizations 

3. Safety precautions
	- risk assessment
	- check powerlines 
	- weather conditions
	- first-aid kit
	- safety goggles
	- visible vest
	- helmet
	- fire extinguisher

4. Flight planning
	- flight mission planning: 1) path planning based on mapping objectives, 2) elevation check, 3) collision check
	- flight time estimation and battery number
	- RTK GPS and GCP targets
	- GCP placement planning
	- check battery status

5. Before flight on site
	- UAV inspection
	- RC/GCS inspection
	- place GCP
	- lanuch/landing targets
	- potential obstacle check
	- pilot status check

6. During the flight
	- maintain a clear line of sight with the UAV
	- monitor battery levels and ensure a safe return depletion
	- monitor telemetry and UAV status

7. Post flight
	- data processing 
	- data storage
	- data sharing


---
## DJI flight planning
1. check study area  
- View terrain in Google Earth Pro  
- Check FAA airspace: https://faa.maps.arcgis.com/apps/webappviewer/index.html?id=9c2e4406710048e19806ebf6a06754ad   
- Check power lines: https://www.arcgis.com/apps/mapviewer/index.html?layers=d4090758322c4d32a4cd002ffaa0aa12  

2. 1. plan a grid survey using QGroundControl (need a converter)  
- Camera configuration can be set using sample photo metadata, which can be found using any online tools. The metadata can also be found on Mission Planner by uploading a photo sample.
- Don't worry about the camera heading.
- Generate a kml file. However, this kml file uses MSL. The path can be visualized in Google Earth Pro. When importing this path to litchi hub, the AGL uses the number in MSL. Therefore, the actual elevation can be ridiculously high (check path generated from litchi). A converter is needed to convert MSL in the kml file to AGL. 
- The DEM that QGC uses is not accurate. 

2. 2. plan a grid survey using https://ancient.land/  
- Combine horizontal and vertical surveys by combining two csv files

3. examine flight path in Google Earth Pro  
- Make sure no collision along the path, between home and path start, and between home and path end.

4. unload mission to litchi hub: https://flylitchi.com/hub   
- Config the camera capture interval. (Make sure the resulting time from the interval and maximum speed is large enough.) 
- Make sure to save the mission in litchi hub.
- Open litchi app to download the mission.

## Litchi tips
### 1. path mode
Litchi has two path modes: straight and curve. For straight mode, the UAV will stop at each waypoint. Thus, using straight mode can significantly increase the flight time. 

Curve mode enables UAV to have a smooth transition, which may save flight time. The smoothness depends on UAV velocity and curve size. The transition is smoother when the velocity direction is on one direction. On the contrary, if the UAV is changing from ascending to descending, the transition may slow down very much. When the velocity change is too large, the transition can be slow. 

A small curve size may increase the transition time; a large curve size may cause loss of mapping at waypoints. Default curve size only works for selected or newly added waypoints. On Litchi Hub, using ctrl + click can select multiple waypoints and all waypoints. On Litchi app, using the multiple selection function can select multiple and all waypoints. It is important to notice that the altitude should be set relative to **current** when setting curve, then the pre-planned elevation won't be changed. 

### 2. speed
Litchi setting has maximum cruise velocity and maximum flight velocity. Once changed, they are effective immediately. The maximum cruise velocity is the maximum velocity a UAV can fly when executing mission waypoints. The maximum flight velocity is the maximum velocity when the UAV is controlled using N mode. Note that when switching to S mode, the maximum velocity can exceed the maximum flight velocity in the setting.
