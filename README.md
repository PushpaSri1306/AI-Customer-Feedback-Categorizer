# 🚀 Production-Grade AI Customer Feedback Parser

An automated data pipeline that transforms unstructured, messy customer reviews into deterministic, categorized JSON schemas using the **Google Gemini API**, **Pydantic**, and **Pandas**.

---

## 📌 The Real-World Problem
E-commerce and food delivery platforms receive thousands of unstructured reviews daily. 
* **Human limitations:** A manual support team cannot read and sort 10,000+ reviews a day.
* **LLM limitations:** Standard LLMs (like ChatGPT) reply in conversational, long-winded paragraphs (e.g., *"Sure! Here is my analysis..."*). Relational databases and backend automated systems cannot parse or query unstructured conversational text directly.

---

## 💡 The Solution
This project uses **Structured Outputs** with strict **Pydantic schemas** to force the AI model to behave like an automated database clerk. 
* Enforces zero-hallucination data validation using `Literal` enums.
* Sets `temperature=0.0` for strict, deterministic, and repeatable results.
* Converts raw AI output into a tabular **Pandas DataFrame** that exports directly to CSV, Excel, or SQL databases.

[ Messy Customer Review ]│▼[ Python Data Pipeline ]  ── (Sends text + Pydantic Schema) ──►  [ Google Gemini API ]│▼[ Clean Tabular CSV / SQL ]  ◄── (Returns structured JSON data) ───────┘

---

## 🛠️ Tech Stack & Key Concepts

| Tool / Parameter | Role & Purpose |
| :--- | :--- |
| **Python 3.10+** | Core programming language for the data pipeline. |
| **`google-genai` SDK** | Google's official client library for interacting with Gemini models. |
| **`gemini-2.5-flash`** | Lightweight, high-speed model optimized for structured data extraction. |
| **`Pydantic` Schema** | Enforces strict type boundaries (`BaseModel`, `Field`) to guarantee specific JSON output structures. |
| **`Literal` Enums** | Restricts allowable choices for key attributes (e.g., Urgency: `Low`, `Medium`, `High`). |
| **`temperature=0.0`** | Removes random sampling, forcing consistent, deterministic outputs across edge cases. |
| **`Pandas`** | Converts raw dictionaries into structured dataframes and exports to CSV formats. |

---

## 📋 Target Data Schema (`ReviewAnalysis`)

The pipeline forces Gemini to map every raw input to this exact structure:

```python
class ReviewAnalysis(BaseModel):
    category: Literal["Delivery Issue", "Food Quality", "Payment/Billing", "App Bug", "Positive Experience", "Other"]
    sentiment: Literal["Positive", "Neutral", "Negative"]
    urgency_level: Literal["Low", "Medium", "High"]
    key_issue_summary: str
    suggested_action: str
```

---
## Install required dependencies:

```bash
pip install google-genai pydantic pandas
Configure API Key:
```
Open main.py and insert your API key:

```Python
API_KEY = "YOUR_GEMINI_API_KEY_HERE"
```
Run the script:

```Bash
python main.py
```
---

## 📊 Sample Output
Running the script produces a structured CSV file (customer_feedback_analysis.csv):


| Original_Review | category | sentiment | urgency_level | key_issue_summary | suggested_action |
| :--- | :--- | :--- | :--- | :--- | :--- |
| I ordered biryani 90 minutes ago... | Delivery Issue | Negative | High | Order delayed by 90 minutes with unreachable rider. | Contact rider immediately or issue full refund. |
| My money got debited twice... | Payment/Billing | Negative | High | Duplicate deduction of ₹650 on failed order. | Verify gateway log and initiate ₹650 refund. |
| The packaging was neat... | Positive Experience | Positive | Low | Customer praised hot food and packaging quality. | Send thank you note or loyalty points. |
| The app crashed three times... | App Bug | Negative | Medium | App crashes when applying checkout coupon. | Escalate checkout coupon bug to mobile tech team. |

Developed as an applied AI project demonstrating structured data pipelines with LLMs.





