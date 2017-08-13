# Error Examples

Need to know
  - how node / js executes
  - async / callback pattern

Synchronous code will typically use try/catch 


    my_sync_function = () ->
      console.log 'In the sync function'
      throw Error 'A sync error'

    try 
      my_sync_function()
      console.log 'you shouldnt seem me sync'
    catch e
      console.log 'Sync error caught'


but what happens when you use this for async functions that take longer to execute?


    my_async_function = () ->
      console.log 'Async for the win'
      setTimeout () -> throw Error 'An async error', 1000

    try 
      my_async_function()
      console.log 'you shouldnt see me async'
    catch e
      console.log 'Async error caught'


Here (explain JS execution order) my_async_function is called and placed in the queue, then the next line is executed and control leaves the try block.  Only then does the async function execute and throw - and the context is no longer in the tyr/catch block.

What to do instead...

  callback(err)
  callback(null, success)