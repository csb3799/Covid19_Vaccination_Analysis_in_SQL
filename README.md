# Covid-19 Pandemic: A Comprehensive Data Analysis with SQL & Tableau


- **Brief Introduction**

The covid-19 Pandemic was one of a kind that took many lives. I embarked on a journey to get insights on Covid's impact on the World. 



- **The Dataset:**

The analysis conducted on the COVID-19 pandemic using the World Health Organization (WHO) dataset focuses on critical metrics such as total cases, death counts, infection rates, vaccination rollouts, and the virus's impact across different continents.


**The following questions are what this analysis seeks to answer:**
- What is the Percentage of Population Vaccinated
- What is the number of Deaths By Continent
- Which is the Continents with the highest death count per population
- Which is Countries with Highest Infection Rate Compared to Population
- What is the Total cases & population percentage infected
- What is the Total Cases vs Total Deaths in Nigeria



**Let's select the data that we are going to be using:**

```SQL
-- Let's select the data that we are going to be using

SELECT location, date, total_cases, new_cases, total_deaths, population
FROM CovidDeaths
WHERE continent is not null and  total_deaths is not null
ORDER BY 1,2
```



## Total Cases vs Total Deaths in Nigeria
```SQL
-- Shows likelihood of dying if you contract covid in Nigeria

SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS DeathsPercentage
FROM CovidDeaths
WHERE location LIKE '%Nigeria' 
AND continent IS NOT NULL
ORDER BY 1,2,
```

## Total Cases vs. Total Deaths in Nigeria
- The initial reported Covid-19 death in Nigeria occurred on 23rd March 2020 when there were 40 confirmed cases. By 30th April 2021, Nigeria had a total of 165,110 confirmed cases and 2,063 deaths, giving a death rate of approximately 1.25%. This indicates that out of every 100 people infected with Covid-19 in Nigeria, roughly 1.25 succumbed to the virus.

- Findings: Nigeria had a relatively lower death rate compared to the global average, which hovered around 2%. This lower death rate could be attributed to several factors including a younger population or less severe strains of the virus during this period.


 ## Total Cases vs Population
```SQL
-- Shows what percentage of population infected with Covid

SELECT location, date, population, total_cases, 
	  (total_cases/population)*100 AS PercentPopulationInfected 
FROM CovidDeaths
ORDER BY 1,2
```


**Total Cases vs. Population: Percentage of Population Infected**

- Query Analysis: This query shows the percentage of the population that was infected with Covid-19 in different countries. Countries like Andorra reported an infection rate of 17% of their population. In Nigeria, however, by 30th April 2021, 0.08% of the population was infected (based on Nigeria's population of approximately 206 million).

- Findings: Nigeria's infection rate as a percentage of the total population was significantly lower compared to many other countries, particularly in Europe. This may indicate under-reporting of cases or limited testing capacity.



