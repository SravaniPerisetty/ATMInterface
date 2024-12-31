import java.util.*;

public class ATMInterface {

    // Class to represent a User Account
    static class Account {
        int accountNumber;
        String pin;
        double balance;
        List<String> transactionHistory;

        Account(int accountNumber, String pin, double balance) {
            this.accountNumber = accountNumber;
            this.pin = pin;
            this.balance = balance;
            this.transactionHistory = new ArrayList<>();
        }

        void deposit(double amount) {
            balance += amount;
            transactionHistory.add("Deposited: $" + amount);
        }

        boolean withdraw(double amount) {
            if (amount > balance) {
                return false;
            }
            balance -= amount;
            transactionHistory.add("Withdrawn: $" + amount);
            return true;
        }

        void addTransaction(String transaction) {
            transactionHistory.add(transaction);
        }

        List<String> getTransactionHistory() {
            return transactionHistory;
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Map<Integer, Account> accounts = new HashMap<>();

        System.out.println("Welcome to the ATM System!");

        while (true) {
            System.out.println("\nMain Menu:");
            System.out.println("1. Create Account");
            System.out.println("2. Access Existing Account");
            System.out.println("3. Exit");
            System.out.print("Select an option: ");
            int mainChoice = scanner.nextInt();

            switch (mainChoice) {
                case 1: // Create Account
                    System.out.print("Enter a new Account Number: ");
                    int newAccountNumber = scanner.nextInt();
                    if (accounts.containsKey(newAccountNumber)) {
                        System.out.println("Account number already exists. Try a different number.");
                        break;
                    }
                    System.out.print("Set a 4-digit PIN: ");
                    String newPin = scanner.next();
                    System.out.print("Enter initial deposit amount: ");
                    double initialDeposit = scanner.nextDouble();

                    accounts.put(newAccountNumber, new Account(newAccountNumber, newPin, initialDeposit));
                    System.out.println("Account created successfully!");
                    break;

                case 2: // Access Existing Account
                    System.out.print("Enter Account Number: ");
                    int accountNumber = scanner.nextInt();

                    System.out.print("Enter PIN: ");
                    String pin = scanner.next();

                    Account account = accounts.get(accountNumber);

                    if (account != null && account.pin.equals(pin)) {
                        System.out.println("Login successful!");

                        while (true) {
                            System.out.println("\nATM Menu:");
                            System.out.println("1. Check Balance");
                            System.out.println("2. Deposit Money");
                            System.out.println("3. Withdraw Money");
                            System.out.println("4. View Transaction History");
                            System.out.println("5. Logout");
                            System.out.print("Select an option: ");

                            int choice = scanner.nextInt();

                            switch (choice) {
                                case 1:
                                    System.out.println("Your current balance is: $" + account.balance);
                                    break;

                                case 2:
                                    System.out.print("Enter amount to deposit: ");
                                    double depositAmount = scanner.nextDouble();
                                    account.deposit(depositAmount);
                                    System.out.println("Deposit successful!");
                                    break;

                                case 3:
                                    System.out.print("Enter amount to withdraw: ");
                                    double withdrawAmount = scanner.nextDouble();
                                    if (account.withdraw(withdrawAmount)) {
                                        System.out.println("Withdrawal successful!");
                                    } else {
                                        System.out.println("Insufficient balance!");
                                    }
                                    break;

                                case 4:
                                    System.out.println("Transaction History:");
                                    account.getTransactionHistory().forEach(System.out::println);
                                    break;

                                case 5:
                                    System.out.println("Logging out. Goodbye!");
                                    break;

                                default:
                                    System.out.println("Invalid option. Please try again.");
                            }

                            if (choice == 5) {
                                break;
                            }
                        }
                    } else {
                        System.out.println("Invalid account number or PIN. Please try again.");
                    }
                    break;

                case 3: // Exit
                    System.out.println("Thank you for using the ATM System. Goodbye!");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }
}
