## Day 1

**Q1: Describe what an event is, and why it might be useful to a client.**

An event is a way to communicate that something happened on the blockchain. This makes tracking data easier.

**Q2: Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.**

![BabyJacobEvent](https://user-images.githubusercontent.com/100196621/160272173-807455e2-b53c-4260-b73f-64bb3063ccfc.png)

**Q3: Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.**

![BabyJacobPre:PostConditions](https://user-images.githubusercontent.com/100196621/160272519-23f946b6-f7e0-439b-8387-9409b91b9576.png)

**Q4: For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.**

* numberOne - Yes, Jacob has the length of 5.
* numberTwo - Yes, Jacob has the length of 0 or more.
* numberThree - No, the value would remain 0.

![Pre:PostConditionQ4](https://user-images.githubusercontent.com/100196621/160272580-6ccbee03-d115-470f-849f-13fb1ab82f32.png)


## Day 2

**Q1: Explain why standards can be beneficial to the Flow ecosystem.**

Standards make it easier to interact with multiple contracts.

**Q2: What is YOUR favourite food?**

The food I get most excited about is korean barbeque! Also crave burgers the most. XD

**Q3: Please fix this code (Hint: There are two things wrong):**

**The contract interface:**

![ContractInterface](https://user-images.githubusercontent.com/100196621/160321115-bf5ad380-73b4-4817-9341-8649b970ae9f.png)

**The implementing contract:**

![ImplementingContract](https://user-images.githubusercontent.com/100196621/160321122-ab5606f3-c54e-415d-8ca5-a84712e7ff84.png)


## Day 3

**Q1: What does "force casting" with `as!` do? Why is it useful in our Collection?**

The "force cast" operator `as!` will "downcast" the generic @NonFungibleToken type to a more specific resource type. The reason we want to “downcast” an @NonFungibleToken is because all NFTs on Flow follow the Non-Fungible Token Standard. Meaning, anyone could deposit any type of resource into our Collection. The “force cast” operator `as!` is useful because it will only deposit the specific type of resource we want to live inside our Collection.

**Q2: What does `auth` do? When do we use it?**

The `auth` keyword is an "authorized reference". Similiar to the `as!` operator, the `auth` will "downcast" a generic type to a more specific type, but for references. We would use this in the `borrow` function of the contract if we want to allow the public to read other data than what the &NonFungibleToken contains. Which the Non-Fungible Token Standard only requires an id reference.

**Q3: This last quest will be your most difficult yet. Take this contract**

```
import NonFungibleToken from 0x02
pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return &self.ownedNFTs[id] as &NonFungibleToken.NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

**and add a function called borrowAuthNFT just like we did in the section called "The Problem" above.** 

![CryptoPoopBorrow](https://user-images.githubusercontent.com/100196621/160345422-c31ac383-c5c1-4e68-b3d7-af3f80efbae2.png)

**Then, find a way to make it publically accessible to other people so they can read our NFT's metadata.**

![CryptoPoopPublic](https://user-images.githubusercontent.com/100196621/160345412-76804317-a5d8-4323-a3bc-184f54b655ac.png)

**Then, run a script to display the NFTs metadata for a certain id.**
**You will have to write all the transactions to set up the accounts, mint the NFTs, and then the scripts to read the NFT's metadata.**

**Created Collection**
![CryptoPoopsCollection](https://user-images.githubusercontent.com/100196621/160505123-3611703c-0bea-4687-8a68-03dca35fcfaf.png)

**Mint NFT**
![CryptoPoopsMint](https://user-images.githubusercontent.com/100196621/160505141-763f5039-e341-4014-b181-cf3440f3cd33.png)

**Read NFT Metadata**
![CryptoPoopsScript](https://user-images.githubusercontent.com/100196621/160505148-2eabe61c-0e1b-45e3-8f89-3ad16f5ba3ad.png)


