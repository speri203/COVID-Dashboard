## COVID-19 Dashboard with Tableau

---

COVID-19 has been ravaging the world throughout the year 2020 and has been having lasting effects on the economy, individuals health, and peoples behavior. I wanted to do a basic analysis to see how the virus has been developing in the United States. Recently I have found out that Google Cloud Compute is offering $300 credit to use their services. I wanted to leverage Google BigQuery in-order to query their public datasets to practice my visualization skills.

The datasets utilized to do the analysis are called  ***covid19_usafacts,** **covid_19_weathersource_com,*** and ***nst-est2019-02*** which holds population data on the state level of granularity***.*** The query to generate the data is as follows:

```sql
SELECT 
(covid.county_fips_code, 
covid.county_name, 
covid.state, 
covid.date, 
covid.confirmed_cases, 
covid.deaths,
weather.min_temperature_air_2m_f, 
weather.max_temperature_air_2m_f, 
weather.avg_temperature_air_2m_f)
FROM 
`flash-ward-289518.covid_us.summary` AS covid,
`bigquery-public-data.covid19_weathersource_com.county_day_history` AS weather
WHERE 
(covid.county_fips_code = weather.county_fips_code AND covid.date = weather.date)
```

The data is at the state/county level so it is extensive and detailed. The data collected range from January 22nd, 2020 to September 13th, 2020. The data contains **nine** headers and they are as follows

- County Federal Information Processing Standard (FIPS) Codes
- County Name
- State (Data on Alaska is not presented)
- Date (January 22nd-September 13th)
- Confirmed Cases (Total Cases as presented on Google)
- Deaths
- Minimum Air Temperature
- Maximum Air Temperature
- Average Air Temperature

The total size of exported data is ~ 40 mb and contain 733488 rows.

### Insight

---

![screenshots/Confirmed_Cases_by_Date.png](screenshots/Confirmed_Cases_by_Date.png)

The map above illustrates the number of cases, deaths, and mortality rate (death/cases * 100) at the state level. The granularity can also be changed to county level, but this gives a better overall picture. The colors of the states are based on mortality rate. The highest mortality rates are concentrated to the north-east quadrant of the United States with Connecticut, New Jersey, and new York having the largest mortality rates. It should be noted that this insight is not based on population density in the respective states, but rather just deaths vs cases.

Population at the state level was then added to see how population plays a role in mortality rate.

![screenshots/Confirmed_Cases_by_Date_(Population).png](screenshots/Confirmed_Cases_by_Date_(Population).png)

The map is now colored based on population. The three states mentioned above have a large population, but by no-means are they the most populous states in the country. Texas and California have a greater population. It can then be deduced that it is not population alone is not a good indicator, but rather the density of population within the state. This helps answer the question as to why Texas and California don't have a higher mortality rate. 

The dataset was clustered (using K-Means) to identify which states are related in terms of cases and deaths. The "outliers" are CA, TX, FL, and NY which stand out compared to the other states. The reasoning for this can possibly be due to the population, but it is hard to deduce what other factors contribute with the data presented.

![screenshots/CasesDeaths_w_Clusters.png](screenshots/CasesDeaths_w_Clusters.png)

Lastly, I tried to do an analysis comparing the number of cases vs. temperature. I had the initial hypothesis inferring that as the temperature rises, so will the COVID cases. This might have been especially true since COVID started making a huge impact at the beginning of the year as we were reaching the middle/end of the winter period. As we changed transitioned into the Spring/Summer season, people are more inclined to go outside and enjoy the weather.

![screenshots/Temperature_vs_Cases.png](screenshots/Temperature_vs_Cases.png)

The illustration shows the distribution of temperature vs cases/death. This graph again states the strong correlation between the number of cases and deaths. The graph also proves the hypothesis stated initially, as the temperature increases more cases arise due to people being more "carefree" and discarding precautions. The peak happens right in the mid 70's which is considered as a perfect temperature by many. There is a drop off that should be noticed if the temperature rises too much. This can be concluded as people staying indoors to avoid the "scorching" temperatures. 

### Dashboard

---

The dashboard created from the workbooks has been uploaded to my Tableau Public profile. It can be accessed at : 

[Tableau Public](https://public.tableau.com/profile/sai.peri#!/vizhome/Covid_analysis_16002098975580/Dashboard1)

The workbooks are filtered by date, this is due to each row in the dataset stating the stats on that day and not a difference between previous days (comparing t to t-1). Changing the date feature will automatically adjust the data displayed on the dashboard. The map is also interactive if you are interested in looking at the stats for a particular state.
