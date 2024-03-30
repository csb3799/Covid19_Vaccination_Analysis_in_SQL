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



**Countries with Highest Infection Rate compared to Population**
```SQL
-- Countries with Highest Infection Rate compared to Population

SELECT location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS PercentPopulationInfected
FROM CovidDeaths
WHERE continent is not null 
GROUP BY location, population
ORDER BY PercentPopulationInfected DESC
```


## Countries with Highest Infection Rate Compared to Population

- The highest infection rates relative to population occurred in countries with smaller populations. Andorra had the highest infection rate, where more than 17% of the population contracted Covid-19. This demonstrates the disproportionate impact the virus had on smaller, densely populated countries.

- Findings: Smaller nations, particularly in Europe, were hit hard by the pandemic, as they faced significant infection rates, unlike larger countries such as Nigeria.


## Countries with Highest Death Count per Population
```SQL
-- Countries with Highest Death Count per Population

SELECT location, MAX(cast(total_deaths as int)) AS TotalDeathsCount
FROM CovidDeaths
WHERE continent is not null 
GROUP BY location
ORDER BY TotalDeathsCount DESC
```


## Countries with Highest Death Count per Population

- The United States recorded the highest total number of Covid-19 deaths, reflecting both its large population and the significant spread of the virus within its borders.

- Findings: Although the US had a relatively advanced healthcare system, the sheer volume of cases overwhelmed the system, leading to a high number of fatalities.


## Breaking it down by Continent
```SQL
-- Showing contintents with the highest death count per population

SELECT continent, MAX(cast(Total_deaths as int)) as TotalDeathCount
FROM CovidDeaths
WHERE continent IS NOT NULL 
GROUP BY continent
ORDER BY TotalDeathCount DESC
```


## Continents with the Highest Death Count per Population

- North America had the highest death count, with 576,232 deaths, followed by South America with 403,783 deaths. This shows the severe impact Covid-19 had on these continents, especially in densely populated areas.

- Findings: North and South America had the highest death tolls, largely due to delayed containment measures, overwhelmed healthcare systems, and perhaps differences in population health and preparedness.


## Global Covid-19 Numbers

```SQL
-- Percentage of deaths by total cases

SELECT SUM(CAST(new_cases AS INT)) AS Total_cases, 
SUM(CAST(new_deaths AS INT)) AS Total_Deaths, 
SUM(CAST(new_deaths AS INT))/ SUM(new_cases )*100 AS DeathsPercentage
FROM CovidDeaths
WHERE continent IS NOT NULL 
ORDER BY 1,2
```   


## Global Covid-19 Numbers: Death Percentage

- The global death rate was calculated as 2.11%, indicating that globally, about 2 in every 100 people who contracted Covid-19 succumbed to the virus. This reflects the overall impact of the pandemic.

- Findings: The global death rate provides a stark reminder of the severity of the pandemic. However, regional differences in healthcare capacity, early interventions, and government actions led to significant variations in death rates across continents.



## Total Population vs Vaccinations
```SQL
-- Shows Percentage of Population that has recieved at least one Covid Vaccine

WITH popvsvac (Continent, Location, Date, Population, New_vaccinations, RollingPeopleVaccinated) AS  
(
SELECT dea.continent, dea.location, dea.date,dea.population, vac.new_vaccinations, 
SUM(CONVERT(int,vac.new_vaccinations)) OVER (partition BY dea.location ORDER BY dea.location, dea.date) AS RollingPeopleVaccinated
FROM CovidDeaths dea
JOIN CovidVaccinations vac
ON dea.location = vac.location
and dea.date = vac.date
WHERE dea.continent IS NOT NULL
)
SELECT*, (RollingPeopleVaccinated/Population)*100 AS PercentRollingPeopleVaccinated
FROM popvsvac
```
