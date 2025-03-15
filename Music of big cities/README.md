# Project Objective
To test hypotheses about the musical preferences of Yandex Music users from Moscow and St. Petersburg using real data.

# Hypotheses
1. User activity varies depending on the day of the week, and these patterns differ between Moscow and St. Petersburg.
2. Different music genres dominate in Moscow and St. Petersburg on Monday mornings. The same applies to Friday evenings — genre preferences depend on the city.
3. Moscow and St. Petersburg have different musical preferences: Moscow users listen to more pop music, while St. Petersburg users prefer Russian rap.

# Data
User behavior data is stored in the `yndx_music.csv` file. The quality of the data is unknown.
Column names:
* `userID` — user identifier
* `Track` — track name
* `artist` — artist name
* `genre` — genre name
* `City` — user's city
* `time` — listening start time
* `Day` — day of the week

# Research Stages
1. Data overview
2. Data preprocessing
3. Testing hypotheses
4. Research conclusion

# Research Conclusion
After testing the hypotheses, we established:
1. In both Moscow and St. Petersburg, user activity depends on the day of the week. The first hypothesis is fully confirmed.
2. The music genres that users listen to don't significantly differ throughout the week in both cities. The only exception is Monday, when:
   * Moscow users prefer "world" music
   * St. Petersburg users prefer jazz and classical music
   The second hypothesis is partially confirmed, as data has missing values that may affect the results.
3. Moscow and St. Petersburg have similar musical tastes. Genre differences are not pronounced among the majority of users. The third hypothesis is not supported by the data.
