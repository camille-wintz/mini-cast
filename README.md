# mini-cast

In typescript, there is no native way to do something like this:

    // Maybe you got this from the server ?
    var myObj = {  
        name: 'Boris',  
        age: 3  
    };

    class Yak{
        name: string;
        age: number;

        logYak() => {
            console.log(this.name);
        }    
    }

    var myYak= <Yak>myObj;
    // this doesn't work.
    myYak.logYak();
    // false
    myYak instanceof Yak

So I've made a very small library doing exactly that.

    import { Mix } from 'minicast';

    var myObj = {  
        name: 'Boris',  
        age: 3,
        owner: {
            name: 'Patrick'
        }  
    };

    class Human{
        name: string;
    
        logHuman(){
            console.log(this.name);
        }
    }

    class Yak{
        name: string;
        age: number;
        owner: Human;
        
        constructor(){
            this.owner = new Human();
        }

        logYak(){
            console.log(this.name);
        }
    }

    var myYak = Mix.castAs(Yak, myObj);

    // logs "Boris"
    myYak.logYak();
    // logs "Patrick"
    myYak.human.logHuman();
    // true
    myYak instanceof Yak
    myYak.human instanceof Human


There is also an extension for arrays.

    var yaks = new TypedArray<Yak>(Yak);
    yaks.push({ name: 'Clarissa' });
    
    // true
    yaks[0] instanceof Yak
    // works
    yaks[0].logYak();


#### Documentation


###### Mix  

Cast an object in a class:

    var myYak = Mix.castAs(Yak, myObj);

Cast in cast:

    class Human{
    }

    class Yak{
        constructor(){
            // this is imperative
            this.owner = new Human();
        }
    }

    var myYak = Mix.castAs(Yak, { owner: {} });

Use the consructor:

    //myData is sent as a parameter to the constructor
    var myYak = Mix.castAs(Yak, myObj, myData);

Cast arrays:

    var myYaks = Mix.castArrayAs(Yak, myArray, myData);

###### TypedArray

Create a TypedArray:

    var yaks = new TypedArray<Yak>(Yak);

Fill with data:

    yaks.load([{ name: 'yak1', age: 2 }, { name: 'yak2', age: 3 });

Get the initial class at runtime:

    var yak = new yaks.className();

Get native array from typed array:

    yaks.asArray();