
# ATM Interface


We have all come across ATMs in our cities and it is built on Java. This complex project consists of five different classes and is a console-based application. When the system starts the user is prompted with user id and user pin. On entering the details successfully, then ATM functionalities are unlocked. The project allows to perform following operations:


#####  1)Check Balance 

##### 2)Withdraw 
##### 3)Deposit 
##### 4)Transactions History 
##### 5)Transfer 
##### 5)Quit




```java
import java.util.Scanner;
import java.util.ArrayList;
import java.util.List;

class Transaction {
    private String type;
    private double amount;

    public Transaction(String type, double amount) {
        this.type = type;
        this.amount = amount;
    }

    public String getType() {
        return type;
    }

    public double getAmount() {
        return amount;
    }
}

class User {
    private int userId;
    private int pin;
    private double balance;
    private List<Transaction> transactionHistory;

    public User(int userId, int pin, double balance) {
        this.userId = userId;
        this.pin = pin;
        this.balance = balance;
        this.transactionHistory = new ArrayList<>();
    }

    public int getUserId() {
        return userId;
    }

    public int getPin() {
        return pin;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        balance += amount;
        transactionHistory.add(new Transaction("Deposit", amount));
    }

    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            transactionHistory.add(new Transaction("Withdrawal", amount));
        } else {
            System.out.println("Insufficient balance.");
        }
    }

    public void transfer(User recipient, double amount) {
        if (balance >= amount) {
            withdraw(amount);
            recipient.deposit(amount);
            transactionHistory.add(new Transaction("Transfer to " + recipient.getUserId(), amount));
            System.out.println("Transfer successful.");
        } else {
            System.out.println("Insufficient balance for the transfer.");
        }
    }

    public List<Transaction> getTransactionHistory() {
        return transactionHistory;
    }
}

public class ATM {
    public static void main(String[] args) {
        User user = new User(12345, 6789, 10000.0); 
        Scanner scanner = new Scanner(System.in);
        int userId, pin;
        double amount;
        int choice;

        System.out.println("Welcome to the ATM!");

        // User login
        System.out.print("Enter User ID: ");
        userId = scanner.nextInt();
        System.out.print("Enter PIN: ");
        pin = scanner.nextInt();

        if (userId == user.getUserId() && pin == user.getPin()) {
            System.out.println("Login successful.");

            do {
                System.out.println("\nSelect an option:");
                System.out.println("1. Check Balance");
                System.out.println("2. Deposit");
                System.out.println("3. Withdraw");
                System.out.println("4. Transfer");
                System.out.println("5. Transaction History");
                System.out.println("6. Quit");

                System.out.print("Enter your choice: ");
                choice = scanner.nextInt();

                switch (choice) {
                    case 1:
                        System.out.println("Balance: ₹" + user.getBalance());
                        break;
                    case 2:
                        System.out.print("Enter deposit amount: ₹");
                        amount = scanner.nextDouble();
                        user.deposit(amount);
                        System.out.println("Deposit successful.");
                        break;
                    case 3:
                        System.out.print("Enter withdrawal amount: ₹");
                        amount = scanner.nextDouble();
                        user.withdraw(amount);
                        break;
                    case 4:
                        System.out.print("Enter recipient's User ID: ");
                        int recipientId = scanner.nextInt();
                        System.out.print("Enter transfer amount: ₹");
                        amount = scanner.nextDouble();
                        if (recipientId == user.getUserId()) {
                            System.out.println("You can't transfer to yourself.");
                        } else {
                            user.transfer(new User(recipientId, 0, 0.0), amount);
                        }
                        break;
                    case 5:
                        List<Transaction> history = user.getTransactionHistory();
                        System.out.println("\nTransaction History:");
                        for (Transaction transaction : history) {
                            System.out.println(transaction.getType() + ": ₹" + transaction.getAmount());
                        }
                        break;
                    case 6:
                        System.out.println("Thank you for using the ATM. Goodbye!");
                        break;
                    default:
                        System.out.println("Invalid choice. Please select a valid option.");
                }
            } while (choice != 6);
        } else {
            System.out.println("Login failed. Please check your User ID and PIN.");
        }
    }
}

