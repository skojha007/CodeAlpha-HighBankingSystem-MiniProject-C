# CodeAlpha High Banking System — Mini Project

A complete **Bank Account Management System** written in C, built as a mini project for the CodeAlpha internship. It runs entirely in the terminal and persists all data to binary and text files — no database required.

---

## Features

### Customer Operations
| # | Feature | PIN Required |
|---|---------|-------------|
| 1 | Create a new bank account (with 4-digit PIN setup) | — |
| 2 | Deposit funds | ✅ |
| 3 | Withdraw funds | ✅ |
| 4 | Check account balance | — |
| 5 | List all accounts | — |
| 6 | View personal transaction history | — |
| 7 | Transfer funds between accounts | ✅ |
| 8 | Change PIN | ✅ |
| 9 | Close account | ✅ |

### Admin Operations (password protected)
| # | Feature |
|---|---------|
| 1 | View all accounts with balances |
| 2 | View full transaction log |
| 3 | Search transaction history by account number |
| 4 | Apply annual interest rate to all accounts (with preview) |

---

## Getting Started

### Prerequisites
- GCC compiler (or any C99-compatible compiler)

### Compile
```bash
gcc main.c -o bank
```

### Run
```bash
./bank
```

---

## Usage

On first run, no accounts exist — choose **option 1** to create one.

```
*************************************
*   BANK ACCOUNT MANAGEMENT SYSTEM  *
*************************************
1.  Create New Account
2.  Deposit
3.  Withdraw
4.  Balance Enquiry
5.  List All Accounts
6.  Transaction History
7.  Transfer Funds
8.  Change PIN
9.  Close Account
10. Admin Mode
11. Exit
Enter your choice:
```

### Creating an Account
```
Enter Account Number: 1001
Enter Account Holder Name: John Doe
Enter Initial Deposit Amount: Rs.1000
Set a 4-digit PIN: 1234
Confirm PIN: 1234
Account created successfully! (Account Number: 1001)
```

### Deposit Example
```
Enter Account Number: 1001
Enter PIN for Account 1001: 1234
Enter amount to deposit: Rs.500
Amount Rs.500.00 deposited successfully!
New Balance: Rs.1500.00
```

### Transfer Example
```
Enter Source Account Number: 1001
Enter PIN for Account 1001: 1234
Enter Destination Account Number: 1002
Enter amount to transfer: Rs.200
Transfer successful!
Rs.200.00 transferred from Account 1001 to Account 1002.
```

### Admin Mode
Access with the master password (`admin123`):
```
*** ADMIN MENU ***
1. View All Accounts
2. View Full Transaction Log
3. Search Transactions by Account
4. Apply Interest to All Accounts
5. Back to Main Menu
```

**Interest preview before applying:**
```
--- Interest Preview at 6.00% per annum ---
Acct No.     Holder Name       Current Bal    Interest    New Balance
1001         John Doe          Rs.1500.00     Rs.90.00    Rs.1590.00
1002         Jane Smith        Rs.800.00      Rs.48.00    Rs.848.00
Apply this interest to all 2 account(s)? (y/n):
```

---

## Security

- Each account is protected by a **4-digit PIN**
- PIN verification allows a maximum of **3 attempts** before access is denied
- Lockout attempts are recorded in the transaction log
- Admin mode uses a **master password** with a 3-attempt lockout
- To change the admin password, update `#define ADMIN_PASSWORD` in `main.c`

---

## File Structure

```
main.c            — Full source code (single file)
account.dat       — Binary file storing all account data (auto-created)
transactions.log  — Human-readable log of every transaction (auto-created)
```

> **Note:** `account.dat` and `transactions.log` are created automatically on first run. Do not edit `account.dat` manually — it is a binary file.

---

## Transaction Log Format

Every operation is timestamped and appended to `transactions.log`:

```
[2026-06-14 10:32:05] Acct:1001   | ACCOUNT OPENED    | Amount: Rs.   1000.00 | Balance: Rs.   1000.00
[2026-06-14 10:33:12] Acct:1001   | DEPOSIT           | Amount: Rs.    500.00 | Balance: Rs.   1500.00
[2026-06-14 10:35:44] Acct:1001   | TRANSFER OUT      | Amount: Rs.    200.00 | Balance: Rs.   1300.00
[2026-06-14 10:35:44] Acct:1002   | TRANSFER IN       | Amount: Rs.    200.00 | Balance: Rs.    700.00
[2026-06-14 10:40:01] Acct:1001   | INTEREST APPLIED  | Amount: Rs.     78.00 | Balance: Rs.   1378.00
```

Logged event types: `ACCOUNT OPENED`, `DEPOSIT`, `WITHDRAWAL`, `TRANSFER OUT`, `TRANSFER IN`, `INTEREST APPLIED`, `PIN CHANGED`, `PIN LOCKOUT`, `ACCOUNT CLOSED`

---

## Validation Rules

| Rule | Detail |
|------|--------|
| Deposit amount | Must be > 0 |
| Withdrawal amount | Must be > 0 and ≤ current balance |
| Transfer amount | Must be > 0, source must have sufficient funds, source ≠ destination |
| PIN | Must be exactly 4 digits (1000–9999), confirmed on creation |
| Interest rate | Must be between 0 and 100 |
| Duplicate accounts | Account numbers must be unique |
| Max accounts | 100 accounts per file |

---

## Technical Details

- **Language:** C (C99)
- **Storage:** Binary file (`fwrite` / `fread`) for accounts, plain text for logs
- **Architecture:** Single-file (`main.c`), struct-based data model
- **Libraries:** `stdio.h`, `stdlib.h`, `string.h`, `time.h`

---

## Project Structure

```c
struct BankAccount {
    int   accountNumber;
    char  holderName[50];
    float balance;
    int   pin;
};
```

Key functions:

| Function | Purpose |
|----------|---------|
| `createAccount()` | Add a new account with PIN |
| `deposit()` | Credit funds after PIN check |
| `withdraw()` | Debit funds after PIN check |
| `transferFunds()` | Atomic debit/credit between accounts |
| `changePin()` | Update PIN after verifying old one |
| `closeAccount()` | Delete account after PIN + confirmation |
| `verifyPin()` | 3-attempt PIN gate |
| `logTransaction()` | Append timestamped entry to log |
| `viewTransactionHistory()` | Filter log by account number |
| `adminMode()` | Password-protected admin sub-menu |
| `adminApplyInterest()` | Preview and apply annual interest |
| `saveAllToFile()` | Persist all accounts to `account.dat` |
| `loadAllFromFile()` | Load all accounts on startup |

---

## Author

Developed as part of the **CodeAlpha Internship** — C Programming Mini Project.
