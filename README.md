# Fitbit Time Series Project

## About the Project
### Goals
- Clean the current data set. Document the code that takes the original source to the workable data.
- Draw conclusions on the individual who wore the fitness tracker
- Make predictions on the two weeks of missing data
### Background
Scenario:
>A man wearing a lab coat and a worried expression bursts into your office.
>
>"I need help!" he says. "I mixed up the labels and have one extra!"
>
>Before you can ask what he has an extra of, he throws a USB thumb drive in your direction. As you wonder who still uses thumb drives, the man in the lab coat rushes back out the door.
>
>"Oh, and I need to know what the missing next 2 weeks will look like too" he says on his way out.
>
>Before you can think of a question to ask, he's out of sight.
>
>You open up the files on the drive and find the data.
>
>Even though you just started a week ago, as a data scientist for Big Research Co., you know several things:
>
>- Your company is running multiple different experiments: drug trials, testing different fitness equipment, and some "very ethical" human experimentation.
>- Everyone in every experiment wears a fitbit.
>- No one seems to have time for smalltalk.
>- Everyone on staff has a fitbit.
>
>It's a pretty good guess that this data comes from somebody's fitbit, but you don't know who it belongs to, it could be from someone participating in a research experiment, or a staff member just going about their days.


### Deliverables
- A notebook containing my analysis
- Predictions for the missing two weeks worth of data in a separate csv file.
- The above information distilled into two slides that can be shared with a general audience. Include at least one visualization, and make sure that the visualization is clearly labeled.

### Acknowledgments
- Data (fitbit)
- Codeup Curriculum
- [FitBit Floors](https://help.fitbit.com/articles/en_US/Help_article/1141.htm)
- [FitBit Activity](https://help.fitbit.com/articles/en_US/Help_article/1379.htm?Highlight=activity)

## Data Dictionary
| Feature                | Description                                                             | Data Type      |
|------------------------|-------------------------------------------------------------------------|----------------|
| date                   | The date in format: yyyy-mm-dd.                                         | datetime/index |
| calories_burned        | The number of total calories burned that day.                           | integer        |
| steps                  | The number of steps tracked that day.                                   | integer        |
| distance               | The number of miles traveled tracked that day.                          | float          |
| floors                 | The number of floors (about 10 feet or 3 meters) climbed that day.      | float          |
| minutes_sedentary      | The total number of minutes the person isn't walking or taking steps.   | integer        |
| minutes_lightly_active | The total number of minutes the person is within their 'fat burn' zone. | float          |
| minutes_fairly_active  | The total number of minutes the person is within their 'cardio' zone.   | float          |
| minutes_very_active    | The total number of minutes the person is within their 'peak' zone.     | float          |
| activity_calories      | The total number of calories the person has burned during activity.     | integer        |
| month                  | The numerical representation of the month. i.e. 1-Jan, etc.             | integer        |
| weekday                | The day of the week. i.e. Monday, Tuesday, etc.                         | object         |

## Initial Thoughts & Hypotheses
### Thoughts
- The data is mostly the activity of the individual including: date, calories burned, steps, distance, floors, minutes, sedentary, minutes lightly active, minutes fairly active, minutes very active, activity calories.
- These will be the features worth exploring
- Data is from April of 2018 to the end of the year
- Food log, and calories in sections are inconsistent and should be omitted since the majority are null values.
- May need to change some of the features to match the data
- Going to look into the calories burned over time.
- What part of the year does this individual burn the most calories?
- Does this individual burn more calories on the weekends?
- Based on activity level can we determine how fit this individual is?

### Hypotheses
- The steps and calories burned have a direct correlation.
- This individual burns more calories in the summertime.
- Floors and calories burned have a direct correlation.

## Project Steps
### Acquire
1. Import necessary modules
  - pandas
  - numpy
  - matplotlib
  - seaborn
2. Clean up the data in an spreadsheet for easier reading into a Pandas DataFrame
  - Food log and Calories in sections are mostly null so they were eliminated
  - Saved to a CSV in local repository
3. Peeked into and summarized data (columns, rows, data types, nulls, etc.)
 
### Prepare
1. Data Cleaning
  - Dropped last 22 days/rows since we will be predicting them (they were null).
  - Lowercase the features, use a '_' to replace whitespace (best practice convention).
  - Change `date` column --> 'datetime' type
  - Change the index --> `date`, sorted index by date
  - Drop remaining `date` column
  - Change the commas in `calories_burned`, `steps`, `minutes_sedentary`, `activity_calories` --> '_'
  - Change columns to 'int' type
2. Summarized the preppared data
  - Peek into cleaned data
3. Split the data into train and test for exploration
  - Split by percentage
  - Visualized split

### Explore
1. Plotted the distribution (histograms) of each of the 9 target variables
  - Noted the patterns
2. Plotted targets by categorical variables
  - Visualized average for each target by month 
  - Visualied average for each target by weekday
    - bar and box plots

### Model
Ran 3 basic models that used a single value to predict on the validate data set and measured the performances using RMSE.

1. Last Observed Value: The simplest method for forecasting is to predict all future values to be the last observed value.
  - Make predictions using the last value in train as a prediction for every single day forward.
  - Plot the actual vs. the predicted values to compare for each target variable
  - Evaluated each variable by its RMSE, displayed in a DataFrame
2. Simple Average: Take the simple average of historical values and use that value to predict future values.
  - Make predictions using the average as a prediction for every single day forward..
  - Plot the actual vs. the predicted values to compare for each target variable
  - Evaluated each variable by its RMSE, displayed in a DataFrame
3. Moving Average: the average over the last 30-days will be used as the forecasted value.
  - Make predictions using the last 30-day average in train as a prediction for every single day forward.
  - Plot the actual vs. the predicted values to compare for each target variable
  - Evaluated each variable by its RMSE, displayed in a DataFrame
  
Ran 2 more complex models to predict on the validate data set and measured the performances using RMSE.

1. Holt's Linear Trend: Exponential smoothing applied to both the average and the trend (slope). α / smoothing_level: smoothing parameter for mean. Values closer to 1 will have less of a smoothing effect and will give greater weight to recent values. β / smoothing_slope: smoothing parameter for the slope. Values closer to 1 will give greater weight to recent slope/values.
  - Make predictions using a slope factor and default parameters in train as a prediction for every single day forward.
  - Plot the actual vs. the predicted values to compare for each target variable
  - Evaluated each variable by its RMSE, displayed in a DataFrame
2. Previous Cycle: Uses period in the training data as a forecast to predict future values.
  - Make predictions using the last two weeks as predictions for the next two weeks.
  - Plot the actual vs. the predicted values to compare for each target variable
  - Evaluated each variable by its RMSE, displayed in a DataFrame

**Best Model: 30 Day Moving Average**
- Performed ~10% better than Last Observed Value (baseline).
- Used single values to make prediction on the missing two weeks.
- Created a CSV of the next two week predictions

### Conclusions
>**Clean the current data set to obtain workable data**: 
>I used a spreadsheet to clean up the data initially to be read into a DataFrame from a CSV. I cleaned the data and preprocessed it for exploration and modeling.
>
>**Draw conclusions on the individual who wore the fitness tracker**: 
>This person is a weekend warrior. They stay active on weekends and probably just focus on work duting the week. They're most active in the summer which is typical for the vacation season. They may be hiking in the early fall since floors are highest in September.
>
>**Make predictions on the two weeks of missing data**: 
>We are basing all our predictions on the 30 day rolling average since those predictions have the smallest chance of error.
    
<b>Next Steps</b>
- There isn't enough data. It's was difficult to see any trends or seaonality with less than a years worth of data. With more data, we can spot long term results.

- The model used were fairly basic. Exploring/testing new and refined models would improve results. Possibly tweaking some of the parameters and trying a prophet model may do some good.

### Tools & Requirements
Python | Pandas | NumPy | Seaborn | Matplotlib | Sci-Kit Learn | Statsmodels (Latest Versions)

## License
Standard

## Creators
Brandon Martinez
