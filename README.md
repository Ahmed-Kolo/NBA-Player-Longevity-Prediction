# NBA Player Longevity Prediction: Gaussian Naive Bayes Classifier

## Project Overview

This project analyzes engineered NBA player data using Python and Scikit-learn to build a Gaussian Naive Bayes classification model. The goal is to predict whether a player will have a career longevity of 5 or more years. This analysis translates statistical predictions into actionable scouting insights, evaluating model performance using business-relevant metrics like Precision and Recall, and critically assessing the model's underlying assumptions.

## Dataset

The dataset used for this project is `extracted_nba_players_data.csv`. It contains various engineered statistics for NBA players. The target variable, `target_5yrs`, indicates whether a player played for 5 or more years (1) or less than 5 years (0). The features include:
- `fg`: Field Goal Percentage
- `3p`: 3-Point Percentage
- `ft`: Free Throw Percentage
- `reb`: Rebounds per game
- `ast`: Assists per game
- `stl`: Steals per game
- `blk`: Blocks per game
- `tov`: Turnovers per game
- `total_points`: Total points scored
- `efficiency`: Player efficiency rating

### Data Exploration and Preprocessing

- The dataset contains 1340 entries and 11 columns.
- All features are numerical (`float64` or `int64`).
- No missing values were identified in any of the columns.
- The target variable (`target_5yrs`) was successfully separated from the feature set.

## Modeling Approach: Gaussian Naive Bayes

### Model Selection
Gaussian Naive Bayes was chosen for this binary classification task due to its simplicity, speed, and suitability for continuous features. It provides a probabilistic framework for classification.

### Data Splitting
The dataset was split into training and testing sets with an 80/20 ratio, ensuring stratified sampling to maintain the class distribution in both sets. This allows for unbiased evaluation of the model's performance on unseen data.

### Implementation
- The `GaussianNB` classifier from `sklearn.naive_bayes` was initialized and trained on the `X_train` and `y_train` data.

## Model Evaluation

The model's performance was evaluated using a Confusion Matrix, Classification Report, Precision, and Recall. For a scouting department, minimizing false positives ('busts') and false negatives ('missed talent') are critical.

### Confusion Matrix
```
[[79 23]
 [74 92]]
```
- **True Negatives (TN):** 79 players correctly predicted as not having 5+ years longevity.
- **False Positives (FP):** 23 players incorrectly predicted as having 5+ years longevity (Type I error - 'busts').
- **False Negatives (FN):** 74 players incorrectly predicted as not having 5+ years longevity (Type II error - 'missed talent').
- **True Positives (TP):** 92 players correctly predicted as having 5+ years longevity.

### Classification Report
```
              precision    recall  f1-score   support

           0       0.52      0.77      0.62       102
           1       0.80      0.55      0.65       166

    accuracy                           0.64       268
   macro avg       0.66      0.66      0.64       268
weighted avg       0.69      0.64      0.64       268
```

### Key Performance Metrics for Scouting:

-   **Precision for Longevity (Class 1): 0.80**
    -   When the model predicts a player *will* have longevity (5+ years), it is correct 80% of the time. This is good for minimizing 'busts' – players scouted extensively who don't pan out.

-   **Recall for Longevity (Class 1): 0.55**
    -   Out of all players who *actually* achieve longevity, the model correctly identifies only 55% of them. This means a significant 45% of genuinely long-lasting players are **missed** by this model, representing a risk of 'missed talent'.

## Naive Bayes "Independence Assumption" Discussion

Gaussian Naive Bayes fundamentally assumes that features are conditionally independent given the class variable. In basketball statistics, this assumption is often violated. For instance, 'total_points' is highly correlated with 'fg' (field goal percentage) and 'ast' (assists). The correlation matrix visually confirmed these interdependencies among features.

While Naive Bayes can still perform reasonably well in practice despite violations of this assumption, it's a limitation. The model might not fully capture the complex, synergistic relationships between player statistics, potentially leading to simplified 'reasoning' for its predictions and potentially suboptimal predictive power compared to models that account for feature dependencies.

## Model Reliability and Limitations for a Scouting Department

### Strengths:
-   **Simplicity and Speed:** Quick to train and deploy.
-   **Probabilistic Predictions:** Provides insights into the likelihood of longevity.
-   **Decent Precision for Longevity:** Helps minimize 'busts' by being 80% correct when predicting a long career.

### Limitations:
-   **Violation of Independence Assumption:** This is a major concern, as many NBA stats are highly correlated, which the model does not explicitly account for.
-   **High False Negatives for Longevity (Missed Talent):** The model misses 45% of players who *do* achieve longevity, posing a significant risk of overlooking valuable talent.
-   **Oversimplification:** May not capture the nuanced factors contributing to NBA longevity due to its simplified assumptions.

### Recommendations for Scouting Integration:

1.  **Complementary Tool:** Use this model as an *initial screening tool*, not the sole decision-maker. It can help flag potential candidates for further human scouting.
2.  **Focus on High Precision:** Leverage the 80% precision for longevity predictions. Players highly rated by this model are strong candidates for deeper analysis.
3.  **Mitigate Missed Talent:** Be aware of the high false negative rate for longevity. **Do not dismiss players solely based on a negative prediction**; manual review is crucial to avoid missing future stars.
4.  **Explore Advanced Models:** For a more comprehensive understanding and improved predictive power, consider integrating more sophisticated machine learning models (e.g., Logistic Regression, Random Forests, Gradient Boosting) that can better handle feature dependencies and complex interactions.

In essence, while this Gaussian Naive Bayes model offers a fast and interpretable first pass at predicting player longevity, its inherent assumptions and limitations necessitate its use in conjunction with expert human judgment and potentially more advanced analytical techniques.
