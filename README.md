# Financial Wealth Planning Tool

A personalized financial planning calculator that analyzes your income, expenses, debts, and goals to suggest wealth-creation strategies.

**Live at:** https://mohanmedicharla.github.io

---

## 📋 What This Tool Does

1. **Analyzes your financial situation** – Income, expenses, debts, insurance, emergency fund
2. **Recommends prioritized strategies** – Insurance → EF → Debt Payoff → SIP → Goals
3. **Calculates timelines** – How long to reach each milestone
4. **Suggests investments** – Based on your investment tenure
5. **Evolves with you** – Add new details as you go

---

## 🎯 Financial Logic (What It Recommends)

### Priority Sequence:
1. **Health & Term Insurance** (if missing) – Get these first
   - Smart recommendations based on family size, income, and dependents
   - Suggests coverage amounts based on your situation
2. **Emergency Fund** – 6, 9, or 12 months of obligations
3. **Debt Payoff** – Highest interest first (special rules for short tenure/low interest loans)
   - Gold Loans treated as regular debts (prioritized by interest rate)
4. **SIP & Investments** – Based on your investment tenure
5. **Long-term Goals** – Land, house, etc.

### Debt Payoff Rules:
- **Tenure > 1 year:** Clear highest interest debt first
- **Tenure ≤ 1 year:** Clear next highest interest debt instead
- **Interest < 9%:** Low interest alert – suggest maintaining EF & investing instead

### Investment Suggestions:
- **Tenure ≥ 8 years:** Equity Mutual Funds recommended
- **Tenure < 8 years:** (Details being added)

---

## 🚀 How to Deploy to GitHub Pages

### Step 1: Log into GitHub
Go to: https://github.com/mohanmedicharla/mohanmedicharla.github.io

### Step 2: Upload the File
1. Click **"Add file"** button (top right)
2. Click **"Upload files"**
3. Drag & drop `financial-calculator.html` OR click "choose your files"
4. Click **"Commit changes"**

### Step 3: Wait & Visit
- GitHub automatically publishes within 1 minute
- Visit: https://mohanmedicharla.github.io/financial-calculator.html

---

## 📝 How to Use the Tool

### Input Section:
1. **Personal Details** 
   - Monthly Income
   - Monthly Obligations (rent, EMIs, utilities, groceries, insurance)
   - Your Age
   - Family Size
   - Number of Dependents
   - Employment Status
2. **Insurance Coverage**
   - Health Insurance (Yes/No + Coverage Amount + Family Floater/Individual)
   - Term Insurance (Yes/No + Sum Assured Amount)
3. **Emergency Fund** 
   - Current balance
   - Target level (6/9/12 months)
4. **Debts**
   - All loans including Gold Loan, Personal, Home, Auto, etc.
   - Amount, interest rate, tenure for each
5. **Current Investments**
   - SIP amount, invested amount in ETFs/MFs
6. **Investment Tenure**
   - How many years can you stay invested?
7. **Goals** (Optional)
   - Land purchase, house, etc.

### Output:
- Personalized insurance recommendations (based on family size, income, dependents)
- Emergency fund progress & timeline
- Debt payoff priority list (including Gold Loan)
- Investment strategy
- Monthly surplus capacity
- Prioritized action plan

---

## 🔧 How to Modify/Update This Tool

### Edit the HTML File:
1. Go to GitHub repo
2. Click on `financial-calculator.html`
3. Click ✏️ (Edit) icon
4. Make changes
5. Click "Commit changes"
6. Wait 1 minute, refresh your browser

### Examples of Easy Changes:

**To change EF target options:**
Find this section (around line 280):
```
<option value="6">Minimum (6 months obligations)</option>
<option value="9">Ideal (9 months obligations)</option>
<option value="12">Safest (12 months obligations)</option>
```

**To change debt priority logic:**
Find the `analyzeDebts()` function (around line 450) and modify the sorting logic.

**To add new investment suggestions:**
Find `generateRecommendations()` function and add recommendations for different tenures.

---

## 📚 File Structure

```
mohanmedicharla.github.io/
├── financial-calculator.html    (Main file - entire app)
├── README.md                     (This file)
└── FINANCIAL_LOGIC.md           (Detailed rules documentation)
```

---

## 🎓 Understanding the Code Structure

### HTML Section (Lines 1-250)
- Form inputs for user data
- Sections for Income, Insurance, EF, Debts, Investments, Goals

### CSS Section (Lines 250-400)
- Styling for mobile-friendly responsive design
- Color scheme: Purple (#667eea)

### JavaScript Section (Lines 400-700+)
- `addDebt()` – Add debt fields dynamically
- `addGoal()` – Add goal fields dynamically
- `generatePlan()` – Collect form data
- `analyzeDebts()` – Debt payoff logic
- `checkInsurance()` – Insurance recommendations
- `generateRecommendations()` – Create output recommendations

---

## 🐛 Future Enhancements (Planned)

- [ ] Investment tenure brackets (3-5 yrs, 5-7 yrs, 8+ yrs) with specific suggestions
- [ ] More detailed debt payoff timeline calculations
- [ ] SIP step-up planning (₹25K → ₹40K → ₹60K schedule)
- [ ] Tax planning recommendations
- [ ] Monthly savings projection chart
- [ ] Scenario planning ("What if my salary increases?")
- [ ] Export plan as PDF/Excel
- [ ] Multi-language support

---

## 📞 Questions?

This is an MVP. As you use it, send feedback on:
- What calculations are wrong?
- What recommendations are missing?
- What features you want to add?

Everything is documented and ready to modify. Start simple, evolve together. 🚀

---

**Version:** 1.0 (MVP)  
**Last Updated:** 2026  
**Author:** Mohan Medicharla
