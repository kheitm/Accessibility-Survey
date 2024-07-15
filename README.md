
## Accessibility Survey

### Project Overview
This project outlines data analysis for one of two surveys that were run to investigate the barriers, pain points, and strategies used by students with individual needs (SmiBs) i.e. students with disabilities and students with family care demands, in the University/Hochschule system in Germany. This information provided information on the level of digital accessibility in eductaional materials and was used in the design and development of opensource products to support these students: (i) the [BlindDate website](https://shuffle-project.github.io/blinddate/) which provides a virtual encounter with SmiBS through interactive personas, and serious games; and (ii) [MELVIN](https://melvin.shuffle-projekt.de/de-DE/auth) a live, multiuser video caption editor and accessible player.

### Data Sources
The data is taken from a LimeSurvey exported .csv file consisting of answers from 695 respondents and 23 open-ended and scaled items, with a combination of quantitative and qualitative data: 
* __Front Interactions__ (1.6+ million records) includes visitor activity on the website by visitor and by session, from front page clickthroughs to various pages including checkout.
* __Purchases__ (126 546 records) focuses on user purchase activity including type of subscription (monthly, quarterly, annual) and the purchase price and date.
* __Front Visitors__ (244 670 records) is a link between the other two tables by referencing user_id and visitor_id.
 
### Tools


### Data Cleaning/Preparation
The main preparation neeeded to get the data ready for further processing included:
* Checking for NULL values and removing records where necessary
* Removing test users (i.e. users who paid $0 for a subscription) from the data
* [Re-formatting website page names to use aliases](pre-processing/url_to_alias.sql)
* [Changing subscription type from code to text](pre-processing/reformat_purchase_type.sql)


### Data Analysis
The data were extracted using [SQL queries](user_journey_analysis/user_journey_by_quarter.sql) on MySQLWorkbench to show the 
<img src="src/user_journey_file.png?raw=true"/>

#### Insights


These data were visualized on Tableau as seen in the following examples:

<img src="user_journey_analysis/Q1_Subscription_Types.png?raw=true" width="50%"/>

<img src="user_journey_analysis/Q1_Average_Length_of_Journey.png?raw=true" width="50%"/>


### Further Data Analysis


