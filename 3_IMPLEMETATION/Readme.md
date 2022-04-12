import pickle
import os
import pathlib
class Account:
    accNo = 0
    name = ''
    deposit=0
    type = ''
    
    def create_account(self):
        self.accNo= int(input("Enter the account no : "))
        self.name = input("Enter the account holder name : ")
        self.type = input("Ente the type of account [C/S] : ")
        self.deposit = int(input("Enter The Initial amount(minimum 500 for Saving and 1000 for current: "))
        print("\n\nCongrats! New Account Created")
    
    def show_account(self):
        print("Account accNober : ", self.accNo)
        print("Account Holder Name : ", self.name)
        print("Type of Account", self.type)
        print("Balance : ", self.deposit)
    
    def modify_account(self):
        print("Account accNober : ", self.accNo)
        self.name = input("Modify Account Holder Name :")
        self.type = input("Modify type of Account :")
        self.deposit = int(input("Modify Balance :"))
        
    def deposit_amount(self, amount):
        self.deposit += amount
    
    def withdraw_amount(self, amount):
        self.deposit -= amount
    
    def report(self):
        print(self.accNo, " ", self.name , " ",self.type, " ", self.deposit)
    
    def get_account_no(self):
        return self.accNo
    def get_acccount_holder_name(self):
        return self.name
    def get_account_type(self):
        return self.type
    def get_deposit(self):
        return self.deposit
    
def intro():
    print("\t\t\t\t\tBANK MANAGEMENT SYSTEM developed by JAI GORA")
    input()

def write_account():
    account = Account()
    account.create_account()
    write_accounts_file(account)

def write_account():
    file = pathlib.Path("accounts.data")
    if file.exists ():
        infile = open('accounts.data', 'rb')
        mylist = pickle.load(infile)
        for item in mylist :
            print(item.accNo, " ", item.name, " ", item.type, " ", item.deposit )
        infile.close()
    else :
        print("No records to display")
        

def display_sp(accNo): 
    file = pathlib.Path("accounts.data")
    if file.exists ():
        infile = open('accounts.data', 'rb')
        mylist = pickle.load(infile)
        infile.close()
        found = False
        for item in mylist:
            if item.accNo == accNo:
                print("Your account Balance is = ", item.deposit)
                found = True
    else:
        print("No records to Search")
    if not found :
        print("No existing record with this accNober")

def deposit_and_withdraw(accNo1, accNo2): 
    file = pathlib.Path("accounts.data")
    if file.exists ():
        infile = open('accounts.data', 'rb')
        mylist = pickle.load(infile)
        infile.close()
        os.remove('accounts.data')
        for item in mylist :
            if item.accNo == accNo1:
                if accNo2 == 1:
                    amount = int(input("Enter the amount to deposit : "))
                    item.deposit += amount
                    print("Your account is updated")
                elif accNo2 == 2:
                    amount = int(input("Enter the amount to withdraw : "))
                    if amount <= item.deposit:
                        item.deposit -=amount
                    else:
                        print("Amount is large! ***ERROR 404***")
                
    else :
        print("No records to Search")
    outfile = open('newaccounts.data', 'wb')
    pickle.dump(mylist, outfile)
    outfile.close()
    os.rename('newaccounts.data', 'accounts.data')

def delete_account(accNo):
    file = pathlib.Path("accounts.data")
    if file.exists ():
        infile = open('accounts.data', 'rb')
        oldlist = pickle.load(infile)
        infile.close()
        newlist = []
        for item in oldlist:
            if item.accNo != accNo:
                newlist.append(item)
        os.remove('accounts.data')
        outfile = open('newaccounts.data', 'wb')
        pickle.dump(newlist, outfile)
        outfile.close()
        os.rename('newaccounts.data', 'accounts.data')
     
def modify_account(accNo):
    file = pathlib.Path("accounts.data")
    if file.exists ():
        infile = open('accounts.data', 'rb')
        oldlist = pickle.load(infile)
        infile.close()
        os.remove('accounts.data')
        for item in oldlist:
            if item.accNo == accNo:
                item.name = input("Enter the Account Holder Name : ")
                item.type = input("Enter the Account Type (C/S) : ")
                item.deposit = int(input("Enter the Amount : "))
        
        outfile = open('newaccounts.data', 'wb')
        pickle.dump(oldlist, outfile)
        outfile.close()
        os.rename('newaccounts.data', 'accounts.data')
  
def write_accounts_file(account): 
    
    file = pathlib.Path("accounts.data")
    if file.exists ():
        infile = open('accounts.data', 'rb')
        oldlist = pickle.load(infile)
        oldlist.append(account)
        infile.close()
        os.remove('accounts.data')
    else :
        oldlist = [account]
    outfile = open('newaccounts.data', 'wb')
    pickle.dump(oldlist, outfile)
    outfile.close()
    os.rename('newaccounts.data', 'accounts.data')
ch= ''
accNo= 0
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
    elif ch =='2':
        accNo = int(input("\tEnter The account No. : "))
        deposit_and_withdraw(accNo, 1)
    elif ch == '3':
        accNo = int(input("\tEnter The account No. : "))
        deposit_and_withdraw(accNo, 2)
    elif ch == '4':
        accNo = int(input("\tEnter The account No. : "))
        display_sp(accNo)
    elif ch == '5':
        write_account();
    elif ch == '6':
        accNo =int(input("\tEnter The account No. : "))
        delete_account(accNo)
    elif ch == '7':
        accNo = int(input("\tEnter The account No. : "))
        modify_account(accNo)
    elif ch == '8':
        print("\tThanks for using bank managemnt system")
        break
    else :
        print("Please enter the valid field")
    
    ch = input("Press <ENTER> ") 
    
