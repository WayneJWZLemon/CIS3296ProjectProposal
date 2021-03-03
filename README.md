# BankSim

## Requirements

The **learning objective** of this lab assignment is to inspect and come up with solutions to deal with race conditions in a multi-thread program. We are provided with the initial source code that simulates the money transfer process between different bank accounts. However, the source code has no ciritcal section protection and therefore a part of our tasks is to implement solutions to avoid race conditions.

* Task 1

  Race conditions may occur within the methods defined in the Account class, specifically the **withdraw** and the **deposit** methods. In the main class, there are 10 threads created and if two of these threads called one of the two methods for the same bank account, then there would be a fight over accessing the class variable "balance." For example, say we have thread1 and thread2 that are both making a transfer from account1 to account 2, before thread1 withdrawing from the account1 balance, thread2 is also scheduled to access the balance variable. Then depending on the thread scheduler, the thread that is scheduled last would be the one to adjust the value of balance. As for the other thread, the program would appear as if it never make a withdrawal request.

* Task 2

  Added "**synchronized**" keyword to the **withdraw** and **deposit** methods in the Account class to prevent more than one transferring thread accessing the same global balance variable.

* Task 3

  Created a **TestingThread** class and refactored the **Bank.test** method into the new TestingThread.run method. Also added a new private Thread variable within the Bank class and have the **Bank.test** method set up the test thread's initialization and calls its **start** method.

* Task 4 & Task 5

  Added a **Semaphore** object with the initial permit count of 10 to the Bank class. Surrounded the body statements in the **Bank.transfer** method with **Semaphore acquire()** and **release()** in try-catch-finally blocks. This ensures only one thread is allowed to use that resource. Surrounded the body statements in the **TestingThread.run()** with Semaphore to make sure the test thread waits for all the transfer threads to finish the current transfer, then take over the testing procedure. (Done by setting the semaphore's initial permit number to 10)

* Task 6

  Added a **waitForAvailableFunds** method to the Account class, which checks if the amount being transfer is great than the avaibale balance, if that is the case, we use the **wait()** to suspend the current thread until another despoit is made. To complement that, **notifyAll()** is added to the Account.deposit(), which wakes all the threads that are waiting for the monitor of the account object. The waitForAvailableFunds method is placed at the top of the Bank.transfer() as a checking point. 

* Task 7

  Added a private boolean variable **open** to the Bank and implemented **closeBank()** which let the current thread that is executing on the Bank object to change the variable open to false and then notify the threads that are accessing the account object. Added a Bank object to the Account and implemented a getter method to the varialble open in the Bank class, and waitForAvailableFunds() is checking whether the transfer amount is greater than the current balance as well as the availability of the Bank itself. Also, the Bank.transfer() is checking whether the bank is open or not before moving. In the TransferThread class, after one thread finishes the 10,000 transfers it calles the Bank.closeBank() to notify other transferring threads to stop.

## Teamwork

Wayne Zheng

* Task 1
  * Discuss and design the UML sequence diagram as a group
* Task 2
  * Added synchronized objece locks to the Account.withdraw() and Account.deposit()
* Task 3
  * Created a TestingThread class, implemented the constructor and run methods
  * Added a test thread to the Bank class
  * Refactored the Bank.test() to call the test thread's start method
* Task 4 & Task 5
  * Added Semaphore to the Bank class and protected the critical section inside the Bank.transfer() using Semaphore acquire and release methods accordingly. 

Maguire Qvale

* Task 1
  * Discuss and design the UML sequence diagram as a group

* Task 6
  * Implemented Account.waitForAvailableFunds()
  * Added notifyAll() to the Account.deposit()
* Task 7
  * Added a boolean variable open to the Bank class
  * Implemented Bank.isOpen() and Bank.closeBank()
  * Added bank.closeBank() to the TransferThread.run()

## Testing



## UML Sequence Diagram
![alt text](https://github.com/CIS-SoftwareDesign-S21/banksim-04-zheng-qvale-banksimlab/blob/master/BankSimSequece.png "HowRaceConditionCanOccur")






