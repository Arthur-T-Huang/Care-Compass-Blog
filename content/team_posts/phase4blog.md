---
title: "Project - Phase IV"
date: 2025-06-13
draft: false
description: "Proof of Concept - Starting Deployment"
slug: "phase4post"
tags: ["project", "Setup"]
authors:
  - "katherineahn"
  - "anoushkaabroal"
  - "arthurhuang"
  - "shivenajwaliya"
showAuthorsBadges: false
---

# Project Reflection

After five weeks, we have put together our platform Care Compass. We have spent our time improving and implementing our machine learning models, as well as creating our tables and updating our API Layer so that the backend integrates seamlessly with the front end. Our platform provides users with future predictions of healthcare values, information about similar healthcare systems, and country information in our country profile. Our UI has been updated throughout this process, and is now user-friendly with streamlit resources such as buttons, drag-and-drop, and sliders. Below, we articulate a bit more in-depth about our work processes, and we include screenshots of some of the front end pages.

# REST API Matrix
We cleaned up the backend by auditing and reorganizing our REST API routes to match the final structure of the application. We removed several unused country routes that were never called by the frontend, and deleted outdated comparison routes, since their functionality was now handled through our ML-based endpoints. We also moved the recommendation route from its own recommendations blueprint into the country blueprint to simplify the structure—there was no need for an entire folder just for one route. After making these changes, we updated the REST API matrix to reflect the current set of endpoints. [Phase 3 Blog Post](https://arthur-t-huang.github.io/Care-Compass-Blog/team_posts/phase3post/#rest-api) updated accordingly. 


# ML Models 

## Autoregressive Model: 

The autoregressive model was a new addition during this phase which was aimed at replacing the linear regression model from phase 3, which was a poor fit for the World Health Organization data we were trying to predict, which is time series data. The autoregressive model is accessible by two of the user person types. The first being the student user, who is able to interact with the autoregressive model on their country comparator page which allows them to see a graph of the historic and predicted values of up to three countries for one of the given W.H.O datasets (live births, general practitioners per 10,000 population, and total expenditure). Specifically the user selects up to three different countries at the top of the page. The user then has the option to see a table for The second is the policy maker user who interacts with the model on the features over time page which allows them to input a country, date, and returns a graph of all the countries values (historic and predicted) up to that point for a given dataset. In order to test the accuracy and reliability of our model, we cross validated the model and calculated the mean squared error, r^2, and residuals of the model for each of our datasets, which can be seen in the images below. For the general practitioners model, the r^2 of 0.88 demonstrates that the regression model fits the variation of the data well. This is further supported by the mean squared error of 0.04 which shows very little difference between the model predicted values and the actual values on average. In terms of residuals, the residuals over order graph seen below does not seem to demonstrate any significant patterns/trends within the data, which is an indication that the model does not possess autocorrelation. This is the same conclusion with the model for live births, that has a r^2 of 0.79, a MSE of 0.007, and a residuals over order graph without pattern. However, the total expenditure model possessed an r^2 of 0.14 and had an mse of 314, which indicates that the autoregressive model is not a great representation of total expenditure. Based on these analyses, it can be concluded that the autoregressive model can be considered a “good” model for predicting general practitioners and live births over time, but not for total expenditure.

### General Practitioners Analysis:
![genprac_value](genprac_values.png)
![genprac_graph](genprac_graph.png)

###Live Births Analysis:
![livebirths_value](livebirths_values.png)
![livebirths_graph](livebirths_graph.png)

###Total Expenditure Analysis:
![expenditure_value](expenditure_values.png)
![expenditure_graph](expenditure_graph.png)

## Cosine Similarity Model:

The cosine similarity model that we worked on during phase 3 is now fully implemented. The user is now able to rank their priority of the healthcare factors: Prevention, Risk Environment, Health Systems, International Norms Compliance, Rapid Response, and Detection & Reporting. The user can drag-and-drop to rank these factors, and then use sliders to fine tune their ranking weights. Each factor also has a little information tooltip, to explain what each factor really means (a bit of information on the subcategories in the ghs_index). A route is used to get the cosine similarity information after the calculations are put into a dataframe. The visualizations include a bar chart that has the top five countries and their similarity score, as well as a gradient map that is colored based on how similar the country’s healthcare system is to the given country. This model is also used in the Country Profile page, where the sidebar has the top five similar countries based on healthcare factors.  


# Data Models

Our data model has continued to get altered and tweaked alongside our code to power such an interactive platform that is Care Compass. We have created a relational database so as to present the ecosystem of our project, displaying how key components interrelate. Below you can see the relational database in question:

![image](relationalDatabase.png)

The core entities of our application are the Users, along with UserRoles, and Countries. The Users/UserRoles tables store information regarding the id, name, user archetype, email, and associated country. The Country table is a quite central node, linking all health metrics and additional country information in order to ensure everything is tied to the correct country. Further looking at the tables referencing the Country table, this is exclusively done using a country’s code; some tables you can see are the CountryArticles and CountryInformation tables, which house generated qualitative data, as well as six feature variables our app tracks over time (infant mortality, life expectancy, etc.), along with GHS index (country healthcare score). As you can see the six feature variables have the same format, storing country, year, and value so a future edit to simplify our data model could be to consolidate that into one feature table, with a FK id that indicates which feature variable it is. As you can see, the overall score differs from the feature variables as it aggregates six other factors like prevention, rapid response, etc. Moving back down, our last table is the Favorites table; which has a relationship with CountryArticles and Users. How the Favorites table works is when a user clicks to ‘favorite’ an article under a CountryProfile, this articleID is added to the table which can then be retrieved later on; further, an article can be ‘unfavorited’. 

Along the side, you may see there are a few tables used for storing information regarding our ML models. So, for our regression model we store our hyperparameters per country and the training/testing datasets; and for the cosine similarity we store the weights of each health-related factor of the GHS Index.

# Screenshots of Front-End
## Home Page
The Care Compass home page welcomes users with a clean, intuitive interface designed to guide different user types to their personalized healthcare insights.

![Home Page](home_full.png)

### Choose Your Role
Three distinct pathways cater to different user needs:
- **Resident**: Access personalized healthcare recommendations and country profiles
- **Student**: Explore country comparisons and country profiles
- **Policymaker**: Analyze features over time and target certain healthcare scores

A **Why Choose Care Compass?** section at the bottom

The streamlined login process allows users to quickly select their role and access the tools most relevant to their needs, making complex healthcare data accessible to everyone.

## Relocating Resident Home Page
The personalized Relocating resident dashboard provides a centralized hub for all healthcare relocation tools and resources.

![Resident Home](resident_home_full.png)

### Core Features
- **Customize Your Move**: Plan your healthcare relocation journey with personalized recommendations
- **Explore Countries**: Discover detailed healthcare profiles and compare medical systems worldwide
- **Saved Resources**: Access your collection of favorited healthcare articles and guides

**Platform Stats** and **Quick Tips** section at the bottom 


## Customize Your Move
Our Customize Your Move tool helps you discover countries with healthcare systems that match your personal priorities through an intuitive 3-step process.
![Full Interface](custom_move_full.png)

### Learn More
![Learn Section](custom_move_learn.png)
Understand each factor and the math behind the calculation

### How It Works
![Selection Interface](custom_move_selection.png)
**Step 1:** Select your current country  
**Step 2:** Drag and rank 6 healthcare factors by importance (Prevention, Health System, Rapid Response, etc.)     
**Step 3:** Fine-tune each factor with sliders (0-10 scale)    
Click "Find My Matches" to see your personalized results!   

### View Your Results
**Bar Chart**
![Bar Chart](custom_move_bar.png)
See your top 5 matching countries with similarity scores

**World Map**
![World Map](custom_move_map.png)
Visualize global matches with color-coded similarity scores

## Country Healthcare Profiles
Explore comprehensive healthcare information and insights for countries worldwide.

![Country Profile](country_profile_full.png)

### Key Features
- **Country Selection**: Search or select from dropdown to explore any country's healthcare system
- **General Information**: Overview of the country's geography, culture, and basic healthcare infrastructure
- **Healthcare System Overview**: Detailed analysis of healthcare services, insurance systems, and regulatory oversight
- **Similar Healthcare Systems**: Compare with countries that have comparable healthcare characteristics (shown with similarity scores)

### Latest Healthcare Articles
Stay informed with curated articles specific to each country

### Learn More
![Learn Section](country_profile_learn.png)
Understand the content of country profiles and the math behind the similar healthcare systems

The page provides a complete healthcare profile with real-time articles and similarity comparisons, making it easy to understand any country's healthcare landscape at a glance.

## Favorite Articles

Build a personalized library of healthcare insights from around the world.

![Healthcare Article Collection](favorite_articles_full.png)

### Key Features
- **Personal Library**: Save articles from different countries in one place
- **Quick Start Guide**: Navigate → Browse → Save → Access your favorites
- **Article Management**: View saved articles with source info and easy removal

### Learn More
![Learn Section](favorite_articles_learn.png)

**How it works**: Navigate to country profiles, click the star icon to save articles, and manage your collection with the remove button.

**Why build a collection**: Track global healthcare trends, compare systems internationally, and quickly access relevant research for your work.

Start exploring country pages to curate your personalized healthcare knowledge base.


## Student Home Page

Your personalized dashboard for researching and comparing global healthcare systems. 

![Student Home](student_home_full.png)

### Core Features
- **Compare Countries**: Evaluate countries side-by-side on several health metrics   
- **Country Profiles**: Discover detailed healthcare profiles and compare medical systems worldwide   
- **Saved Articles**: Access your collection of favorited healthcare articles and guides   

**Platform Stats** and **Student Success Tips** sections at the bottom

## Country Healthcare Comparator

Compare healthcare systems across multiple countries with data-driven insights and trend analysis.

![Country Comparator](country_comparator_full.png)

### Key Features
- **Quick Start Guide**: 4-step process to compare countries and explore projected trends
- **Country Selection**: Compare up to 3 countries simultaneously 
- **Healthcare Trends**: Track and forecast key metrics through 2035
- **Visual Analytics**: Interactive charts showing historical and projected data

### Learn More
![Learn Section](country_comparator_learn.png)

**Key Metrics**: Life expectancy, infant mortality, healthcare workforce, health expenditure, financial protection, and birth rate indicators

**Forecasting Technology**: Autoregressive models analyze WHO data to project trends through 2035

### How It Works
![Selection Interface](country_comparator_selection.png)

**Step 1:** Select 2-3 countries to compare  
View instant comparison table showing all 6 healthcare metrics across selected countries

**Step 2:** Choose one metric from the results table and click "Generate Chart" to visualize trends

### View Trends
![Trends Chart](country_articles_graph.png)

Track your selected metric over time with historical data and projections through 2035, helping you understand healthcare system trajectories.


## Policymaker Home Page

Drive evidence-based healthcare policy decisions with comprehensive analytics and tracking tools.

![Policymaker Home](policymaker_home_full.png)

### Core Features
- **Track Progress Over Time**: Monitor healthcare metrics evolution, analyze trends, and identify intervention areas
- **Set & Monitor Targets**: Establish performance targets and track progress against benchmarks for strategic planning

**Platform Stats** and **Policy Insights & Tips** sections at the bottom

## Features Over Time

Visualize historical data and future projections for key healthcare indicators to inform policy decisions.

![Healthcare Trends](features_over_time_full.png)

### Key Features
- **Quick Start Guide**: 4-step process to analyze trends and projections
- **Country & Year Selection**: Choose any country and set target year for projections (up to 2100)
- **Indicator Analysis**: Track live births, general practitioners, or health expenditure trends
- **Data Visualization**: Historical data with ML-powered predictions and downloadable datasets

### Learn More
![Learn Section](features_over_time_learn.png)

**Available Indicators**: Live births per 1,000 population, general practitioners per 10,000 people, and total health expenditure per capita

**Forecasting Technology**: Autoregressive models predict future values using historical patterns from WHO data

### View Analysis
![Trends Graph](features_over_time_graph.png)

Interactive chart displays historical trends (blue line) and predicted values (red dashed line) with key statistics including data points, historical/predicted counts, and calculation time. Download data as CSV for further analysis. 

## Healthcare Target Calculator

Set ambitious healthcare goals and track when they'll be achieved based on current trends.

![Healthcare Target Calculator](target_scores_full.png)

### Key Features
- **Quick Start Guide**: 4-step process to analyze country data and set target values
- **Current Status Review**: View 6 key healthcare metrics for your selected country
- **Projection Analysis**: Calculate when targets will be reached based on historical trends

### Learn More
![Learn Section](target_scores_learn.png)

**Projection Technology**: Linear regression analysis of historical trends to estimate achievement timelines

**Available Indicators**: Life expectancy, infant mortality, live births, general practitioners, health expenditure, and impoverished households

### How It Works
![Selection Interface](target_scores_selection.png)

**Step 1:** Select a country to analyze  
**Step 2:** Review current healthcare status across 6 key metrics  
**Step 3:** Set target values and click "Calculate Projections" to see when goals will be achieved

### View Results
![Results Summary](target_scores_chart.png)

Summary table shows current values, targets, trends, and projected achievement dates with status indicators.

![Detailed Analysis](target_scores_graph.png)

Interactive graphs display trend projections with confidence scores, helping visualize the path to achieving healthcare targets.

## Get to Know Care Compass

Learn about our mission to simplify global healthcare data through machine learning and analytics.

![About Page](about_full.png)


## Meet Our Team

Connect with the passionate minds behind Care Compass and explore our research insights.

![Contact Info](contact_info_full.png)

### Key Features
- **Team Introduction**: Meet the four team members driving Care Compass forward
- **Research & Insights**: Access data-driven research, methodology deep-dives, and policy trend analysis
- **Direct Contact**: Email links for each team member to collaborate on healthcare research

### Resources
Visit the Care Compass Blog for country spotlights, methodology insights, and healthcare policy analysis to make complex health data accessible for everyone.

**Visit our github repository to view our project in its full depth:**
[Github Repository for Project](https://github.com/RemainingDelta/Care-Compass) 

