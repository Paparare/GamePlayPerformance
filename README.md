# Predict Student Performance from Game Play
Machine learning competition to predict whether students' answers are correct based on gameplay behavior data. 
We need to analyze students' in-game behavior data, such as click events, hover time, game interface etc., and build models to predict whether students' answers to specific questions are correct.

## Data Processing
we used the with_columns function in the polars library to add new columns to the dataset. These new columns mainly include:

**elapsed_time_diff**: Calculated the diff value of the 'elapsed_time' column (time from session start to event being recorded), and grouped the results by each session and level.

Absolute value of diff for **screen_coor_x** and **screen_coor_y** columns: These two columns represent click coordinates referring to the player's screen. After processing, they can reflect players' behavior patterns.

Absolute value of diff for **room_coor_x** and **room_coor_y** columns: These two columns represent click coordinates referring to the room in the game. By analyzing players' in-game behavior patterns, it can provide clues to predict whether players' answers are correct.

Filled missing values in the **fqid** and **text_fqid** columns to prevent issues in subsequent processing.

Finally, we split the data into three subsets according to the value of the **level_group** column (which group of levels and questions this row belongs to), corresponding to the three question checkpoints in the game (Level 4, Level 12 and Level 22).

## Feature Engineering
Feature engineering is the core part of this solution. We designed different features for each level group ('0-4', '5-12', '13-22').

- Basic statistical features: We calculated the number of events in each session, number of unique values for each categorical feature, and standard deviation, mean, min, max and sum for each numerical feature. These basic statistical features provide basic information about the session.

- Time difference statistical features: We further calculated statistical features of the time difference of events under each unique value of each categorical feature, including count, standard deviation, mean, max, min and sum. These features can reflect the time difference students spend on different events, thus reflecting students' learning patterns.

- Specific business logic features: We designed some specific features for particular game elements and events. For example, we calculated the total time and activity counts of users reaching bingo status in the logbook, reader and journals elements in the game. These features can reflect students' specific behaviors and interaction patterns in the game.

The design and calculation of the above features are done in the feature_engineer function.
