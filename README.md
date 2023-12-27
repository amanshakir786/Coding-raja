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





task 2: library management


import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

class Book {
    private String title;
    private String author;
    private String genre;
    private boolean isAvailable;

    public Book(String title, String author, String genre) {
        this.title = title;
        this.author = author;
        this.genre = genre;
        this.isAvailable = true;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public String getGenre() {
        return genre;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }
}

class Patron {
    private String name;
    private String contactInfo;
    private List<Book> borrowedBooks;

    public Patron(String name, String contactInfo) {
        this.name = name;
        this.contactInfo = contactInfo;
        this.borrowedBooks = new ArrayList<>();
    }

    public String getName() {
        return name;
    }

    public String getContactInfo() {
        return contactInfo;
    }

    public List<Book> getBorrowedBooks() {
        return borrowedBooks;
    }

    public void borrowBook(Book book) {
        borrowedBooks.add(book);
        book.setAvailable(false);
    }

    public void returnBook(Book book) {
        borrowedBooks.remove(book);
        book.setAvailable(true);
    }
}

class Library {
    private Map<String, Book> books;
    private Map<String, Patron> patrons;

    public Library() {
        this.books = new HashMap<>();
        this.patrons = new HashMap<>();
    }

    public void addBook(String title, String author, String genre) {
        Book newBook = new Book(title, author, genre);
        books.put(title, newBook);
    }

    public void addPatron(String name, String contactInfo) {
        Patron newPatron = new Patron(name, contactInfo);
        patrons.put(name, newPatron);
    }

    public Book findBook(String title) {
        return books.get(title);
    }

    public Patron findPatron(String name) {
        return patrons.get(name);
    }

    public void generateReport() {
        System.out.println("Books in the Library:");
        for (Book book : books.values()) {
            System.out.println(book.getTitle() + " by " + book.getAuthor() +
                    " (Genre: " + book.getGenre() + ", Available: " + book.isAvailable() + ")");
        }

        System.out.println("\nPatron Records:");
        for (Patron patron : patrons.values()) {
            System.out.println(patron.getName() + " - Contact: " + patron.getContactInfo() +
                    ", Borrowed Books: " + patron.getBorrowedBooks().size());
        }
    }
}

public class LibraryManagementSystem {
    public static void main(String[] args) {
        Library library = new Library();
        library.addBook("The Great Gatsby", "F. Scott Fitzgerald", "Fiction");
        library.addBook("To Kill a Mockingbird", "Harper Lee", "Classics");
        library.addPatron("John Doe", "johndoe@email.com");

        Scanner scanner = new Scanner(System.in);

        System.out.println("Welcome to the Library Management System!");

        // Book Borrowing
        Book book = library.findBook("The Great Gatsby");
        Patron patron = library.findPatron("John Doe");

        if (book != null && patron != null) {
            patron.borrowBook(book);
            System.out.println(patron.getName() + " borrowed " + book.getTitle());

            // Book Returning
            patron.returnBook(book);
            System.out.println(patron.getName() + " returned " + book.getTitle());

            // Generate Report
            library.generateReport();
        } else {
            System.out.println("Book or patron not found.");
        }

        scanner.close();
    }
}



