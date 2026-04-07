# AI Banking System 




## 📖 Overview

### Project Description
The **AI Banking System** is a web-based banking application built with Python and Streamlit, designed to provide a comprehensive digital banking experience with AI-powered features, fraud detection, and real-time analytics.

### Key Objectives
- Provide secure user authentication and account management
- Enable real-time financial transactions (deposits/withdrawals)
- Implement AI-powered banking assistant for customer queries
- Detect fraudulent transactions using machine learning
- Visualize financial data with interactive analytics

### Target Users
- Individual users seeking digital banking solutions
- Educational institutions for teaching fintech concepts
- Developers learning about banking system architecture
- Startups prototyping banking applications

---

## 🏗️ System Architecture

### Technology Stack

#### Frontend
- **Streamlit**: Web application framework
- **Plotly**: Interactive data visualization
- **HTML/CSS**: Custom styling and layouts

#### Backend
- **Python 3.x**: Core programming language
- **Pandas**: Data manipulation and analysis
- **NumPy**: Numerical computations
- **Scikit-learn**: Machine learning algorithms

#### Security
- **Hashlib**: SHA-256 password hashing
- **Session State**: Secure session management

#### Deployment
- **Google Colab**: Development environment
- **ngrok**: Secure tunnel for public access
- **Streamlit Cloud**: Alternative hosting option

### System Components

```
┌─────────────────────────────────────────┐
│         User Interface (Streamlit)       │
├─────────────────────────────────────────┤
│  Authentication │ Dashboard │ AI Chat    │
│  Transactions   │ Analytics │ Security   │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│       Session State Management          │
│  - User Data    - Transactions          │
│  - Current User - Account Info          │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│         Business Logic Layer            │
│  - Authentication  - Fraud Detection    │
│  - Transactions    - AI Assistant       │
└─────────────────────────────────────────┘
           ↓
┌─────────────────────────────────────────┐
│          Data Storage Layer             │
│  (In-Memory Session State)              │
└─────────────────────────────────────────┘
```

---

## 🚀 Installation & Setup

### Prerequisites
- Google Colab account (free)
- ngrok account (free tier sufficient)
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Stable internet connection

### Step-by-Step Installation

#### Step 1: Prepare Google Colab
1. Open [Google Colab](https://colab.research.google.com)
2. Create a new notebook
3. Ensure Python 3.x runtime is selected

#### Step 2: Get ngrok Authentication Token
1. Visit [ngrok Dashboard](https://dashboard.ngrok.com/signup)
2. Create free account (email verification required)
3. Navigate to "Your Authtoken" section
4. Copy your unique authentication token

#### Step 3: Deploy Application
1. Copy the complete code from the artifact
2. Paste into a single Colab cell
3. Locate line: `NGROK_TOKEN = ""`
4. Insert your token: `NGROK_TOKEN = "your_token_here"`
5. Run the cell (Ctrl+Enter or Shift+Enter)

#### Step 4: Access Application
1. Wait for setup completion (30-60 seconds)
2. Copy the generated public URL
3. Open URL in new browser tab
4. Application is now accessible globally

### Configuration Options

```python
# Port Configuration
SERVER_PORT = 8501  # Default Streamlit port

# Initial Balance Settings
DEFAULT_INITIAL_BALANCE = 1000.00
MINIMUM_INITIAL_DEPOSIT = 100.00

# Fraud Detection Parameters
FRAUD_CONTAMINATION_RATE = 0.1  # 10% threshold
MINIMUM_TRANSACTIONS_FOR_FRAUD = 10

# Session Settings
SESSION_TIMEOUT = 3600  # 1 hour (seconds)
```

---

## 🎯 Features Documentation

### 1. User Authentication System

#### Registration
**Purpose**: Create new user accounts with secure credentials

**Process Flow**:
1. User provides username and password
2. System validates uniqueness of username
3. Password hashed using SHA-256 algorithm
4. Account number generated (format: ACC00001)
5. Initial balance deposited
6. Account creation timestamp recorded

**Code Reference**:
```python
def create_account(user, pwd, balance=1000):
    if user in st.session_state.users:
        return False
    st.session_state.users[user] = {
        "password": hash_password(pwd),
        "balance": balance,
        "account": f"ACC{len(st.session_state.users):05d}",
        "created": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }
    return True
```

**Validation Rules**:
- Username: Non-empty, unique
- Password: Non-empty (recommend 8+ characters)
- Initial deposit: Minimum $100.00

#### Login
**Purpose**: Authenticate existing users

**Process Flow**:
1. User enters credentials
2. System retrieves stored password hash
3. Input password hashed and compared
4. Session established if match successful
5. Redirect to dashboard

**Security Features**:
- Password never stored in plain text
- SHA-256 cryptographic hashing
- Session-based authentication
- Automatic logout on session end

---

### 2. Account Dashboard

#### Overview
Central hub displaying account summary and recent activity

#### Components

**Balance Display**
- Real-time account balance
- Currency formatted ($X,XXX.XX)
- Updated after each transaction

**Transaction Counter**
- Total number of transactions
- Separate counts for deposits/withdrawals
- Historical tracking

**Spending Summary**
- Total withdrawal amount
- Percentage of balance spent
- Budget tracking capability

**Recent Transactions**
- Last 5 transactions displayed
- Chronological order (newest first)
- Quick view of account activity

---

### 3. Transaction Management

#### Deposit Functionality

**Purpose**: Add funds to account

**Input Parameters**:
- Amount: Positive float value
- Description: Text description of deposit source

**Process**:
1. Validate amount > 0
2. Add amount to current balance
3. Record transaction in history
4. Update balance display
5. Show success notification

**Example**:
```python
if st.button("Deposit"):
    st.session_state.users[current_user]["balance"] += amount
    add_transaction(current_user, "deposit", amount, description)
    st.success(f"Deposited ${amount:.2f}")
```

#### Withdrawal Functionality

**Purpose**: Remove funds from account

**Input Parameters**:
- Amount: Positive float value
- Description: Purpose of withdrawal

**Validation**:
- Amount must be ≤ current balance
- Amount must be > 0

**Process**:
1. Check sufficient balance
2. Deduct amount from balance
3. Record transaction
4. Update display
5. Show confirmation

**Error Handling**:
- Insufficient funds: Display error message
- Invalid amount: Prevent submission

#### Transaction History

**Features**:
- Complete transaction log
- Filterable by date/type
- Sortable columns
- Export to CSV capability
- Timestamp for each transaction

**Data Fields**:
- Username
- Transaction type (deposit/withdrawal)
- Amount
- Description
- Timestamp (YYYY-MM-DD HH:MM:SS)
- Balance after transaction

---

### 4. AI Banking Assistant

#### Overview
Natural language processing assistant for banking queries

#### Supported Queries

**Balance Inquiries**
- "What is my balance?"
- "How much money do I have?"
- "Show my balance"

**Response**: Current account balance

**Account Information**
- "What is my account number?"
- "Show account details"
- "Account info"

**Response**: Account number and creation date

**Transaction History**
- "Show my transactions"
- "Transaction history"
- "What transactions did I make?"

**Response**: Transaction count and reference to history tab

**Spending Analysis**
- "How much have I spent?"
- "Total withdrawals"
- "What did I spend?"

**Response**: Total withdrawal amount

**Deposit Summary**
- "How much did I deposit?"
- "Total deposits"

**Response**: Total deposit amount

#### Query Processing Algorithm
```python
def ai_chatbot(query):
    query_lower = query.lower()
    
    if "balance" in query_lower:
        return get_balance_response()
    elif "account" in query_lower:
        return get_account_info()
    elif "transaction" in query_lower:
        return get_transaction_summary()
    # ... additional conditions
```

#### Quick Action Buttons
- Check Balance
- View Account
- Transaction Count
- Help Information

---

### 5. Fraud Detection System

#### Technology
**Machine Learning Algorithm**: Isolation Forest

**Purpose**: Identify anomalous transactions that may indicate fraud

#### How It Works

**Step 1: Data Collection**
- Minimum 10 transactions required
- Transaction amounts used as features

**Step 2: Model Training**
```python
features = df[["amount"]].values.reshape(-1, 1)
iso_forest = IsolationForest(contamination=0.1, random_state=42)
predictions = iso_forest.fit_predict(features)
```

**Step 3: Anomaly Detection**
- Algorithm identifies outliers
- Transactions significantly different from pattern flagged
- Contamination rate: 10% (adjustable)

**Step 4: Results Display**
- List of suspicious transactions
- Visual indicators (warning symbols)
- Detailed transaction information

#### Security Metrics
- Total transactions analyzed
- Number of suspicious transactions detected
- Security score percentage
- Trend analysis over time

#### Limitations
- Requires minimum transaction history
- Based on amount patterns only
- False positives possible
- Real-time detection not implemented

---

### 6. Financial Analytics

#### Visualization Components

**Spending Breakdown (Pie Chart)**
- Categories by transaction description
- Percentage distribution
- Interactive tooltips
- Color-coded segments

**Balance Timeline (Line Chart)**
- Balance changes over time
- Trend visualization
- Markers for each transaction
- Smooth curve interpolation

**Transaction Type Distribution (Bar Chart)**
- Deposits vs Withdrawals
- Count comparison
- Color differentiation
- Clear labels

#### Summary Statistics

**Calculated Metrics**:
- Total Deposits: Sum of all deposit amounts
- Total Withdrawals: Sum of all withdrawal amounts
- Average Transaction: Mean transaction value
- Net Change: Deposits - Withdrawals
- Transaction Frequency: Transactions per day/week

#### Export Capabilities
- Download transaction history as CSV
- Date range filtering
- Custom report generation
- Print-friendly format

---

## 🔧 Technical Specifications

### Data Structures

#### User Object
```python
{
    "password": str,        # SHA-256 hashed password
    "balance": float,       # Current account balance
    "account": str,         # Account number (ACC00001)
    "created": str         # Creation timestamp
}
```

#### Transaction Object
```python
{
    "username": str,        # Account owner
    "type": str,           # "deposit" or "withdrawal"
    "amount": float,       # Transaction amount
    "description": str,    # Transaction description
    "timestamp": str,      # Transaction time
    "balance": float       # Balance after transaction
}
```

### Session State Variables

```python
st.session_state.users = {}              # All user accounts
st.session_state.current_user = None     # Logged-in user
st.session_state.transactions = []       # All transactions
```

### Function Reference

#### Authentication Functions

**hash_password(pwd: str) -> str**
- Input: Plain text password
- Output: SHA-256 hash string
- Purpose: Secure password storage

**create_account(user: str, pwd: str, balance: float) -> bool**
- Creates new user account
- Returns True if successful, False if username exists

**authenticate(user: str, pwd: str) -> bool**
- Validates user credentials
- Returns True if credentials match

#### Transaction Functions

**add_transaction(user: str, txn_type: str, amount: float, desc: str) -> None**
- Records transaction in history
- Updates transaction log
- Captures timestamp and balance

#### Analysis Functions

**detect_fraud(df: DataFrame) -> list**
- Input: Transaction DataFrame
- Output: List of suspicious transaction indices
- Algorithm: Isolation Forest

### Performance Considerations

**Scalability**:
- In-memory storage: Suitable for demos/prototypes
- Recommended for < 100 concurrent users
- Transaction limit: ~10,000 per session

**Optimization Opportunities**:
- Database integration (SQLite, PostgreSQL)
- Caching mechanisms
- Async operations
- Load balancing


---

