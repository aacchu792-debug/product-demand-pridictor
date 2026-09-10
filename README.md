#  Product Demand Predictor

A machine-learning powered web application for **predicting product demand, analyzing historical sales patterns, forecasting future demand, and evaluating regression models** — entirely in the browser.

The application combines historical demand data, product information, pricing, promotions, holidays, customer activity, seasonality, and time-series features to generate demand predictions and business-oriented insights.

## \ Features

* **Demand Prediction**

  * Predict expected demand for a selected product and date.
  * Supports Linear Regression and Random Forest Regression.
  * Provides demand classification such as:

    * Very Low Demand
    * Low Demand
    * Average Demand
    * High Demand
    * Very High Demand

* **Multi-Day Forecasting**

  * Generate future demand forecasts for multiple days.
  * Displays predicted demand ranges based on model RMSE.
  * Uses recursive lag and rolling-demand information for future predictions.

* **Machine Learning Models**

  * Linear Regression baseline model.
  * Custom Random Forest Regression model.
  * Automatic model comparison.
  * Model selection based on RMSE and R².

* **Feature Engineering**

  * Date features
  * Day of week
  * Month
  * Quarter
  * Weekend indicator
  * Previous sales
  * Previous demand
  * Lag-1 demand
  * Lag-7 demand
  * Lag-30 demand
  * Rolling 7-observation demand
  * Rolling 30-observation demand
  * Price
  * Stock availability
  * Customer count
  * Promotion indicator
  * Holiday indicator
  * Product category encoding
  * Seasonal encoding
  * Stock-to-demand ratio

* **Demand Analytics**

  * Product-level demand summaries.
  * Category demand analysis.
  * Monthly demand trends.
  * Seasonal demand patterns.
  * Promotion impact analysis.
  * Price vs demand analysis.
  * Stock vs demand analysis.
  * Customer activity vs demand analysis.

* **Model Performance Dashboard**

  * MAE
  * MSE
  * RMSE
  * R²
  * Actual vs predicted demand
  * Residual analysis
  * Feature importance
  * Regression coefficients

* **Dynamic Business Insights**

  * Automatically identifies demand patterns.
  * Highlights promotional demand lift.
  * Identifies high-demand categories.
  * Detects seasonal demand peaks.
  * Reports influential model features.

* **CSV Upload**

  * Upload a custom CSV dataset.
  * Automatically preprocess and retrain the models.

* **PDF Reports**

  * Generate downloadable PDF reports containing:

    * Dataset statistics
    * Prediction results
    * Model comparison
    * Analytical insights
    * Visualizations

* **Client-Side Machine Learning**

  * No dedicated ML backend is required.
  * Data preprocessing and model training happen directly in the browser.

---

##  Machine Learning Pipeline

The application follows a complete machine-learning workflow:

```text
CSV Dataset
     │
     ▼
Data Loading
     │
     ▼
Data Cleaning & Validation
     │
     ▼
Missing Value Imputation
     │
     ▼
Chronological Sorting
     │
     ▼
Feature Engineering
     │
     ├── Date Features
     ├── Lag Features
     ├── Rolling Features
     ├── Business Features
     └── Categorical Encoding
     │
     ▼
80/20 Chronological Split
     │
     ├───────────────┐
     ▼               ▼
Linear Regression   Random Forest
     │               │
     └───────┬───────┘
             ▼
       Model Evaluation
             │
             ▼
     Model Comparison
             │
             ▼
      Demand Prediction
             │
             ▼
     Forecast & Insights
```

### Train/Test Strategy

Instead of randomly shuffling observations, the application uses an **80/20 chronological split**.

```text
Historical Data
──────────────────────────────────────────
         80%                    20%
       Training                 Testing
──────────────────────────────────────────►
             Time →
```

This approach is more appropriate for demand forecasting because future observations should not be used to train the model.

---

##  Feature Engineering

The application derives additional features from the raw demand dataset.

### Time Features

* Year
* Month
* Day
* Day of week
* Week of year
* Quarter
* Weekend indicator

### Historical Demand Features

* Previous demand
* Lag-1 demand
* Lag-7 demand
* Lag-30 demand
* Rolling 7-observation demand
* Rolling 30-observation demand

Lag and rolling features are calculated using **strictly previous observations**, helping prevent target leakage.

### Business Features

* Price
* Previous sales
* Stock availability
* Customers
* Promotion
* Holiday
* Stock-to-demand ratio
* Product category
* Season

Categorical variables are converted into numerical features using one-hot encoding.

---

##  Models

### 1. Linear Regression

Linear Regression acts as the baseline model and learns the relationship between demand and the engineered features.

It is useful for:

* Establishing a simple baseline.
* Understanding linear relationships.
* Inspecting feature coefficients.

### 2. Random Forest Regression

The application also implements a custom Random Forest regression engine.

The model uses multiple decision trees and combines their predictions to capture nonlinear relationships between demand and variables such as:

* Promotions
* Price
* Customers
* Seasonality
* Historical demand
* Stock availability

The application compares both models using their test-set performance.

### Model Selection

The model with the lower RMSE is selected as the preferred model:

```text
Lower RMSE → Better predictive performance
Higher R²   → Better explained variance
```

---

## 📈 Evaluation Metrics

The Model Performance section reports:

| Metric | Description                        |
| ------ | ---------------------------------- |
| MAE    | Average absolute prediction error  |
| MSE    | Average squared prediction error   |
| RMSE   | Root mean squared prediction error |
| R²     | Explained variance of the model    |

The application also provides:

* Actual vs predicted values
* Residual distribution
* Maximum positive error
* Maximum negative error
* Median absolute error
* Feature importance
* Linear regression coefficients

---

## Forecasting

The forecasting engine can generate multi-day demand predictions.

For each forecasted date, the application calculates:

* Date features
* Estimated customers
* Promotion effect
* Holiday effect
* Weekend effect
* Historical demand context
* Rolling demand
* Stock-to-demand ratio

The forecast then recursively feeds predicted demand back into subsequent predictions to simulate autoregressive demand behavior.

Each prediction includes an estimated range based on the selected model's RMSE.

---

##  Project Structure

```text
product-demand-predictor/
│
├── public/
│   └── data/
│       └── product_demand.csv
│
├── scripts/
│   └── generate_data.js
│
├── src/
│   ├── components/
│   │   ├── ActualVsPredictedChart.tsx
│   │   ├── ActualVsPredictedChart.tsx
│   │   ├── ChartCard.tsx
│   │   ├── DemandDetails.tsx
│   │   ├── DemandForecastChart.tsx
│   │   ├── DemandPredictionForm.tsx
│   │   ├── DemandTable.tsx
│   │   ├── FeatureImportanceChart.tsx
│   │   ├── ForecastTable.tsx
│   │   ├── Header.tsx
│   │   ├── ModelMetrics.tsx
│   │   ├── PredictionResult.tsx
│   │   ├── ReportButton.tsx
│   │   ├── ResidualChart.tsx
│   │   ├── Sidebar.tsx
│   │   └── StatCard.tsx
│   │
│   ├── ml/
│   │   ├── dateFeatures.ts
│   │   ├── encoding.ts
│   │   ├── lagFeatures.ts
│   │   ├── linearRegression.ts
│   │   ├── modelTraining.ts
│   │   ├── preprocessing.ts
│   │   ├── randomForest.ts
│   │   └── rollingFeatures.ts
│   │
│   ├── pages/
│   │   ├── Dashboard.tsx
│   │   ├── DemandAnalysis.tsx
│   │   ├── DemandPredictor.tsx
│   │   └── ModelPerformance.tsx
│   │
│   ├── services/
│   │   └── dataset.ts
│   │
│   ├── utils/
│   │   ├── demandAnalysis.ts
│   │   ├── forecastUtils.ts
│   │   ├── insights.ts
│   │   ├── metrics.ts
│   │   ├── predictionUtils.ts
│   │   └── reportGenerator.ts
│   │
│   ├── App.tsx
│   └── types.ts
│
├── .env.example
├── .gitignore
├── index.html
├── package.json
└── README.md
```

---

## Tech Stack

### Frontend

* React
* TypeScript
* Vite
* Tailwind CSS
* Lucide React

### Data & Visualization

* PapaParse
* Recharts

### Machine Learning

* Custom Linear Regression implementation
* Custom Random Forest Regression implementation
* Client-side feature engineering and model evaluation

### Reporting

* jsPDF
* html2canvas

---

##  Installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/product-demand-predictor.git
cd product-demand-predictor
```

### 2. Install dependencies

```bash
npm install
```

### 3. Start the development server

```bash
npm run dev
```

The application will be available at:

```text
http://localhost:3000
```

### 4. Build for production

```bash
npm run build
```

### 5. Preview the production build

```bash
npm run preview
```

### 6. Type-check the project

```bash
npm run lint
```

---

##  Dataset

The project includes a generated product-demand dataset located at:

```text
public/data/product_demand.csv
```

The dataset contains variables such as:

```text
demand_id
date
product_id
product_category
price
previous_sales
stock_available
promotion
holiday
customers
season
previous_demand
demand
```

The repository also includes a data-generation script:

```bash
node scripts/generate_data.js
```

The generated data represents multiple product categories including:

* Electronics
* Home & Living
* Apparel
* Sports & Fitness
* Groceries

---

##  Using a Custom Dataset

The application supports uploading your own CSV file through the interface.

The uploaded dataset should contain the required demand-related fields:

```text
demand_id
date
product_id
product_category
price
previous_sales
stock_available
promotion
holiday
customers
season
previous_demand
demand
```

After upload, the application automatically:

1. Parses the CSV.
2. Validates and cleans the records.
3. Handles missing numerical values.
4. Sorts observations chronologically.
5. Generates engineered features.
6. Trains both ML models.
7. Evaluates model performance.
8. Generates new analytical insights.

---

##  Application Sections

### Dashboard

Provides a high-level overview of:

* Total records
* Products
* Categories
* Average demand
* Average price
* Average stock
* Demand trends
* Key insights

### Demand Predictor

Allows users to enter product and business conditions and generate a demand prediction.

### Demand Analysis

Explores historical relationships between:

* Products
* Categories
* Price
* Stock
* Customers
* Promotions
* Seasons
* Demand

### Model Performance

Provides detailed evaluation of the Linear Regression and Random Forest models.

---

##  PDF Reporting

Users can generate a PDF report containing:

* Historical dataset summary
* Prediction scenario
* Model comparison
* MAE, MSE, RMSE and R²
* Key analytical insights
* Forecast visualizations

This makes the application useful not only for experimentation but also for presenting model results to stakeholders.

---

##  Privacy & Architecture

The application is designed around a client-side architecture.

```text
User
 │
 ▼
React Application
 │
 ├── CSV Parsing
 ├── Data Processing
 ├── Feature Engineering
 ├── Model Training
 ├── Prediction
 ├── Visualization
 └── Report Generation
```

There is no required external ML API or dedicated backend for the prediction pipeline.

This means uploaded datasets can be processed directly within the browser.

---

##  Limitations

This project is intended primarily as an **educational and demonstration machine-learning application**.

Important limitations include:

* The included dataset is generated/simulated data rather than a production retail dataset.
* Forecast accuracy depends heavily on the quality and size of the supplied dataset.
* The Random Forest implementation is custom rather than based on a dedicated ML library.
* Forecast intervals are approximated from test-set RMSE and should not be interpreted as statistically guaranteed prediction intervals.
* Recursive multi-day forecasting can accumulate prediction errors over time.
* The model should be retrained and validated using real historical business data before production use.

---

##  Future Improvements

Potential improvements include:

* [ ] Add XGBoost/Gradient Boosting
* [ ] Add dedicated time-series models such as ARIMA/Prophet
* [ ] Add hyperparameter optimization
* [ ] Add cross-validation
* [ ] Add automated feature selection
* [ ] Add model persistence
* [ ] Add real-time inventory recommendations
* [ ] Add reorder-point calculations
* [ ] Add safety-stock recommendations
* [ ] Add product-level forecasting
* [ ] Add confidence/prediction intervals with statistically rigorous methods
* [ ] Add authentication and multi-user dashboards
* [ ] Connect to a production database
* [ ] Add cloud deployment
* [ ] Add automated model monitoring

---

##  Use Cases

This project can be used as a foundation for:

* Retail demand forecasting
* Inventory planning
* Promotional analysis
* Product performance analysis
* Sales forecasting
* Machine-learning demonstrations
* Data science portfolios
* Business analytics dashboards
* ML model comparison

---

##  Project Goal

The goal of this project is to demonstrate how a complete machine-learning workflow can be integrated into a modern web application:

**Data → Cleaning → Feature Engineering → Model Training → Evaluation → Prediction → Forecasting → Business Insights**

Rather than treating machine learning as a standalone notebook, this project demonstrates how predictive models can be integrated into an interactive analytics dashboard.

---

##  Disclaimer

Predictions generated by this application are machine-learning estimates based on historical data. They are not guaranteed future demand values and should not be treated as professional inventory, financial, or business advice.

---


