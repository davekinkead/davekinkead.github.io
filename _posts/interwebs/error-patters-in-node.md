# Design Patterns for Errors in NodeJS

Node uses Continuation Passing Styles (https://en.wikipedia.org/wiki/Continuation-passing_style) AKA callbacks as a key design pattern.

Node's speed is dependent on this callback based design but the callbacks themselves can be designed with any method signature.  Then convention to solve this comes from node core and is error first 


    function callback(err, data) {
      if (err) {
        doSomthing()
      } 
    }

Strategies for dealing with an error:

  - throwing an exception
    - when are they thrown & what is their context?
    - when should they be thrown vs returned
    - only works for syncronous code
  - propogating it back to the callback function?
  - events?
    - process.on('error'/'uncaughtException')
    - very bad
    - domain.on('error')
    - new behaviour

  - rule of thumb
    - Try/catch for synchronous code
    - error first callback for asynchronous code
    - emit an error for eventful code
  - explain why for each



##  The Callback Pattern

First, you define a function (the contract) which does something and may generate an error.  If there is an error, call the callback with the error as an argument.  If all is good, call the callback with null and the results.

Next, you use the contract by invoking it with the arguments and a callback function.  This an be named or anonymous but either way will need to accept an error first list of arguments.


    function contract (arg, callback) {
      if (somethingBadHappens) {
        return callback(Error('Something bad happened'))
      }

      // do the important stuff here
      return callback(null, "results")
    }

    contract(arg, function(err, data) {
      if (err) {
        // do something with the error
      }  

      // do something with the result
    })

