# Day 1

**Deploy contract & Execute a script**

![JacobTuckerIsTheBest](https://user-images.githubusercontent.com/100196621/157376014-8e5f753b-49d8-4031-ba03-b42ceed67763.png)

# Day 2

**Q1: Explain why we wouldn't call changeGreeting in a script.**

We wouldn't call changeGreeting in a script because that calls for modifying data on a blockchain. A script can only view data on the blockchain, not change it.

**Q2: What does the AuthAccount mean in the prepare phase of the transaction?**

It allows us to take in a user's AuthAccount to access the information/data in ones account.

**Q3: What is the difference between the prepare phase and the execute phase in the transaction?**

The prepare phase *can* access information in an account, the execute phase *cannot*.

**Q4: Add two new things inside your contract:**

**A variable named myNumber that has type Int (set to 0).**

![1](https://user-images.githubusercontent.com/100196621/157380659-4bcad3a6-09c3-475d-83a2-48dad68ab8bb.png)

**A function named updateMyNumber that takes in a new number named newNumber as a parameter that had type Int and updated myNumber to be newNumber**

![2](https://user-images.githubusercontent.com/100196621/157380799-5279c64e-3aa0-445a-866f-c23b6e40b3f0.png)

**Add a script that reads myNumber from the contract**

![3](https://user-images.githubusercontent.com/100196621/157380845-c31ba741-2323-4a19-af87-c613f282acf1.png)

**Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function.**

![4](https://user-images.githubusercontent.com/100196621/157380966-f9d01ed7-6db4-4c98-ab01-1d0a58da15f0.png)

**Verify that your number changed by running the script again.**

![5](https://user-images.githubusercontent.com/100196621/157380999-2b95c2d2-be40-47a2-a9aa-3ded87b951ca.png)


# Day 3

**Q1: Initialize an array**

![InitializeArray](https://user-images.githubusercontent.com/100196621/157385117-d2bebd9e-7c37-4b83-87b8-340adf68ff58.png)

**Q2: Initialize a dictionary**

![InitializeDictionary](https://user-images.githubusercontent.com/100196621/157385239-3dc795c7-b60d-4865-8ce1-db3fa44145f9.png)

**Q3: Explain what the force unwrap operator does, with an example.**

In this script, we want to get the value that is mapped to "Hiro", which is "ZeroTwo" ;). But being a key inside a dictionary, the value will come back as optional. (Get your act together Hiro!) This happens because whenever accessing values of a dictionary, it will always return as an optional. In order for "Hiro" to get mapped to "ZeroTwo", he will need to make great exclamation (!) of his love for her to *return*. By doing so, **it gets rids of the optional thus returning the *actual* type to access the true value.** *~uwu* :3

![TheReturnOfZeroTwo](https://user-images.githubusercontent.com/100196621/157412491-920aa9b3-1737-4f4a-90cb-8db0ee622c1e.png)

**Q4: Using the picture below explain...**

 * **What the error message means**

The error message means that the script was expecting to get a String type, but it's getting an optional.

 * **Why we're getting this error**

We're getting an error because we set the script to return a String type.

 * **How to fix it**

We need to *unwrap* the optional by placing an ! after the key to get the actual type.


# Day 4

**Q1, Q2, Q3: Deploy a new contract that has a struct, dictionary or array and function.**

![SwtBobaContract](https://user-images.githubusercontent.com/100196621/157619871-f98233fa-01ac-4503-9df1-7c2eb55e20f0.png)

**Q4: Add a transaction to call that function in step 3.**

![SwtBobaTransactoin](https://user-images.githubusercontent.com/100196621/157620009-0170a699-d646-466c-a733-93e823f363a6.png)

**Q5: Add a script to read the struct you defined.**

![SwtBobaScript](https://user-images.githubusercontent.com/100196621/157620112-6fe1d60c-ff99-4eab-b3ff-2e154ff364bc.png)
