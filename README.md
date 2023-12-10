# c-project
#include <iostream>
#include <vector>
#include <algorithm>
#include <iomanip>

using namespace std;

class Account {
private:
    int accountNumber;
    string accountHolder;
    double balance;

public:
    // Constructor
    Account(int number, string holder, double initialBalance)
        : accountNumber(number), accountHolder(holder), balance(initialBalance) {}

    // Accessors
    int getAccountNumber() const { return accountNumber; }
    string getAccountHolder() const { return accountHolder; }
    double getBalance() const { return balance; }

    // Functions
    void deposit(double amount) {
        balance += amount;
    }

    void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
        } else {
            cout << "Insufficient funds!" << endl;
        }
    }
};

class Bank {
private:
    vector<Account> accounts;

public:
    void addAccount() {
        int number;
        string holder;
        double initialBalance;

        cout << "Enter account number: ";
        cin >> number;

        cout << "Enter account holder's name: ";
        cin.ignore();  // Clear the newline left in the input buffer
        getline(cin, holder);

        cout << "Enter initial balance: ";
        cin >> initialBalance;

        Account newAccount(number, holder, initialBalance);
        accounts.push_back(newAccount);

        cout << "Account added successfully." << endl;
    }

    void removeAccount() {
        int accountNumber;
        cout << "Enter account number to remove: ";
        cin >> accountNumber;

        auto it = find_if(accounts.begin(), accounts.end(),
                          [accountNumber](const Account& acc) {
                              return acc.getAccountNumber() == accountNumber;
                          });

        if (it != accounts.end()) {
            accounts.erase(it);
            cout << "Account removed successfully." << endl;
        } else {
            cout << "Account not found." << endl;
        }
    }

    void displayAllAccounts() const {
        cout << "Account\tHolder\tBalance" << endl;
        for (const auto& account : accounts) {
            cout << account.getAccountNumber() << "\t" << account.getAccountHolder() << "\t$"
                 << fixed << setprecision(2) << account.getBalance() << endl;
        }
    }

    void performTransactions() {
        int accountNumber;
        cout << "Enter account number: ";
        cin >> accountNumber;

        auto it = find_if(accounts.begin(), accounts.end(),
                          [accountNumber](const Account& acc) {
                              return acc.getAccountNumber() == accountNumber;
                          });

        if (it != accounts.end()) {
            double amount;
            cout << "Enter transaction amount: ";
            cin >> amount;

            char choice;
            cout << "Enter 'D' for deposit or 'W' for withdrawal: ";
            cin >> choice;

            if (choice == 'D' || choice == 'd') {
                it->deposit(amount);
                cout << "Deposit successful." << endl;
            } else if (choice == 'W' || choice == 'w') {
                it->withdraw(amount);
                cout << "Withdrawal successful." << endl;
            } else {
                cout << "Invalid choice." << endl;
            }
        } else {
            cout << "Account not found." << endl;
        }
    }
};

void displayMenu() {
    cout << "\nMenu:\n";
    cout << "1. Display All Accounts\n";
    cout << "2. Add an Account\n";
    cout << "3. Remove an Account\n";
    cout << "4. Perform Transactions\n";
    cout << "5. Exit\n";
}

int main() {
    Bank bank;

    int choice;
    do {
        displayMenu();
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                bank.displayAllAccounts();
                break;
            case 2:
                bank.addAccount();
                break;
            case 3:
                bank.removeAccount();
                break;
            case 4:
                bank.performTransactions();
                break;
            case 5:
                cout << "Exiting program.\n";
                break;
            default:
                cout << "Invalid choice. Try again.\n";
        }

    } while (choice != 5);

    return 0;
}
