# 1000models - Extreme Model Parameterization

A demonstration of how parameterization enables scaling from a single data source to thousands of specialized production-grade ML models.

```
┌─────────────────┐       ┌────────────────────────┐      ┌──────────────────────────────┐
│  Data Source    │       │     Feature Store      │      │      Parameterized Models    │
│                 │       │                        │      │                              │
│  demand_df =    │       │ fs.get_or_create_      │      │  # For each item,location:   │
│  pd.read_csv(   │──────▶│ feature_group(         │─────▶│  models = {                  │
│  'demand.csv')  │       │   name='demand',       │      │    'RandomForest': rf_model, │
│                 │       │   primary_key=['item', │      │    'XGBoost': xgb_model      │
└─────────────────┘       │   'location'])         │      │  }                           │
                          └────────────────────────┘      │  # Select best               │
                                                          │  best_model = min(rmse)      │
                                                          └────────────────┬─────────────┘
                                                                           │
                                                                           ▼
┌─────────────────┐       ┌────────────────────────┐      ┌──────────────────────────────┐
│  Production     │       │   Model Selection      │      │     Model Registry           │
│  Predictions    │       │                        │      │                              │
│                 │       │  model = mr.get_best_  │      │  model_api.save(             │
│  prediction =   │◀──────│  model(                │◀─────│    name=f"model_item{item}_  │
│  model.predict( │       │    name=f"model_item   │      │    loc{location}",           │
│  time_bucket)   │       │    {item}_loc{loc}")   │      │    metrics=metrics)          │
└─────────────────┘       └────────────────────────┘      └──────────────────────────────┘
```

## Parameterization Strategy

1000models demonstrates how to use parameterization to:

- Transform a single data source into **thousands of specialized ML models**
- Train a **separate model for each item×location combination**
- Automatically **select the best algorithm** for each scenario
- **Scale effortlessly** from hundreds to thousands of models
- Maintain a **single MLOps workflow** while handling massive complexity

## Interactive Notebooks

The primary way to use this project is through our Jupyter notebooks, which are numbered in sequence:

1. **[Feature Pipeline](notebooks/01_feature_pipeline.ipynb)**: Prepare and upload demand data
2. **[Training Pipeline](notebooks/02_training_pipeline.ipynb)**: Train specialized models by parameter
3. **[Inference Pipeline](notebooks/03_inference_pipeline.ipynb)**: Generate predictions with best model selection

Each notebook is self-contained and includes detailed guidance.

## Quick Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/1000models.git
cd 1000models

# Install dependencies
pip install -r requirements.txt

# Test connection to Hopsworks
python test_connection.py
```

## Parameterization Workflow

The system follows this parameterized approach in sequence:

1. **Feature Preparation**
   - Process data into a single feature store
   - Create a unified view of demand data by item and location

2. **Parameterized Model Training**
   - Automatically generate training subsets for each parameter combination
   - Train competing models (RandomForest and XGBoost) for each parameter set
   - Select best model based on performance metrics
   - Register all models with full parameter metadata

3. **Intelligent Model Selection**
   - Dynamically select the best-performing model for each parameter combination
   - Generate predictions using the optimal model for each use case

## Design Principles

- **Parameter-Driven Architecture**: Structure determined by business parameters
- **Automatic Model Selection**: System chooses best model type for each parameter combo
- **Notebook-First Interface**: Interactive, self-documenting workflow
- **MLOps Integration**: Complete cycle from data to production predictions

## Technical Stack

- **Feature Management**: Hopsworks Feature Store
- **Model Training**: Scikit-learn and XGBoost with automatic selection
- **Model Registry**: Hopsworks Model Registry with parameter metadata
- **Visualization**: Matplotlib (in notebooks)

---

This project demonstrates how extreme parameterization creates specialized models that significantly improve forecasting accuracy while maintaining a clean, unified MLOps workflow.