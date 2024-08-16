# ATM
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

class Account {
    private String userId;
    private String pin;
    private double balance;
    private Map<String, Double> transactionHistory;

    public Account(String userId, String pin, double balance) {
        this.userId = userId;
        this.pin = pin;
        this.balance = balance;
        this.transactionHistory = new HashMap<>();
    }

    public String getUserId() {
        return userId;
    }

    public String getPin() {
        return pin;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public void addTransaction(String transactionId, double amount) {
        transactionHistory.put(transactionId, amount);
    }

    public Map<String, Double> getTransactionHistory() {
        return transactionHistory;
    }
}

public class ATM {
    private Account account;
    private Scanner scanner;

    public ATM(Account account) {
        this.account = account;
        this.scanner = new Scanner(System.in);
    }

    public void start() {
        System.out.println("Welcome to ATM System");
        System.out.print("Enter User ID: ");
        String userId = scanner.next();
        System.out.print("Enter PIN: ");
        String pin = scanner.next();

        if (userId.equals(account.getUserId()) && pin.equals(account.getPin())) {
            System.out.println("Login Succeskssful");
            atmMenu();
        } else {
            System.out.println("Invalid User ID or PIN");
        }
    }

    private void atmMenu() {
        while (true) {
            System.out.println("ATM Menu:");
            System.out.println("1. Transaction History");
            System.out.println("2. Withdraw");
            System.out.println("3. Deposit");
            System.out.println("4. Transfer");
            System.out.println("5. Quit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    displayTransactionHistory();
                    break;
                case 2:
                    withdraw();
                    break;
                case 3:
                    deposit();
                    break;
                case 4:
                    transfer();
                    break;
                case 5:
                    System.out.println("Goodbye!");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    private void displayTransactionHistory() {
        System.out.println("Transaction History:");
        for (Map.Entry<String, Double> entry : account.getTransactionHistory().entrySet()) {
            System.out.println("Transaction ID: " + entry.getKey() + ", Amount: " + entry.getValue());
        }
    }

    private void withdraw() {
        System.out.print("Enter amount to withdraw: ");
        double amount = scanner.nextDouble();
        if (amount > account.getBalance()) {
            System.out.println("Insufficient balance");
        } else {
            account.setBalance(account.getBalance() - amount);
            account.addTransaction("WD-" + System.currentTimeMillis(), -amount);
            System.out.println("Withdrawal successful. New balance: " + account.getBalance());
        }
    }

    private void deposit() {
        System.out.print("Enter amount to deposit: ");
        double amount = scanner.nextDouble();
        account.setBalance(account.getBalance() + amount);
        account.addTransaction("DP-" + System.currentTimeMillis(), amount);
        System.out.println("Deposit successful. New balance: " + account.getBalance());
    }

    private void transfer() {
        // Implement transfer functionality
        System.out.println("Transfer functionality is not implemented in this demo");
    }

    public static void main(String[] args) {
        Account account = new Account("user123", "1234", 1000.0);
        ATM atm = new ATM(account);
        atm.start();
    }
}
