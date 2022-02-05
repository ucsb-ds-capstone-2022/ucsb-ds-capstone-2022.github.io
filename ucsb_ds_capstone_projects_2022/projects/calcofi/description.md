# CalCOFI
Project description

- What data is the team working with? This should be answerable at a certain level of detail – the reader should be able to describe the main features/variables, observations, and how data were collected. Often, together with verbal explanation, a few example rows or a table of variable names and descriptions is a helpful way to present this information; of course, for large datasets, or for teams who are in the process of obtaining data, purely qualitative description may have to suffice.
# Data Description:
## Data Collection:
CalCOFI is a project that has been runing for a long time, and as a result their data collection methods follow closely with protocol to maintain consistency over the many years the project has been running. Data is taken 4 times a year (once a quarter). A ship takes scientists out to the same set of coordinates every year. At those coordinates, a CTD Rosette (pictured below) is lowered into the water, and at set discrete depths, ocean water samples are taken. Many parameters are measured with the samples taken such as Oxygen concentration, pH, salinity, temperature etc. Because of the size of the dataset, for development purposes we have cut down on the time range we are looking at to the years **2000-2021**.
![CTD Rosette Bottle being used in ocean water](https://wp.calcofi.org/wp/wp-content/uploads/2020/03/1904RL_sta93-50_CTDSurface-1024x770.jpg "CTD Rosette Bottles used to Sample Oceanwater")
![Visual representation of sampling method used by CalCOFI](https://user-images.githubusercontent.com/30590837/149233121-d5e2e83e-b72a-41e5-9e83-00ef40877b43.png "Visual representation of sampling methods")

## Data definitions
We also cut down on the parameters used partly for development purposes, partly because we are only interested in certain parameters as of right now. The table below represents the parameters we use, and what they represent:
| **Field Name** | **Units**                | **Description**                                                                                                                                                          |   |
|----------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
| Cst_Cnt        | n.a.                     | Cast Count - All CalCOFI casts ever conducted, consecutively numbered                                                                                                    |   |
| Btl_Cnt        | n.a.                     | "Bottle Count - All CalCOFI bottles ever sampled, consecutively numbered"                                                                                                |   |
| Sta_ID         | n.a.                     | Line and Station [Line] [Station]                                                                                                                                        |   |
| Depth_ID       | n.a.                     | Uses the Cast_ID prefix ([Century]-[Year][Month][ShipCode]-[CastType][Julian Day]-[CastTime]-[Line][Sta]) but adds three additional variables: [Depth][Bottle]-[Rec_Ind] |   |
| Depthm         | meters                   | Bottle depth in meters                                                                                                                                                   |   |
| T_degC         | degrees Celsius          | Water temperature in degrees Celsius                                                                                                                                     |   |
| Salnty         | Practical Salinity Scale | Salinity (Practical Salinity Scale 1978)                                                                                                                                 |   |
| O2ml_L         | milliliters per liter    | Milliliters oxygen per liter of seawater                                                                                                                                 |   |
| R_Depth        | meters                   | Reported Depth (from pressure) in meters                                                                                                                                 |   |
| pH1            | pH scale                 | pH (the degree of acidity/alkalinity of a solution)                                                                                                                      |   |
| pH2            | pH scale                 | pH (the degree of acidity/alkalinity of a solution) on a replicate sample                                                                                                |   |
| Cruise_ID  | n.a.            | Cruise identifier [Year]-[Month]-[Day]-C-[Ship Code]                                             |
| Cruise     | n.a.            | Cruise Name [Year][Month]                                                                        |
| Cruz_Sta   | n.a.            | Cruise Name and Station [Year][Month][Line][Station]                                             |
| Cast_ID    | n.a.            | Cast Identifier [Century] - [YY][MM][ShipCode] - [CastType][Julian Day] - [CastTime]-[Line][Sta] |
| Sta_ID     | n.a.            | Line and Station                                                                                 |
| Quarter    | n.a.            | Quarter of the year                                                                              |
| Date       | time            | Date (Month Day Year)                                                                            |
| Year       | n.a.            | Year                                                                                             |
| Month      | n.a.            | Month                                                                                            |
| Lat_Dec    | decimal degrees | Observed Latitude in decimal degrees                                                             |
| Lon_Dec    | decimal degrees | Observed Longitude in decimal degrees                                                            |
| St_Line    | n.a.            | Nearest Standard Line                                                                            |


