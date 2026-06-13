# Financial Logic Documentation

This document explains every financial rule, calculation, and recommendation logic used in the Financial Wealth Planning Tool.

---

## 📋 Table of Contents

1. [Insurance Recommendations](#insurance-recommendations)
2. [Emergency Fund Calculation](#emergency-fund-calculation)
3. [Debt Analysis & Payoff Logic](#debt-analysis--payoff-logic)
4. [Investment Strategy](#investment-strategy)
5. [Priority Sequencing](#priority-sequencing)
6. [Formulas & Calculations](#formulas--calculations)

---

## 🛡️ Insurance Recommendations

### Current Logic:

The tool analyzes your insurance coverage based on **family size, age, dependents, and income**.

#### Health Insurance:
```
INPUT: Health Insurance status, Coverage amount, Family Floater/Individual

IF No Health Insurance:
    STATUS: URGENT (Red Flag)
    RECOMMENDATION: Get Health Insurance immediately
    SUGGESTED_AMOUNT: 
        Family Size 1-2: ₹10-15L
        Family Size 3: ₹15-20L
        Family Size 4+: ₹20-25L
    TYPE: Family Floater recommended for better value

ELSE IF Health Insurance exists:
    Calculate: Suggested Coverage = Family Size × Base Amount
    
    IF Current Coverage < Suggested:
        STATUS: WARNING (Yellow Flag)
        RECOMMENDATION: Increase coverage
        MESSAGE: "Current: ₹X | Suggested: ₹Y for family size Z"
    
    ELSE:
        STATUS: GOOD (Green)
        MESSAGE: "Coverage is adequate"
```

#### Term Insurance:
```
INPUT: Term Insurance status, Sum Assured, Number of dependents

IF No Term Insurance:
    STATUS: URGENT (Red Flag)
    RECOMMENDATION: Get Term Insurance immediately
    SUGGESTED_AMOUNT: Income × 120 (10 years of annual income)
    EXAMPLE: ₹1L monthly income → ₹1.2 Crore cover suggested

ELSE IF Term Insurance exists:
    Calculate: Recommended = Monthly Income × 120
    
    IF Current Coverage < Recommended:
        STATUS: WARNING (Yellow Flag)
        RECOMMENDATION: Increase coverage
        MESSAGE: "Current: ₹X | Suggested: ₹Y for {dependents} dependent(s)"
    
    ELSE:
        STATUS: GOOD (Green)
        MESSAGE: "Coverage is adequate for {dependents} dependent(s)"
```

### Suggestion Algorithm:

```
Health Insurance Suggestion:
- Family Floater is preferred (covers entire family)
- Coverage: 10-15L per family member minimum
- Young & healthy: 10L adequate
- Family with seniors/kids: 15-20L recommended

Term Insurance Suggestion:
- Rule: 10x annual income (120 months of salary)
- More dependents = higher cover needed
- Pure protection (term) is cheapest option
- Review every 3-5 years
```

### Output Examples:

**Example 1 - Missing Both:**
```
🚨 Health Insurance Missing
Get Health Insurance immediately. Recommended ₹15L for family size 3.

🚨 Term Insurance Missing
Get Term Insurance immediately. Recommended ₹1.2 Crore (10x annual income).
```

**Example 2 - Have Both but Low Coverage:**
```
⚠️ Health Insurance Coverage Low
Current: ₹5L | Suggested: ₹15L for family size 3
Increase coverage to ₹15L Family Floater policy.

✅ Term Insurance Adequate
Coverage: ₹1.5 Crore is good for 2 dependents.
```

---

## 🆘 Emergency Fund Calculation

### Inputs:
- **Current EF Balance:** How much you have saved
- **Monthly Obligations:** Fixed monthly expenses (rent, EMI, utilities, groceries, insurance)
- **EF Target Level:** User selects 6, 9, or 12 months

### Calculation:

```
EF Target Amount = Monthly Obligations × Target Level

Example:
Monthly Obligations = ₹50,000
Target = 9 months
EF Target Amount = 50,000 × 9 = ₹4,50,000
```

### Shortfall Calculation:

```
EF Shortfall = EF Target Amount - Current EF Balance

IF Shortfall > 0:
    Monthly Surplus = Monthly Income - Monthly Obligations
    Months to Reach Target = Shortfall / Monthly Surplus (rounded up)
ELSE:
    EF Target is already met
    Status: GREEN ✅
```

### Example:

```
Current EF = ₹2,50,000
EF Target = ₹4,50,000 (9 months × ₹50K obligations)
Shortfall = ₹2,00,000

Monthly Income = ₹1,00,000
Monthly Obligations = ₹50,000
Monthly Surplus = ₹50,000

Months to Reach = 2,00,000 / 50,000 = 4 months

Output: "Build EF in ~4 months if you save your entire surplus"
```

### Recommendation Logic:

```
IF Current EF < EF Target:
    STATUS: WARNING (Yellow flag)
    RECOMMENDATION: Make EF your Priority 2 (after insurance)
    MESSAGE: "Shortfall: ₹X | Timeline: ~Y months"
ELSE:
    STATUS: GOOD (Green flag)
    RECOMMENDATION: Move to next priority (Debt or SIP)
    MESSAGE: "EF meets your target. Good position!"
```

---

## 💳 Debt Analysis & Payoff Logic

### Input Requirements:
- Debt Name (e.g., "Personal Loan", "Gold Loan")
- Outstanding Amount (₹)
- Interest Rate (%) p.a.
- Tenure Remaining (months)

**Note:** Gold Loan is entered here like any other debt, with its interest rate and tenure.

### Debt Priority Algorithm:

```
STEP 1: Sort all debts by Interest Rate (highest to lowest)

STEP 2: For each debt in sorted list:
    IF Tenure Remaining > 12 months:
        Priority = High
        Recommendation = "Clear this first"
    
    ELSE IF Tenure Remaining ≤ 12 months:
        Priority = Skip this one
        Recommendation = "Clear next highest interest instead"
    
    IF Interest Rate < 9%:
        Flag = "Low Interest Alert"
        Recommendation = "Maintain EF + Invest instead of aggressive payoff"
    
    ELSE:
        Flag = "High Interest"
        Recommendation = "Prioritize payoff"

STEP 3: Create priority list based on above rules
```

### Example 1: Multiple Debts, Long Tenure

```
Input:
- Personal Loan: ₹5,00,000 @ 12% | 36 months remaining
- Home Loan: ₹20,00,000 @ 8% | 180 months remaining
- Gold Loan: ₹1,00,000 @ 10% | 24 months remaining

Analysis:
1. Personal Loan: 12% (highest) + 36 months (> 12) = CLEAR FIRST ✅
2. Gold Loan: 10% (next) + 24 months (> 12) = CLEAR SECOND
3. Home Loan: 8% (low interest) + 180 months = Can invest while paying

Output Priority List:
1. Personal Loan (12%, 36 mo)
2. Gold Loan (10%, 24 mo) - Regular debt, same priority logic
3. Home Loan (8%, 180 mo) - Low interest, maintain EF + invest
```

### Example 2: Debt with Short Tenure

```
Input:
- Personal Loan: ₹3,00,000 @ 15% | 8 months remaining
- Car Loan: ₹2,00,000 @ 10% | 48 months remaining

Analysis:
Personal Loan: 15% (highest) BUT tenure = 8 months (≤ 12)
Rule applies: "Skip this, clear next highest instead"
Next highest = Car Loan (10%) with 48 months

Output Priority List:
1. Car Loan (10%, 48 mo) - Even though interest is lower, tenure is longer
2. Personal Loan (15%, 8 mo) - Skip first due to short tenure

Reasoning: Personal loan ends in 8 months anyway, focus on longer-tenure debt
```

### Example 3: Low Interest Alert

```
Input:
- Home Loan: ₹20,00,000 @ 7.5% | 240 months remaining

Analysis:
Interest Rate = 7.5% (< 9% threshold)
Flag = "Low Interest Rate"

Recommendation:
"This is a low-interest debt. Maintain your Emergency Fund and consider investing 
the surplus at expected 12%+ returns instead of aggressive debt payoff."
```

### Calculation: Monthly EMI (for reference)

```
Monthly EMI = Outstanding Amount / (Tenure in Months / 12)

Example:
Personal Loan: ₹5,00,000 outstanding, 36 months remaining
Monthly EMI = 5,00,000 / (36 / 12) = 5,00,000 / 3 = ₹1,66,667 (approx)

Note: This is simplified. Actual EMI includes interest calculations.
For now, we use this approximation to show monthly impact.
```

---

## 📈 Investment Strategy

### Current Logic:

```
INPUT: Investment Tenure (years the user can stay invested)

IF Tenure ≥ 8 years:
    RECOMMENDATION = Equity Mutual Funds
    EXPLANATION = "You have 8+ years. Equity MFs are best for wealth creation."
    SUGGESTED_PORTFOLIO = "Nifty 50 + Flexi Cap + Mid Cap + Small Cap mix"

ELSE IF Tenure < 8 years:
    RECOMMENDATION = "To be refined - depends on specific bracket"
    EXPLANATION = "Details being added as tool evolves"
```

### Future Enhancement: Tenure Brackets

**(To be added based on your guidance)**

```
Tenure 2-3 years:
    - Debt Funds (for stability)
    - Liquid Funds (for liquidity)
    - Allocation: 60% Debt / 40% Liquid

Tenure 4-5 years:
    - Balanced Funds (mix of equity & debt)
    - Allocation: 50% Equity MF / 50% Debt Funds

Tenure 5-7 years:
    - Mid-Cap Funds
    - Flexi Cap Funds
    - Allocation: 70% Equity MF / 30% Debt

Tenure 8+ years:
    - Nifty 50 ETF / Index Funds (10%)
    - Flexi Cap MF (30%)
    - Mid-Cap MF (20%)
    - Small-Cap MF (40%)
```

### SIP Recommendations:

**Current Logic:**
```
Available Surplus = Monthly Income - Monthly Obligations

SIP Suggestion = Available Surplus (or a portion of it)

Future Enhancement:
- Suggest step-up every year (₹25K → ₹40K → ₹60K)
- Calculate projected corpus @ 12% p.a.
- Show timeline to reach ₹1 crore
```

**Example:**
```
Monthly Income: ₹1,00,000
Monthly Obligations: ₹50,000
Available Surplus: ₹50,000

SIP Suggestion: "Start with ₹25,000/month SIP"
Step-up Plan:
- Year 1-2: ₹25,000/month
- Year 3-4: ₹40,000/month
- Year 5+: ₹60,000/month

Projected Corpus (@ 12% p.a. returns):
- After 10 years: ~₹60-70 lakhs
- After 15 years: ~₹1.5+ crores
```

---

## 🎯 Priority Sequencing

### The Recommended Order (FIXED):

```
1. Health & Term Insurance
   ├─ Why: Foundation of financial security
   ├─ Action: Get if missing (besides employer ones)
   └─ Cost: ~₹5K-15K/year

2. Emergency Fund
   ├─ Why: Protects against unexpected events
   ├─ Target: 6, 9, or 12 months of obligations
   └─ Action: Build to your selected target

3. Debt Payoff
   ├─ Why: Reduces financial burden
   ├─ Strategy: Highest interest first (with tenure rules)
   └─ Action: Clear high-interest debts systematically

4. SIP & Investments
   ├─ Why: Builds wealth long-term
   ├─ Strategy: Based on tenure & surplus
   └─ Action: Start SIP, build investment corpus

5. Long-term Goals
   ├─ Why: Fulfill dreams (land, house, etc.)
   ├─ Timeline: 5-15 years
   └─ Action: Plan after other pillars are solid
```

### Decision Tree:

```
START: User fills form

├─ Insurance Missing?
│  ├─ YES → URGENT: Get insurance first, pause other planning
│  └─ NO → Continue
│
├─ EF < Target?
│  ├─ YES → Priority 2: Build EF immediately
│  └─ NO → Continue
│
├─ Any Debt?
│  ├─ YES → Priority 3: Payoff using debt algorithm
│  └─ NO → Continue
│
├─ Monthly Surplus?
│  ├─ YES → Priority 4: Start/Increase SIP
│  └─ NO → Focus on increasing income or reducing obligations
│
└─ Goals Listed?
   ├─ YES → Priority 5: Plan with remaining surplus
   └─ NO → Revisit after other priorities done
```

---

## 🧮 Formulas & Calculations

### Emergency Fund Shortfall:

```
EF Shortfall = EF Target Amount - Current EF Balance

IF Shortfall < 0:
    EF Shortfall = 0 (already met)
```

### Months to Reach EF Target:

```
Monthly Surplus = Monthly Income - Monthly Obligations

IF Monthly Surplus > 0:
    Months to Target = EF Shortfall / Monthly Surplus
    (Round UP to nearest integer)
ELSE:
    Message: "Insufficient surplus. Reduce obligations or increase income."
```

### Total Debt Outstanding:

```
Total Debt = SUM of all debt amounts

Example:
Personal Loan: ₹5,00,000
Home Loan: ₹20,00,000
Gold Loan: ₹1,00,000
Total = ₹26,00,000
```

### Monthly Surplus:

```
Monthly Surplus = Monthly Income - Monthly Obligations

This is the "available capacity" for:
- EF building
- Debt payoff
- SIP investing
- Other savings
```

### SIP Corpus Projection (Future):

```
Corpus = P × [((1 + r)^n - 1) / r]

Where:
P = Monthly SIP amount
r = Monthly return rate (annual / 12)
n = Number of months

Example:
P = ₹25,000/month
r = 1% monthly (12% p.a.)
n = 120 months (10 years)

Corpus = 25,000 × [((1.01)^120 - 1) / 0.01]
       = 25,000 × 30.4
       = ₹7,60,000

(This is simplified; actual returns vary)
```

---

## 🔄 Logic Update Log

### Version 1.0 (MVP - Current):
- ✅ Insurance check logic
- ✅ EF calculation & timeline
- ✅ Debt priority algorithm (with tenure & interest rate rules)
- ✅ Investment tenure recommendation (8+ years = Equity)
- ✅ Priority sequencing
- ⏳ Investment tenure brackets (3-5, 5-7 years) - To add
- ⏳ SIP step-up planning - To add
- ⏳ Detailed EMI calculations - To add

### Next Updates (Based on Feedback):
- [ ] Add tenure brackets with specific suggestions
- [ ] Add SIP step-up planning logic
- [ ] Add goal timeline projections
- [ ] Add tax planning recommendations

---

## 📝 Notes for Future Modifications

### To Add New Rule:

1. **Document it here** (in FINANCIAL_LOGIC.md)
2. **Find the corresponding function** in the HTML code
3. **Add the logic** (usually in JavaScript `generateRecommendations()`)
4. **Test with examples** to ensure it works correctly
5. **Update README** if user-facing behavior changes

### Example: Adding a New Debt Rule

```
If you want to add: "Prioritize Gold Loans if interest > 10%"

Step 1: Document the rule here
Step 2: Find analyzeDebts() function in the code
Step 3: Add: 
    if (debt.name.includes("Gold") && debt.rate > 10) {
        debt.priority = "HIGHEST";
    }
Step 4: Test with a Gold Loan @ 11%
Step 5: Update README with new rule
```

---

## 🎓 Examples for Testing

### Test Case 1: Young Professional Starting Out

```
Input:
- Monthly Income: ₹75,000
- Obligations: ₹40,000
- Health Insurance: Yes
- Term Insurance: No ← FLAG
- Current EF: ₹2,00,000
- EF Target: 9 months (₹3,60,000)
- Debt: Personal Loan ₹3,00,000 @ 12% | 24 months
- SIP: None
- Investment Tenure: 10 years

Expected Output:
1. GET TERM INSURANCE (urgent)
2. Build EF: Shortfall ₹1,60,000 | ~5 months
3. Pay Personal Loan: 12% is high, tenure > 12 months = Clear first
4. Start SIP: ₹35K/month available
5. Work towards goals
```

### Test Case 2: Established Professional with Multiple Debts

```
Input:
- Monthly Income: ₹2,00,000
- Obligations: ₹1,00,000
- Insurance: Yes (both)
- Current EF: ₹10,00,000
- EF Target: 12 months (₹12,00,000)
- Debts:
  - Home Loan: ₹30,00,000 @ 8% | 180 months
  - Personal Loan: ₹5,00,000 @ 14% | 36 months
  - Gold Loan: ₹2,00,000 @ 10% | 6 months
- Current SIP: ₹30,000/month
- Investment Tenure: 15 years

Expected Output:
1. Insurance: ✅ Good
2. EF: Almost at target, ₹2,00,000 shortfall | ~1 month
3. Debt Priority:
   - Personal Loan (14%, 36 mo) = CLEAR FIRST
   - Gold Loan (10%, 6 mo) = Skip (short tenure), clear next
   - Home Loan (8%, 180 mo) = Low interest, maintain EF + invest
4. Investment: ✅ 15 years tenure → Aggressive equity MF portfolio
5. SIP: Increase to ₹40K/month based on surplus
6. Goals: Can work on land purchase in 2-3 years
```

---

**End of Documentation**

For questions or to suggest logic changes, refer back to the main README.md
