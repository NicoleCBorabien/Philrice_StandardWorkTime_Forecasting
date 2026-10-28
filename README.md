# PhilRice Standard Work Time Forecast App — ARIMA / Holt only

Extract this ZIP. With Python 3.12 or 3.13 installed, double-click RUN_APP.bat. First run installs dependencies; subsequent runs open the local app. Build instructions and GitHub Actions workflow are retained for Windows EXE generation.

## Methods
Only nonseasonal ARIMA and Holt’s linear trend (undamped) are eligible. ARIMA includes the original 12 low-order specifications plus autoregressive orders 3–6 for d=0 and d=1, and drift variants for d=1. All candidates use identical validation weeks. Extra AR lags allow recurring weekly patterns to be learned within ARIMA; they do not guarantee oscillating forecasts. ARIMA stationarity and invertibility are enforced and nonconverged fits are rejected. No seasonal, AutoReg, regression, or constant fallback methods are used.

Validation uses the last ceil(20% of rows), fitting on earlier rows and predicting one week at a time, then adding the actual observation. Lowest raw RMSE wins; refit on full history. A failed refit is excluded rather than silently replaced with a last-value forecast. Future negative forecasts are floored at zero; validation predictions are unmodified.

## Raw RMSE
RMSE = sqrt(mean((actual-prediction)^2)), on the internally stored fractional input scale. 90% is 0.90; RMSE 0.05 corresponds to 5 percentage points. Values are not multiplied by 100 or normalized. Excel cells are ordinary numbers with 10 displayed decimal places and full underlying precision.

## Input and horizon
Employee, Week Start, % Standard Work Time; one observation per employee per week; at least 12 consecutive weeks. Original validation and percentage parser retained. Excel percentage cells are recommended. Missing rows are not replaced with zeros. Values above 100% remain allowed.

Months to forecast: 1–24, default 12; output remains weekly. Supplied sample has 52 weeks per employee ending 23 June 2026. Included results use 12 months, generating 52 forecasts per employee from 30 June 2026 through 22 June 2027. Initial training: 41 weeks; rolling validation: 11 weeks.

## Included output
Results workbook contains Overview, individual weekly forecast and validation sheets, and charts. Methodology and Candidate RMSE sheets have been removed. Input is synthetic dummy data; results illustrate app behavior.

Validation performed using the actual app worker on Linux. Windows EXE compilation/launch was not performed in this environment; this is the updated source/build package.

## Interpretation
A forecast estimates the expected future value; it does not replay unpredictable variation. ARIMA may produce flat or mean-reverting forecasts, while Holt extrapolates a straight trend. No noise is added to create visual fluctuations. Validation predictions use only earlier observations; RMSE describes predictive error and does not promise a close visual match. Model selection on these same validation weeks means RMSE is a tuning score, not an independent final test estimate.
