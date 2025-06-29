**# Stock Price Prediction Using Stacking Ensemble**

This project predicts stock prices using a stacking ensemble model. The code is written in a single Jupyter Notebook and is divided into three main parts.

---

**1. Data Preparation**

- Loads and preprocesses historical stock data (OHLC - Open, High, Low, Close).
- Applies **MinMax scaling** to normalize the values between 0 and 1.
- Uses a **sliding window technique**:
  - For example, the previous 280 days are taken as **input features**.
  - The target label is either:
    - The **return** for a future day (in section 1 and 2), or
    - The **price** directly (in section 3).
- Data is split into:
  - **70% Training**
  - **15% Validation**
  - **15% Testing**

---

**2. Feature & Label Extraction**

- For each sample:
  - `X[i]` = an array of past 280 stock prices (features)
  - `y[i]` = the future value to predict:
    - In first two sections: **returns** for future days
    - In the third section: **prices** directly
- Labels (`y`) are generated **for multiple future target days** (e.g., day 1 to day 14 or more).
- A new model is trained for each target day.

---

**3. Model Training (Stacking Ensemble)**

- Base models are trained individually for each day:
  - E.g., Linear Regression, Ridge Regression, Random Forest, etc.
- A **meta-learner** (usually Linear Regression) takes the predictions from all base models and learns to combine them.
- This stacking technique helps improve prediction robustness and reduce overfitting.
- Models for each future day are saved using `joblib` for reuse in prediction.

---

**4. Prediction and Graph Plotting**

- The trained stacking model is used to predict:
  - **Returns** in the first two sections
  - **Prices directly** in the third section
- For return-based predictions:
  - Returns are converted back into prices using:
    ```
    predicted_price = current_price * (1 + predicted_return)
    ```
- Graphs are plotted to visualize:
  - Actual vs Predicted prices over multiple future days
  - Prediction trends and deviations

---

**5. Buy / Sell / Hold Signal Generation**

- Signals are generated based on the **direction and magnitude** of the predicted price movement:
  - If `predicted_price > current_price + threshold`: **Buy**
  - If `predicted_price < current_price - threshold`: **Sell**
  - Else: **Hold**
- This helps simulate a basic decision-making mechanism based on predictions.
- Threshold is tunable and used to avoid false signals due to minor fluctuations.

---

**How to Run**

1. Open the notebook `stock_data_stcking_model_with_graph.ipynb` in Jupyter or Google Colab.
2. Run all cells **in order** — each section builds on the previous one.
3. Ensure the required Python packages are installed:
   ```bash
   pip install pandas numpy scikit-learn matplotlib joblib
