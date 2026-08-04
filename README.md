import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# File path
file_path = r"C:\Users\Student\Downloads\FAOSTAT_data_en_8-4-2026 (2).csv"

# Read CSV
df = pd.read_csv(file_path)

# Keep only Yield data
df = df[df["Element"] == "Yield"]

# Convert columns to numeric
df["Year"] = pd.to_numeric(df["Year"], errors="coerce")
df["Value"] = pd.to_numeric(df["Value"], errors="coerce")

# Remove missing values
df = df.dropna(subset=["Year", "Value"])

# Plot for each crop
for item in df["Item"].unique():

    crop = df[df["Item"] == item].sort_values("Year")

    x = crop["Year"].values
    y = crop["Value"].values

    # Linear regression
    slope, intercept = np.polyfit(x, y, 1)

    # Predicted values
    y_fit = slope * x + intercept

    # Equation text
    equation = f"y = {slope:.2f}x + {intercept:.2f}"

    # Plot
    plt.figure(figsize=(8,5))
    plt.scatter(x, y, color="blue", label="Observed Yield")
    plt.plot(x, y_fit, color="red", linewidth=2,
             label=f"Trendline\n{equation}")

    plt.title(f"{item}: Year vs Yield")
    plt.xlabel("Year")
    plt.ylabel("Yield (kg/ha)")
    plt.grid(True)
    plt.legend()

    # Display slope and equation on graph
    plt.text(
        0.05, 0.95,
        f"Slope = {slope:.2f}\n{equation}",
        transform=plt.gca().transAxes,
        fontsize=10,
        verticalalignment="top",
        bbox=dict(facecolor="white", alpha=0.8)
    )

    plt.tight_layout()
    plt.show()

    print(f"{item}")
    print(f"Slope = {slope:.4f}")
    print(f"Equation: {equation}")
    print("-" * 40)
[FAOSTAT_data_en_8-4-2026 (2).csv](https://github.com/user-attachments/files/30689127/FAOSTAT_data_en_8-4-2026.2.csv)
