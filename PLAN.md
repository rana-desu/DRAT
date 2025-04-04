# Disaster Resource Allocation Tool [WIP]

An intelligent algorithmic-driven resource allocation tool that prioritizes distribution of relief supplies based on real-time needs accessed via crowdsourced data. <br>
As of now, this resource allocation tool primarily concerns with the disasters people face in india. [subject-to-change]

## Crowdsourced Data
### Data Collection Sources
1. A user-interface like website or mobile apps.
2. Newschannels (RSS Feed).
3. Scouting helicopters with a pre-established channel.

### Data Categories
- Type of disaster
    1. Pre-defined precautions.
    2. Rescue & Resource type.
- Number of people stuck in disaster
    1. Amount
    2. Location/Coords
- On basis of injury
    1. Type of injury caused
    2. Amount
- Categories of people (priority-based)

## Classification
### Resources
1. Food support.
2. Medical support.
3. Emergency evacuation.

### Disasters
1. Heavy Rainfalls -> Floods.
2. Strong Winds & Heavy Rainfalls -> Cyclones.
3. Landslides.
4. Heatwaves.
5. Earthquakes.
6. Heavy Snowfalls.
7. Forest Fires.

## Generalised Priority Calculation (independent of disaster types): `PriorityEngine`
### Class Models
- **Food Supply Prioritization**: `starvation_priority()` <br>
Makes decision about how many people are currently surviving and to whom the food should be supplied.
    1. Number of Days <br>
    Threshold: If starving for 3+ days, then food delivery is a MUST/mandatory.
    2. Number of People <br>
    Threshold: Availaibility of ration resources.
    3. Type of people (Child, Older Citizens, Adult)
    3. Location from base to affected location. <br>
    example: <br>
    Considering 2 people starving for 5-7 days, and 10 people starving for 1-2 days: 2 people gets priority.
    
- **Medical Support Prioritization**: `medical_priority()`
- **Evacuation Prioritization**: `evacuation_priority()`

## Implementation
### BACKEND
1. Integrate INSAT-3D weather data + NDMA risk maps.
2. Use crowdsourced reports (e.g., MyGov Sahyog app).
3. Prioritize responses via severity scores (floods > heatwaves > pollution).
4. Data filtering.

### UI/UX
- Data Submitting Interfaces
    1. Users
    2. Scouting Helicopters & SAR teams
    3. News & Social Media

### Data Gathering Streams
System has three trackers for collected data (high-to-low priority).
1. Reports from scouting helicopters and SAR teams.
2. Direct ground-level report from: Live location, User Reports on Website/App.
3. Reports gathered from news and social media.
    
### SQLite Database
Data is to be normalised based on the above streams and their priorities.
1. **Auth Data Table**: Data from `stream (1)`
2. **User Data Table**: Data from `stream (2)`
3. **News Data Table**: Data from `stream (3)` <br>
Data from the above table is processed, normalised, and stored in a MASTER table. <br>
4. **Master Table**: Stores processed data from table Auth, User, and News data tables <br>
All the tables above must be updated after every resource allocation and extraction attempt. <br>
5. **Log Table**: Maintains logs of table Auth, User, and Master tables <br>
This can be further utilised to train ML models. <br>
6. **Priority Log Table**: Maintains logs for data generated by PriorityEngine <br>