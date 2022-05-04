
## Progress Update and Reflection
Our team has been working with CalCOFI’s oceanographic data collected from the chemical analysis of seawater samples off the coast of California. Our project is one of exploratory analysis that aims to produce a myriad of interactive visualizations that allow for users to understand the spatial and temporal variability of oxygen concentration in the ocean. Our project also aims to detect and highlight the occurrence and prevalence of hypoxia (low oxygen concentration) “hotspots” in the sampling region.  
Our first step was to merge two different datasets provided by CalCOFI - one that stored the physical, chemical, and biological measurements from the seawater analysis, and another that stored spatial and temporal information about where and when the seawater sample was taken. As the entire dataset includes observations from 1949 through 2021, we subsetted the data to work with observations from 1970-2022 for the development of our visualizations. 
We have begun generating various graphs with the raw data that would allow users to grasp the spatial variability of oxygen concentration. One example of such a graph is the station line depth profile graph shown in Figure 1. This graph captures the varying oxygen concentration along one sampling line, for each of the 4 quarters (Winter, Spring, Summer, Fall) of a single year — thus, it illustrates how oxygen varies spatially during each season by depth and distance from shore. 
 
<figure>
	<img src="Images/Station line depth profile(2015 - Line 76.7).png">
	<figcaption>Figure 1. Oxygen concentration recorded at different depths and distances from shore along sampling line 76.7 in 2015. Depths (along the y-axis) are binned such that each bin contains a roughly equal number of observations.<figcaption>
<figure>
		
Another example of a graph that explores the spatial variation of oxygen concentration is the thermocline plot displayed in Figure 2. This plot showcases how 3 significant features of seawater — oxygen concentration, salinity, and temperature — change as depth increases. Since the data from CalCOFI samples water at depths of over 500m below the surface, these depth profile plots help us recognize the depths at which these significant properties have the greatest variation. This will help guide us when we move on to identifying significant hypoxic events off the California coast.

<figure>
	<img src="Images/Thermocline plot - 2012/3.png">
	<figcaption>Figure 2. The above depth profile plot plots the change is oxygen concentration (measured in mL of O2 per liter of seawater), salinity (measured in the practical salinity scale), and temperature (degrees Celsius) against depth from surface (meters). Each individual line in the graph represents the measurements taken from seawater at a single sampling station.<figcaption>
<figure>


We have also begun using the Shiny package in R — which enables us to build an interactive web app — to create a dashboard that displays a map of the CalCOFI sampling locations (Figure 3). The shiny dashboard takes the user’s inputted year and quarter and highlights the stations on the map that were sampled at that time. The user inputs will also be used to generate the corresponding graphs from Figures 1 and 2. 

<figure>
	<img src="Images/basemap.png">
	<figcaption>Figure 3. Screenshot from dashboard. User input panel on the left and side. Base map displaying station locations on right hand side. Stations are highlighted in red if they were sampled at the user inputed time.<figcaption>
<figure>

		

The generation of the exploratory plots above have helped us visualize the variation in oxygen concentration close to shore. We now face the task of defining thresholds for hypoxia so that we may clearly identify significant hypoxic events that we can then highlight through our graphs. Our initial efforts have also encouraged us to think about how we’d like to link all our visualizations together using the interactive features in shiny. Creating our base map helped us imagine how we might allow users to interact directly with the map to pull up visualizations related to the areas of California Coast they’re interested in. 

## Next Steps
Our immediate next steps involve improving the user interface for Shiny to create a more complete dashboard. This will include adjusting the appearance of the dashboard, perfecting plot appearances, and adding descriptive text to incorporated figures. We will also add more interactive elements, such as allowing users to select a certain sampling line to pull up the corresponding Fig. 1 plot, and highlighting the stations of the selected line in the corresponding Fig. 2 plot. 
From an exploratory standpoint, we are beginning to work on spatial interpolation — generating a model that generates continuous values from the discrete data we are working with. This, combined with how we define hypoxic events, will allow us to create 3D visualizations and animations to highlight hotspots off the California coast. We are also hoping to conduct principal component analysis to be used in our final analysis and data story.
