# COVID-19 Data Exploration with SQL Project Walkthough Plus Visualization with Tableau

### Dashboard Link : https://public.tableau.com/views/Covid-19Dashboard_17117468984870/Dashboard1?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link


# Introduction

Welcome to the COVID-19 data analysis & Visualization project report, where we’ll be examining the data up until March 2024. Throughout this exploration, we’ll cover data collection, cleaning, SQL-based exploration, and visualization in Tableau. The goal of this project is to provide an informative look at the pandemic’s progression through data analysis. Whether you’re interested in the numbers or seeking insights into the COVID-19 landscape, this project aims to offer valuable information. Also, I’m currently searching for a Data Analyst position, if you’re interested you can find my contacts here :

#### GITHUB: https://github.com/EsmerlynG

#### LINKEDIN: www.linkedin.com/in/esmerlyn-garabito-06776a236

#### EMAIL: Pg08859p@pace.edu

The data analyzed in this project was sourced from this website. (https://ourworldindata.org/covid-deaths)

## 1.Preparing the Dataset:


The first step in any data analysis is to acquire the data. The data on the coronavirus pandemic is updated daily and free to reuse and is credible and reliable, it is even used for research, however be sure to cite it when you use it.



![Snap_Count](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/7b34792e-2097-4f75-b278-9d654fbc72ab)


As you can see here there is that circle that we can push back in time, so I did that to have the data as old as possible up to today, and you might have it differently, but push back that circle then click on download and choose csv.



![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/21a3de3c-47c6-40cd-a868-9bd0e96bf4fa)


## 1.1 Data exploration and preparation


Now that you have your dataset, it is important to explore it and understand it before doing anything. I think this is the most crucial phase, because any misunderstanding might lead you to the wrong path, you can lose hours afterwards and repeat all the process, so we take our time and understand the data well.


![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/06ef9866-6ed1-4258-90d4-6a69a5584096)

As you might have expected, the data has more than 10K rows, and many columns, which in my opinion is amazing! For the columns we have for example:

- [iso_code] which is the international standard for country codes. 
 - The [continent] on which the data is concerned as well as the country which here is named [location].

- The date on which the data was recorded as well as the [new_cases] in that day, the [total_cases], [new_deaths], [total_deaths], [new_vaccinations], [total_vaccinations], data about the [new_tests] and [people_fully_vaccinated]
- Demographic data about each country such as the population, [population_density] (number of people divided by land area, measured in square kilometers) , [median_age], [aged_65_older] which is the share of the population that is 65 years and older, [aged_70_older] also the share of the population that is 70 years and older, [extreme_poverty] (share of the population living in extreme poverty), human_development_index which is a composite index of life expectancy, education, and per capita income indicators.


1.First to prepare our data, we will go all the way to the Population collumn and select it, next we press Ctrl 'X' (Command 'X' on Mac depending on settings), Now we will go back to collumn 'E' select it then right click and click on 'Insert Cut Cells'.  We will only work on selective data so delete everything after column Z and save the file as a Excel Workbooks (. xlsx) named CovidDeaths.

2.We restore the deleted collumns and now select everything from collumn Z to collumn E and delete it. Now we save the file as a Excel Workbooks (. xlsx) named CovidVaccinations.


After files are cleaned and saved. Head to Microsoft SQL Server and create a database called Covid19.


## 1.2 Create a database based on the data
Now that we have the files that we want, we’re going to Microsoft SQL Server and we will create a new Database. Go to the menu and right click on [Databases], then select [New Database], type in the name of the database, for me I will name it Covid19 and then click on OK and it will create the database for you.

![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/fd68b3f9-fe4a-4c49-88fb-596da0e7ced2)

If you check the tables, it will be empty, and this is where we’re going to put our excel files. To import our we will right click on our Database, and select [Tasks] after that we will select [Import Data]

![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/0bd801cf-17d8-482c-a558-847ffd43246a)

Click on next when the window open then click on the arrow and choose the data source as Microsoft Excel then choose your file location

![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/1e6cde72-c101-4c55-ac54-7c67c5bc449b)

And now we have to say which database will hold the data, choose the destination Microsoft OLE DB Provider for SQL Server and your Database Covid19 also be sure that you are in the right Server.

![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/032cfb9a-c2d3-4d5d-9c48-c943547758c7)

Click on next and name your table appropriately in Destination.

![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/cdf673d8-30c2-4927-9efa-ff4dcc515686)

Then you will run immediately so click on finish two times.

![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/368b77dd-66f7-45d7-8655-4d7c0c844a35)

Repeat the same process for each excel file, I will name my tables as follows: CovidDeaths, CovidVaccinations.

## 2. SQL queries

Now that our database is filled with data, let’s have some fun and practice our sql, we will go from basic to more complex queries, I hope you will do all of them and not ignore any.

## 2.1 Check if the data is loaded correctly for each table

This one is pretty simple, all you have to do is select everything from each table and check it manually.
        
        
        Select *
        From Covid19..CovidDeaths
       

        Select *
        From Covid19..CovidVaccinations
       
## 2.2 Select Data that we are going to be starting with

        Select Location, date, total_cases, new_cases, total_deaths, population
        From Covid19..CovidDeaths
        Where continent is not null 
        order by 1,2

## 2.3 Total Cases vs Total Deaths

Shows the likelihood of dying if you contract covid in your country. Here you can pick any country to your liking, I personaly choose the U.S

        Select Location, date, total_cases,total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
        From Covid19..CovidDeaths
        Where location like '%states%'
        and continent is not null 
        order by 1,2

## 3. Total Cases vs Population

Shows what percentage of the population is infected with Covid.

        Select Location, date, Population, total_cases,  (total_cases/population)*100 as PercentPopulationInfected
        From Covid19..CovidDeaths
        Where location like '%states%'
        order by 1,2

## 4. Countries with Highest Infection Rate compared to Population

        Select Location, Population, MAX(total_cases) as HighestInfectionCount,  Max((total_casespopulation))*100 as PercentPopulationInfected
        From Covid19..CovidDeaths
        Group by Location, Population
        order by PercentPopulationInfected desc

## 5. Countries with Highest Death Count per Population

        Select Location, MAX(cast(Total_deaths as int)) as TotalDeathCount
        From Covid19..CovidDeaths
        Where continent is not null 
        Group by Location
        order by TotalDeathCount desc

## 6. Showing contintents with the highest death count per population

The goal here is to begin breaking the data down by continent.

        Select continent, MAX(cast(Total_deaths as int)) as TotalDeathCount
        From Covid19..CovidDeaths
        Where continent is not null 
        Group by continent
        order by TotalDeathCount desc

## 7. GLOBAL NUMBERS

        Select SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM(New_Cases)*100 as DeathPercentage
        From Covid19..CovidDeaths
        where continent is not null 
        order by 1,2

## 8.Total Population vs Vaccinations

We cant forget that we still haven't used one of our tables, [TotalVaccinations], so lets put it to use by joining it together with our CovidDeaths table. We will join them on 'location' because it is much more specific than 'continent' and we wil also join them on date. Heres a tip, give your tables an alies so that you don't have to type out the entire table name every time. 

Here we will also be using partition and window functions, the reason for this is that we want all all the new_vaccination numbers to add up consecutivly so that the data presents itself much more organised. 


![Snap_1](https://github.com/EsmerlynG/PortfolioProjects/assets/164951580/4a0f823c-8eaf-4c3f-a6a2-33a9543103a4)



        Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
        , SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
        From Covid19..CovidDeaths dea
        Join Covid19..CovidVaccinations vac
	           On dea.location = vac.location
	           and dea.date = vac.date
        where dea.continent is not null 
        order by 2,3

## 9. Using CTE to perform Calculation on Partition By in previous query

        With PopvsVac (Continent, Location, Date, Population, New_Vaccinations, RollingPeopleVaccinated) 
        as
        (
        Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
        , SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.  location, dea.Date) as RollingPeopleVaccinated
        From Covid19..CovidDeaths dea
        Join Covid19..CovidVaccinations vac
	                On dea.location = vac.location
	                and dea.date = vac.date
        where dea.continent is not null 
        )
        Select *, (RollingPeopleVaccinated/Population)*100
        From PopvsVac


## 10. Using Temp Table to perform Calculation on Partition By in previous query

        DROP Table if exists #PercentPopulationVaccinated
        Create Table #PercentPopulationVaccinated
        (
        Continent nvarchar(255),
        Location nvarchar(255),
        Date datetime,
        Population numeric,
        New_vaccinations numeric,
                RollingPeopleVaccinated numeric
        )

        Insert into #PercentPopulationVaccinated
        Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
        , SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
        From Covid19..CovidDeaths dea
        Join Covid19..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date

        Select *, (RollingPeopleVaccinated/Population)*100
        From #PercentPopulationVaccinated

## 10. Creating Views to store data for visualizations in Tableau 

        Create View PercentPopulationVaccinated as
        Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
        , SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.  location, dea.Date) as RollingPeopleVaccinated
        From Covid19..CovidDeaths dea
        Join Covid19..CovidVaccinations vac
	             On dea.location = vac.location
	             and dea.date = vac.date
        where dea.continent is not null 


        Create View TotalCases_vs_TotalDeaths as
        Select Location, date, total_cases,total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
        From Covid19..CovidDeaths
        Where location like '%states%'
        and continent is not null 


        Create View TotalCases_vs_Population as
        Select Location, date, Population, total_cases,  (total_cases/population)*100 as PercentPopulationInfected
        From Covid19..CovidDeaths
        Where location like '%states%'


        Create View HighestInfectionRateComparedToPop as
        Select Location, Population, MAX(total_cases) as HighestInfectionCount,  Max((total_cases/      population))*100 as PercentPopulationInfected
        From Covid19..CovidDeaths
        Group by Location, Population


        Create View HighestDeathCountPerPopulation as
        Select Location, MAX(cast(Total_deaths as int)) as TotalDeathCount
        From Covid19..CovidDeaths
        Where continent is not null 
        Group by Location 


