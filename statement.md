# FinTrack: Problem Statement & Scope Specification

## 1. Problem Statement
Managing personal finances across disparate bank accounts, credit cards, and cash channels remains a fragmented and error-prone process for modern individuals and young professionals. Most consumers struggle with overspending due to the absence of real-time threshold notifications, resulting in overdraft penalties and unmanageable credit card debt. Furthermore, commercial personal finance software frequently requires proprietary cloud logins, paid subscriptions, and intrusive tracking, creating privacy risks for sensitive financial ledger data. 

There is an acute need for a lightweight, secure, and privacy-preserving personal finance management engine that operates reliably in local terminal environments, enforces strict financial constraints (minimum balances, overdraft protection, credit limits), delivers real-time budget threshold warnings, and provides automated analytics without relying on third-party cloud infrastructure.

## 2. Project Scope
The **FinTrack** platform addresses this challenge by providing a terminal-native, object-oriented financial management engine built in Java. 

### In-Scope:
- **Multi-Type Account Portfolio Management**: Encapsulated models for Savings Accounts (with APY interest calculation and minimum balances), Checking Accounts (with configurable overdraft cushions and penalty fees), and Credit Accounts (with credit limit enforcement and balance tracking).
- **Transaction Processing & Inter-Account Transfers**: Atomic fund movements (Income, Expense, Transfer) maintaining double-entry conservation of capital and robust validation.
- **Proactive Budget Guardrails**: Monthly category spending allocations with real-time automated warnings when expenditures breach safety thresholds (e.g., 80%) and strict alerts upon budget exhaustion.
- **Financial Analytics & Cash-Flow Telemetry**: Dynamic metrics computed via the Java Streams API, including net savings rate, monthly inflow/outflow, and category-wise spending distributions.
- **Zero-Dependency Persistence**: Local CSV file storage enabling offline operation, data portability, and auditability.
- **Dual Execution Interface**: Interactive menu-driven console UI for everyday use, and a non-blocking headless execution pipeline (`--demo`) for automated evaluation and CI/CD testing.

### Out-of-Scope:
- Direct integration with proprietary online banking APIs (Plaid, Open Banking).
- Graphical UI (Swing/JavaFX) in compliance with the terminal-first evaluation mandate.

## 3. Target Users
1. **Students & Young Professionals**: Individuals seeking an intuitive, zero-cost, privacy-first tool to track income, control daily discretionary spending, and prevent accidental overdrawing.
2. **Privacy-Conscious Individuals**: Users who decline to link their bank credentials or personal transaction journals to commercial cloud applications.
3. **Academic Evaluators & Software Engineers**: Technical evaluators reviewing modular object-oriented software engineering, clean architecture, and test-driven Java development.

## 4. High-Level Features
- **Polymorphic Account Architecture**: Clean abstract domain model supporting differentiated business rules across savings, checking, and revolving credit facilities.
- **Real-Time Spending Sentinel**: Early-warning alerting mechanism evaluated at the instant an expense is recorded.
- **Streams-Powered Financial Intelligence**: Declarative, functional queries computing cash-flow balances and identifying top expenditure drivers.
- **Tabular Terminal Presentation**: Beautifully formatted ASCII ledger and portfolio tables with optional ANSI color highlighting.
- **Automated Headless Testability**: Integrated `--demo` and self-verifying test suite ensuring automated grading scripts run smoothly without interactive blocking.