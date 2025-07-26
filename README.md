## 🔄 Interpolation of Pension Fund Assets & Contributions

One of the most critical steps in this project was **dealing with data sparsity**, especially for the key variables:
- `Pension_Assets` – Total assets held by complementary pension funds.
- `Pension_Contributions` – Annual contributions made to these funds.

### ⚠️ Data Challenges Faced

- **Extremely limited sample size**: Only **11 annual observations (2012–2022)** were available for both variables.
- **Lack of quarterly data**: Most official data sources (e.g., ISSM, INE) only reported **annual aggregates**, despite GDP and other macroeconomic variables being evaluated quarterly.
- **Non-uniform reporting standards**: Discrepancies in units, formats, and classification across years introduced inconsistencies.
- **Statistical modeling constraints**: Classical time-series models (like VAR) require significantly more data points than what was initially available.

### 🧠 Strategy for Overcoming These Issues

To enable robust modeling despite the small sample size, we implemented a **multi-phase interpolation and sampling strategy**:

#### 1. **Quarterly Interpolation**
Annual values for `Pension_Assets` and `Pension_Contributions` were interpolated to quarterly frequency using:

- 📈 **Cubic Spline Interpolation**: Smooth, continuous curves fitted to each variable across the 11-year span. This method respects the curvature and implied seasonal behavior more than linear methods.
  
- 📘 **Denton Method (offline)**: A method often used by national statistics agencies, particularly effective for **benchmarking quarterly series to annual totals** while preserving growth patterns.

#### 2. **Synthetic Expansion for Modeling**
- The interpolated quarterly data increased the number of observations from 11 to **44 usable quarters**.
- This transformed the dataset from **too small for machine learning** to **sufficiently rich for multivariate regression and ensemble models**.

> ⚡ **Without this interpolation**, time-series forecasting using Random Forest or even Linear Regression would risk **overfitting** or **invalid conclusions** due to the underdetermined system.

### 🧪 Quality Control & Validation

- The sum of interpolated quarterly values was **matched back to the known annual totals**, ensuring internal consistency.
- The shape and trend of the interpolated data were cross-validated against macroeconomic expectations (e.g., growth in GDP, inflation shocks, etc.).
- Additional macro variables like **GDP**, **interest rate**, and **inflation** were also adjusted to quarterly frequency to ensure time alignment and comparability.

---

> ✅ **Result**: Interpolation enabled us to preserve the **economic story** within the data while building a high-resolution dataset capable of supporting rigorous modeling.
