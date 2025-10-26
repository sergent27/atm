# atm
school project

import mysql.connector

con = mysql.connector.connect(
    host="localhost",
    user="root",
    password="",
    database="atm_system"
)
cursor = con.cursor()
print("✅ Database connected successfully!")


'''create_table_query = """
CREATE TABLE IF NOT EXISTS accounts (
    account_no INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    card_no VARCHAR(20) UNIQUE,
    pin VARCHAR(10),
    balance DECIMAL(10,2) DEFAULT 0.00,
    mobile_no VARCHAR(15),
    email VARCHAR(100)
);
"""


cur.execute(create_table_query)


print("✅ Table 'accounts' created successfully!")


cur.close()
con.close()'''



'''insert_query = "INSERT INTO accounts (name, card_no, pin, balance, mobile_no, email) VALUES (%s, %s, %s, %s, %s, %s)"
data = ("abc", "1234567890", "4321", 10000.00, "9876543210", "PPM@example.com")

cursor.execute(insert_query, data)
con.commit()'''

print("✅ Data inserted successfully!")

con.close()

# Main menu function
def main_menu():
    while True:
        print("\n--- ATM SYSTEM ---")
        print("1. Admin Login")
        print("2. User Login")
        print("3. Exit")
        choice = input("Enter choice: ")

        if choice == '1':
            admin_login()
        elif choice == '2':
            user_login()
        elif choice == '3':
            print("Exiting... Thank you!")
            break
        else:
            print("Invalid choice. Try again.")

# Admin login
def admin_login():
    print("\n--- Admin Login ---")
    admin_id = input("Enter Admin ID: ")
    admin_password = input("Enter Password: ")

    cursor.execute("SELECT * FROM admins WHERE admin_id=%s AND password=%s", (admin_id, admin_password))
    result = cursor.fetchone()

    if result:
        print("Admin login successful.")
    else:
        print("Invalid Admin credentials.")

# User login and operations
def user_login():
    print("\n--- User Login ---")
    acc_no = input("Enter Account Number: ")
    pin = input("Enter PIN: ")

    cursor.execute("SELECT * FROM users WHERE acc_no=%s AND pin=%s", (acc_no, pin))
    result = cursor.fetchone()

    if result:
        print("Login successful.")
        atm_menu(acc_no)
    else:
        print("Invalid account or PIN.")

# ATM options menu
def atm_menu(acc_no):
    while True:
        print("\n--- ATM MENU ---")
        print("1. Check Balance")
        print("2. Deposit")
        print("3. Withdraw")
        print("4. Change PIN")
        print("5. Logout")
        choice = input("Enter choice: ")

        if choice == '1':
            check_balance(acc_no)
        elif choice == '2':
            deposit_amount(acc_no)
        elif choice == '3':
            withdraw_amount(acc_no)
        elif choice == '4':
            change_pin(acc_no)
        elif choice == '5':
            print("Logged out.")
            break
        else:
            print("Invalid choice. Try again.")

# Check balance
def check_balance(acc_no):
    cursor.execute("SELECT balance FROM users WHERE acc_no=%s", (acc_no,))
    balance = cursor.fetchone()[0]
    print("Your balance is: Rs.", balance)

# Deposit
def deposit_amount(acc_no):
    amount = float(input("Enter amount to deposit: "))
    cursor.execute("UPDATE users SET balance = balance + %s WHERE acc_no=%s", (amount, acc_no))
    con.commit()
    print("Rs.", amount, "deposited successfully.")

# Withdraw
def withdraw_amount(acc_no):
    amount = float(input("Enter amount to withdraw: "))
    cursor.execute("SELECT balance FROM users WHERE acc_no=%s", (acc_no,))
    current_balance = cursor.fetchone()[0]

    if amount <= current_balance:
        cursor.execute("UPDATE users SET balance = balance - %s WHERE acc_no=%s", (amount, acc_no))
        con.commit()
        print("Rs.", amount, "withdrawn successfully.")
    else:
        print("Insufficient balance.")

# Change PIN
def change_pin(acc_no):
    new_pin = input("Enter new 4-digit PIN: ")
    cursor.execute("UPDATE users SET pin=%s WHERE acc_no=%s", (new_pin, acc_no))
    con.commit()
    print("PIN changed successfully.")



main_menu()
