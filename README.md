# Thread-runner [![Build Status](https://travis-ci.org/redshark1802/thread-runner.svg)](https://travis-ci.org/redshark1802/thread-runner)

Limit the number of asynchronous threads. Useful for doing a large amount of HTTP requests and other heavy tasks. 

```javascript

var Thread = require('thread-runner'),
    //workStack has to be an array of elements that need to be processed
    workStack = ['6', '5', '4', '3', '2' , '1', '0'];


/**
 * @param stackElement single element from workStack
 * @param callback
 */
var threadedFunction = function (stackElement, callback, stack) {

    var timeout = stackElement * 1000;
    console.log(timeout);

    setTimeout(function () {
        
        /**
         * you even can add new elements to the stack
         * stack.push('1');
         *
         */
    
        return callback(timeout);
    }, timeout);
};

//will be called once every element in workStack has been processed by threadedFunction.
var workStackDone = function(results){
    console.log('all results', results);
};

var thread = new Thread(threadedFunction, workStack, workStackDone);

//set the number of concurrent threads, default 10
thread.setMaxThreads(2);

//will be called every time threadedFunction is done processing a stackElement
var singleResult = function(result){
    console.log('single result', result);
};

thread.setListener(singleResult);

//start the runner
thread.run();

```