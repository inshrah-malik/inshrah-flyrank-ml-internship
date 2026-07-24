# Capstone Report — CTR Prediction

- **Author:** Inshrah Malik
- **Lane:** CTR Prediction (Click-Through Rate Prediction)
- **Repo:** https://github.com/inshrah-malik/inshrah-flyrank-ml-internship
- **Date:** 24 July 2026

---

## 0. Abstract

This project investigates whether machine learning can predict the Click-Through Rate (CTR) of webpages using search performance and user engagement data from the FlyRank ML Internship dataset. The analysis used the `fact_content_daily_performance` dataset with selected Google Search Console and engagement features while excluding identifier and leakage-prone columns. Two regression models, Linear Regression and Random Forest Regressor, were trained and evaluated using Mean Absolute Error (MAE), Mean Squared Error (MSE), and R². The results showed that the Linear Regression model outperformed the Random Forest model, achieving an MAE of 0.00906, an MSE of 0.00118, and an R² score of 0.02475. These predictions can help FlyRank editors prioritize webpages for SEO improvements and content optimization.
---

## 1. Problem Framing

The goal of this project is to support content optimization decisions by predicting the Click-Through Rate (CTR) of webpages.

- **Decision supported:** Which webpages should be prioritized for SEO and content optimization.
- **Unit of analysis:** One webpage on one reporting date.
- **Output:** Predicted CTR score.
- **Human action:** Editors review pages with low predicted CTR and improve titles, meta descriptions, page content, or user experience.
- **Cost of incorrect predictions:** Incorrect predictions may cause valuable optimization effort to be spent on pages that do not require improvement while missing pages that genuinely need attention.

Machine learning is suitable because CTR depends on multiple interacting factors that are difficult to evaluate manually. A predictive model can learn these relationships and consistently rank pages according to their optimization priority.

---

## 2. Data Safety

This project uses the **FlyRank ML Internship** dataset, specifically the `fact_content_daily_performance` table. Only search performance and user engagement features relevant to CTR prediction were used.

### Features Used

The model was trained using the following features:

- impressions
- avg_position
- pageviews
- sessions
- scroll_events

### Columns Excluded

The following columns were excluded from model training:

- report_date
- client_hash_id
- content_hash_id
- clicks

### Why These Columns Were Excluded

- `report_date` was excluded because it is a timestamp and was not used as a predictive feature.
- `client_hash_id` and `content_hash_id` are pseudonymous identifiers. They were used only for identifying records and were not included as model features because they do not contain meaningful predictive information.
- `clicks` was excluded because it is used to calculate the target variable (CTR = clicks / impressions). Including it as a feature would introduce data leakage and make the evaluation unrealistic.

No client-identifying information or personally identifiable data appears anywhere in the `work/` directory. All identifiers used in the dataset are hashed and were excluded from model training.

## 3. Baseline

A Linear Regression model was selected as the baseline because it is a simple, transparent, and widely used regression algorithm. It provides an interpretable benchmark for evaluating whether a more complex machine learning model can improve CTR prediction.

The baseline model was evaluated using the same train-test split and performance metrics as the final model.

| Model | MAE | MSE | R² |
|------|------:|------:|------:|
| Linear Regression | 0.00906 | 0.00118 | 0.02475 |

The baseline achieved a Mean Absolute Error (MAE) of **0.00906**, a Mean Squared Error (MSE) of **0.00118**, and an R² score of **0.02475**. These values provide the reference point for comparing the Random Forest model.
---



## 4. Model / Analysis

The final model evaluated in this project was a Random Forest Regressor. Random Forest was selected because it can capture nonlinear relationships between search performance and user engagement metrics while requiring minimal feature engineering.

### Features Used

The model was trained using the following input features:

- impressions
- avg_position
- pageviews
- sessions
- scroll_events

### Features Excluded

The following columns were excluded because they are identifiers, unavailable, or could introduce data leakage:

- report_date
- client_hash_id
- content_hash_id
- clicks (used to calculate the target variable CTR)

### Target

The target variable was Click-Through Rate (CTR), calculated as:

CTR = clicks / impressions

The Random Forest model was trained to predict the CTR of each webpage using the selected search performance and engagement features.
### Features Used

- gsc_impressions
- gsc_avg_position
- ga4_pageviews
- ga4_sessions
- ga4_users
- sessions_organic
- sessions_direct
- sessions_social
- scroll_events

### Features Excluded

The following columns were excluded because they are identifiers, unavailable, or could introduce data leakage:

- report_date
- client_hash_id
- content_hash_id
- client_has_gsc
- client_has_ga4
- gsc_data_available
- ga4_data_available
- gsc_clicks

## Target

The target variable was Click-Through Rate (CTR), calculated as:

CTR = gsc_clicks / gsc_impressions

The Random Forest model was trained using the selected search performance and engagement features to predict the CTR of each webpage.

---

## 5. Evaluation

Both models were evaluated on the same test dataset using Mean Absolute Error (MAE), Mean Squared Error (MSE), and the coefficient of determination (R²).

| Model | MAE | MSE | R² |
|------|------:|------:|------:|
| Linear Regression | 0.00906 | 0.00118 | 0.02475 |
| Random Forest | 0.00850 | 0.00148 | -0.21712 |

### Error Analysis

The Random Forest model produced a slightly lower Mean Absolute Error (0.00850) compared with the Linear Regression baseline (0.00906). However, it also produced a higher Mean Squared Error (0.00148) and a negative R² score (-0.21712).

The negative R² value indicates that the Random Forest model explained the variation in CTR less effectively than the baseline and performed worse than simply predicting the average target value. Although the average absolute prediction error was marginally lower, the overall predictive performance was weaker.

Based on these evaluation results, the Linear Regression model demonstrated better overall performance and generalization on the test dataset.



---

## 6. Interpretation

The results indicate that the selected search and engagement features contain some useful information for predicting CTR. However, the available features were not sufficient for the Random Forest model to outperform the simpler Linear Regression baseline.

The relatively low R² values suggest that CTR is influenced by additional factors that were not included in the dataset, such as page content quality, title optimization, user intent, and other SEO-related variables.

This outcome highlights that increasing model complexity does not necessarily improve prediction performance. A simpler and more interpretable model performed better for this dataset.

Future work could improve prediction accuracy by incorporating additional search, content, and behavioral features and by tuning the Random Forest hyperparameters.

---
## 7. Recommendation

Based on the model outputs, FlyRank editors can use CTR predictions as a decision-support tool to identify webpages that may benefit from optimization.

Recommended actions include:

- Prioritize pages with high impressions but low predicted CTR.
- Improve page titles and meta descriptions.
- Update page content to better match user search intent.
- Increase user engagement through improved readability and content quality.

The Linear Regression model is currently the more reliable model for this dataset because it demonstrated better overall predictive performance. The predictions should be used to support editorial decisions rather than replace human judgment, particularly for pages with limited historical data.
---

## 8. Reproducibility

The project can be reproduced using the following steps.

```bash
git clone https://github.com/inshrah-malik/inshrah-flyrank-ml-internship

cd inshrah-flyrank-ml-internship

pip install -r requirements.txt

jupyter notebook work/FlyRank_CTR_Capstone.ipynb
```

### Random Seed

```
random_state = 42
```

### Environment

- Python 3.11
- pandas
- numpy
- scikit-learn
- matplotlib
- datasets
- transformers (if applicable)

Running the notebook from a fresh clone reproduces all preprocessing, training, evaluation, and reported metrics.

---

## 9. Acknowledgments & Data Credit

This project was built using the **FlyRank ML Internship dataset**.

Data Source:

https://flyrank.ai

The FlyRank dataset is used for educational and research purposes as part of the FlyRank Machine Learning Internship.
