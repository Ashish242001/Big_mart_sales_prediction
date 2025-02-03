1.	Data Exploration and Distribution Check

Before diving into model building, we performed a comprehensive data exploration to understand the structure of the dataset and the distribution of the features. The initial steps involved the following:

•	Sweetviz: We used Sweetviz for a quick, visual data analysis. This helped us understand the over-all data distribution, identify any skewness, detect null values, and check for unique values in each column. Sweetviz provides insightful visualizations of:
•	Feature distributions: Helps identify if data is normally distributed or skewed.
•	Null Values: Indicates if any columns have missing data.
•	Unique Value Counts: Shows the number of unique values per feature, which helped us un-derstand the categorical features.
•	Value Statistics: Provides statistical summaries for numerical features like mean, standard deviation, etc.

2.	Data Preparation

Once the data exploration was complete, the next step was to prepare the data for modeling. This phase involved cleaning the data and performing necessary transformations:

•	Handling Nulls and Zeros:
•	Item Weight: Missing values in Item_Weight were handled using linear interpolation.
•	Item Visibility: Zeros in Item_Visibility were replaced using PCHIP interpolation (Piecewise Cubic Hermite Interpolating Polynomial), which is particularly effective when data points exhibit non-linear behavior.

•	Log Transformation:

•	Item Visibility: The Item_Visibility feature was highly skewed. To normalize its distribution, we applied a log transformation.


•	Standardizing Categorical Variables:
•	Item Fat Content: The Item_Fat_Content feature had two categories with variations in naming (e.g., low fat, Low Fat, lf). We standardized these variations to a single category Low Fat.
•	Item Identifier: The Item_Identifier feature had three categories that represented different types of items (e.g., FD, DR, NC). We kept only the first two letters (e.g., FD, DR, NC) for consistency.
•	Categorical Encoding:
We experimented with several encoding methods for categorical variables:
•	Label Encoding: This method assigns a unique integer to each category.
•	Target Encoding: This method replaces each category with the mean of the target variable for that category.
•	CatBoost's Own Categorical Encoding: CatBoost provides a built-in method for encoding categorical variables that helps it handle categorical data efficiently.
•	After evaluating the performance of each method, Ordinal Encoding performed the best for our data. This method assigns a numerical label to each category, which worked well for the model and provided the best results.

3.	Feature Engineering

Feature engineering is a critical part of the modeling process. We created new features that capture important relationships between the existing variables. Some of the newly created features include:
•	Price per Unit Weight: This was calculated as the ratio of the Item_MRP (Maximum Retail Price) to Item_Weight. This feature indicates the price of the product per unit of weight and can be useful for identifying high-value products.
•	Visibility Within Rank Type: We created this feature by calculating the rank of Item_Visibility within each Item_Type. This helps in understanding how a product’s visibility compares to oth-ers within the same category.

After creating these new features, we proceeded with two important steps:
•	Feature Importance: We used a Random Forest model to check the importance of each feature. This helped us identify which features contributed the most to predicting the target variable.
•	Multicollinearity Check: We examined the correlation between features using a Correlation Ma-trix and VIF (Variance Inflation Factor). Based on the results, we dropped highly correlated fea-tures that were contributing to multicollinearity.

4.	 Modeling

For the model selection, we experimented with various algorithms to predict the target variable (Item_Outlet_Sales). The models included both regression-based methods and tree-based models:
•	Regression Models: We started with basic regression models to establish a baseline perfor-mance.
•	Tree-Based Models: We explored tree-based algorithms like Decision Trees, Random Forest, and Gradient Boosting Machines (GBM) to capture more complex relationships between the features and the target variable.
•	Ensemble Models: We also applied ensemble methods like XGBoost and LightGBM to further improve the model’s accuracy.

However, after trying multiple models, CatBoost emerged as the top performer, yielding the best re-sults in terms of RMSE (Root Mean Squared Error).

Hyperparameter Tuning:
To fine-tune the performance of the CatBoost model, we performed Grid Search Cross-Validation. This allowed us to experiment with different values for parameters such as:
•	Iterations: Number of boosting rounds.
•	learning_rate: Step size at each iteration while moving towards a minimum of the loss function.
•	depth: Depth of the decision trees.
•	l2_leaf_reg: Regularization parameter to control overfitting.
Using Grid Search, we were able to identify the optimal combination of hyperparameters that resulted in the best model performance.

5.	 Model Evaluation

Finally, we evaluated the performance of our model on the test set using key metrics:
•	RMSE (Root Mean Squared Error): This was the primary evaluation metric. The lower the RMSE, the better the model’s predictions are.
•	Other Metrics: We also considered additional metrics such as Mean Absolute Error (MAE) and R-squared to assess the model's accuracy from different perspectives.
