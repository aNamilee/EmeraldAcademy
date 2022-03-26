## Day 1

**Q1: Explain what lives inside of an account.**

1. Contract code - when contracts get deployed, the code lives inside of an account.
2. Account storage - accounts on Flow can store their own data, so all data gets stored here.

**Q2: What is the difference between the `/storage/`, `/public/`, and `/private/` paths?**

* `/storage/` - only the account owner has access
* `/public/` - accessible to everyone
* `/private/` - only the account owner has access and those the account owner gives access to

**Q3: What does `.save()` do? What does `.load()` do? What does `.borrow()` do?**

* `.save()` - saves data in account storage
* `.load()` - loads data out from an account storage
* `.borrow()` - gets a reference to data from account storage

**Q4: Explain why we couldn't save something to our account storage inside of a script.**

To save data to our account storage, we would need `AuthAccount` which can only be done in the prepare phase of the transaction.

**Q5: Explain why I couldn't save something to your account.**

You can only sign a transaction for an account that you own. 

![HamiltonKeys](https://user-images.githubusercontent.com/100196621/159817110-3cb91e8b-2cb8-406d-a069-257b579b73c0.GIF)

**Q6: Define a contract that returns a resource that has at least 1 field in it.**

![SwtStabbyResource](https://user-images.githubusercontent.com/100196621/159829626-1c35b290-02d3-4243-89dc-e54518c52265.png)

**Then, write 2 transactions:**

* **A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.**

![Swt LoadStabby](https://user-images.githubusercontent.com/100196621/159829648-26df3362-b4bc-4206-a07e-6b650118bb66.png)

* **A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.**
   
![Swt BorrowStabbyRef](https://user-images.githubusercontent.com/100196621/159829668-e4966e3a-f2be-4664-a5ba-2826c5289d14.png)


## Day 2

**Q1: What does `.link()` do?**

The `.link()` function creates a capability from the account storage to a `/public/` or `/private/` path to expose certain data for others to read.

**Q2: In your own words (no code), explain how we can use resource interfaces to only expose certain things to the `/public/` path.**

You can choose not to expose certain things by having your resource implement a resource interface that doesn't contain certain variables or functions. Then linking a reference to that resource interface.

**Q3: Deploy a contract that contains a resource that implements a resource interface.**

![SwtEzmeResourceInterface](https://user-images.githubusercontent.com/100196621/159870440-88c320cf-b8d4-4eea-a7a0-81c18a310004.png)

**Then, do the following:**
* **In a transaction, save the resource to storage and link it to the public with the restrictive interface.**

![SwtEzmeTransaction](https://user-images.githubusercontent.com/100196621/159870461-d3cb13a1-e7ef-40fc-8ca0-1a6ff535d007.png)

* **Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.**

![SwtEzmeScriptError](https://user-images.githubusercontent.com/100196621/159870379-21741af4-5a89-405a-bbe1-f1eab43a6544.png)

* **Run the script and access something you CAN read from. Return it from the script.**

![SwtEzmeScript](https://user-images.githubusercontent.com/100196621/159870391-f3aaad3c-762f-4415-b0f8-e69668154991.png)


## Day 3

**Q1: Why did we add a Collection to this contract? List the two main reasons.**

So we can store multiple NFTs inside that Collection to then store that Collection on one path in the account storage. Then by having a Collection that can store multiple NFTs, others are able to deposit into that Collection.

**Q2: What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")**

We would need to define a destroy function that will destroy the resources inside of the resource they're contained in, if that resource was to ever be destroyed.

**Q3: Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.**

* **Idea #1: Do we really want everyone to be able to mint an NFT? 🤔.**

We can create a resource and place the `create` function inside so only those who have that resource can call that function to mint.

* **Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?**

We want to add a function inside our Collection resource that will borrow a reference of our NFT so others can read the information without withdrawing it. Then we will need to define that function in the resource interface that implements that Collection resource so others can call that function instead.


## Day 4

**Take our NFT contract so far and add comments to every single resource or function explaining what it's doing in your own words. Something like this:**

[Ch4Day4Quest.pdf](https://github.com/aNamilee/EmeraldAcademy/files/8354575/Ch4Day4Quest.pdf)
