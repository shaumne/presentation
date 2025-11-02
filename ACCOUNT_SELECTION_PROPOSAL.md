# Account Selection Feature - Implementation Proposal

## 📋 Your Request

> "I want the ability to select whether I am running this strategy on an evaluation account or a funded account. I'd prefer to copy and paste the name of the account I want to trade for the day in the config file, and then run the code."

**Perfect! This is easy to implement. I have 3 approaches for you to choose from:**

---

## 🎯 Option A: Simple Copy/Paste Approach

### How it works:

**Current setup:**
```env
# .env file
TOPSTEPX_ACCOUNT_NAME=50KTC-V2-294402-27510665
```

**New setup:**
```yaml
# config.yaml file
account:
  name: "50KTC-V2-294402-27510665"  # ← Copy/paste account name here daily
  type: "evaluation"                 # ← Change to "funded" when using funded account
```

### Daily workflow:

**Day 1 - Trading on Evaluation Account:**
```yaml
account:
  name: "50KTC-V2-294402-27510665"
  type: "evaluation"
```
```bash
python -m bot.main --mode demo
```

**Day 2 - Trading on Funded Account:**
```yaml
account:
  name: "150KTC-FUNDED-12345678"  # Copy/paste your funded account
  type: "funded"
```
```bash
python -m bot.main --mode live
```

### ✅ Pros:
- Very simple
- Just copy/paste the account name
- Clear what you're trading

### ⚠️ Cons:
- Must type/paste full account name each time
- Risk of typos

---

## 🎯 Option B: Account Presets (RECOMMENDED)

### How it works:

```yaml
# config.yaml file
accounts:
  # Define all your accounts once (with friendly names)
  eval_50k: "50KTC-V2-294402-27510665"
  eval_100k: "100KTC-V2-294402-99999999"
  funded_main: "150KTC-FUNDED-12345678"
  funded_backup: "200KTC-FUNDED-87654321"

# Just change this line daily:
active_account: "eval_50k"  # ← Change to "funded_main" tomorrow
```

### Daily workflow:

**Day 1 - Trading on 50K Evaluation:**
```yaml
active_account: "eval_50k"  # ← Just change this one word
```
```bash
python -m bot.main --mode demo
```

**Day 2 - Trading on Funded Account:**
```yaml
active_account: "funded_main"  # ← Just change this one word
```
```bash
python -m bot.main --mode live
```

### ✅ Pros:
- Setup once, use forever
- No copy/pasting account numbers
- No typo risk
- Quick to switch (one word change)
- Can see all your accounts at a glance

### ⚠️ Cons:
- Initial setup required (one time)

---

## 🎯 Option C: Advanced with Auto-Detection

### How it works:

```yaml
# config.yaml file
accounts:
  - name: "50KTC-V2-294402-27510665"
    label: "Eval 50K"
    type: "evaluation"
    enabled: true     # ← Bot uses this one
    
  - name: "150KTC-FUNDED-12345678"
    label: "Funded Main"
    type: "funded"
    enabled: false    # ← Not used today
    
  - name: "100KTC-V2-294402-99999999"
    label: "Eval 100K"
    type: "evaluation"
    enabled: false    # ← Not used today
```

### Daily workflow:

**Day 1 - Trading on Evaluation:**
```yaml
accounts:
  - name: "50KTC-V2-294402-27510665"
    label: "Eval 50K"
    type: "evaluation"
    enabled: true     # ← Set to true
    
  - name: "150KTC-FUNDED-12345678"
    label: "Funded Main"
    type: "funded"
    enabled: false    # ← Set to false
```

**Day 2 - Trading on Funded:**
```yaml
accounts:
  - name: "50KTC-V2-294402-27510665"
    label: "Eval 50K"
    type: "evaluation"
    enabled: false    # ← Set to false
    
  - name: "150KTC-FUNDED-12345678"
    label: "Funded Main"
    type: "funded"
    enabled: true     # ← Set to true
```

The bot automatically uses the account with `enabled: true`

### ✅ Pros:
- Most professional approach
- Track metadata (labels, types)
- Can enable/disable accounts
- Clear documentation of all accounts

### ⚠️ Cons:
- More lines to manage
- Slightly more complex

---

## 📊 Comparison Table

| Feature | Option A (Simple) | Option B (Presets) ★ | Option C (Advanced) |
|---------|-------------------|---------------------|---------------------|
| **Ease of Setup** | ⭐⭐⭐⭐⭐ Very Easy | ⭐⭐⭐⭐ Easy | ⭐⭐⭐ Medium |
| **Daily Use** | ⭐⭐⭐ Copy/Paste | ⭐⭐⭐⭐⭐ One Word | ⭐⭐⭐⭐ Toggle |
| **Typo Risk** | ⭐⭐ Medium | ⭐⭐⭐⭐⭐ None | ⭐⭐⭐⭐⭐ None |
| **Account Management** | ⭐⭐ Poor | ⭐⭐⭐⭐⭐ Excellent | ⭐⭐⭐⭐⭐ Excellent |
| **Professional** | ⭐⭐⭐ Good | ⭐⭐⭐⭐⭐ Very Good | ⭐⭐⭐⭐⭐ Best |

**★ = Recommended**

---

## 🎯 My Recommendation: **Option B**

### Why?

1. **Best Balance**: Easy to use, powerful features
2. **Quick Switching**: Change one word, done
3. **No Typos**: Account names stored once
4. **Professional**: Clean, organized config
5. **Scalable**: Add more accounts anytime

### Example of Your Daily Routine:

**Every Morning:**
1. Open `config.yaml`
2. Find line: `active_account: "eval_50k"`
3. Change to: `active_account: "funded_main"`
4. Save file
5. Run bot: `python -m bot.main --mode live`

**That's it! 5 seconds of work.**

---

## 💬 Questions for You

### 1. Which option do you prefer?
- [ ] **Option A**: Simple copy/paste
- [ ] **Option B**: Account presets (recommended)
- [ ] **Option C**: Advanced with auto-detection
- [ ] **Other**: Tell me what you'd like

### 2. How many accounts do you have?
- Evaluation accounts: ___
- Funded accounts: ___

### 3. Do you want to keep account info in `.env` or move everything to `config.yaml`?
- [ ] **Keep in .env** (more secure, but less flexible)
- [ ] **Move to config.yaml** (easier to manage, recommended)

### 4. Should the bot automatically detect account type (evaluation vs funded)?
- [ ] **Yes**: Bot detects from account name and uses appropriate mode
- [ ] **No**: I'll manually specify demo/live mode when starting bot

---

## 🚀 Implementation Timeline

Once you tell me your preference:

```
Step 1: Update config.yaml structure        [5 minutes]
Step 2: Update config.py to read accounts   [10 minutes]
Step 3: Update main.py to use new config    [5 minutes]
Step 4: Test with your accounts            [10 minutes]
────────────────────────────────────────────
Total: ~30 minutes
```

---

## 📝 What Changes in Your Files

### Will Change:
- ✏️ `config.yaml` - Add account section
- ✏️ `config.py` - Read account from config
- ✏️ `main.py` - Use account from config
- ✏️ `README.md` - Update documentation

### Won't Change:
- ✅ `strategy.py` - No changes
- ✅ `api_client.py` - No changes
- ✅ `order_manager.py` - No changes
- ✅ `market_data.py` - No changes
- ✅ Strategy logic - Stays exactly the same

**Risk Level: Very Low** ✅

---

## 🎯 Example: Option B Implementation

### Your config.yaml would look like:

```yaml
# ============================================
# ACCOUNT CONFIGURATION
# ============================================
accounts:
  # Evaluation Accounts
  eval_50k: "50KTC-V2-294402-27510665"
  eval_100k: "100KTC-V2-XXXXX-XXXXXXXX"  # Add your other eval account
  
  # Funded Accounts
  funded_main: "150KTC-FUNDED-XXXXXXXX"  # Add when you get funded
  funded_backup: "200KTC-FUNDED-XXXXXXXX"  # Add if you have backup

# ============================================
# DAILY SETTING - Change this line every day
# ============================================
active_account: "eval_50k"  # ← TODAY: Trading on 50K eval
                            # ← TOMORROW: Change to "funded_main"

# ============================================
# STRATEGY CONFIGURATION (unchanged)
# ============================================
strategy:
  name: "NQ Asia 8:25 Breakout"
  contract_symbol: "NQ"
  session_start: "20:00"
  # ... rest of your strategy settings
```

---

## 🎬 Next Steps

**Please reply with:**

1. Your preferred option (A, B, or C)
2. Account names you want to configure
3. Any specific requirements

**Then I'll:**

1. Implement your choice
2. Test it thoroughly
3. Update all documentation
4. Show you examples

---


**Waiting for your decision! Once you choose, I'll implement it right away.** 🚀

---

*This feature adds account flexibility without changing your proven strategy.*

