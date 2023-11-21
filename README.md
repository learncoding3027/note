"The footfall for the required day is calculated using footfall of the previous two weeks of that specific day." 

1.	Data Processing:  
•	Footfall undergoes various transformations. 
•	Time-related calculations are performed.
•	Cumulative sum of Activity/Counter is done for each group in ‘EID’ and ‘Gender’. 
•	Additional mapping is done for ‘SG’, ‘TC’ and ‘City.’
2.	Randomization and filtering:
•	A Random number is generated and used for sorting the data.
•	Filtering of data can be done by count of lines and by rand 0.67 or rand 0.5 or tbh_even (tbh_even is count  by of ‘EID’, ‘2hdp’,’Activity/counter ’ ) respectively. 

"The footfall for the required day is calculated using footfall of the previous two weeks of that specific day." 
1.	First we read the footfall csv which contains details like EID (Restaurant id), Date, Time, Activity/Counter, Gender, Key ID, Lat, Long and username.
2.	Time- related calculations are preformed where we convert time to seconds, hourly day part, two hourly date part also based on the columns EID and gender we group the number of male-female in that Restaurant at that time.
3.	Additional mapping is done for ‘SG’, ‘TC’ and ‘City.’
4.	We concatenate the previous two weeks footfall of that specific days.
6.	Filtering of data can be done by count of lines and by rand 0.67 or rand 0.5 or tbh_even (tbh_even is count  by of ‘EID’, ‘2hdp’,’Activity/counter ’ ) respectively. 


