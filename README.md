# Credit_Card_project
'''This project has been made to practice and exercise of creating credit card for daily usage and understanding how they work in python'''
Modelling a credit card class - Parts taken from "Data Structures and Algorithms in Python"

'''


################ Parent class of any credit card ##############################

class CreditCard:
    '''A consumer credit card.'''     #docstring, the first set of comments after the name of class is considered the help for that class. help(CreditCard)  
    

    def __init__ (self, customer, bank, acnt, limit, balance =0 ):   #The constructor is the very first method
        '''Create a new credit card instance.

        The initial balance is zero.

        customer: the name of the customer (e.g., John Bowman )
        bank: the name of the bank (e.g., California Savings )
        acnt : the acount identifier (e.g., 5391 0375 9387 5309 )
        limit: credit limit (measured in dollars)
        '''
        self._customer = customer 
        self._bank = bank
        self._account = acnt
        self._limit = limit
        self._balance = balance       # we start with a balance of zero, this is private, nobody can change it

    def get_customer(self):   #The get functions are a must, they're called accessor functions (methods)
        '''Return name of the customer.'''
        return self._customer

    def get_bank(self):
        '''Return the bank s name.'''
        return self._bank

    def get_account(self):
        '''Return the card identifying number (typically stored as a string).'''
        return self._account

    def get_limit(self):
        '''Return current credit limit.'''
        return min(self._limit)

    def get_balance(self):
        return self._balance
    
    def private_balance(self, balance):
        self._balance = balance
    
    
    def set_limit(self, limit):   # a setter would change the attribute values, we are allowing change to the limit
        self._limit = limit

    
    
    def charge(self, purchase):
        while purchase <= 0:
            try:
                purchase = int(input('Enter charge amount: '))
                if purchase <= 0:
                    print('The charge amount must be positive.')
            except ValueError:
                print('That is an invalid amount.')
            except EOFError:
                print('There was an unexpected error reading input.')
    
        if purchase + self._balance > self._limit:
            print("Card Declined")
            return False 
        else:
            self._balance += purchase
            self._limit -= purchase
            return True 
        


    def make_payment(self,payment):
        '''Process customer payment that reduces balance.'''
    
        while payment <= 0:
            try:
                payment = int(input('Enter your payment:'))
                if payment <= 0:
                    print('Please enter an positive number')
            except ValueError:
                    print ('That is an invalid input')
            except EOFError:
                    print ('Error, retry.')
            pass

        self._balance += payment
        self._limit += payment



    def __str__(self):

        """ Returns a string representation of self """
        return "\ncustomer: " + str(self._customer) + "\nbank: " + str(self._bank) + "\naccount: " + str(self._account) + "\nlimit: " + str(self._limit) + "\nbalance: " + str(self._balance)
        

# C-2.28 ,C2.29, C-2.30 


class PredatoryCreditCard(CreditCard):
    def __init__(self, customer, bank, acnt, limit, apr,pay_min,late_fee):
        super().__init__(customer, bank, acnt, limit)
        self.apr = apr 
        self.pay_min = pay_min
        self.late_fee = late_fee
        self.num_charges = 0
        self.paid_amount = 0
        self.total_added = 0
        
   
   
    def charge(self, price):
        success = super().charge(price)
        if success:
            self.num_charges += 1
            if self.num_charges > 10:
                self.private_balance (self.get_balance() + 1)
        else:
            self.private_balance(self.get_balance ()+ 5)
        return success
    
    
    def make_payment(self, payment):
        super().make_payment(payment)
        self.paid_amount += payment
    
    

    def process_month(self):
        if self.get_balance > 0:
            monthly_factor = pow(1 + self.apr, 1/12)
            self.private_balance (self._balance() * monthly_factor)
            self.num_charges = 0

     
        minimum_payment_due = self._balance *  self.pay_min

       
        if self.paid_amount < minimum_payment_due:
            self._balance +=  self.late_fee 
            print(f"Late fee applied. New balance: {self._balance}")

        self.num_charges = 0
        self.paid_amount = 0

############### Testing the class ########################################     
'''    
if __name__ == "__main__":  #uses test cases to test the class inside the same script file
    my_visa = CreditCard("Sareh Taebi", "CITI", '1234 5678 1234 5678',1600, 10)
    
    my_vi = CreditCard("Jack ", "Express", '1234 5670 1234 5678',100, 1)
    
    my_ve = CreditCard("Micheal ", "ww ", '1234 5679 1234 5678',1000, 600)
    print(my_visa)
'''


'''
    print(my_visa,my_ve,my_vi)

    my_visa.charge(0)
    
    my_vi.charge(0)

    my_ve.charge(0)

    print(my_visa,my_ve,my_vi)
'''


'''
if __name__ == "__main__":
    visa = PredatoryCreditCard('sarah', 'wellsfargo', '62863 928 0282', 2000, 100)

    for i in range(12):
        visa.charge(20)
        print(f"Charge {i+1}: Balance = {visa.get_balance()}")

    visa.process_month()
    print( visa.get_balance())
''' 

'''
if __name__ == "__main__":
    visa = PredatoryCreditCard("daisy", "Bank of America","12822 222 132",1000,0.0825,0.02,10)

visa.charge(1000)
visa.make_payment(15) 
visa.process_month()
print(visa.get_balance)
'''

if __name__ == "__main__":
    visa = PredatoryCreditCard("daisy", "Bank of America","12822 222 132",1000,0.0825,0.02,10)

visa.charge(500)
print(visa.get_balance())
    

visa.private_balance(200)  
print(visa.get_balance())
