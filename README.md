Report 
Title:
Report- Data Preprocessing,EDA, Model Building and Evaluation
Prepared by:
Samhitha Angara

Introduction 
This report outlines the process of selecting, building, and evaluating a machine learning model to predict customer churn. The goal is to identify customers at risk of leaving and provide actionable insights to help mitigate this risk.

The following features from the dataset were used:

•	CustomerID: A unique identifier for each customer.

•	Age: The age of the customer (numerical).

•	Gender: The gender of the customer (categorical; Male/Female).

•	Marital_Status: The marital status of the customer (categorical; options include Single, Married, Divorced, Widowed).

•	Income_Level: Classification of the customer’s income (categorical; Low, Medium, High).

•	Amount_Spent: The amount spent by the customer during each purchase (continuous numerical).

•	ChurnStatus: Indicates whether the customer has churned (binary; yes/no).

•	Product_Category: Different types of products purchased (categorical; Electronics, Clothing, Furniture, Books, Groceries).

•	InteractionType: Various forms of customer interaction (categorical; Inquiry, Complaint, Feedback).

•	LoginFrequency: The frequency of customer logins (numerical).


The data was thoroughly checked for missing values, incorrect values and duplicates before starting the analysis.
Exploratory Data Analysis (EDA)

Data Transformation

The following transformations have been made to the data:

-	All categorical variables were transformed using one-hot encoding. This conversion helped create a numerical representation for each categorical variable, allowing algorithms to process them efficiently. For each category in a variable, a new binary column was created. Since each category is represented in its own column, any loss of the significant information was prevented.

-	The Total_Amount_Spent column was created by aggregating the individual transactions for each customer. Specifically, the Amount_Spent values from each customer’s transaction history were summed up to generate a single cumulative amount per customer. This step not only simplifies the dataset by consolidating each customer’s spending into a single record but also eliminated the redundancy of having multiple rows per customer.

-	The Count_of_Products_Purchased column was calculated to represent the total number of products purchased by each customer, regardless of product category. This feature captured the overall volume of purchases made by each customer by aggregating all transaction records into a single total count.

Univariate Analysis of Total_Amount_Spent 
The data demonstrated mild positive skewness suggesting that very few customers had a very high total expenditure. Since, the tail is shorter and the data showed no outliers this data was used as such to carry out the analyses. 
   
Bivariate Analysis of Gender, Marital Status, Income Level, Interaction_Type and Product Category by ChurnStatus

-	 Gender and Churn Status:

The count plot indicates that female customers have a higher retention rate compared to male customers. This could imply that your female clientele may be more satisfied with the service or product offered, or they might have different usage patterns or loyalty behaviors.

-	Marital Status:

When looking at the "Widowed" category, the individuals also show a higher number of retained customers. This demographic might have unique needs or preferences that influence their likelihood to remain loyal to a service or brand.

- Income Level

The "High" income level group displays a strong retention rate as well. Higher income customers might have different expectations, and if their needs are met effectively, they are likely to continue their relationship with the company.

-	Interaction Type 

The data shows interesting patterns in how different types of customer interactions relate to churn status. Customers who filed complaints were slightly more likely to leave, with 64 churned compared to 233 who stayed. Feedback givers had 67 churned versus 240 retained, showing feedback as a critical touchpoint. Interestingly, inquiries were the least associated with churn, with only 52 customers leaving out of 279 who contacted for queries. This suggests that proactive customer engagement, especially around resolving complaints and addressing feedback, may be crucial in retaining customers and reducing churn rates.

 
-	Product Category 

In each category, the number of customers who stayed is significantly higher than those who left, with Books showing the greatest retention (508 stayed vs. 125 left). This trend suggests that, overall, customer satisfaction remains high across all categories.

Identification of Outliers

Box plots have been created for the remaining variables, and no outliers have been identified in the data.

Correlation

A correlation analysis was conducted to examine the relationships between all variables and the response variable, ChurnStatus, in order to identify significant and relevant features for model building. Pearson correlation was employed to assess linear correlations; however, no significant associations were found between ChurnStatus and the predictor variables. This lack of correlation may be attributed to the binary nature of both the predictor and response variables, which can limit the ability to capture linear relationships effectively.
 
The exploratory data analysis (EDA) revealed no significant correlations between the response variable ChurnStatus and other predictor variables, primarily due to their binary nature. Despite this, categorical analyses indicated potential patterns that may influence customer churn, particularly in relation to customer feedback and complaints.

The imbalanced nature of the dataset posed challenges in model training and evaluation. To address this, I implemented the Adaptive Synthetic Sampling (ADASYN) technique to generate synthetic examples of the minority class, thereby enhancing the model's ability to learn from underrepresented data. I further employed Random Search for hyperparameter optimization and evaluated the model's performance using key metrics: ROC-AUC, Precision, Recall, and F1 Score.

Dataset Overview

•	Dataset Description: The dataset consists of features that are used to predict ChurnStatus that is a binary outcome (0 or 1). However, the target class is imbalanced, with a higher number of instances belonging to the majority class (Customer retained class)

•	Original Class Distribution:
-	Majority class (0): 79.6%
-	Minority class (1): 20.4%
 
Methodology

Data Preprocessing
•	Data Splitting: The dataset was split into training and testing subsets using a stratified approach to ensure that both subsets reflect the original class distribution.

Resampling with ADASYN

•	ADASYN Application: The ADASYN method was applied to the training dataset to generate synthetic instances of the minority class. This technique focuses on generating samples in regions where the minority class is underrepresented.

Model Training

•	Random Forest Classifier: A Random Forest classifier was chosen for its robustness and ability to handle imbalanced data.
•	Hyperparameter Tuning: Random Search was utilized to explore different hyperparameter configurations to identify the best-performing model settings.
•	Feature importance analysis identified key predictors of churn, such as transaction amount and login frequency.
   Model Evaluation
•	Evaluation Metrics: The model's performance was assessed using the following metrics:
-	ROC-AUC: Measures the ability of the model to distinguish between the classes especially for an imbalanced dataset.
-	Precision: The ratio of true positive predictions to the total predicted positives.
-	Recall: The ratio of true positive predictions to the total actual positives.
-	F1 Score: The harmonic mean of Precision and Recall, providing a balance between the two.

Results

Model Performance

•	Best Model Parameters from Random Search:

-	n_estimators: 200
-	max_depth: 30
-	min_samples_split: 2
-	min_samples_leaf: 1

Evaluation Metrics

•	Best ROC-AUC Training Score: 0.92

•	Precision: class 0 – 0.79, class 1- 0.15

•	Recall: class 0 – 0.93, class 1 – 0.05

•	F1 Score: class 0 – 0.86, class 1 – 0.07

Conclusion

Although the best ROC_AUC score of 0.92 was noticed, the ROC_AUC score drops to 0.52 on test sets this could be due to the imbalanced data and insufficient data to train the minority group. Other scores taken into consideration for evaluating the model are Precision, Recall, F1- Score and Accuracy. ChurnStatus 0 exhibits strong performance with a high precision of 0.79 and a recall of 0.93, indicating that the model effectively identifies the majority class. In contrast, ChurnStatus 1 struggles significantly, evidenced by a low precision of 0.15 and an alarmingly low recall of 0.05, highlighting the model's difficulties in accurately predicting the minority class. This imbalance is further underscored by the F1 Score for Class 1, which stands at just 0.07, reflecting a failure to achieve a balance between precision and recall for the minority class—a crucial aspect in the context of imbalanced datasets.




