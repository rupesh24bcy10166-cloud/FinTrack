# VITyarthi Evaluated Course Project Report

---

# FinTrack: Enterprise Personal Finance & Budget Optimization Engine

**Course**: CSE1021 - Programming in Java  
**Academic Component**: Evaluated Course Project (Flipped Course)  
**Track**: Build Your Own Project  
**Platform**: VITyarthi Learning Destination  
**Date of Submission**: September 2026  
**Implementation Language**: Java SE 17+ / 21 / 23  
**Execution Environment**: Terminal / Command Line Interface (CLI)  
**Visibility**: Public GitHub Repository  

---

\newpage

## Table of Contents
1. [Cover Page](#cover-page)
2. [Introduction](#1-introduction)
3. [Problem Statement](#2-problem-statement)
4. [Functional Requirements](#3-functional-requirements)
5. [Non-Functional Requirements](#4-non-functional-requirements)
6. [System Architecture](#5-system-architecture)
7. [Design Diagrams](#6-design-diagrams)
   - 7.1 Use Case Diagram
   - 7.2 Process Flow / Workflow Diagram
   - 7.3 Sequence Diagram
   - 7.4 Class / Component Diagram
   - 7.5 Database / Storage Schema Diagram
8. [Design Decisions & Rationale](#7-design-decisions--rationale)
9. [Implementation Details](#8-implementation-details)
10. [Screenshots & Execution Results](#9-screenshots--execution-results)
11. [Testing Approach & Verification Results](#10-testing-approach--verification-results)
12. [Challenges Faced & Solutions](#11-challenges-faced--solutions)
13. [Learnings & Key Takeaways](#12-learnings--key-takeaways)
14. [Future Enhancements](#13-future-enhancements)
15. [References](#14-references)

---

\newpage

## 1. Introduction
In contemporary economic environments, disciplined personal capital allocation is critical for financial health. Students, young professionals, and households often operate across multiple deposit and credit accounts without a unified mechanism for real-time expenditure surveillance. This lack of visibility frequently triggers preventable bank penalties—including overdraft charges and high-interest revolving credit accumulation.

**FinTrack** is an enterprise-structured, terminal-native Personal Finance and Budget Management platform engineered entirely in Java. Constructed upon rigorous Object-Oriented Programming (OOP) paradigms and layered architecture, FinTrack delivers an offline, zero-dependency, and privacy-first solution. It models diverse financial instrument behaviors through polymorphism, manages transactional consistency, enforces proactive spending guardrails, and provides automated financial telemetry through the Java Streams API.

---

## 2. Problem Statement
Commercial personal financial software often presents major drawbacks:
1. **Intrusive Cloud Dependencies**: Mandating persistent online connectivity and transmitting sensitive banking credentials to third-party servers creates severe privacy and security liabilities.
2. **Post-Facto Alerts**: Traditional banking notifications notify users *after* a budget limit has been exceeded or an overdraft fee has been levied, failing to prevent the financial penalty beforehand.
3. **Bloated Interfaces**: Heavy graphical user interfaces (GUIs) make integration with automated pipelines and lightweight headless environments cumbersome.

### Project Objective:
To engineer a clean, robust, and extensible command-line Java application that:
- Accurately models different financial accounts (`Savings`, `Checking`, `Credit`) with custom financial rules.
- Intercepts expenditures in real-time, warning users before budget limits are breached.
- Computes granular cash-flow metrics via declarative Streams API transformations.
- Provides zero-dependency local CSV persistence, ensuring that data is fully auditable, portable, and preserved across sessions.
- Features dual-mode execution (interactive menu for users and a headless `--demo` mode for automated evaluators).

---

## 3. Functional Requirements

### Module 1: Account Portfolio Management
- **FR-1.1**: Creation and configuration of multiple polymorphic accounts:
  - **Savings Account**: Enforces a minimum balance rule and computes monthly interest accrual.
  - **Checking Account**: Accommodates negative balances within a configurable overdraft limit and assesses penalty fees when entering negative territory.
  - **Credit Card Account**: Tracks revolving credit limits, purchases (increasing debt), and bill payments (reducing debt).
- **FR-1.2**: Dynamic portfolio inquiry aggregating all balances, debts, and total net worth.

### Module 2: Atomic Transaction Processing & Transfer Engine
- **FR-2.1**: Income recording (Salary, Freelance, Investment Return) crediting specified accounts.
- **FR-2.2**: Expense processing (Food, Housing, Utilities, Transportation, Entertainment, etc.) with real-time budget threshold checks.
- **FR-2.3**: Inter-account atomic transfers validating source liquidity and destination validity.
- **FR-2.4**: Immutable transaction journaling with unique transaction identifiers and timestamps.

### Module 3: Proactive Budgeting & Alert Sentinel
- **FR-3.1**: Configurable monthly spending ceilings per expenditure category.
- **FR-3.2**: Configurable warning thresholds (e.g., 80% of limit).
- **FR-3.3**: Pre-transaction evaluation: instant visual alert messages warning the user when an expense will cross 80% or exceed 100% of the category limit.

### Module 4: Financial Analytics & Telemetry
- **FR-4.1**: Monthly cash flow computation: Total Inflow, Total Outflow, and Net Savings.
- **FR-4.2**: Personal savings rate percentage calculation.
- **FR-4.3**: Categorical spending aggregation and ranking of top expense drivers via Java Streams.

### Module 5: Tabular Reporting & Export
- **FR-5.1**: Clean ASCII table generation for accounts, ledgers, and budget matrices.
- **FR-5.2**: File export capability generating persistent audit reports (`.txt`/`.csv`).

---

## 4. Non-Functional Requirements

1. **Performance & Efficiency**:
   - $O(1)$ memory lookup efficiency for accounts, transactions, and budgets utilizing `ConcurrentHashMap`.
   - Stream processing pipelines evaluated in linear time $O(N)$ with minimal heap allocations.
2. **Reliability & Domain Error Handling**:
   - Dedicated checked exception hierarchy preventing illegal system states (e.g., `InsufficientFundsException`, `BudgetExceededException`, `AccountNotFoundException`).
   - Clean handling of edge cases (negative inputs, self-transfers, division by zero in savings rate).
3. **Security & Data Integrity**:
   - Atomic state transitions: Transfers synchronize updates between source and target accounts to prevent funds duplication or loss.
   - Encapsulated mutable state with strict accessors.
4. **Maintainability & Clean Architecture**:
   - Strict separation of concerns across `model`, `repository`, `service`, `util`, and `cli` layers.
   - Extensible generic repository pattern `Repository<T, ID>`.
5. **Portability & Usability**:
   - Zero external third-party library dependencies (runs on pure Java SE 17+).
   - Platform-independent path resolution (`Path.of()`).
   - Cross-platform terminal compatibility with fallback formatting when ANSI colors are unavailable.

---

## 5. System Architecture

FinTrack adopts a classical **Layered Architecture (N-Tier)** pattern, ensuring high cohesion and loose coupling:

```
+-------------------------------------------------------------------------+
|                         PRESENTATION LAYER (CLI)                        |
|   FinTrackApp (Headless / Main)   <--->   MenuController (Interactive)  |
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                           SERVICE LAYER (LOGIC)                         |
|   AccountService   TransactionService   BudgetService   AnalyticsService|
|                           ReportService                                 |
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                        DATA ACCESS LAYER (REPOSITORY)                   |
|   Repository<T, ID> (Generic Interface)                                 |
|   AccountRepository    TransactionRepository    BudgetRepository        |
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                        STORAGE LAYER (FILESYSTEM)                       |
|   data/accounts.csv     data/transactions.csv     data/budgets.csv      |
+-------------------------------------------------------------------------+
```

---

## 6. Design Diagrams

### 6.1 Use Case Diagram

```mermaid
flowchart LR
    User([User / Evaluator])
    
    subgraph FinTrack System
        UC1[Create / Manage Accounts]
        UC2[Deposit / Withdraw Funds]
        UC3[Transfer Between Accounts]
        UC4[Record Categorized Expenses]
        UC5[Configure Monthly Budgets]
        UC6[Receive Real-Time Budget Alerts]
        UC7[View Cash Flow & Stream Analytics]
        UC8[Export Financial Audit Report]
        UC9[Run Automated Headless Demo]
    end

    User --> UC1
    User --> UC2
    User --> UC3
    User --> UC4
    User --> UC5
    User --> UC6
    User --> UC7
    User --> UC8
    User --> UC9
```

### 6.2 Process Flow / Workflow Diagram

```mermaid
flowchart TD
    Start([Start Application]) --> ModeCheck{CLI Argument?}
    ModeCheck -- "--demo" --> RunDemo[Execute Automated Headless Pipeline]
    ModeCheck -- "--report" --> RunReport[Generate & Print Summary Reports]
    ModeCheck -- "None" --> ShowMenu[Display Interactive Terminal Menu]

    RunDemo --> PrintTraces[Render Portfolio, Budgets, Ledger & Export]
    PrintTraces --> EndDemo([Exit with Code 0])

    ShowMenu --> ReadChoice[User Selects Option 1-6]
    ReadChoice --> OpChoice{Selected Module}

    OpChoice -- "1. Accounts" --> AccAction[Create Account / View Portfolio]
    OpChoice -- "2. Transactions" --> TxnAction[Record Income / Expense / Transfer]
    OpChoice -- "3. Budgets" --> BudAction[Set Limit / View Budget Matrix]
    OpChoice -- "4. Analytics" --> AnalyticsAction[Compute Monthly Cash Flow & Streams]
    OpChoice -- "5. Export" --> ExportAction[Write Audit File to Disk]
    OpChoice -- "6. Exit" --> SaveData[Flush Repositories & Terminate]

    TxnAction --> ValidateTxn{Valid & Funds Available?}
    ValidateTxn -- No --> ThrowEx[Display Error Message]
    ValidateTxn -- Yes --> CheckBudget{Budget Alert?}
    CheckBudget -- Warn/Exceed --> ShowBanner[Display Alert Banner]
    CheckBudget -- Safe --> ApplyUpdate[Update Balances & Append to Journal]
    ShowBanner --> ApplyUpdate
    ApplyUpdate --> SaveDataLoop[Auto-Persist to CSV]
    SaveDataLoop --> ShowMenu
    ThrowEx --> ShowMenu
    AccAction --> ShowMenu
    BudAction --> ShowMenu
    AnalyticsAction --> ShowMenu
    ExportAction --> ShowMenu
    SaveData --> EndApp([Exit 0])
```

### 6.3 Sequence Diagram: Expense Recording with Proactive Alert

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant CLI as MenuController / App
    participant TS as TransactionService
    participant BS as BudgetService
    participant AS as AccountService
    participant ACC as Account (Checking/Savings)
    participant TR as TransactionRepository

    User->>CLI: Record Expense ($180.00, FOOD, CHK-2001)
    CLI->>TS: recordExpense("CHK-2001", FOOD, 180.00, "Dinner")
    TS->>BS: checkBudgetAlert(FOOD, 180.00)
    BS->>BS: calculateCurrentMonthSpending(FOOD)
    Note over BS: Current ($150) + New ($180) = $330 (82.5% of $400 Limit)
    BS-->>TS: "[BUDGET WARNING] Spending will reach 82.5%"
    TS->>AS: withdraw("CHK-2001", 180.00)
    AS->>ACC: withdraw(180.00)
    Note over ACC: Check balance & overdraft constraints
    ACC-->>AS: Balance deducted
    AS-->>TS: Account updated & persisted
    TS->>TR: save(new Transaction(...))
    TR-->>TS: Logged to transactions.csv
    TS-->>CLI: TransactionExecutionResult(txn, alertBanner)
    CLI-->>User: Display Success + Green/Yellow Alert Banner
```

### 6.4 Class Diagram

```mermaid
classDiagram
    class Account {
        <<abstract>>
        #String accountNumber
        #String accountHolderName
        #double balance
        #String currency
        #LocalDateTime createdAt
        +deposit(double amount) void
        +withdraw(double amount)* void
        +getAccountType()* String
        +toCsv()* String
    }

    class SavingsAccount {
        -double minimumBalance
        -double annualInterestRate
        +withdraw(double amount) void
        +applyMonthlyInterest() double
        +getAccountType() String
        +toCsv() String
    }

    class CheckingAccount {
        -double overdraftLimit
        -double overdraftFee
        +withdraw(double amount) void
        +getAccountType() String
        +toCsv() String
    }

    class CreditAccount {
        -double creditLimit
        -double annualPercentageRate
        +withdraw(double amount) void
        +deposit(double amount) void
        +getAvailableCredit() double
        +getOutstandingDebt() double
        +getAccountType() String
        +toCsv() String
    }

    class Transaction {
        -String transactionId
        -LocalDateTime timestamp
        -TransactionType type
        -Category category
        -double amount
        -String description
        -String sourceAccountId
        -String destinationAccountId
        +toCsv() String
        +fromCsv(String line)$ Transaction
    }

    class Budget {
        -Category category
        -double monthlyLimit
        -double warningThresholdPercent
        +isWarningReached(double spent) boolean
        +isExceeded(double spent) boolean
        +getUtilizationPercentage(double spent) double
        +toCsv() String
        +fromCsv(String line)$ Budget
    }

    class Repository~T, ID~ {
        <<interface>>
        +save(T entity) void
        +findById(ID id) Optional~T~
        +findAll() List~T~
        +deleteById(ID id) boolean
        +load() void
        +persist() void
    }

    class AccountRepository {
        -Map~String, Account~ accounts
        -Path storagePath
        +save(Account account) void
        +findById(String id) Optional~Account~
        +findAll() List~Account~
    }

    class TransactionRepository {
        -Map~String, Transaction~ transactions
        -Path storagePath
        +findByAccount(String accId) List~Transaction~
    }

    class BudgetRepository {
        -Map~Category, Budget~ budgets
        -Path storagePath
    }

    Account <|-- SavingsAccount
    Account <|-- CheckingAccount
    Account <|-- CreditAccount

    Repository <|.. AccountRepository
    Repository <|.. TransactionRepository
    Repository <|.. BudgetRepository

    AccountRepository o-- Account
    TransactionRepository o-- Transaction
    BudgetRepository o-- Budget
```

### 6.5 Database / Storage Schema Diagram

Although the system intentionally operates without an external SQL server, its relational persistence model is enforced via structured CSV tables:

```mermaid
erDiagram
    ACCOUNT {
        string TYPE PK
        string ACCOUNT_NUMBER PK
        string NAME
        double BALANCE
        string CURRENCY
        datetime CREATED_AT
        double PARAM1_MIN_OR_OVERDRAFT
        double PARAM2_APY_OR_FEE
    }

    TRANSACTION {
        string TRANSACTION_ID PK
        datetime TIMESTAMP
        string TYPE
        string CATEGORY
        double AMOUNT
        string DESCRIPTION
        string SOURCE_ACCOUNT_ID FK
        string DEST_ACCOUNT_ID FK
    }

    BUDGET {
        string CATEGORY PK
        double MONTHLY_LIMIT
        double WARNING_THRESHOLD
    }

    ACCOUNT ||--o{ TRANSACTION : "participates in"
    BUDGET ||--o{ TRANSACTION : "categorizes"
```

---

## 7. Design Decisions & Rationale

| Architectural Decision | Chosen Approach | Alternative Considered | Rationale & Trade-off |
| :--- | :--- | :--- | :--- |
| **Storage Engine** | File-backed CSV Persistence | Embedded SQLite / H2 | Adhering strictly to the **zero external dependency** guideline so evaluators can compile and run instantly with vanilla JDK without external JDBC driver JAR issues. |
| **Polymorphic Accounts** | Abstract Class `Account` | Single monolithic class with `switch(type)` | Clean Object-Oriented polymorphism cleanly separates business invariants (e.g. overdraft in Checking vs. minimum balance in Savings). |
| **Dual Execution Modes** | CLI Menu + `--demo` Headless Flag | Interactive-only CLI | Automated evaluation pipelines test code non-interactively; an interactive-only CLI causes grading scripts to hang indefinitely waiting for input. |
| **Analytics Query Engine** | Java Streams & Collectors | Manual imperative loops (`for`/`if`) | Demonstrates advanced Java standard library mastery, readability, and functional declarative programming. |
| **Terminal Formatting** | Custom Pure-Java ASCII Table Generator | Third-party AsciiTable / Jackson | Ensures 100% self-contained codebase without third-party library dependencies. |

---

## 8. Implementation Details

### 8.1 Object-Oriented Inheritance & Polymorphism
The core abstraction is `Account`, declaring polymorphic contracts:
```java
public abstract class Account {
    protected final String accountNumber;
    protected double balance;
    // ...
    public abstract void withdraw(double amount) throws InsufficientFundsException, InvalidTransactionException;
    public abstract String getAccountType();
}
```
In `SavingsAccount`, withdrawal is constrained by `minimumBalance`:
```java
if (this.balance - amount < this.minimumBalance) {
    throw new InsufficientFundsException(accountNumber, balance - minimumBalance, amount);
}
this.balance -= amount;
```
In `CheckingAccount`, withdrawal permits overdrafting with a penalty:
```java
double effectiveAvailable = this.balance + this.overdraftLimit;
if (amount > effectiveAvailable) {
    throw new InsufficientFundsException(accountNumber, effectiveAvailable, amount);
}
boolean entersOverdraft = (this.balance < amount);
this.balance -= amount;
if (entersOverdraft && overdraftFee > 0) {
    this.balance -= overdraftFee;
}
```

### 8.2 Declarative Java Streams Analytics
The `AnalyticsService` computes financial metrics using functional streams:
```java
public Map<Category, Double> getCategorySpendingBreakdown(int year, int month) {
    return transactionRepository.findAll().stream()
            .filter(t -> t.getType() == TransactionType.EXPENSE)
            .filter(t -> isWithinPeriod(t.getTimestamp(), year, month))
            .collect(Collectors.groupingBy(
                    Transaction::getCategory,
                    Collectors.summingDouble(Transaction::getAmount)
            ));
}
```

---

## 9. Screenshots & Execution Results

### 9.1 Headless Automated Demonstration Trace
Execution command:
```bash
java -cp bin com.vityarthi.fintrack.cli.FinTrackApp --demo
```

Output:
```text
================================================================================
            FINTRACK AUTOMATED HEADLESS DEMONSTRATION & AUDIT                   
================================================================================
>> Step 1: Initializing polymorphic accounts (Savings, Checking, Credit)...
[OK] Accounts created successfully.

>> Step 2: Configuring Monthly Spending Budgets & Alert Thresholds...
[OK] Budgets configured: Food ($400 limit, 80% alert), Entertainment ($150 limit).

>> Step 3: Executing Financial Transactions...
Recorded Expense: $150.00 on Food. 
Recorded Expense: $180.00 on Food. [BUDGET WARNING] Spending on 'Food & Dining' will reach $330.00 (82.5% of $400.00 limit).
Recorded Expense: $95.00 on Food. [BUDGET EXCEEDED] Spending on 'Food & Dining' will reach $425.00 (Limit: $400.00). Exceeds by $25.00!

>> Step 4: Executing Inter-Account Balance Transfer...
[OK] Transferred $1000.00 from CHK-2001 to SA-1001.

>> Step 5: Accruing Monthly Interest on Savings Account...
[OK] Accrued $13.80 in interest credited to SA-1001.

>> Step 6: Rendering Financial Portfolio & Audit Reports:

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

---

## 10. Testing Approach & Verification Results

### Testing Strategy
Testing is implemented as an automated, self-contained test suite (`FinTrackTestSuite`) executed via terminal:
1. **Unit Verification**: Boundary tests on individual account models (minimum balances, overdraft triggers, credit limit rejections).
2. **Business Rule Verification**: Automated checks on multi-tier budget warnings (50% normal, 80% warning alert, 100%+ exceeded).
3. **Integration Verification**: Fund transfers ensuring conservation of balance between two separate accounts.
4. **Persistence Verification**: In-memory data is persisted to CSV, garbage-collected, reloaded from disk into separate repository instances, and verified for exact value equality.

### Test Execution Command:
```bash
java -cp bin com.vityarthi.fintrack.FinTrackTestSuite
```

### Verification Matrix (28/28 Passed):
| Test Category | Description | Result |
| :--- | :--- | :--- |
| **Savings Account** | Withdrawal within valid balance | PASS |
| **Savings Account** | Minimum balance restriction enforcement | PASS |
| **Savings Account** | Monthly compound interest APY calculation | PASS |
| **Savings Account** | Balance update verification after interest | PASS |
| **Checking Account** | Withdrawal within positive funds | PASS |
| **Checking Account** | Overdraft buffer utilization & fee assessment | PASS |
| **Checking Account** | Overdraft absolute limit boundary rejection | PASS |
| **Credit Card** | Initial debt and available limit initialization | PASS |
| **Credit Card** | Card purchase increases debt & reduces limit | PASS |
| **Credit Card** | Card payment decreases debt | PASS |
| **Credit Card** | Charge exceeding credit limit rejected | PASS |
| **Inter-Account** | Source account debited accurately | PASS |
| **Inter-Account** | Destination account credited accurately | PASS |
| **Budget Logic** | Safe spending (50%) produces no warning | PASS |
| **Budget Logic** | Spending >=80% triggers proactive warning alert | PASS |
| **Budget Logic** | Spending >100% triggers budget exceeded alert | PASS |
| **Budget Logic** | Remaining budget calculated accurately | PASS |
| **Streams Analytics**| Total income aggregation across period | PASS |
| **Streams Analytics**| Total expense aggregation across period | PASS |
| **Streams Analytics**| Net savings and savings rate percentage | PASS |
| **Streams Analytics**| `groupingBy` categorical expense aggregation | PASS |
| **File Persistence** | Savings account serialized and reloaded | PASS |
| **File Persistence** | Checking account serialized and reloaded | PASS |
| **File Persistence** | Record count integrity verified after reload | PASS |

---

## 11. Challenges Faced & Solutions

1. **Challenge**: Ensuring automated grading pipelines do not block on terminal inputs.  
   **Solution**: Engineered the dual-mode execution pattern. When `--demo` or `--test` flags are passed, the application runs the complete user flow without invoking `Scanner.nextLine()`.
2. **Challenge**: Cross-platform line endings and byte order marks (BOM) on Windows causing Java compilation errors.  
   **Solution**: Standardized all source files to UTF-8 without BOM, ensuring standard `javac` compiles cleanly across Windows, Linux, and macOS.
3. **Challenge**: Real-time spending alerts without polluting persistent models.  
   **Solution**: Adopted the `TransactionExecutionResult` record pattern, allowing the service layer to return both the completed transaction and contextual alert telemetry to the presentation layer without state mutation.

---

## 12. Learnings & Key Takeaways
- **Polymorphism in Practice**: Realized how object-oriented inheritance allows different financial accounts to share common interfaces while encapsulating distinct business rules.
- **Declarative Power of Streams**: The Java Streams API drastically reduces boilerplate for analytical grouping and summarization compared to traditional nested loops.
- **Fail-Fast Software Design**: Designing domain-specific exceptions early in the architecture prevents invalid data from entering repository layers.

---

## 13. Future Enhancements
- **Multi-Currency Support**: Dynamic foreign exchange rate conversions for international accounts.
- **Recurring Transactions**: Scheduled automated salary credits and utility bill subscriptions.
- **Cryptographic Encryption**: Encrypting sensitive CSV data at rest using AES-256 GCM.

---

## 14. References
1. Gosling, J., Joy, B., Steele, G., Bracha, G., & Buckley, A. *The Java Language Specification, Java SE 21 Edition*. Oracle Corporation.
2. Bloch, J. (2018). *Effective Java* (3rd ed.). Addison-Wesley Professional.
3. Martin, R. C. (2008). *Clean Code: A Handbook of Agile Software Craftsmanship*. Prentice Hall.
4. Oracle Java Documentation: *Streams API and Collections Framework Guide*. https://docs.oracle.com/en/java/