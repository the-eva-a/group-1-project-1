# Group 1 Project 1: Health Measures and Early Death Analysis

## Project Overview

This project explores the relationships between various health measures and early deaths (years of potential life lost). Using public health datasets from U.S. counties, we aim to identify key indicators that contribute to premature mortality and develop insights to inform health policy and intervention strategies.

## Table of Contents
1. [Installation](#installation)
2. [Usage](#usage)
3. [Data](#data)
4. [Methodology](#methodology)
5. [Results](#results)
6. [Contributors](#contributors)
7. [License](#license)

## Installation

1. Clone the repository and install the dependencies:

 ```bash
   git clone https://github.com/the-eva-a/group-1-project-1.git
   cd group-1-project-1
   pip install -r requirements.txt
```
2. **Optional: Census API Key Setup** (Only for Data Retrieval)
If you would like to run the data cleaning process or retrieve new data from the Census API, you will need to set up an API key. Obtain one from the [U.S. Census Bureau API](https://api.census.gov/data/key_signup.html).

After obtaining your API key, create a `.env` file in the root of the project directory and add the following line:
``` makefile
CENSUS_API_KEY=your_api_key_here
```
The project uses the `environs` package to load environment variables, including your API key.
## Usage
1. **Running the Analysis:**
If you want to use the preprocessed data, you can directly run the analysis notebooks located in `notebooks/` directory without the need for the Census API key.
2. **Running `Preliminary Data Cleaning` or Retrieving New Data**:
If you want to retrieve fresh data from the Census API or re-run the data cleaning process, ensure that your .env file is properly set up with your Census API key. You can then run the preliminarydatacleaning.ipynb notebook to pull and clean the data using the Census API.
Example usage in the notebook:
``` python
from environs import Env
import requests

env = Env()
env.read_env()

# Use the API key
api_key = env("CENSUS_API_KEY")
response = requests.get(f"https://api.census.gov/data/2019/acs/acs5?key={api_key}&get=NAME,B01003_001E&for=state:*")
```
### Data
This project uses data from the following sources:
1. County Health Data: Public health statistics for U.S. counties were sourced from [County Health Rankings & Roadmaps](https://www.countyhealthrankings.org/). This dataset includes key health measures, such as premature death rates, quality of life, and health behaviors, which are used to explore health outcomes across counties.
2. **Census API**: Additional data was retrieved from the U.S. Census Bureau's [API](https://www.census.gov/data/developers/data-sets.html). The API was used to pull specific demographic and economic variables to complement the health data.
3. Geospatial Data: Shapefiles for US state and county boundaries was used for experimental mapping. This data was also from the [United States Census Bureau] (https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html)

#### Data Files:
In the `data` folder there are two pre-cleaned data sets. 
- `county_data.csv`: Contains health measures for each U.S. county
- `state_data.csv`: Contains health measures for each U.S. state

## Methodology

### Data Preprocessing
The data cleaning process involved several steps to ensure we had the most relevant and usable data for analysis:
- **Header Transformation**: The original dataset had two rows of headers. We transformed these into a single row for easier manipulation and analysis.
- **Filtering Relevant Data**: We retained only the rows that had at least 90% of their data available, focusing on columns that seemed most relevant to the study.
- **Missing Values**: While we did not perform specific missing value imputation or outlier detection, the analysis methods used (such as non-normality tests) are robust to incomplete data.
- **Libraries Used**: Most of the preprocessing was handled using the `pandas` library for efficient data manipulation.

### Analysis
We performed statistical analyses to explore the relationships between various health measures and early death rates:
- **Correlation Analysis**: We tested the relationships between factors (e.g., healthcare access, health behaviors) and early death rates. Both linear regressions and non-parametric correlation tests were conducted to capture relationships across different types of data.
  - We used the `scipy.stats` package for performing these tests, including:
    - **Linear Regressions**: To examine how well one variable could predict early death.
    - **Normality Tests**: To determine whether data followed a normal distribution, which guided our choice of correlation tests.
    - **Non-Normal Correlation Tests**: When normality assumptions were violated, we used non-parametric tests like Spearman’s rank correlation.
- **Scale of Analysis**: After initial explorations at the county level, we determined that working at the state level provided more coherent insights, given the scope and limitations of the dataset.

### Visualizations
Visualizations played a critical role in understanding and presenting our findings:
- **Geographic Maps**: Choropleth maps were generated to visualize the geographic distribution of health measures and premature death rates across the 50 states. This helped identify spatial patterns in the data.
- **Pair Plotting**: Pair plots were used to visualize relationships between multiple variables simultaneously, giving a clear view of correlations and interactions.
- **Linear Regressions**: Scatter plots with linear regression lines were created to show the strength and direction of relationships between health indicators and early death rates.
  
The visualizations were created using libraries such as `plotly.express`, `matplotlib.pyplot`, and `seaborn`.

### Modeling
- **Linear Regression**: We opted for linear regression models to explore how individual health factors impact early death rates. Linear regression was chosen due to its interpretability and its ability to quantify relationships between a predictor (health measures) and the response (early deaths). This helped us determine which factors might have the strongest influence on early death.
- **Choropleth Maps**: Geospatial analysis was done using choropleth maps to visualize differences in health outcomes across states, helping to identify regions of high concern.
## Results

### Key Findings
Our analysis identified several key factors that influence early mortality across the U.S.:
- **Strong Correlations**:
  - **Median Household Income**: States with higher median household incomes tend to have significantly lower early mortality rates. This suggests that wealthier populations may have better access to healthcare and healthier lifestyles.
  - **Adult Obesity Rate**: High obesity rates are strongly correlated with increased early mortality. States with higher obesity rates also have higher rates of lifestyle-related diseases, which contribute to early deaths.
  - **Food Environment Index**: States with better food environments (e.g., access to healthy, affordable food) generally see lower early mortality rates, highlighting the importance of nutrition in public health outcomes.

- **Moderate Correlations**:
  - **Exercise Access** and **Primary Care Physician Rate**: These factors showed a moderate correlation with early mortality, suggesting that while access to healthcare and exercise are important, they may not be as impactful as economic and dietary factors.
  - **Poor Physical Health Days**: States where residents report more poor physical health days also tend to have higher premature death rates.

- **Surprisingly Weak Correlations**:
  - **Excess Drinking** and **Housing Inadequacy**: Despite being associated with poor health outcomes, these factors did not show as strong a correlation with early mortality as initially expected.

### Geographic Trends
- **States with Low Early Mortality Rates**:
  - **Massachusetts**: Near-universal healthcare coverage has contributed to significantly lower early mortality rates.
  - **Minnesota**: Consistently low rates of smoking and high blood pressure contribute to low mortality rates.
  - **Hawaii**: A low obesity rate and a strong cultural emphasis on community and health have helped Hawaii achieve one of the lowest early mortality rates.
  - **Utah**: Low smoking rates and a strong culture of exercise contribute to better overall health outcomes.

- **States with High Early Mortality Rates**:
  - **Mississippi**: The highest diabetes rate in the country leads to many serious health complications, contributing to high early mortality.
  - **West Virginia**: High obesity and smoking rates are major contributors to the state's high early mortality rate.
  - **New Mexico**: Injuries, including unintentional poisonings and firearm injuries, are leading causes of early mortality in the state.
  - **Louisiana**: A high poverty rate has significant implications for access to healthcare, leading to worse health outcomes.

### Visual Results
- **Choropleth Maps**: Our geographic visualizations revealed strong regional patterns in early mortality rates. The Southern states generally show higher rates of early mortality, whereas states in the Northeast and Western U.S. fare much better in terms of health outcomes.
- **Linear Regression and Pair Plots**: These plots visually demonstrated the strength of relationships between various health indicators (e.g., obesity, income) and early mortality. States with lower median household incomes and higher obesity rates consistently showed higher early death rates.

### Conclusion
Our analysis highlights that economic and lifestyle factors, particularly **median household income**, **obesity rates**, and the **food environment**, are some of the most impactful determinants of early mortality in the U.S. Addressing these factors through targeted public health initiatives could significantly reduce premature death rates across the country.

### Limitations
While our findings are valuable, we acknowledge some limitations:
- **Chronic Disease**: Premature death as a measure may not fully capture the burden of chronic diseases, especially if the age limit is set too low.
- **Lag in Intervention Effects**: Statewide interventions may take years to reflect in data, meaning that current measures may not show the impact of recent public health efforts.
- **Rare Events**: Premature deaths, particularly in smaller populations, are relatively rare, which can sometimes skew the data.

## Contributors
- Eva Anderson ([github](https://github.com/the-eva-a))
- Kelsie Bumadianne ([github](https://github.com/kbumadianne))
- Heather Eby ([github](https://github.com/ebyheather))
- Abigail Husain ([github](https://github.com/ahusain718))
