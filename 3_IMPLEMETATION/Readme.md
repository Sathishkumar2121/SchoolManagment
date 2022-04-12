import pickle
import os
import pathlib


class Account:
    """it is used to get an inputs"""
    acc_no = 0
    name = ''
    deposit = 0
    type = ''

    def create_account(self):
	    """it is used to create an account"""
        self.acc_no = int(input("Enter the account no : "))
        self.name = input("Enter the account holder name : ")
        self.type = input("Ente the type of account [C/S] : ")
        print("Enter The Initial amount")
        self.deposit = int(input("minimum 500 for Saving and 1000 for current: "))
        print("\n\nCongrats! New Account Created")

    def show_account(self):
		"""it is used to show an account details"""
        print("Account acc_number : ", self.acc_no)
        print("Account Holder Name : ", self.name)
        print("Type of Account", self.type)
        print("Balance : ", self.deposit)

    def modify_account(self):
		"""it is used to modify an account"""
        print("Account acc_number : ", self.acc_no)
        self.name = input("Modify Account Holder Name :")
        self.type = input("Modify type of Account :")
        self.deposit = int(input("Modify Balance :"))

    def deposit_amount(self, amount):
		"""it is used to deposit an account"""
        self.deposit += amount

    def withdraw_amount(self, amount):
		"""it is used to withdraw an account"""
        self.deposit -= amount

    def report(self):
		"""it is used to report an account"""
        print(self.acc_no, " ", self.name, " ", self.type, " ", self.deposit)

    def get_account_no(self):
		"""it is used to get an account"""
        return self.acc_no

    def get_acccount_holder_name(self):
		"""it is used to get an account holder name"""
        return self.name

    def get_account_type(self):
		"""it is used to get an account type"""
        return self.type

    def get_deposit(self):
		"""it is used to get an deposit"""
        return self.deposit


def intro():
	"""it is used to INTRO"""
    print("\t\t\t\t\tBANK MANAGEMENT SYSTEM developed by SATHISH")
    input()


def write_account():
	"""it is used to get account details"""
    account = Account()
    account.create_account()
    write_accounts_file(account)


def write_account():
	"""it is used to write an account"""
    file = pathlib.Path("accounts.data")
    if file.exists():
        infile = open('accounts.data', 'rb')
        mylist = pickle.load(infile)
        for item in mylist:
            print(item.acc_no, " ", item.name, " ", item.type, " ", item.deposit)
        infile.close()
    else:
        print("No records to display")


def display_sp(acc_no):
	"""it is used to display"""
    file = pathlib.Path("accounts.data")
    found = False
    if file.exists():
        infile = open('accounts.data', 'rb')
        mylist = pickle.load(infile)
        infile.close()

        for item in mylist:
            if item.acc_no == acc_no:
                print("Your account Balance is = ", item.deposit)
                found = True
    else:
        print("No records to Search")
    if not found:
        print("No existing record with this acc_number")


def deposit_and_withdraw(acc_no1, acc_no2):
	"""it is used to withdraw and deposit"""
    file = pathlib.Path("accounts.data")
    if file.exists():
        infile = open('accounts.data', 'rb')
        mylist = pickle.load(infile)
        infile.close()
        os.remove('accounts.data')
        for item in mylist:
            if item.acc_no == acc_no1:
                if acc_no2 == 1:
                    amount = int(input("Enter the amount to deposit : "))
                    item.deposit += amount
                    print("Your account is updated")
                elif acc_no2 == 2:
                    amount = int(input("Enter the amount to withdraw : "))
                    if amount <= item.deposit:
                        item.deposit -= amount
                    else:
                        print("Amount is large! ***ERROR 404***")

    else:
        print("No records to Search")
    outfile = open('newaccounts.data', 'wb')
    pickle.dump(mylist, outfile)
    outfile.close()
    os.rename('newaccounts.data', 'accounts.data')


def delete_account(acc_no):
	"""it is used to delete an account"""
    file = pathlib.Path("accounts.data")
    if file.exists():
        infile = open('accounts.data', 'rb')
        oldlist = pickle.load(infile)
        infile.close()
        newlist = []
        for item in oldlist:
            if item.acc_no != acc_no:
                newlist.append(item)
        os.remove('accounts.data')
        outfile = open('newaccounts.data', 'wb')
        pickle.dump(newlist, outfile)
        outfile.close()
        os.rename('newaccounts.data', 'accounts.data')


def modify_account(acc_no):
	"""it is used to modify """
    file = pathlib.Path("accounts.data")
    if file.exists():
        infile = open('accounts.data', 'rb')
        oldlist = pickle.load(infile)
        infile.close()
        os.remove('accounts.data')
        for item in oldlist:
            if item.acc_no == acc_no:
                item.name = input("Enter the Account Holder Name : ")
                item.type = input("Enter the Account Type (C/S) : ")
                item.deposit = int(input("Enter the Amount : "))

        outfile = open('newaccounts.data', 'wb')
        pickle.dump(oldlist, outfile)
        outfile.close()
        os.rename('newaccounts.data', 'accounts.data')


def write_accounts_file(account):
	"""it is used to write files of an account"""
    file = pathlib.Path("accounts.data")
    if file.exists():
        infile = open('accounts.data', 'rb')
        oldlist = pickle.load(infile)
        oldlist.append(account)
        infile.close()
        os.remove('accounts.data')
    else:
        oldlist = [account]
    outfile = open('newaccounts.data', 'wb')
    pickle.dump(oldlist, outfile)
    outfile.close()
    os.rename('newaccounts.data', 'accounts.data')


ch = ''
acc_no = 0
intro()

while ch != 8:
    print("\tMAIN MENU")
    print("\t1. New Account")
    print("\t2. Deposit Amount")
    print("\t3. Withdraw Amount")
    print("\t4. Balance Enquiry")
    print("\t5. All Account Holder List")
    print("\t6. Close an Existing Account")
    print("\t7. Modify Information of any Account")
    print("\t8. Exit")
    print("\tSelect Your Option from 1 to 8")
    ch = input()
    if ch == '1':
        write_account()
    elif ch == '2':
        acc_no = int(input("\tEnter The account No. : "))
        deposit_and_withdraw(acc_no, 1)
    elif ch == '3':
        acc_no = int(input("\tEnter The account No. : "))
        deposit_and_withdraw(acc_no, 2)
    elif ch == '4':
        acc_no = int(input("\tEnter The account No. : "))
        display_sp(acc_no)
    elif ch == '5':
        write_account()
    elif ch == '6':
        acc_no = int(input("\tEnter The account No. : "))
        delete_account(acc_no)
    elif ch == '7':
        acc_no = int(input("\tEnter The account No. : "))
        modify_account(acc_no)
    elif ch == '8':
        print("\tThanks for using bank managemnt system")
        break
    else:
        print("Please enter the valid field")

    ch = input("Press <ENTER> ")

