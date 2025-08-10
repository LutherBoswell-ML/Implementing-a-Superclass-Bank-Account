# Implementing-a-Superclass-Bank-Account
Implement a superclass BankAccount that has the following fields and methods.

<img width="1052" height="506" alt="Screenshot 2025-08-10 at 7 09 14 PM" src="https://github.com/user-attachments/assets/68831945-0998-4bd3-8a46-038e9dd5d12d" />

SOURCE CODE 

// BankAssignmentVSCode.java
// Single-file version for quick run in VS Code.
// Run: javac BankAssignmentVSCode.java && java BankAssignmentVSCode
// Or click "Run" in VS Code.

import java.text.NumberFormat;

public class BankAssignmentVSCode {
    public static void main(String[] args) {
        // Demo: Checking account with 1.5% interest
        CheckingAccount ca = new CheckingAccount("Luther", "Boswell", 1001, 0.015);

        ca.displayAccount();
        System.out.println();

        ca.deposit(250.00);
        System.out.println("Deposited $250.00");
        ca.displayAccount();
        System.out.println();

        ca.processWithdrawal(60.00); // normal
        ca.displayAccount();
        System.out.println();

        ca.processWithdrawal(300.00); // overdraft + $30 fee
        ca.displayAccount();
        System.out.println();

        // Base BankAccount demo
        BankAccount ba = new BankAccount("Boswell", "Luther", 2002);
        ba.deposit(100.00);
        ba.withdrawal(40.00);
        ba.accountSummary();
    }
}

// ===== Superclass =====
class BankAccount {
    private String firstName;
    private String lastName;
    private int accountID;
    private double balance;

    public BankAccount() {
        this.balance = 0.0;
    }

    public BankAccount(String firstName, String lastName, int accountID) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.accountID = accountID;
        this.balance = 0.0;
    }

    public void deposit(double amount) {
        if (amount < 0) throw new IllegalArgumentException("Deposit amount cannot be negative.");
        balance += amount;
    }

    public void withdrawal(double amount) {
        if (amount < 0) throw new IllegalArgumentException("Withdrawal amount cannot be negative.");
        balance -= amount;
    }

    public String getFirstName() { return firstName; }
    public void setFirstName(String firstName) { this.firstName = firstName; }

    public String getLastName() { return lastName; }
    public void setLastName(String lastName) { this.lastName = lastName; }

    public int getAccountID() { return accountID; }
    public void setAccountID(int accountID) { this.accountID = accountID; }

    public double getBalance() { return balance; }

    protected void adjustBalance(double delta) { this.balance += delta; }

    public void accountSummary() {
        NumberFormat nf = NumberFormat.getCurrencyInstance();
        System.out.println("===== Account Summary =====");
        System.out.println("Name:        " + firstName + " " + lastName);
        System.out.println("Account ID:  " + accountID);
        System.out.println("Balance:     " + nf.format(balance));
    }
}

// ===== Subclass =====
class CheckingAccount extends BankAccount {
    private double interestRate; // e.g., 0.015 = 1.5%

    public CheckingAccount() { super(); }

    public CheckingAccount(String firstName, String lastName, int accountID, double interestRate) {
        super(firstName, lastName, accountID);
        this.interestRate = interestRate;
    }

    public double getInterestRate() { return interestRate; }
    public void setInterestRate(double interestRate) { this.interestRate = interestRate; }

    public void processWithdrawal(double amount) {
        if (amount < 0) throw new IllegalArgumentException("Withdrawal amount cannot be negative.");

        double current = getBalance();
        NumberFormat nf = NumberFormat.getCurrencyInstance();

        if (amount <= current) {
            super.withdrawal(amount);
            System.out.println("Withdrawal of " + nf.format(amount) + " processed. No fee applied.");
        } else {
            double overdraftFee = 30.00;
            adjustBalance(-(amount + overdraftFee));
            System.out.println("Overdraft withdrawal of " + nf.format(amount) + " processed.");
            System.out.println("A $30 overdraft fee has been assessed.");
            System.out.println("Your new balance is " + nf.format(getBalance()) + " (negative indicates overdraft).");
        }
    }

    public void displayAccount() {
        super.accountSummary();
        System.out.println("Interest Rate: " + (interestRate * 100.0) + "%");
    }
}
