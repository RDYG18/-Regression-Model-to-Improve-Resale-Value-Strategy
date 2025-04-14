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

---
Bivariate Analysis


<img src="https://github.com/user-attachments/assets/7814462f-e070-43c7-9745-b27e34976776" width="870"/>


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

---
**Analysis of Important Variables as Price Influencers**

RAM

The RAM is one of the most important components of a phone, as it directly affects the device's speed, multitasking capabilities, and overall user experience. In other words brands with higher RAM models may demand higher resale prices and target different customer segments.  


![image](https://github.com/user-attachments/assets/7f816ea0-f9c7-4ba4-8d84-25e8b06c0e19)

Boxplot insights: 

- **OnePlus**, **Nokia**, **Honor**, and **Huawei** offer higher RAM devices, meaning targeting mid to high end segments.

- **Infinix**, **Micromax**, and **Karbonn** consistently feature lower RAM, focusing on the budget market.

- **Samsung**, **Xiaomi**, **Oppo**, and **Vivo** show a wide range of RAM values, reflecting a diverse lineup from entry level to premium models.

- **Apple** maintains lower RAM levels, consistent with its hardware software optimization strategy.

---
BATERRY

Since smartphones have become an essential tool in our daily lives, we need devices with high battery capacity to support daily usage. However, as observed in the correlation heatmap and weight distribution analysis, an increase in battery capacity often results in a heavier devices. 

 **note**: For this boxplot, I selected devices with battery capacities above 4500 mAh in order to focus on models with large batteries.  

![image](https://github.com/user-attachments/assets/e466c6a6-c758-482f-ab1a-b0200c378e4e)

Boxplot insights: 

- **Huawei**, **Lenovo**, **LG**, **Apple**, **Asus**, **Acer**, **Alcatel**, and **Samsung** show high median weights, often exceeding 400g, likely due to large batteries or heavier components.

- **Lenovo** and **LG** stand out for having wide weight variability, suggesting robust or large sized device designs.

- **Honor**, **ZTE**, and **Xiaomi** have lower median weights (around 200–250g), indicating a good balance between battery capacity and lightweight design.

- **Infinix**, **Vivo**, **Realme**, **Google**, and **Gionee** display compact weight distributions, reflecting consistency in design with minimal variation.

- **Samsung**, **Apple**, and **Huawei** present notable outliers, pointing to some heavier models, possibly due to premium materials or larger displays.

---
SCREEN

The demand for large screens, driven by entertainment and social media use, is pushing users to buy better phones and tablets increasing device prices.  

 **note**: This barplot includes only devices with screen sizes larger than 6 inches (the standard threshold) to focus on phones with larger displays. The shape of the filtered dataset is (2511, 15).


<img src="https://github.com/user-attachments/assets/c5e4158d-63ae-4c7e-b30c-b0719d9915ba" width="800"/>



Barplot insights: 

- **Huawei** and **Samsung** lead in large-screen devices, with 382 and 326 models respectively, highlighting a strong focus on display size.

- The **"Others"** category also contributes significantly, showing that the large-screen trend extends beyond major brands.

- **Honor** and **Vivo** follow with over 170 devices each, suggesting that large screens are standard across their product lines.

- **Lenovo**, **LG**, and **Xiaomi** maintain solid counts above 140, indicating a balance between screen size, performance, and affordability.

- **Oppo** and **Asus**, while lower in count, still offer large-screen models, likely focusing on other components .

---
SELFIE CAMERA

One of the most important aspects of a phone or tablet perhaps the most important is the camera. In particular, the selfie camera plays a key role, as a high quality front camera often increases the device’s price.

 **note**: In order to analyze which brands offer the best selfie cameras on the market, we filtered the dataset to include only devices with front cameras higher than 8 MP. The shape of the filtered dataset is (1494,15).

 <img src="https://github.com/user-attachments/assets/ff35446f-f6ba-4206-af41-ce9f45e61f72" width="800"/>

 barplot insights: 

 - **Huawei** leads featuring front cameras above 8 MP, showing a strong focus on selfie camera quality.

- **Vivo**, **Samsung**, and **Oppo** follow closely, reinforcing their position in selfie focused markets.

- **Xiaomi** and **Honor** also contribute significantly,reflecting strong mid range camera offerings.

- **LG**, **Motorola**, and **ZTE** appear at the lower end, possibly indicating less emphasis on front camera resolution.

- The **"Others"** category, suggests that many lesser-known brands also compete in the high selfie camera segment.

---
MAIN CAMERA

While the selfie camera plays a key role in today’s social media-driven usage, the main (rear) camera remains a critical factor in a device’s overall value and performance. High resolution rear cameras are strongly associated with premium models, as they are essential for photography, video recording, and professional content creation.

**note**: In order to analyze which brands offer the best main cameras on the market, we filtered the dataset to include only devices with main cameras higher than 16 MP. The shape of the filtered dataset is (218,15).

<img src="https://github.com/user-attachments/assets/d4ccdc27-812d-4326-8b4d-1cefc2842fe1" width="800"/>


barplot insights: 

- **Sony** leads the category, highlighting a strong focus on high quality main cameras.

- **Motorola** and **"Others"** also are in the podium of main cameras.  

- **HTC**, **ZTE**, **Nokia**, and **Microsoft** fall in the mid-range, indicating a moderate emphasis on main camera resolution.

- **Meizu**, **Samsung**, and **Asus** appear at the lower end, suggesting they either prioritize other features or offer more balanced camera specifications.

---
4G OR 5G NETWORKS

The presence of 4G and 5G connectivity features affects the normalized resale price of phones and tablets in the second hand market. 

![image](https://github.com/user-attachments/assets/4faca113-65a2-470a-bcae-3cb09ea49ccc)

boxplot insights: 

- Devices with 4G support have higher and more consistent resale prices, confirming 4G as a standard feature in the used market.

- 5G enabled devices show even higher median prices, often linked to newer or premium models especially considering that, at the time this data was collected, 5G was still an emerging technology.


---
PRICES ACROSS YEARS

From 2013 to 2019, used device prices showed a steady increase, suggesting that newer models retain more resale value. Although there was a slight drop in 2020 possibly due to market saturation or rapid technological advancement the overall trend indicates that used phones are becoming more valuable, driven by better durability, higher original prices, and growing interest in sustainable tech.

  <img src="https://github.com/user-attachments/assets/e289b222-5b60-4b19-a66e-cb5cc972ba60" width="800"/>

---
# Data Preprocessing

## Missing value tratment 

As mentioned earlier, we identified 491 missing values in key columns such as main_camera_mp, selfie_camera_mp, int_memory, ram, battery, and weight. To handle them, we applied group-based imputation by grouping the data by brand_name and release_year, two columns without missing values. We used the median within each group to fill the missing values, as these columns contain outliers and the median is more robust than the mean. This method provides more accurate and context aware imputations compared to using a single global value.


<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/b70c08b1-01e4-45d4-b70c-9d08d0907699" width="200"/></td>
    <td><img src="https://github.com/user-attachments/assets/9cefb3d4-8d9c-4a1c-9733-8eec5620700d" width="200"/></td>
  </tr>
</table>

---
## Feature Engineering

We are going to create a new column, years_since_release, by calculating the difference between the current year (2021) and the release_year of the product, using 2021 as a reference year to avoid altering the analysis of the recorded years. This transformation is more useful than the original release_year because the product's age is often more relevant for predictive models. After creating years_since_release, we drop the release_year column to avoid redundancy.

  <img src="https://github.com/user-attachments/assets/5c9e6046-fb24-4c2c-be78-e31e168490d3" width="500"/>

variable insights: 

The majority of devices were released between 3 and 7 years ago, with a median age of around 5 years. The distribution is balanced and free of outliers, making it suitable for modeling resale trends, depreciation, and user preferences across different product ages.

## Outlier check and Treatment 

There are several outliers present in the dataset. However, we decided not to treat them, as they reflect valid and realistic values rather than errors or anomalies. Removing these points could distort the natural distribution and compromise the integrity of the analysis.


 <img src="https://github.com/user-attachments/assets/9dd4f5d3-e1c3-4968-97b0-50621709a846" width="500"/>

---
## Data preparation for modeling

To prepare the data for modeling, we first separated the target variable normalized_used_price from the independent variables. We then added a constant term to the features to account for the intercept in the linear regression model. Since the dataset includes categorical features, we applied one-hot encoding using pd.get_dummies, dropping the first level of each category to avoid multicollinearity and converting the result to integer type. After preprocessing, we split the dataset into training and testing sets using a 70:30 ratio with a fixed random state for reproducibility. This setup ensures that the data is clean, numerical, and properly structured for training a Linear Regression model and evaluating its performance on unseen data.

--- 

# Modeling Linear Regression 

 We first trained a baseline linear regression model using all available features after encoding and preprocessing. This initial model served as a reference to identify multicollinearity and statistically insignificant variables, which i will treathtem later. The model’s performance was assessed using standard regression metrics. 
 
![image](https://github.com/user-attachments/assets/cb3a7ebc-5a82-41f1-8aeb-02313f32804f)
![image](https://github.com/user-attachments/assets/efd22dba-f8c2-4285-b6c8-b90a2e4a582b)

Baseline model insights: 

The model explains over 80% of the variance in the target variable and shows low prediction error on both training and test sets, indicating strong generalization and no overfitting. The MAE is around 0.20, meaning the model predicts normalized prices with a small average error. As expected, RMSE is slightly higher due to its sensitivity to larger errors. A MAPE of ~4.8% confirms the model can predict resale prices within a narrow and acceptable margin of error, making it suitable for real-world applications.

## Test for Multicollinearity and Treatment

To detect multicollinearity among the independent variables, I applied the Variance Inflation Factor (VIF). The test revealed that some numerical variables exhibited high multicollinearity, which could negatively affect the model's reliability. To address this, I removed three variables with high VIF values: screen_size, brand_name_Apple, and brand_name_Others. After dropping these columns, the remaining numerical features showed VIF scores below the common thresholds (under 5 or 10), indicating that multicollinearity was successfully reduced and the model is now more stable and interpretable.

---
## Treat high p-values 

To improve the model, I removed variables with p-values greater than 0.05, as they are not statistically significant. Since p-values can change when variables are dropped, I applied an iterative approach: at each step, I removed the variable with the highest p-value, rebuilt the model, and repeated the process until all remaining features had p-values ≤ 0.05. This method ensures that only meaningful predictors are retained, improving the model's interpretability and stability.

---

## Final model 

The **Linear Regression** model explains **80%** of the variation in resale prices and predicts used device values with an average error of just **4.8%**. This makes it a reliable tool for estimating second-hand value and supporting smarter pricing decisions. Also, the model shows which aspects of a phone increase its resale value and which brands retain their value.

### Key Aspects:

- Devices with more memory (**RAM**) are worth more in the second hand market, increasing resale value by **0.0344 units**.
- Both **rear** and **selfie camera** quality have a positive impact  the higher the megapixels, the better the resale value, increasing it by **0.0261** and **0.0182 units**, respectively.
- **Heavier phones** tend to have higher resale prices, possibly due to better build quality or more features  increases resale value by **0.0017**.
- **4G connectivity** increases resale value by **0.0872 units** compared to non 4G devices.
- The **normalized new price** shows that premium models retain value better, as they often have superior features, materials, and brand reputation.  
- For every **1 unit increase** in normalized new price, the resale value increases by **0.3005 units** in the normalized used price.
- **Battery capacity** has a small positive effect, increasing resale value by **0.00001771 units**.
- **Years since release** negatively impacts price each additional year decreases value by **0.0119 units**.
- Devices with **uncommon operating systems** tend to have lower resale value, decreasing it by **0.1625 units**.

### Brands Value:

- Brands like **Nokia**, **Xiaomi**, **Asus**, and **BlackBerry** tend to hold value better over the years.
- **Micromax**, **Motorola**, and **ZTE** have lower resale prices as time passes.


![image](https://github.com/user-attachments/assets/89b45ff9-794a-42f8-b9d1-7a66dcc7e733)
![image](https://github.com/user-attachments/assets/51004fd7-8ba9-4c20-a62c-39bfb06f6a9c)



<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/d20b2577-5518-4d20-baf4-68cf2bf9362b" width="800"/></td>
    <td><img src="https://github.com/user-attachments/assets/a50646ca-47c9-4527-bbca-9992b357f358" width="800"/></td>
  </tr>
</table>

---
## Recommendations
- Focus on acquiring newer devices with higher RAM, quality cameras, and 4G connectivity, as these features are strongly associated with better resale value and greater appeal to second hand buyers.

- Prioritize sourcing premium models that had a high original price, as they tend to retain more value and generate higher profit margins in the resale market.

- Capitalize on brand perception by targeting devices from brands with strong resale performance, such as Nokia and Xiaomi, which consistently hold value better over time.

- Explore product diversification, particularly in the resale of complementary tech devices like smartwatches, which may attract niche segments of tech-oriented consumers.

- Collect more data like customer demographic data (e.g., age, income, preferences) to better understand buyer behavior and tailor pricing strategies for different market segments.

- It is recommended that ReCell launch a targeted sustainability campaign to strengthen its brand and attract environmentally conscious customers. By promoting the reuse of electronic devices, the company can reduce production-related CO₂ emissions, minimize toxic waste, and stand out in the refurbished tech market. This strategy not only aligns with global sustainability trends, but also offers a clear competitive advantage and access to a growing segment of eco-aware consumers.

---
## Conclusion 

<p align="justify">

By applying these insights, ReCell can enhance profitability, manage inventory more efficiently, and better align product offerings with market demand, focusing on devices that truly add value rather than overstocking. As a forward looking strategy, ReCell could also replicate this model to expand into other refurbished tech categories such as smartwatches or accessories, unlocking new growth opportunities.

Additionally, I recommend launching a sustainability focused campaign to strengthen ReCell’s ESG profile by promoting device reuse and circular economy practices. This approach not only reduces environmental impact but also appeals to a growing segment of consumers who actively support responsible business models.

Combining data driven decisions with sustainability and product diversification will reinforce ReCell’s competitive position and support long term success in the evolving sustainable economy.

</p>
