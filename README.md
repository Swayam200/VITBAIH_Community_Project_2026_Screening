# BankMind Challenge
<img width="485" height="205" alt="vitbaih-logo" src="https://github.com/user-attachments/assets/b0b1471f-3625-4b8e-bbde-afef461cb605" />

**VITB AI Innovators Hub - Community Project Screening Task**

> Build something. Understand what you built. Ship it.

---

## What's this about?

We're building an **AI-Driven Cross-Sell Recommendation System** for banks - essentially, a smart tool that tells Relationship Managers which financial product to pitch to which customer, and why. Think credit cards, loans, insurance - all matched to the right person at the right time using real customer data and ML.

It's a real project. Multi-agent architecture, ML models, an LLM explanation layer, a live dashboard - the works. If you get selected, you're actually building pieces of this.

Before that happens, we need to see *you* build something. That's what this task is.

**One important thing upfront**: we don't care if you use AI tools. ChatGPT, Claude, Copilot - go for it. What we *do* care about is whether you understood what ended up in your repo. There's a section at the end of this README called `EXPLANATION.md` that will make that immediately obvious. Fair warning.

---

## The dataset

We're using the **UCI Bank Marketing Dataset** - data from a real Portuguese bank's customer outreach campaign. It has ~45,000 customer records with demographic info (age, job, education), financial info (account balance, existing loans), and most importantly, a column that tells you whether that customer actually subscribed to a new product.

That last column is literally what we're trying to predict in our actual project. So you're not working on some contrived toy problem - you're working on a version of the real thing.

📥 Download: [UCI Bank Marketing Dataset](https://archive.ics.uci.edu/dataset/222/bank+marketing) - use `bank-full.csv`

Key columns worth knowing:
- `age`, `job`, `marital`, `education` - who the customer is
- `balance` - average yearly account balance (€)
- `housing`, `loan` - what products they already have
- `y` - did they subscribe? (`yes` / `no`) - this is your target variable

---

## Pick a track

There are three tracks. Pick **one** based on what you actually want to do. You're evaluated only against your track - nobody's getting penalized for not doing all three. Do one track properly rather than three tracks badly.

---

### Track A - Data Analyst

*Good fit if: you know Python basics and want to work on insights, dashboards, and making data tell a story.*

Here's the task: explore the dataset and build a Streamlit dashboard that surfaces insights a bank RM would actually find useful.

Specifically:
1. Load the data, check what you're working with - shape, dtypes, missing values, class distribution
2. Answer these four business questions with plots:
   - Which job types have the highest subscription rate?
   - Is there a pattern between account balance and likelihood to subscribe?
   - How does subscription rate differ across age groups? (bin into 18-30, 31-45, 46-60, 60+)
   - Does having an existing housing loan make someone less likely to take a new product?
3. Build a Streamlit dashboard showing these insights - make it interactive if you can (dropdowns, filters, sliders)
4. Write your `EXPLANATION.md` (more on that below)

**Submit:** your script/notebook + Streamlit app + EXPLANATION.md

---

### Track B - ML Engineer

*Good fit if: you want to get your hands on building and evaluating a real ML model.*

Here's the task: build a model that predicts which customers are likely to subscribe, actually evaluate it properly, and interpret what it learned.

1. Do a focused EDA - check the class distribution, encode your categoricals, handle missing values. You don't need 15 plots. Just enough to understand what you're feeding into the model.

2. Train and compare two models:
   - Baseline: Logistic Regression
   - Main model: your choice - Random Forest, XGBoost, CatBoost, LightGBM, whatever you want to try

3. Evaluate both properly. Report accuracy, precision, recall, and F1. Print a `classification_report`. Don't just look at accuracy.

4. Pick 5 customers from your test set (at least 2 predicted "yes", 2 predicted "no") and print a readable summary showing their features + the model's prediction + probability score

5. Write your `EXPLANATION.md`

**Submit:** notebook or script + saved model file (`.pkl`) + EXPLANATION.md

---

### Track C - System Builder

*Good fit if: you want to build the backend API layer, not just the model.*

Here's the task: train a model and wrap it in a FastAPI service that other applications (like a dashboard) can call.

1. First, complete Track B steps 1–3. You need a working model before you can serve it.

2. Build a FastAPI app with these two endpoints:

   **`POST /predict`**
   
   Takes customer data as JSON. Returns something like:
   ```json
   {
     "will_subscribe": true,
     "probability": 0.78,
     "top_factors": ["high balance", "no existing loans"]
   }
   ```

   **`GET /health`**
   
   Returns `{ "status": "ok", "model": "your-model-name" }`. Basic sanity check.

3. Make sure FastAPI's auto-generated docs work. Go to `/docs` - it should show your endpoints. If it doesn't, fix it.

4. **Bonus (genuinely worth doing):** Add a `POST /explain` endpoint that calls the Groq API and returns a plain-English explanation of the prediction. It's free, it's fast, and it's literally one of the core features of our actual project.

   Sign up at [console.groq.com](https://console.groq.com) and use `llama-3.3-70b-versatile` or `meta-llama/llama-4-scout-17b-16e-instruct` - both are on the free tier. Here's a prompt template to start from:

   ```
   Customer profile:
   - Age: {age}, Job: {job}, Balance: {balance}
   - Existing loans: Housing={housing}, Personal={loan}
   - Model prediction: {probability}% chance of subscribing

   In 2-3 sentences, explain why this customer would or would not likely
   subscribe to a term deposit, and how an RM should approach the conversation.
   ```

5. Write your `EXPLANATION.md`

**Submit:** FastAPI app + model file + a README with curl examples showing how to hit your endpoints + EXPLANATION.md

---

## EXPLANATION.md - this is the important bit

Every submission must have an `EXPLANATION.md` that answers the questions below for your track. Be honest and concise - 2-4 sentences per answer is enough. We're not grading on word count.

Why does this exist? Because it's the one part that's hard to fake. Code can be copy-pasted. These answers can only be written correctly if you actually ran the code and looked at the output.

**Everyone answers these:**
1. What percentage of customers in your dataset have `y = yes`? What does this imbalance mean for how you'd evaluate a model?
2. Which job category had the highest subscription rate? Does this make sense to you intuitively?

**Track B adds:**

3. Which feature had the highest importance in your tree-based model? Why do you think that is?
4. Why is F1 a better metric than accuracy for this particular dataset?
5. Pick one of your 5 sample predictions. Do you actually agree with the model's call, given that customer's features? Walk through your thinking.

**Track C adds (plus all of Track B's questions):**

6. What would likely break first if 200 RMs were hitting your `/predict` endpoint simultaneously? What's one thing you'd change?
7. What does the LLM explanation actually add over just showing a probability score?

---

## How to submit

1. Put everything in a public GitHub repo - name it `bankmind-[yourname]`
2. Your repo should have:
   - All your code
   - `EXPLANATION.md` with every answer filled in
   - `requirements.txt`
   - A brief `README.md` explaining how to run it (we will actually try to run it)
3. Share the link using submission form that will be UPDATED here shortly.

**Deployment bonus:** If you deploy your app anywhere (Streamlit Cloud, Render, HuggingFace Spaces, Vercel - anything) and drop a live link in your README, that goes in your favour. Not required, but it shows you didn't stop at "it works on my machine."

---

## What we're actually looking at

| | What we check |
|---|---|
| **Does it run** | We clone, install requirements, and run it. If it errors out immediately, review stops there. |
| **Correct approach** (40%) | Did you use the right methods? Do your choices make sense? |
| **EXPLANATION.md** (30%) | Did you understand what you built? This is the real filter. |
| **Code quality** (20%) | Is it readable? Do you have comments explaining non-obvious parts? |
| **Bonus / extra effort** (10%) | Deployment, SHAP values, the Groq explain endpoint - anything above the spec |

A Logistic Regression with a sharp EXPLANATION.md will score higher than a CatBoost model with an empty one. We mean that.

---

## Resources

**Getting started**
- [UCI Bank Marketing Dataset](https://archive.ics.uci.edu/dataset/222/bank+marketing)
- [Pandas docs](https://pandas.pydata.org/docs/) - `read_csv`, `groupby`, `value_counts` will get you far
- [Scikit-learn user guide](https://scikit-learn.org/stable/user_guide.html)

**For Track B/C**
- [CatBoost quickstart](https://catboost.ai/docs/en/concepts/python-quickstart) - handles categoricals natively, worth trying
- [SHAP](https://shap.readthedocs.io/en/latest/) - for feature importance visualisation if you want to go deeper

**For Track C**
- [FastAPI official tutorial](https://fastapi.tiangolo.com/tutorial/) - seriously good docs, start here
- [Groq console](https://console.groq.com) - free API key, use `llama-3.3-70b-versatile` or `meta-llama/llama-4-scout-17b-16e-instruct`

**Deployment (optional)**
- [Streamlit Community Cloud](https://streamlit.io/cloud)
- [Vercel free tier](https://vercel.com)
- [HuggingFace Spaces](https://huggingface.co/spaces)

---

Questions? Open a GitHub issue on this repo and we will get back to you ASAP.

Good luck. Show us what you can build.

- *VITB AI Innovators Hub Core Team*
