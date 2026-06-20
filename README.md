# 💻 Laptop Price Predictor

A machine learning web application that estimates the market price of a laptop (in euros) from its specifications. A **Random Forest** regression model is trained on the public *Laptop Price* dataset and served through a small **Flask** web app with a clean, responsive form.

---

## ✨ Features

- Predicts laptop price from 9 specs: RAM, weight, touchscreen, IPS display, brand, type, OS, CPU and GPU.
- Trained `RandomForestRegressor` (tuned with `GridSearchCV`) saved as a pickle for instant inference.
- Lightweight Flask app — no database, no build step.
- Modern, mobile-friendly UI with a live result panel.

---

## 📂 Project Structure

```
Laptop_price predict/
├── model/
│   ├── Model building.ipynb     # Data cleaning, training & model selection
│   └── prdictor.pickle          # Trained Random Forest model
├── website/
│   ├── app.py                   # Flask application
│   ├── model/
│   │   └── prdictor.pickle      # Model copy loaded by the app
│   └── Templates/
│       ├── index.html           # Form + result UI (styles inlined)
│       └── static/
│           └── style.css
├── requirements.txt
├── .gitignore
└── README.md
```

> Note: Flask loads the model from `website/model/prdictor.pickle`. If you retrain, copy the new pickle there.

---

## 📊 Dataset

The model is trained on the **Laptop Price** dataset (1,300+ laptops), loaded directly in the notebook from:

```
https://raw.githubusercontent.com/dineshpiyasamara/LaptopPricePredictor/master/model%20building/laptop_price.csv
```

The target variable is **`Price_euros`**.

---

## 🧠 Model Pipeline

The notebook (`model/Model building.ipynb`) performs:

1. **Cleaning** — strip units from `Ram` (`"8GB" → 8`) and `Weight` (`"1.37kg" → 1.37`).
2. **Feature engineering**
   - `Touchscreen` and `IPS` flags derived from `ScreenResolution`.
   - `cpu_name` reduced to: Intel Core i3 / i5 / i7, AMD, other.
   - `gpu_name` reduced to its brand: Intel, AMD, Nvidia.
   - `Company` rare brands grouped into `Other`; `OpSys` grouped into Windows / Mac / Linux / Other.
3. **Encoding** — one-hot encoding (`pd.get_dummies`) of all categorical columns.
4. **Model selection** — Linear Regression, Lasso, Decision Tree and Random Forest compared; Random Forest tuned with `GridSearchCV` and selected as the best estimator.
5. **Export** — the best model is pickled to `prdictor.pickle`.

### Input features (31 total, in model order)

| Group | Columns |
|-------|---------|
| Numeric | `Ram`, `Weight`, `Touchscreen`, `IPS` |
| Company | Acer, Apple, Asus, Dell, HP, Lenovo, MSI, Other, Toshiba |
| Type | 2-in-1 Convertible, Gaming, Netbook, Notebook, Ultrabook, Workstation |
| OS | Linux, Mac, Other, Windows |
| CPU | AMD, Intel Core i3, i5, i7, other |
| GPU | AMD, Intel, Nvidia |

The Flask app builds the feature vector in exactly this order before calling `model.predict`.

---

## 🚀 Getting Started

### 1. Clone and enter the project

```bash
cd "Laptop_price predict"
```

### 2. Create a virtual environment and install dependencies

```bash
python -m venv env
# Windows
env\Scripts\activate
# macOS / Linux
source env/bin/activate

pip install -r requirements.txt
```

### 3. Run the web app

```bash
cd website
python app.py
```

Open **http://127.0.0.1:5001** in your browser, enter the laptop specs, and click **Predict Price**.

### (Optional) Retrain the model

Open `model/Model building.ipynb` in Jupyter, run all cells to regenerate `prdictor.pickle`, then copy it to `website/model/`.

---

## 🛠️ Tech Stack

- **Python** · pandas, NumPy
- **scikit-learn** — Random Forest regression + GridSearchCV
- **Flask** — web server and templating (Jinja2)
- **HTML / CSS** — responsive single-page form

---

## ⚠️ Notes & Limitations

- Predictions are in **euros**, matching the training data; treat them as ballpark estimates.
- The model reflects the laptops in the training set; very new or unusual configurations may be less accurate.
- The pickle was created with a specific scikit-learn version. If you see an `InconsistentVersionWarning`, retrain the model with your installed version for best results.

---

## 📄 License

Provided for educational purposes. Dataset belongs to its original authors.
