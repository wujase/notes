# Designing an ATM

An automated teller machine (ATM) is an electronic telecommunications instrument that provides the clients of a financial institution with access to financial transactions in a public space without the need for a cashier or bank teller. ATMs are necessary as not all the bank branches are open every day of the week, and some customers may not be in a position to visit a bank each time they want to withdraw or deposit money.

## Requirements

- Card reader: to read the users’ ATM cards.
- Keypad: to enter information into the ATM e.g. PIN. cards.
- Screen: to display messages to the users.
- Cash dispenser: for dispensing cash.
- Deposit slot: For users to deposit cash or checks.
- Printer: for printing receipts.
- Communication/Network Infrastructure: it is assumed that the ATM has a communication infrastructure to communicate with the bank upon any transaction or activity.
- Balance inquiry: To see the amount of funds in each account.
- Deposit cash: To deposit cash.
- Deposit check: To deposit checks.
- Withdraw cash To withdraw money from their checking account.
- Transfer funds: To transfer funds to another account.

## How ATM works?

The ATM will be managed by an operator, who operates the ATM and refills it with cash and receipts. The ATM will serve one customer at a time and should not shut down while serving. To begin a transaction in the ATM, the user should insert their ATM card, which will contain their account information. Then, the user should enter their Personal Identification Number (PIN) for authentication. The ATM will send the user’s information to the bank for authentication; without authentication, the user cannot perform any transaction/service.

The user’s ATM card will be kept in the ATM until the user ends a session. For example, the user can end a session at any time by pressing the cancel button, and the ATM Card will be ejected. The ATM will maintain an internal log of transactions that contains information about hardware failures; this log will be used by the ATM operator to resolve any issues.

- Identify the system user through their PIN.
- In the case of depositing checks, the amount of the check will not be added instantly to the user account; it is subject to manual verification and bank approval.
- It is assumed that the bank manager will have access to the ATM’s system information stored in the bank database.
- It is assumed that user deposits will not be added to their account immediately because it will be subject to verification by the bank.
- It is assumed the ATM card is the main player when it comes to security; users will authenticate themselves with their debit card and security pin.

## Use Case Diagram

### Actors

- Operator
- Customer
- Bank Manager

### Processes

- Turning the ATM ON/OFF using the designated Key-Switch.
- Refilling the ATM with cash.
- Refilling the ATM’s printer with receipts.
- Refilling the ATM’s printer with INK.
- Take out deposited cash and checks.
- Balance inquiry: the user can view his/her account balance.
- Cash withdrawal: the user can withdraw a certain amount of cash.
- Deposit funds: the user can deposit cash or checks.
- Transfer funds: the user can transfer funds to other accounts.
- Generate a report to check total deposits.
- Generate a report to check total withdrawals.
- Print total deposits/withdrawal reports.
- Checks the remaining cash in the ATM.

![Use Case Diagram of an ATM](use-case-diagram-of-an-atm.svg)

## Class Diagram

Here are the main classes of the ATM System:

- ATM: The main part of the system for which this software has been designed. It has attributes like ‘atmID’ to distinguish it from other available ATMs, and ‘location’ which defines the physical address of the ATM.
- CardReader: To encapsulate the ATM’s card reader used for user authentication.
- CashDispenser: To encapsulate the ATM component which will dispense cash.
- Keypad: The user will use the ATM’s keypad to enter their PIN or amounts.
- Screen: Users will be shown all messages on the screen and they will select different transactions by touching the screen.
- Printer: To print receipts.
- DepositSlot: User can deposit checks or cash through the deposit slot.
- Bank: To encapsulate the bank which ownns the ATM. The bank will hold all the account information and the ATM will communicate with the bank to perform customer transactions.
- Account: We’ll have two types of accounts in the system: 1)Checking and 2)Saving.
- Customer: This class will encapsulate the ATM’s customer. It will have the customer’s basic information like name, email, etc.
- Card: Encapsulating the ATM card that the customer will use to authenticate themselves. Each customer can have one card.
- Transaction: Encapsulating all transactions that the customer can perform on the ATM, like BalanceInquiry, Deposit, Withdraw, etc.

![Class Diagram of an ATM](class-diagram-of-an-atm.png)

![UML Conventions](uml-conventions.svg)

## Activity Diagram

### Customer Authentication

![Activity Diagram of an ATM - Customer Authentication](activity-diagram-of-an-atm-customer-authentication.svg)

### Withdraw

![Activity Diagram of an ATM - Withdraw](activity-diagram-of-an-atm-withdraw.svg)

### Customer Authentication

![Activity Diagram of an ATM - Deposit Check](activity-diagram-of-an-atm-deposit-check.svg)

### Customer Authentication

![Activity Diagram of an ATM - Transfer](activity-diagram-of-an-atm-transfer.png)

## Sequence Diagram

### Balance Inquiry

![Sequence Diagram of an ATM - Balance Inquiry](sequence-diagram-of-an-atm-balance-inquiry.svg)

## Code

### Constants and Enums

```java
public enum TransactionType {
  BALANCE_INQUIRY, DEPOSIT_CASH, DEPOSIT_CHECK, WITHDRAW, TRANSFER
}

public enum TransactionStatus {
  SUCCESS, FAILURE, BLOCKED, FULL, PARTIAL, NONE
}

public enum CustomerStatus {
  ACTIVE, BLOCKED, BANNED, COMPROMISED, ARCHIVED, CLOSED, UNKNOWN
}

public class Address {
  private String streetAddress;
  private String city;
  private String state;
  private String zipCode;
  private String country;
}
```

### Customer, Card, and Account

```java
// For simplicity, we are not defining getter and setter functions. The reader can
// assume that all class attributes are private and accessed through their respective
// public getter method and modified only through their public setter function.

public class Customer {
  private String name;
  private String email;
  private String phone;
  private Address address;
  private CustomerStatus status;

  private Card card;
  private Account account;

  public boolean makeTransaction(Transaction transaction);
  public Address getBillingAddress();
}

public class Card {
  private String cardNumber;
  private String customerName;
  private Date cardExpiry;
  private int pin;

  public Address getBillingAddress();
}

public class Account {
  private int accountNumber;
  private double totalBalance;
  private double availableBalance;

  public double getAvailableBalance();
}

public class SavingAccount extends Account {
  private double withdrawLimit;
}

public class CheckingAccount extends Account {
  private String debitCardNumber;
}
```

### Bank, ATM, CashDispenser, Keypad, Screen, Printer and DepositSlot

```java
public class Bank {
  private String name;
  private String bankCode;

  public String getBankCode();
  public boolean addATM();
}

public class ATM {
  private int atmID;
  private Address location;

  private CashDispenser cashDispenser;
  private Keypad keypad;
  private Screen screen;
  private Printer printer;
  private CheckDeposit checkDeposit;
  private CashDeposit cashDeposit;

  public boolean authenticateUser();
  public boolean makeTransaction(Customer customer, Transaction transaction);
}

public class CashDispenser {
  private int totalFiveDollarBills;
  private int totalTwentyDollarBills;

  public boolean dispenseCash(double amount);
  public boolean canDispenseCash();
}

public class Keypad {
  public String getInput();
}

public class Screen {
  public boolean showMessage(String message);
  public TransactionType getInput();
}

public class Printer {
  public boolean printReceipt(Transaction transaction);
}

public abstract class DepositSlot {
  private double totalAmount;
  public double getTotalAmount();
}

public class CheckDepositSlot extends DepositSlot {
  public double getCheckAmount();
}

public class CashDepositSlot extends DepositSlot {
  public double receiveDollarBill();
}
```

### Transaction and Its Subclasses

```java
public abstract class Transaction {
  private int transactionId;
  private Date creationTime;
  private TransactionStatus status;
  public boolean makeTransation();
}

public class BalanceInquiry extends Transaction {
  private int accountId;
  public double getAccountId();
}

public abstract class Deposit extends Transaction {
  private double amount;
  public double getAmount();
}

public class CheckDeposit extends Deposit {
  private String checkNumber;
  private String bankCode;

  public String getCheckNumber();
}

public class CashDeposit extends Deposit {
  private double cashDepositLimit;
}

public class Withdraw extends Transaction {
  private double amount;
  public double getAmount();
}

public class Transfer extends Transaction {
  private int destinationAccountNumber;
  public int getDestinationAccount();
}
```
