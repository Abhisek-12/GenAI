hi
# Mood Drift & Emotional Baseline Tracker

This project is a Python-based analytical engine designed to track personal well-being over time. It compares daily user inputs against a calculated statistical baseline to detect "drifts"—significant deviations in mood, energy, or stress levels.

## 🚀 Key Features

* **Adaptive Baseline (EWMA):** Uses Exponentially Weighted Moving Averages to ensure the baseline evolves as the user’s life changes.
* **Z-Score Analysis:** Calculates the statistical significance of daily changes to identify true anomalies.
* **Drift Classification:** Categorizes daily states into `minimal_drift`, `moderate_drift`, or `severe_drift`.
* **Automated Explanations:** Generates human-readable feedback (e.g., "High Stress Alert") based on the primary driver of the drift.
* **Weekly Summaries:** Tracks a 7-day rolling window to identify trends and highlight the week's biggest improvements or drops.

## 📊 How it Works

1.  **Normalization:** Converts raw inputs (1-10 scales) and categorical emotions (e.g., "Exhausted") into a normalized 0-1 range.
2.  **Comparison:** The system calculates how many standard deviations the current day is from the stored mean using Z-scores.
3.  **Calculation:**
    * **Drift Score:** A combined index (0-100) representing total deviation across all metrics.
    * **Anomaly Flag:** Triggered if the Drift Score exceeds a threshold.
4.  **Learning:** After every entry, the system updates the `user_baseline_stats.csv` using the `EWMA_LAMBDA` (default 0.1) to refine future predictions.



[Image of Z-score normal distribution curve]


## 🛠 Prerequisites

* Python 3.x
* `pandas`
* `numpy`

```bash
pip install pandas numpy

📂 File Structure
mood_drift_engine.py: The main script containing the detection logic.

user_baseline_stats.csv: Stores the running Mean and Variance for your metrics (Required).

weekly_history.json: A rolling 7-day log of your normalized data.

📝 Usage
To run the simulation included in the script:
python mood_drift_engine.py

Input Format
The engine expects a dictionary containing:

energy_level (1-10)

mind_clarity (1-10)

stress_level (1-10)

emotion_state (String from EMOTION_MAP)

timestamp (YYYY-MM-DD)
Example Output

{
    "drift_score": 85,
    "classification": "severe_drift",
    "explanation": "High Stress Alert: Your Stress level is significantly above your normal baseline.",
    "weekly_summary": {
        "avg_drift": 42,
        "trend": "stable",
        "biggest_improvement": "Clarity",
        "biggest_drop": "Stress"
    }
}


⚙️ Configuration
You can tune the engine sensitivity by adjusting these constants at the top of the script:

EWMA_LAMBDA: Higher values (e.g., 0.3) make the baseline adapt faster to new changes; lower values (0.05) make it more "stable."

MAX_RAW_DRIFT: Adjusts how sensitive the 0-100 Drift Score is.
