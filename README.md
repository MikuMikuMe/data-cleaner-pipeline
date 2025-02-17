# data-cleaner-pipeline

Creating a complete Python program for a data-cleaning pipeline involves several steps. This pipeline will handle data cleaning tasks such as handling missing values, removing duplicates, standardizing formats, and normalizing/transforming data. Additionally, we'll include error handling and comments to explain the process.

Here's a simple implementation:

```python
import pandas as pd
from sklearn.preprocessing import StandardScaler

# Dummy data loading function
def load_data(file_path):
    """
    Load data from a CSV file into a DataFrame.
    """
    try:
        data = pd.read_csv(file_path)
        print("Data loaded successfully.")
        return data
    except FileNotFoundError as e:
        print(f"Error: {e}")
        return None
    except pd.errors.EmptyDataError as e:
        print(f"Error: No data - {e}")
        return None
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        return None

def handle_missing_values(df, strategy='mean'):
    """
    Handle missing values in the DataFrame.
    
    :param df: pandas DataFrame
    :param strategy: Strategy for filling missing values ('mean', 'median', 'mode', or 'drop')
    """
    try:
        if strategy == 'mean':
            return df.fillna(df.mean())
        elif strategy == 'median':
            return df.fillna(df.median())
        elif strategy == 'mode':
            return df.fillna(df.mode().iloc[0])
        elif strategy == 'drop':
            return df.dropna()
        else:
            print(f"Unknown strategy '{strategy}' specified. Returning original DataFrame.")
            return df
    except Exception as e:
        print(f"Error handling missing values: {e}")
        return df

def remove_duplicates(df):
    """
    Remove duplicate rows from the DataFrame.
    """
    try:
        return df.drop_duplicates()
    except Exception as e:
        print(f"Error removing duplicates: {e}")
        return df

def standardize_columns(df):
    """
    Standardize column names to lowercase with underscores.
    """
    try:
        df.columns = [col.lower().replace(' ', '_') for col in df.columns]
        return df
    except Exception as e:
        print(f"Error standardizing column names: {e}")
        return df

def scale_numeric_data(df):
    """
    Scale numeric columns to have a mean of 0 and standard deviation of 1.
    """
    try:
        numeric_cols = df.select_dtypes(include=['number']).columns
        scaler = StandardScaler()
        df[numeric_cols] = scaler.fit_transform(df[numeric_cols])
        return df
    except Exception as e:
        print(f"Error scaling numeric data: {e}")
        return df

def main(file_path):
    # Load data
    df = load_data(file_path)
    if df is None:
        return
    
    # Print initial data info
    print("Initial data information:")
    print(df.info())

    # Data cleaning steps
    df = handle_missing_values(df, strategy='mean')
    df = remove_duplicates(df)
    df = standardize_columns(df)
    df = scale_numeric_data(df)

    # Print final data info
    print("\nCleaned data information:")
    print(df.info())

    # Save cleaned data
    try:
        output_path = 'cleaned_data.csv'
        df.to_csv(output_path, index=False)
        print(f"Cleaned data saved to {output_path}")
    except Exception as e:
        print(f"Error saving cleaned data: {e}")

# Run the main function
if __name__ == "__main__":
    # Replace 'your_dataset.csv' with your actual dataset path.
    main('your_dataset.csv')
```

### Explanation:

1. **Data Loading**: The program starts by attempting to load a CSV file into a Pandas DataFrame. It handles errors like file not found or empty data.

2. **Handling Missing Values**: Missing values can be filled using several strategies: mean, median, mode, or simply dropping them.

3. **Removing Duplicates**: Duplicate rows are removed to ensure each entry is unique.

4. **Standardizing Column Names**: Column names are formatted to lowercase, with spaces replaced by underscores for consistency.

5. **Scaling Numeric Data**: Numeric columns are scaled using `StandardScaler` to standardize the data.

6. **Error Handling**: Each function includes a try-except block to catch and print errors that may occur during the processing steps.

7. **Running the Program**: The `main` function orchestrates the pipeline, loading the data, cleaning it, and saving the cleaned version.

This template can be expanded and customized based on specific data cleaning needs.