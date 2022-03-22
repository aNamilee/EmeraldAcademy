## Day 1

**Q1: List 3 reasons why structs are different from resources.**

Structs can be copied, overwritten and can be created outside of the contract, while resources cannot.

**Q2: Describe a situation where a resource might be better to use than a struct.**

Resources are more secure than a struct in that they can't be overwritten and has to be explicitly handled. Which makes them great for NFTs.

**Q3: What is the keyword to make a new resource?**

`create`

**Q4: Can a resource be created in a script or transaction?**

No, the create keyword can only be used inside the contract.

**Q5: What is the type of the resource below?**
```
pub resource Jacob {

}
```
**Jacob**, -is a resourceful type :P

**Q6: I Spy 4 things wrong with this code. Please fix them.**
```
//wrong
pub fun createJacob(): Jacob { // there is 1 here
        let myJacob = Jacob() // there are 2 here
        return myJacob // there is 1 here
    }
```
1. Jacob is a resource and needs to be indentfied with the @ symbol.
    * pub fun createJacob(): @Jacob
2. Resources need to be moved with the <- operator.
    * let myJacob <- 
3. Resource needs to be created with the `create` keyword.
    * let myJacob <- create Jacob()
4. We need to move myJacob with the <- operator.
    * return <- myJacob
```
//correct
pub fun createJacob(): @Jacob {
        let myJacob <- create Jacob()
        return <- myJacob
    }
```


## Day 2

**Q1: Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them.**

<img width="1154" alt="SwtCandyArrayDictionaryOfResources" src="https://user-images.githubusercontent.com/100196621/159177045-04e32e36-b46e-4367-a950-a9ab385117a6.png">

## Day 3

**Q1: Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.**

<img width="870" alt="SwtEricNamContract" src="https://user-images.githubusercontent.com/100196621/159176249-f9373547-3a48-44b8-bca9-f4cdd7f87b68.png">

**Q2: Create a script that reads information from that resource using the reference from the function you defined in part 1.**

<img width="847" alt="SwtEricNamScript" src="https://user-images.githubusercontent.com/100196621/159176251-459e4f7c-575e-4180-ba99-a1bc42678cd1.png">

**Q3: Explain, in your own words, why references can be useful in Cadence.**

References are useful when wanting to read data about a resource. Since resources have to be explicitly handled, without references we would have to withdrawal the resource, read it and then deposit it again. References allow us to read the data of a resource without having to move it.


## Day 4

**Q1: Explain, in your own words, the 2 things resource interfaces can be used for.**

Resource interfaces can be used to specify what is required for something to implement. They can also be used to expose certain things to certain people.

**Q2: Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.**

![SwtFitInterface](https://user-images.githubusercontent.com/100196621/159209894-8ff14a0f-c822-4401-8b08-6aa0442383b0.png)

**Q3: How would we fix this code?**

```
//restricted

pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
    }

    // ERROR:
    // `structure Stuff.Test does not conform 
    // to structure interface Stuff.ITest`
    pub struct Test: ITest {
      pub var greeting: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
}
```
```
//accessible

pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
      pub fun changeGreeting(newGreeting: String): String  //Add changeGreeting function to interface
    }

    pub struct Test: ITest {
      pub var greeting: String
      pub var favouriteFruit: String  //Add favouriteFruit variable to comply with interface

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting
      }

      init() {
        self.greeting = "Hello!"
        self.favouriteFruit = "Paopu"  //Initialize favouriteFruit
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!")
      log(newGreeting)
    }
}
```


## Day 5

**Q1: For today's quest, you will be looking at a contract and a script. You will be looking at 4 variables (a, b, c, d) and 3 functions (publicFunc, contractFunc, privateFunc) defined in SomeContract. In each AREA (1, 2, 3, and 4), I want you to do the following: for each variable (a, b, c, and d), tell me in which areas they can be read (read scope) and which areas they can be modified (write scope). For each function (publicFunc, contractFunc, and privateFunc), simply tell me where they can be called.**

```
access(all) contract SomeContract {
    pub var testStruct: SomeStruct

    pub struct SomeStruct {

        //
        // 4 Variables
        //

        pub(set) var a: String

        pub var b: String

        access(contract) var c: String

        access(self) var d: String

        //
        // 3 Functions
        //

        pub fun publicFunc() {}

        access(contract) fun contractFunc() {}

        access(self) fun privateFunc() {}


        pub fun structFunc() {
            /**************/
            /*** AREA 1 ***/
            /**************/
            
            // VARIABLES //
            // A: Read and write
            // B: Read and write
            // C: Read and write
            // D: Read and write
        
            // FUNCTIONS //
            // access(all)
            // access(contract)
            // access(self)
     
        }

        init() {
            self.a = "a"
            self.b = "b"
            self.c = "c"
            self.d = "d"
        }
    }

    pub resource SomeResource {
        pub var e: Int

        pub fun resourceFunc() {
            /**************/
            /*** AREA 2 ***/
            /**************/
            
            // VARIABLES //
            // A: Read and write
            // B: Read
            // C: Read
        
            // FUNCTIONS //
            // access(all)
            // access(contract)

        }

        init() {
            self.e = 17
        }
    }

    pub fun createSomeResource(): @SomeResource {
        return <- create SomeResource()
    }

    pub fun questsAreFun() {
        /**************/
        /*** AREA 3 ****/
        /**************/
        
        // VARIABLES //
        
        // A: Read and write
        // B: Read
        // C: Read
        
        // FUNCTIONS //
            
        // access(all)
        // access(contract)
            
    }

    init() {
        self.testStruct = SomeStruct()
    }
}
```

```
import SomeContract from 0x01

pub fun main() {
  /**************/
  /*** AREA 4 ***/
  /**************/
  
  // VARIABLES //
  
  // A: Read and write //technically read only
  // B: Read

        
  // FUNCTIONS //
  
  // access(all)

}
```
