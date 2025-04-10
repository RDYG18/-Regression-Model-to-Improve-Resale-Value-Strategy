# Improve Resale Value Strategy

![image](https://github.com/user-attachments/assets/68bfad6f-8fc9-4cfa-a125-6901881bb1e8)

## Company Context

<div align="justify">

In this project, I collaborated with ReCell, a startup aiming to capture value in the rapidly growing market of refurbished and used mobile devices. Recognizing a shift in consumer behavior and a clear market opportunity, the company is leveraging data science to build a dynamic pricing strategy that accurately predicts resale value, identifies the key features influencing pricing, and ultimately maximizes revenue while remaining competitive and promoting sustainable consumption.

ReCell’s strategy is grounded in larger industry trends. The refurbished device market has experienced substantial growth in recent years, driven by increasing demand for affordable alternatives, rising environmental awareness, and strong support from third-party platforms like Amazon and Verizon. According to IDC, the market was projected to reach $52.7 billion by 2023, with a CAGR of 13.6% between 2018 and 2023. The COVID-19 pandemic further accelerated this momentum, as consumers turned toward more practical and sustainable tech solutions.

</div>

## Company Objective

<div align="justify">

ReCell needed a predictive model to estimate resale prices based on device attributes, helping them identify which features most strongly influence value and guide smarter pricing decisions.

</div>

## 📂Dataset

### Dataset Overview
The original dataset contained 3,454 rows and 15 columns. An additional 5,000 synthetic rows were generated to help apply more advanced modeling techniques and demonstrate scalability, model behavior, and business insights, while preserving the original data structure and missing value patterns. This resulted in a total of 8,454 observations with the same structure.

#### Device specs:
Device Specifications:
brand_name, os, screen_size, main_camera_mp, selfie_camera_mp, int_memory, ram, battery, weight, 4g, 5g

#### Usage & Pricing Data:
release_year, days_used, normalized_new_price, normalized_used_price

#### Independent variable 
The target variable is **normalized_used_price**, representing the resale price normalized across brands/models.

#### Missing Values:
To simulate real world data imperfections, a proportion of missing values was intentionally introduced in the synthetic portion of the dataset. These missing values follow the same patterns and frequencies as those found in the original data.

- main_camera_mp  
- selfie_camera_mp  
- int_memory  
- ram  
- battery  
- weight
  
# Exploratory Data Analysis (EDA)

#### Structure and Characteristics 
- Shape: 8,454 rows × 15 columns
- Data types: Mostly float, with some object (categorical) and a few integer columns
- Duplicates: None found
- Missing values: 491 missing entries, mainly in columns related to device specifications (these will be handled in the preprocessing stage)

#### Variable distributions

Univariate Analysis 

Both normalized used and new device prices show an approximately normal distribution, with slight skewness and some outliers.

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/f09c1842-9d5a-48da-be16-1934ad3303f6" width="400"/></td>
    <td><img src="https://github.com/user-attachments/assets/5805bab4-1498-4d92-873f-e760bd9e6f5c" width="400"/></td>
  </tr>
</table>

Samsung, Huawei, and LG are the most represented brands in the dataset, excluding the 'Others' category, which aggregates unregistered brands. Additionally, Android is the dominant operating system (OS) among the listed devices. 

<p>
  <img src="https://github.com/user-attachments/assets/f854fa38-dec8-4efc-a6d2-7083cd142ed9" width="550" style="display:inline-block; vertical-align:top; margin-right:20px;"/>
  <img src="https://github.com/user-attachments/assets/f392e1d8-2774-497b-ab6e-af877f7acf1f" width="400" style="display:inline-block; vertical-align:top;"/>
</p>

Bivariate Analysis 


![image](https://github.com/user-attachments/assets/7814462f-e070-43c7-9745-b27e34976776)

**Positive Correlations** (Potential Collinearity) 

- screen_size and battery (0.81): Devices with larger screens typically have higher battery capacities, which is expected due to increased power consumption.

- screen_size and weight (0.83): Larger screens often contribute to increased device weight.

- battery and weight (0.70): Bigger batteries tend to add more weight to the device.

- normalized_used_price and normalized_new_price (0.83): The used price is heavily influenced by the original retail price.

 **Negative Correlations**  

- days_used and selfie_camera_mp (-0.69): Older devices tend to have lower resolution front cameras

- days_used and normalized_used_price (-0.51): Resale value decreases as devices age

- days_used and battery (-0.49): Older models often have smaller or more degraded batteries

- days_used and selfie_camera_mp (-0.55): Devices that have been used longer are typically older models with lower camera specs

**Potential Collinearity**

The variables screen_size, battery, and weight are highly correlated with each other, which may lead to multicollinearity issues. Similarly, normalized_used_price and normalized_new_price show strong correlation, potentially introducing redundancy if both are used as predictors. For this reason, and as previously mentioned, the focus is placed solely on normalized_used_price.






  
