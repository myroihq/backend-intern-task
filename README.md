# Backend Developer Intern Test: Node.js & PostgreSQL

**Time: 2-3 hours**

---

## Task: Build a Simple REST API for an EMI Calculator with Prepayment Option

**Objective:**  
Develop a simple REST API using Node.js and PostgreSQL to calculate EMI (Equated Monthly Installment) with the added option of prepayments or extra EMI payments. The API should return a month-wise breakdown of payments, including how prepayments impact the loan.

---

## Instructions:

### 1. Database Setup:
- Create a PostgreSQL database with a table to store the following information:
  - `id` (primary key, auto-increment)
  - `loan_amount` (decimal)
  - `interest_rate` (decimal)
  - `loan_tenure_months` (integer)
  - `emi` (decimal)
  - `prepayment_amount` (decimal, default null) - stores any extra payments made during the loan.
  - `remaining_balance` (decimal) - updates based on monthly payments and prepayments.

### 2. API Endpoints:

#### POST `/api/calculate-emi`:
- Accepts user inputs: loan amount, interest rate, loan tenure in months, and an optional prepayment amount.
- Calculates the EMI based on the formula:

EMI = (P * R * (1 + R)^N) / ((1 + R)^N - 1)


  Where:
  - `P` = Loan amount
  - `R` = Monthly interest rate (annual rate/12/100)
  - `N` = Loan tenure (in months)
- If a prepayment or extra EMI is made, recalculate the remaining balance and adjust the loan tenure accordingly.
- Stores the inputs, calculated EMI, prepayments (if any), and updated balance in the PostgreSQL database.
- Returns the calculated EMI, prepayment details, and a **month-wise breakdown** of payments, showing:
  - EMI amount
  - Interest paid
  - Principal paid
  - Prepayment made (if any)
  - Remaining loan balance for each month

#### GET `/api/emis`:
- Fetches and returns all EMI records from the database.

#### GET `/api/emi/:id`:
- Fetches and returns a specific EMI record by its ID, including the month-wise breakdown of payments.

---

## Requirements:
- Use **Express.js** for building the REST API.
- Use **Sequelize ORM** to interact with PostgreSQL.
- Ensure the option for handling **prepayment** or **extra EMI payment** is included, recalculating the remaining balance and updating future EMIs or tenure accordingly.
- Write clean, maintainable code with appropriate comments.
- Handle basic error scenarios (e.g., invalid inputs, database connection failures).
- Ensure that the API follows RESTful principles and responds with JSON.

---

## Deliverables:
- A zipped folder containing:
  - The complete Node.js project.
  - SQL script to create the necessary PostgreSQL table.
  - A `README.md` with instructions on how to run the project (including setting up the database and environment variables).
  
- API should be tested using tools like Postman, and the responses should be provided as screenshots in the submission.

- The **POST** response for EMI calculation should include:
  - The EMI amount
  - The prepayment or extra EMI amount (if applicable)
  - A month-wise breakdown, showing for each month:
    - EMI paid
    - Principal paid
    - Interest paid
    - Prepayment made (if any)
    - Remaining balance

---

### Example API Response:

```json
{
  "loanAmount": 500000,
  "interestRate": 8.5,
  "loanTenureMonths": 60,
  "emi": 10234.65,
  "prepayment": 20000,
  "monthWisePayments": [
    {
      "month": 1,
      "emiPaid": 10234.65,
      "interestPaid": 3541.67,
      "principalPaid": 6692.98,
      "prepayment": 20000,
      "remainingBalance": 473307.02
    },
    {
      "month": 2,
      "emiPaid": 10234.65,
      "interestPaid": 3346.70,
      "principalPaid": 6887.95,
      "prepayment": 0,
      "remainingBalance": 466419.07
    }
  ]
}
