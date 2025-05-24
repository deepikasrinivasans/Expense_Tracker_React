# Expense Tracker (ReactJS)
## Date: 24/05/2025
## Name: Deepika S
## Register Number: 212222230028

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM
## APP.js
```
import React, { useState } from "react";
import "./App.css";

function App() {
  const [transactions, setTransactions] = useState([]);
  const [text, setText] = useState("");
  const [amount, setAmount] = useState("");
  const [editId, setEditId] = useState(null);

  const addTransaction = (e) => {
    e.preventDefault();
    if (!text || !amount) return;

    if (editId) {
      setTransactions(
        transactions.map((t) =>
          t.id === editId ? { ...t, text, amount: parseFloat(amount) } : t
        )
      );
      setEditId(null);
    } else {
      const newTransaction = {
        id: Date.now(),
        text,
        amount: parseFloat(amount),
      };
      setTransactions([newTransaction, ...transactions]);
    }

    setText("");
    setAmount("");
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter((t) => t.id !== id));
    if (id === editId) {
      setEditId(null);
      setText("");
      setAmount("");
    }
  };

  const editTransaction = (id) => {
    const t = transactions.find((t) => t.id === id);
    if (t) {
      setText(t.text);
      setAmount(t.amount);
      setEditId(id);
    }
  };

  const income = transactions
    .filter((t) => t.amount > 0)
    .reduce((acc, t) => acc + t.amount, 0);
  const expense = transactions
    .filter((t) => t.amount < 0)
    .reduce((acc, t) => acc + t.amount, 0);

  return (
    <div className="container">
      <h2>Expense Tracker</h2>
      <div className="balance">
        <h3>Your Balance</h3>
        <h1>${(income + expense).toFixed(2)}</h1>
      </div>

      <div className="summary">
        <div className="income">
          <h4>Income</h4>
          <p className="money plus">+${income.toFixed(2)}</p>
        </div>
        <div className="expense">
          <h4>Expense</h4>
          <p className="money minus">-${Math.abs(expense).toFixed(2)}</p>
        </div>
      </div>

      <h3>History</h3>
      <ul className="list">
        {transactions.map((t) => (
          <li key={t.id} className={t.amount > 0 ? "plus" : "minus"}>
            {t.text} <span>${t.amount.toFixed(2)}</span>
            <div>
              <button
                onClick={() => editTransaction(t.id)}
                className="edit-btn"
              >
                ✎
              </button>
              <button
                onClick={() => deleteTransaction(t.id)}
                className="delete-btn"
              >
                x
              </button>
            </div>
          </li>
        ))}
      </ul>

      <h3>{editId ? "Edit transaction" : "Add new transaction"}</h3>
      <form onSubmit={addTransaction}>
        <div className="form-control">
          <label>Description</label>
          <input
            type="text"
            value={text}
            onChange={(e) => setText(e.target.value)}
            placeholder="Enter text..."
          />
        </div>
        <div className="form-control">
          <label>Amount (negative - expense, positive - income)</label>
          <input
            type="number"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            placeholder="Enter amount..."
          />
        </div>
        <button className="btn">
          {editId ? "Update transaction" : "Add transaction"}
        </button>
      </form>

      <footer className="footer">
        <p>Name: Deepika S | Register Number: 212222230028</p>
      </footer>
    </div>
  );
}

export default App;
```
## APP.css
```
body {
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #c1dfc4, #deecdd);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  max-width: 450px;
  width: 90%;
  background-color: #ffffffcc;
  padding: 30px 25px;
  border-radius: 16px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(8px);
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #2f4f4f;
}

.balance h1 {
  font-size: 2.4rem;
  text-align: center;
  margin-top: 5px;
  color: #1a202c;
}

.summary {
  display: flex;
  justify-content: space-around;
  background: #ecfdf5;
  border-radius: 10px;
  padding: 15px 0;
  margin: 20px 0;
}

.summary h4 {
  margin-bottom: 5px;
  font-weight: 500;
  color: #2d3748;
}

.money.plus {
  color: #38a169;
  font-weight: bold;
}

.money.minus {
  color: #e53e3e;
  font-weight: bold;
}

.list {
  list-style: none;
  padding: 0;
  margin: 20px 0;
  max-height: 200px;
  overflow-y: auto;
}

.list li {
  background: #f0fff4;
  padding: 12px;
  border-right: 6px solid;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.list li.plus {
  border-color: #38a169;
}

.list li.minus {
  border-color: #e53e3e;
}

.delete-btn {
  background: #e53e3e;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.2s ease;
  margin-left: 5px;
}

.delete-btn:hover {
  background: #c53030;
}

.edit-btn {
  background: #f6ad55;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.2s ease;
}

.edit-btn:hover {
  background: #dd6b20;
}

.form-control {
  margin-bottom: 15px;
}

.form-control label {
  display: block;
  margin-bottom: 6px;
  color: #2d3748;
  font-weight: 500;
}

.form-control input {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
  border: 1px solid #cbd5e0;
  border-radius: 6px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.form-control input:focus {
  outline: none;
  border-color: #3182ce;
}

.btn {
  width: 100%;
  padding: 12px;
  background: #2b6cb0;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.3s ease;
}

.btn:hover {
  background: #2c5282;
}

@media (max-width: 500px) {
  .container {
    padding: 20px;
  }
}

```
## OUTPUT
![1outputmwad](https://github.com/user-attachments/assets/11a68a1b-e193-4997-a38d-c5e810123a80)
![mwad](https://github.com/user-attachments/assets/1f336bc3-ca65-47c2-a962-846379058b71)

## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
