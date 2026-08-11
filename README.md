import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
df = pd.read_csv("FAOSTAT_data_en_8-11-2026 (1).csv")
print(df.head())
print(df.columns)
df = df[["Item", "Year", "Value"]]
df["Year"] = pd.to_numeric(df["Year"], errors="coerce")
df["Value"] = pd.to_numeric(df["Value"], errors="coerce")
df = df.dropna()

print(df)
print(df["Item"].unique())
future_years = pd.DataFrame({
    "Year": range(2025, 2031)
})

future_predictions = []

for crop in crops:

    model = models[crop]

    predictions = model.predict(future_years[["Year"]])

    for year, prediction in zip(future_years["Year"], predictions):

        future_predictions.append({
            "Crop": crop,
            "Year": year,
            "Predicted_Yield": prediction
        })

future_df = pd.DataFrame(future_predictions)

print(future_df)
for crop in crops:

    crop_data = df[df["Item"] == crop].copy()

    X = crop_data[["Year"]]
    y = crop_data["Value"]

    model = models[crop]
    years = np.linspace(
        crop_data["Year"].min(),
        2030,
        100
    ).reshape(-1, 1)

    predictions = model.predict(years)

    plt.figure(figsize=(10, 6))
    plt.scatter(
        crop_data["Year"],
        crop_data["Value"],
        label="Actual Yield"
    )
    plt.plot(
        years,
        predictions,
        label="Linear Regression"
    )

    plt.xlabel("Year")
    plt.ylabel("Yield (kg/ha)")
    plt.title(f"{crop} Yield Prediction")
    plt.legend()
    plt.grid(True)

    plt.show()
    prediction_table = future_df.pivot(
    index="Year",
    columns="Crop",
    values="Predicted_Yield"[FAOSTAT_data_en_8-11-2026 (1).csv](https://github.com/user-attachments/files/30922583/FAOSTAT_data_en_8-11-2026.1.csv)

)

print(prediction_table)
