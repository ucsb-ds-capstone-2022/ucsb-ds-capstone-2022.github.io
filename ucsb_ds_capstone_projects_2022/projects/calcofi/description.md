# CalCOFI
Project description

The aim should be to orient the reader to the project domain, datasets, objectives, and motivations. Sufficiently so that one can easily answer the following questions after reading your post:

- Who is the project sponsor? Are you working with a company or in a research lab? What does the company / lab do?

- What is the subject of the team’s project? This is often easiest to characterize through the specific combination of domain or industry application and technical area/task that the project pertains to; for instance, distinguishing terrestrial habitats from satellite images (application) using image classification (technical task). How does the subject relate to the broader activities of the sponsor’s lab / company?

- What data is the team working with? This should be answerable at a certain level of detail – the reader should be able to describe the main features/variables, observations, and how data were collected. Often, together with verbal explanation, a few example rows or a table of variable names and descriptions is a helpful way to present this information; of course, for large datasets, or for teams who are in the process of obtaining data, purely qualitative description may have to suffice.

- What is the team hoping to accomplish (or working on right now)? The reader should have some sense of your goals or questions of interest. If you don’t have much clarity about your objectives presently, that’s okay – you can instead try to articulate the question(s) that are guiding your first steps.

The content can be kept basic:

- domain background and project motivation;

- available dataset(s);

- project objectives.

If it helps you to begin with some structure, create an outline with one header for each item above (but give them good titles – see general suggestions). Review and critique last year’s project updates for further ideas about post organization. You do not need to stick to this particular outline and should do what suits your project best.

Your post should meet the following criteria:

1. domain-specific background relevant to the project is clearly and concisely explained;

2. for at least one primary dataset, the method of data collection, data quantity, and primary attributes (observations and variables) are clearly described;

3. at least one figure is included – this can be any graphic that helps to convey an idea to the reader, and doesn’t necessarily need to be created from your project data;

4. at least one question of interest or project aim is clearly stated.

Don’t worry about getting every last thing right for the initial submission. You’ll do a peer review with another team shortly after submission and will be encouraged to post revisions before recieving feedback from your project mentor. More precisely, here are the logistics around post submission:

- Pull requests to the website repo should be submitted by Friday February 4 at 5:00 pm PST.

- Your team will review another team’s post over the weekend (assignments determined in advance).

- We will do a peer review in class on Monday February 7.

- Revisions should be submitted (via pull request) by the end of the week.

Mentors will provide feedback. Further revisions optional (but encouraged).

# Data Description:
## Data Collection:
CalCOFI is a project that has been runing for a long time, and as a result their data collection methods follow closely with protocol to maintain consistency over the many years the project has been running. Data is taken 4 times a year (once a quarter). A ship takes scientists out to the same set of coordinates every year. At those coordinates, a CTD Rosette (pictured below) is lowered into the water, and at set discrete depths, ocean water samples are taken. Many parameters are measured with the samples taken such as Oxygen concentration, pH, salinity, temperature etc. Because of the size of the dataset, for development purposes we have cut down on the time range we are looking at to the years **2000-2021**.
![CTD Rosette Bottle being used in ocean water](https://wp.calcofi.org/wp/wp-content/uploads/2020/03/1904RL_sta93-50_CTDSurface-1024x770.jpg "CTD Rosette Bottles used to Sample Oceanwater")
![Visual representation of sampling method used by CalCOFI](https://user-images.githubusercontent.com/30590837/149233121-d5e2e83e-b72a-41e5-9e83-00ef40877b43.png "Visual representation of sampling methods")

## Data definitions
We also cut down on the parameters used partly for development purposes, partly because we are only interested in certain parameters as of right now. The table below represents the parameters we use, and what they represent:
| **Field Name** | **Units**                | **Description**                                                                                                                                                          |   || **Field Name** | **Units**       | **Description**                                                                                  |
|----------------|-----------------|--------------------------------------------------------------------------------------------------|
| Cst_Cnt        | n.a.            | "Cast Count - All CalCOFI casts ever conducted, consecutively numbered"                          |
| Cruise_ID      | n.a.            | Cruise identifier [Year]-[Month]-[Day]-C-[Ship Code]                                             |
| Cruise         | n.a.            | Cruise Name [Year][Month]                                                                        |
| Cruz_Sta       | n.a.            | Cruise Name and Station [Year][Month][Line][Station]                                             |
| Cast_ID        | n.a.            | Cast Identifier [Century] - [YY][MM][ShipCode] - [CastType][Julian Day] - [CastTime]-[Line][Sta] |
| Sta_ID         | n.a.            | Line and Station                                                                                 |
| Quarter        | n.a.            | Quarter of the year                                                                              |
| Date           | time            | Date (Month Day Year)                                                                            |
| Year           | n.a.            | Year                                                                                             |
| Month          | n.a.            | Month                                                                                            |
| Lat_Dec        | decimal degrees | Observed Latitude in decimal degrees                                                             |
| Lon_Dec        | decimal degrees | Observed Longitude in decimal degrees                                                            |
| St_Line        | n.a.            | Nearest Standard Line                                                                            |

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


