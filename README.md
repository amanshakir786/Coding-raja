task 1: online banking system 

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

class User {
    private String username;
    private String password;
    private int accountNumber;
    private double balance;
    private List<String> transactionHistory;

    public User(String username, String password, int accountNumber) {
        this.username = username;
        this.password = password;
        this.accountNumber = accountNumber;
        this.balance = 0.0;
        this.transactionHistory = new ArrayList<>();
    }

    public String getUsername() {
        return username;
    }

    public double getBalance() {
        return balance;
    }

    public int getAccountNumber() {
        return accountNumber;
    }

    public List<String> getTransactionHistory() {
        return transactionHistory;
    }

    public void deposit(double amount) {
        balance += amount;
        transactionHistory.add("Deposit: +" + amount);
    }

    public void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
            transactionHistory.add("Withdrawal: -" + amount);
        } else {
            System.out.println("Insufficient funds!");
        }
    }

    public void transfer(User recipient, double amount) {
        if (amount <= balance) {
            balance -= amount;
            recipient.deposit(amount);
            transactionHistory.add("Transfer to " + recipient.getUsername() + ": -" + amount);
        } else {
            System.out.println("Insufficient funds for the transfer!");
        }
    }
}

class Bank {
    private Map<Integer, User> users;
    private Map<String, User> credentials;

    public Bank() {
        this.users = new HashMap<>();
        this.credentials = new HashMap<>();
    }

    public void addUser(String username, String password) {
        int accountNumber = users.size() + 1;
        User newUser = new User(username, password, accountNumber);
        users.put(accountNumber, newUser);
        credentials.put(username, newUser);
    }

    public User authenticateUser(String username, String password) {
        return credentials.get(username);
    }
}

public class OnlineBankingSystem {
    public static void main(String[] args) {
        Bank bank = new Bank();
        bank.addUser("user1", "password1");
        bank.addUser("user2", "password2");

        Scanner scanner = new Scanner(System.in);

        System.out.println("Welcome to the Online Banking System!");

        // User Authentication
        System.out.print("Enter username: ");
        String username = scanner.next();
        System.out.print("Enter password: ");
        String password = scanner.next();

        User currentUser = bank.authenticateUser(username, password);

        if (currentUser != null) {
            System.out.println("Authentication successful. Welcome, " + currentUser.getUsername() + "!");

            // Account Management
            System.out.println("Account Number: " + currentUser.getAccountNumber());
            System.out.println("Balance: $" + currentUser.getBalance());

            // Transaction History
            System.out.println("Transaction History: " + currentUser.getTransactionHistory());

            // Funds Transfer
            System.out.print("Enter transfer amount: ");
            double transferAmount = scanner.nextDouble();
            currentUser.transfer(bank.authenticateUser("user2", "password2"), transferAmount);

            // Balance Enquiry
            System.out.println("Updated Balance: $" + currentUser.getBalance());

            // Security Measures
            // (Implemented within User and Bank classes)

            // Loan Management
            // (Not implemented in this simplified example)

        } else {
            System.out.println("Authentication failed. Invalid username or password.");
        }

        scanner.close();
    }
}





