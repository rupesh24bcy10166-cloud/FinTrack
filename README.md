# FinTrack: Enterprise Personal Finance & Budget Management Engine

[![Language](https://img.shields.io/badge/Language-Java%2017%2B%20%7C%2021%20%7C%2023-orange.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-blue.svg)]()
[![Tests](https://img.shields.io/badge/Tests-28%20Passed-brightgreen.svg)]()
[![Architecture](https://img.shields.io/badge/Architecture-Layered%20OOP-lightgrey.svg)]()

> **Course**: CSE1021 - Programming in Java  
> **Component**: Evaluated Course Project (Flipped Course) - Build Your Own Project  
> **Platform**: VITyarthi

---

## 1. Overview
**FinTrack** is an enterprise-grade, terminal-native Personal Finance and Budget Management system developed in pure Java. Designed around clean Object-Oriented design patterns and a layered architecture (`model`, `repository`, `service`, `util`, `cli`), FinTrack allows users to maintain multi-account financial portfolios, record income/expense/transfer transactions, track category-specific spending limits with automated threshold alerts, and compute functional financial telemetry using the Java Streams API.

FinTrack requires **zero external runtime dependencies** (no external Maven or Gradle plugins required for execution) and operates with local CSV-based file persistence. It is fully executable in terminal environments and features an automated non-interactive `--demo` mode designed specifically for automated grading pipelines.

---

## 2. Key Features

### 🏦 Polymorphic Multi-Account Portfolio
- **Savings Account**: Enforces strict minimum balance safeguards and provides automated monthly APY compound interest calculations.
- **Checking Account**: Configurable overdraft protection ceiling with automated overdraft penalty assessment upon balance deficit.
- **Credit Card Account**: Revolving credit facility tracking credit limits, outstanding balances, and available purchasing power.

### 💸 Atomic Transaction Engine
- **Income Processing**: Crediting designated accounts with categorized income logs (Salary, Freelance, Investment Return).
- **Expense Logging**: Debits funds with instant budget validation and threshold alert evaluation.
- **Inter-Account Transfers**: Atomic balance relocation between accounts maintaining double-entry integrity.

### 🛡️ Proactive Budget & Alert Sentinel
- Custom monthly spending ceilings per category (Food, Housing, Utilities, Transportation, Entertainment, etc.).
- Automated real-time alerts when spending reaches configured safety thresholds (e.g., 80%) and strict warning banners upon budget exhaustion.

### 📊 Streams-Powered Analytics & Financial KPIs
- Leverages the Java Streams API (`groupingBy`, `summingDouble`, `filter`, `sorted`) to calculate:
  - Total Monthly Inflow, Outflow, and Net Cash Savings.
  - Personal Savings Rate percentage.
  - Top 5 expenditure drivers and category percentage distribution.

### 📄 Tabular Terminal Reporting & Disk Export
- Dynamic ASCII table formatting for portfolios, ledgers, and budget comparison matrices.
- One-click export of structured audit reports to local disk (`.txt` / `.csv`).

### 🤖 Dual Execution Interface
- **Interactive Console Menu**: Colorized interactive experience for human users.
- **Automated Headless Mode (`--demo`)**: Non-blocking automated demonstration pipeline executing all system capabilities end-to-end for automated grading scripts.

---

## 3. Technologies & Subject Concepts Used

- **Language & Runtime**: Java SE 17 / 21 / 23 (Tested on OpenJDK / Oracle JDK).
- **OOP Principles**:
  - **Abstraction & Inheritance**: Base class `Account` extended by `SavingsAccount`, `CheckingAccount`, and `CreditAccount`.
  - **Polymorphism**: Polymorphic dynamic dispatch of `withdraw()` across different account constraints.
  - **Encapsulation**: Private fields, immutable records, and domain invariants.
  - **Interfaces & Generics**: Generic CRUD abstraction `Repository<T, ID>`.
- **Collections & Streams**: `ConcurrentHashMap`, `ArrayList`, `TreeMap`, `Collectors.groupingBy()`.
- **Exception Handling**: Custom checked hierarchy (`FinTrackException`, `InsufficientFundsException`, `BudgetExceededException`, `AccountNotFoundException`, `InvalidTransactionException`).
- **File I/O**: Robust CSV parsing and serial persistence via `java.nio.file` and `BufferedReader`/`PrintWriter`.

---

## 4. Project Directory Structure

```
FinTrack/
├── .gitignore
├── README.md                      # Complete installation, execution & testing guide
├── statement.md                   # Problem statement, target audience & scope
├── PROJECT_REPORT.md              # 15-section project report with UML diagrams
├── data/                          # Persistent storage directory
│   ├── accounts.csv
│   ├── transactions.csv
│   ├── budgets.csv
│   └── audit_demo_export.txt
├── src/
│   └── com/vityarthi/fintrack/
│       ├── cli/
│       │   ├── FinTrackApp.java        # Main CLI entry point
│       │   └── MenuController.java     # Interactive menu controller
│       ├── exception/                  # Custom domain exception hierarchy
│       │   ├── FinTrackException.java
│       │   ├── InsufficientFundsException.java
│       │   ├── AccountNotFoundException.java
│       │   ├── BudgetExceededException.java
│       │   └── InvalidTransactionException.java
│       ├── model/                      # Domain entities & enums
│       │   ├── Account.java
│       │   ├── SavingsAccount.java
│       │   ├── CheckingAccount.java
│       │   ├── CreditAccount.java
│       │   ├── Transaction.java
│       │   ├── TransactionType.java
│       │   ├── Category.java
│       │   └── Budget.java
│       ├── repository/                 # File-backed persistence layer
│       │   ├── Repository.java
│       │   ├── AccountRepository.java
│       │   ├── TransactionRepository.java
│       │   └── BudgetRepository.java
│       ├── service/                    # Business logic & analytics
│       │   ├── AccountService.java
│       │   ├── TransactionService.java
│       │   ├── BudgetService.java
│       │   ├── AnalyticsService.java
│       │   └── ReportService.java
│       └── util/                       # Utilities & formatters
│           ├── ConsoleColors.java
│           ├── TableFormatter.java
│           └── Validator.java
└── test/
    └── com/vityarthi/fintrack/
        └── FinTrackTestSuite.java      # 28-point automated verification suite
```

---

## 5. Installation & Setup

### Prerequisites
- Java Development Kit (JDK) 17 or higher installed.
- Verify your Java installation:
  ```bash
  javac -version
  java -version
  ```

### Step 1: Clone Repository
```bash
git clone https://github.com/<your-username>/FinTrack.git
cd FinTrack
```

### Step 2: Compile the Project
To compile all source code and tests into the `bin` directory:

**Windows (PowerShell):**
```powershell
javac -d bin (Get-ChildItem -Recurse -Filter *.java -Path src, test | Select-Object -ExpandProperty FullName)
```

**Linux / macOS (Bash):**
```bash
mkdir -p bin
javac -d bin $(find src test -name "*.java")
```

---

## 6. Execution Instructions

### A. Run Automated Headless Demo (Recommended for Evaluators)
To execute the automated end-to-end demo without any manual terminal inputs:
```bash
java -cp bin com.vityarthi.fintrack.cli.FinTrackApp --demo
```

### B. Run Interactive Terminal Menu
To launch the interactive, user-driven console interface:
```bash
java -cp bin com.vityarthi.fintrack.cli.FinTrackApp
```

### C. Run Quick Summary Report
To display currently persisted reports directly to terminal:
```bash
java -cp bin com.vityarthi.fintrack.cli.FinTrackApp --report
```

---

## 7. Testing Instructions

FinTrack comes equipped with a self-contained, zero-dependency automated test suite covering 28 unit and integration test assertions (account rules, overdraft penalties, credit limits, budget triggers, streams calculations, and file persistence round-trips).

Run the test suite with:
```bash
java -cp bin com.vityarthi.fintrack.FinTrackTestSuite
```

### Expected Test Output:
```text
==========================================================
          RUNNING FINTRACK AUTOMATED TEST SUITE           
==========================================================
  [PASS] Savings withdrawal within limit
  [PASS] Savings minimum balance enforcement
  [PASS] Savings monthly interest accrual
  [PASS] Savings updated balance after interest
  [PASS] Checking withdrawal within positive balance
  [PASS] Checking overdraft usage & fee assessment
  [PASS] Checking overdraft ceiling enforcement
  [PASS] Credit card initial debt zero
  [PASS] Credit card initial available credit
  [PASS] Credit card purchase debt update
  [PASS] Credit card available limit update
  [PASS] Credit card payment reduces debt
  [PASS] Credit card limit exceed rejection
  [PASS] Transfer source debited
  [PASS] Transfer destination credited
  [PASS] Budget safe at 50% spending
  [PASS] Budget warning triggered at 80% ($400)
  [PASS] Budget warning active at 95% ($475)
  [PASS] Budget exceeded triggered at $501
  [PASS] Remaining limit calculation
  [PASS] Analytics Total Income via Streams
  [PASS] Analytics Total Expenses via Streams
  [PASS] Analytics Net Savings
  [PASS] GroupingBy Category Food Total
  [PASS] GroupingBy Category Entertainment Total
  [PASS] Persistence loaded correct record count
  [PASS] Persistence Savings account loaded properly
  [PASS] Persistence Checking account loaded properly
==========================================================
Test Summary: 28 PASSED, 0 FAILED (Total: 28)
==========================================================
```

---

## 8. Sample Execution Traces & Outputs

### Portfolio Summary
```text
====================== ACCOUNT PORTFOLIO SUMMARY ======================
+----------+-----------+----------------+----------------+----------+
| Type     | Account # | Account Holder | Balance / Debt | Currency |
+----------+-----------+----------------+----------------+----------+
| CREDIT   | CC-3001   | Alex Mercer    |           0.00 | USD      |
| CHECKING | CHK-2001  | Alex Mercer    |        4275.00 | USD      |
| SAVINGS  | SA-1001   | Alex Mercer    |        3694.30 | USD      |
+----------+-----------+----------------+----------------+----------+
Net Worth / Total Liquidity: $7,969.30
=======================================================================
```

### Budget Sentinel Alert Matrix
```text
======================= BUDGET PERFORMANCE REPORT ======================
+-------------------+---------+---------------+-----------+--------+----------+
| Category          | Limit   | Spent (Month) | Remaining | Used % | Status   |
+-------------------+---------+---------------+-----------+--------+----------+
| Food & Dining     | $400.00 |       $425.00 |     $0.00 | 106.3% | EXCEEDED |
| Bills & Utilities | $250.00 |         $0.00 |   $250.00 |   0.0% | OK       |
| Fun & Leisure     | $150.00 |         $0.00 |   $150.00 |   0.0% | OK       |
+-------------------+---------+---------------+-----------+--------+----------+
```

### Cash Flow Telemetry
```text
====================== CASH FLOW & ANALYTICS ==========================
Period             : 2026-09
Total Inflow       : $4,680.50
Total Outflow      : $425.00
Net Cash Savings   : $4,255.50
Savings Rate       : 90.92%
-----------------------------------------------------------------------
Top Expense Breakdown:
  - Food & Dining       : $425.00 (100.0%)
=======================================================================
```