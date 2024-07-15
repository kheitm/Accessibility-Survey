
## Accessibility Survey

### Project Overview
This project outlines data analysis for one of two surveys that were run to investigate the barriers, pain points, and strategies used by students with individual needs (SmiBs) in the University/Hochschule system in Germany. This information provided information on the level of digital accessibility in educational materials and was used in the design and development of opensource products to support these students: (i) the [BlindDate website](https://shuffle-project.github.io/blinddate/) which provides a virtual encounter with SmiBS through interactive personas, and serious games; and (ii) [MELVIN](https://melvin.shuffle-projekt.de/de-DE/auth) a live, multiuser video caption editor and accessible player.

### Data Sources
The data is taken from a LimeSurvey exported .csv file consisting of answers from 695 respondents and 23 open-ended, Yes-No, or scaled items, with a combination of quantitative and qualitative data. Key questions focused on: 
* __Type of Disability__  includes disability categories (e.g. Visual disability, Autism; AD(H)D) as well as family care barriers.
* __Barriers__ examines how well the students can access a range of teaching and learning materials including videos, PowerPoints, readings, live lectures etc.
* __Exams and Accommodations__ focuses on how students deal with their barriers in Exam conditions, and in assignments and what types of support and accommodations are provided.
* __Study Level__ information included type of study( Bachelor, Masters) and which Semester, as students would have different levels of experience dealing with challenges depending on their previous time spent studying.
 

### Tools
![R Badge](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=fff&style=flat)
![Python Badge](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=fff&style=flat)

### Data Cleaning/Preparation
Most initial data prepration was done by the LimeSurvey platform which identified incomplete answers and any missing values. The data were still checked for NULL values, but no records were removed from the analysis. Lime survey also provdies initial descriptive statistics showing the percentage of each answer from the total. 


### Data Analysis
While the data did not initial summarization into percentages of total answers, this was not felt to be sufficient. Analysis focused on two areas:
* Improving the quality of data visualizations for simple descriptive statistics
* Delving into more complex relations in the data such as the fact that many students have more than one barrier type and the resulting scales and Yes-No responses could be influenced by one of these rather than the other(s).
* Deriving sentiment from the open-ended questions through an automated pipeline that could be traingulated with thematic analysis.


#### Insights
These data were visualized on Tableau as seen in the following examples:



### Further Data Analysis


