## Asynchronous Javascript
* Need at least Node version 10.15, because compatibility reasons.
* Prettier can be used with Ctrl-K, Ctrl-F on Windows
* Live server can be closed from the right lower part of the editor
* Code Runner can be used for executing code snippets in the editor, exclusively in a .js
file. The code must be selected before running it, and the shortcut for it is Ctrl + Alt + n.
* V8 itself is synchronous.
* process is an object representing the main thread of the node instance. This is accessible
throughout the entire codebase.
process.nextTick() will execute the current event loop cycle then execute the following
cycle by using the result as a parameter. Excessive use of this functionality will tipically
limit the I/O capabilities of the system.

```javascript
function doAsyncTask(cb) {
    process.nextTick(() => {
        process.nextTick(() => {
            process.nextTick(() => {
                console.log("Async Task Calling Callback - TOK");
                cb();
            });
            console.log("Async Task Calling Callback - TAK");
            cb();
        });
        console.log("Async Task Calling Callback - TIK");
        cb();
    });
}
doAsyncTask(() => {
    console.log(message);
});
let message = "Callback Called";
```

This snippet will output:

<p style="padding-left: 5em;">Async Task Calling Callback - TIK</p>

<p style="padding-left: 5em;">Callback Called</p>

<p style="padding-left: 5em;">Async Task Calling Callback - TAK</p>

<p style="padding-left: 5em;">Callback Called</p>

<p style="padding-left: 5em;">Async Task Calling Callback - TOK</p>

<p style="padding-left: 5em;">Callback Called</p>


* **`setImmediate()`** is a method used for handling async code execution in the event loop in Nodejs. This will execute the code in its body immediately after the current I/O operations in the event loop are completed.

```javascript
function doAsyncTask(cb) {
    setImmediate(() => {
        console.log("Async Task Calling Callback");
        cb();
    });
}
doAsyncTask(() => console.log(message));
let message = "Callback Called";
```

This snippet will output:
<p style="padding-left: 5em;">Async Task Calling Callback</p>
<p style="padding-left: 5em;">Callback Called</p>

## Error handling can be done in 3 ways:
1. Throwing

```javascript
throw err;
```

2. Logging

```javascript
console.error(err);
return;
```

3. Passing the error

```javascript
next(err);
```

<p style="padding-left: 5em;">To pass the result along, the following syntax is used:</p>
    next(null, data);

* try catch construct will not work on asynchronous code.

## When using promises, this basic syntax should be followed:

```javascript
/**
* "exampleOfPromise" is the name of the promise itself
* "new Promise" is instantiating the Promise class, available across the
entire codebase
*/
const exampleOfPromise = new Promise((resolve, reject) => {
    //place the async code in here
    //also, the resolve & reject functionalities must get included into the
    async code, as a way of ending the execution
});
```


## Adding the async block to the promise makes it looks like this:

```javascript
/**
* "exampleOfPromise" is the name of the promise itself
* "new Promise" is instantiating the Promise class, available across the
entire codebase
*/
const exampleOfPromise = new Promise((resolve, reject) => {
    //place the async code in here
    //also, the resolve & reject functionalities must get included into the
    async code, as a way of ending the execution
    setTimeout(() => {
        console.log("the async code block in a promise");
        resolve(); //this resolves the promise, after the code is run
    }, 1000);//the inner function for setTimeout will be executed after
    1000ms, as seen here
});
```

## The complete promise example looks like this (usually they get called inside methods):

```javascript
function asyncTaskExample() {
    /**
    * "exampleOfPromise" is the name of the promise itself
    * "new Promise" is instantiating the Promise class, available across
    the entire codebase
    */
    const exampleOfPromise = new Promise((resolve, reject) => {
        //place the async code in here
        //also, the resolve & reject functionalities must get included
        into the async code, as a way of ending the execution
        setTimeout(() => {
            if (error) {
                console.log("the async code block in a promise");
                resolve(); //this resolves the promise
            } else {
                reject(); //this ends the execution, with error
            }
        }, 1000);//the inner block in setTimeout will be executed after 1s
    });
    return exampleOfPromise;
}
```

## When a promise resolves, we can get notified about the result by attaching handlers to its ‘then’ function, as follows:
* _success_ handler, signifying the ‘resolve’ has been used
* _error_ handler, signifying the ‘reject’ has been used

```javascript
asyncTaskExample().then(
    () => console.log("Task completed"), // _success_ handler
    () => console.log("Task incomplete - error") // _error_ handler
);
```

Example:

```javascript
function asyncExample() {
    let error = true;
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (error) {
                reject('error');
            } else {
                resolve('done');
            }
        }, 1000);
    });
}
asyncExample().then(
    val => console.log(val),
    err => console.error(err)
);
```

* setTimeout() has the role of scheduling the execution of a code snippet after a set
amount of time, described in miliseconds.

* If a promise gets rejected, it cannot be resolved afterwards, and viceversa.

* Here’s an example to illustrate the difference between 3 approaches:

```javascript
/**
Callback version
*/
const fs = require("fs");
function readFile(filename, encoding) {
    fs.readFile(filename, encoding, (err, data) => {
        if (err) return callback('Unable to read from the file. Terminated with the error: ${error}', null);
        return callback(null, data);
    });
});
};
readFile("./files/demofile.txt", "utf-8", function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(JSON.stringify(data));
    }
});
);
/**
Promises version
*/
const fs = require("fs");
function readFile(filename, encoding) {
    return new Promise((resolve, reject) => {
        fs.readFile(filename, encoding, (err, data) => {
            if (err) return reject('Unable to read from the file. Terminated with the error: ${error}');
            return resolve(data);
        });
    });
};
readFile("./files/demofile.txt", "utf-8").then(
    data => console.log(JSON.stringify(data)),
    err => console.log(err)
);
/**
Promises with native Node functions version
*/
const fs = require("fs");
const util = require("util");
const readFile = util.promisify(fs.readFile);
readFile("./files/demofile.txt", "utf-8").then(
    data => console.log(JSON.stringify(data)),
    err => console.log(err)
);
```

* Promises can be initialized with what we can name as ‘immediate resolution’. This means they can be created with a default resolution which will be used. However, the resolution in the body will also be used. So, the flow will be:
1. Go through the immediate resolution (_then_ handler)
1. Pass through the body and execute the asynchronous code
1. Go through the handler in the body (_still_ handler)

## Initialized promises look like this:

```javascript
/**
The initialization is done normally, except that it is appended a
handler already, since it is expected to fail/resolve
*/
let initializedSolvedPromise = Promise.resolve("done");
let initializedResolvedPromise = Promise.reject("failed");
```

## Initialized promises, with a handler call after initialization, look like this:

```javascript
/**
The initialization is done normally, except that it is appended a
handler already,
since it is expected to fail/resolve
*/
let initializedSolvedPromise = Promise.reject("failed");
initializedSolvedPromise.then(
    val => console.log(val),
    err => console.log(err)
);
```

* Unlike callbacks, promises are always asynchronous.

* Chaining promises, and resolved promises, should look like this:

```javascript
/**
Chaining a resolve promise with other promises is done normally
As we go down the chain, each "step" must contain a return statement,
so that the next one will get data to process
Otherwise, the execution of the next "step" will generate an
"undefined" error
*/
let promise = Promise.resolve("done");
promise
    .then(val => {
        console.log(val);
        return "done2";
    })
    .then(val => console.log(val));
```


* Forking a promise chain is actually instantiating again an already existing promise:

```javascript
/**
The initialized promise gets instantiated in the following steps of the
chain. All of these instances are forked from the initial promise, not
chained.
This means that all the forked instances will receive as parameter the
returned parameter of the original promise (in this case... "done")
*/
const baseProm = Promise.resolve("done");
baseProm.then(val => {
    console.log(val);
    return "done2";
});
baseProm.then(val => console.log(val));
```

* Handling the errors from a promise chain can be done in different ways:

```javascript
const fs = require("fs");
const zlib = require("zlib");
function zlibPromise(data) {
    return new Promise((resolve, reject) => {
        zlib.gzip(data, (err, result) => {
            if (err) reject(err);
            resolve(result);
        });
    });
}
function readFile(filename, encoding) {
    return new Promise((resolve, reject) => {
        fs.readFile(filename, encoding, (err, data) => {
            if (err) reject(err);
            resolve(data);
        });
    });
}

/**
Way NO 1
*/
readFile("./files/demofile.txt", "utf-8")
    .then(data => {
        zlibPromise(data)
            .then(
                output => console.log("completed")
            )
            .catch(
                err => { console.log(`${JSON.stringify(err)}`); }
            )
    })
    .catch(
        err => { console.log(`${JSON.stringify(err)}`); }
    )

/**
Way NO 2
*/
readFile("./files/demofile2.txt", "utf-8")
    .then(data => {
        zlibPromise(data)
            .then(
                res => console.log(`Success`),
                err => console.log(`Failed with error: ${err}`)
            )
    }, err => {
        console.log(`${JSON.stringify(err)}`);
    });
```

of which only one is effective and widely used, because the code is readable, and more concise:

```javascript
/**
The good way – this will pass any existing error rejection to the
.catch in the structure
*/
readFile("./files/demofile.txt", "utf-8")
    .then(
        data => {
            return zlibPromise(data);
        })
    .then(
        data => {
            console.log(`Success`);
        })
    .catch(
        err => {
            console.log(`Failed with error: ${err}`);
        })
```

* ‘finally’ is available with promises, in the latest versions of Node (10+). This can be used in promise chains, to ensure that an action is taken regardless of the outcome of the promise chain. ‘finally’ will always get executed (continuing the example previously
used):

```javascript
/**
The good way
*/
readFile("./files/demofile.txt", "utf-8")
    .then(
        data => {
            return zlibPromise(data);
        })
    .then(
        data => {
            console.log(`Success`);
        })
    .catch(
        err => {
            console.log(`Failed with error: ${err}`);
        })
    .finally(_ => {
        console.log("cleaning up");
    })
```

* ‘all’ can be used to make a series of promise executions synchronous. It will firstly execute all the promises in the ‘all’, then continue with the code execution in the _then_ handler:

```javascript
const fs = require("fs");
const util = require("util");
const readFile = util.promisify(fs.readFile);
const files = ["./files/demofile.txt", "./files/demofile.other.txt"];
/**
For each of the file names in the 'files' array, read the contents of
the file and return an array containing the result
*/
let promises = files.map(name => readFile(name, "utf8"));
/**
Wait for the contents from all the files to be read (here '.all' is
used), then continue
*/
Promise.all(promises).then(values => {
    console.log(values);
});
```

* Handling possible errors in the ‘all’ method can be done this way:

```javascript
const fs = require("fs");
const util = require("util");
const readFile = util.promisify(fs.readFile);
const files = ["./files/demofile.txt", "./files/demofile.other.txt"];
/**
For each of the file names in the 'files' array, read the contents of
the file and return an array containing the result
*/
let promises = files.map(name => readFile(name, "utf8"));
/**
Wait for the contents from all the files to be read (here '.all' is used), then continue
*/
Promise.all(promises)
    .then(values => {
        console.log(values);
    })
    /**
    To handle possible errors in the promises included in the 'all' structure, we simply use .catch, the same way we'd use it for chained promises. If all the promises will get rejected, only the first failure will reach the .catch block, and the execution will end
    */
    .catch(err => {
        console.log(err);
    });

* Setting race conditions on promise execution allows for the conclusion of a promise chain by using the result of the first concluded promise:
/**

.race will resolve the entire array, based on the result of the promise
which will get resolved first
*/
//one of these will be the first to "get to the finish line"
/**
the entire array will be solved by the result of the promise that "gets to the finish line" first
*/
let car1 = new Promise(resolve => setTimeout(resolve, 1000, 'Car 1'));
let car2 = new Promise(resolve => setTimeout(resolve, 2000, 'Car 2'));
let car3 = new Promise(resolve => setTimeout(resolve, 3000, 'Car 3'));
Promise.race([car1, car2, car3]).then(value => {
    console.log("Resolved: ", value);
});

* Handling errors in these constructs involves the same approach as used with promise chains:

/**
.race will resolve the entire chain, based on the result of the promise which will get resolved first
*/

//one of these will be the first to "get to the finish line"

/**
The entire chain will be solved by the result of the promise that "gets to the finish line" first. Crashes are handled the same way as with promise chains.
*/
let car1 = new Promise(
(_, reject) =>
setTimeout(reject, 1000, 'First car in the race crashed')
);
let car2 = new Promise(resolve => setTimeout(resolve, 2000, 'Car 2'));
let car3 = new Promise(resolve => setTimeout(resolve, 3000, 'Car 3'));
Promise.race([car1, car2, car3])
.then(value => {
console.log("Resolved: ", value);
})
.catch(err => {
console.log("Failed: ", err);
});
```

* The final homework for the promises section of the section requires a race condition, and handling asynchronism for accomplishing the task of publishing a file while authenticated. The behavior is simulated:

```javascript
/**
publish the file only if authenticated
*/
function authenticate() {
    //return status 200 after 2 seconds - resolved promise
    console.log("Authenticating");
    return new Promise(resolve => setTimeout(resolve, 2000, {
        status: 200
    }));
}
function publish() { //publish the file, after 2 seconds - resolved promise
    console.log("Publishing");
    return new Promise(resolve => setTimeout(resolve, 2000, {
        status: 403
    }));
}
function timeout(sleep) {
    //use this in the race condition, to implement a timeout condition
    return new Promise((resolve, reject) => setTimeout(reject, sleep,
        "timeout"));
}
function safePublish() { //this will simplify the promise chain, and do the
    authentication, if necessary
return publish()
            .then(res => {
                if (res.status === 403) {
                    return authenticate();
                }
                return res;
            })
}
/**
Race the publishing and authentication against the 5 seconds timeout (2 seconds publishing + 2 seconds authentication. The minimum timeout for successfull execution is 4 seconds.
*/
Promise.race([safePublish(), timeout(5000)])
    .then(_ => console.log('Published'))
//if the timeout does not win the race, got into the .catch part, otherwise,
just publish
    .catch(err => {
        if (err === 'timeout') {
            console.error("timeout");
        } else {
            console.log(err);
        }
    });
```

* Using async/await involves using promises in the code and sometimes renouncing the .then and .catch constructs, in favor of a new approach, which is more concise & clear:

```javascript
// the async/await pattern will work with any existing promise
const doAsyncTask = () => Promise.resolve("done");
async function asim() {
// the function in itself needs to get marked as 'async'
// 'await' can only be used inside a function which has been declared as
'async'
let value = await doAsyncTask();
//the .then() construct usage is avoided => compact & concise code
console.log(value);
}
asim();
```

* This pattern can also be used in functions with immediate execution:

```javascript
// the async/await pattern will work with any existing promise
const doAsyncTask = () => Promise.resolve("done");
(async function() { //the function in itself needs to get marked as 'async'
// 'await' can only be used inside a function which has been declared
as 'async'
let value = await doAsyncTask(); //the .then() construct usage is
avoided => compact & concise code
console.log(value);
})();
/**
* Wrapping the functionality in brackets allows us to call it when it is read, automatically.
* This is the equivalent of an anonymous function.
* Also, notice the call is made with just the brackets.
*
* This is an "Immediately-Invoked Function Expression (IIFE)".
* It means it will get called immediately after it gets created.
* Good article on this: http://benalman.com/news/2010/11/immediately-invoked-function-expression/
*/
```

* The async/await pattern will also block the execution loop:

```javascript
const doAsyncTask = () => Promise.resolve("1");
(async function() {
let value = await doAsyncTask();
//the execution is 'blocked' until the 'value' (1) is printed out
//then '2' will be printed out, as opposed to the asynchronous nature of the promises construct => synchronous construct
console.log(value);
console.log("2");
})();
```

* Promise constructs can be used within this pattern:

```javascript
const doAsyncTask = () => Promise.resolve("1");
(async function () {
    doAsyncTask().then(console.log);
    /**
    Giving up on the 'await' keyword means the code will yet again become asynchronous: meaning that the '2' will get printed before '1'.
    */
    console.log("2");
})();
```

*  Fun fact: when using async/await, the entire construct will return to itself a promise! Because of this, the entire way of handling the resolve/reject response from executing a promise can be used within async/await constructs:

```javascript
const doAsyncTask = () => Promise.resolve("1");
let asyncFunction = async function() {
    let value = await doAsyncTask();
    /**
    * The execution here is synchronous. Because of this, it will output:
    * 1
    * 2
    * and then will move on to the 'return' statement
    */
    console.log(value);
    console.log("2");
    return "3"; //this gets treated as a resolve, and must be present, otherwise, the returning value will be 'undefined'
};
asyncFunction()
    // As it can be seen here, the error handling approach is the same that gets used with promises.
    .then(result => {
    console.log(result);
    })
    .catch(err => {
    console.log(err);
});
```

*  Because the block in the async/await construct is now synchronous, when the ‘await’ keyword is used, error handling is done with the try/catch construct:

```javascript
const doAsyncTask = () => Promise.reject("Error");
const asyncFunction = async function () {
    try {
        const value = await doAsyncTask();
    } catch (e) {
        console.error(e);
        return;
    }
};
```

*  Using async/await instead of promises involves using structures that are similar to this:

```javascript
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
const files = ["./files/demofile.txt", "./files/demofile.other.txt"];
let promises = files.map(name => readFile(name, { encoding: "utf8" }));
Promise.all(promises).then(values => {
    console.log(values);
});
```

Which will get translated to this:

```javascript
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
const files = ["./node/files/demofile.txt",
    "./node/files/demofile.other.txt"];
(async () => {
    //files.forEach(function(file){
    /**
    using forEach is wrong, because it turns the loop from an async construct, to a sync construct that is because forEach is by definition blocking also, apparently, forEach is slower
    */
    for (let file of files) {
        let fileData = await readFile(file);
        console.log(fileData);
    }
})();
```

Or to this construct(which is the most effective):

```javascript
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
const files = ["./node/files/demofile.txt",
    "./node/files/demofile.other.txt"];
/**
* This is the most effective approach, by using async/await and promise.all
*/
(async () => {
    let promises = files.map(name => readFile(name, "utf8"));
    let filesData = await Promise.all(promises);
    console.log(filesData.toString());
})();
```

*  Using ‘async’ before a method does not guarantee that the respective method will become asynchronous. There must exist a valid combination between async-await, to ensure asynchronism, in this case:

```javascript
/**
This will actually work synchronously, mostly because async, without await is not asynchronous, so it will print:
1
2
Finished
*/
async function printLine1() {
    console.log("1");
}
async function printLine2() {
    console.log("2");
}
async function main() {
    printLine1();
    printLine2();
}
let ayncExec = await main() {
    console.log("Finished");
};
```

Using ‘await’ in certain places in the codebase, to take advantage of the functions that have been declared as ‘async’:

```javascript
/**
This will actually work asynchronously, and main() still needs to be defined as being 'async', because we need to be using the 'await' keyword in the function body.
1
Finished
2
*/
async function printLine1() {
    console.log("1");
}
async function printLine2() {
    console.log("2");
}
async function main() {
    let asyncOne = await printLine1();
    let asyncTwo = await printLine2();
}
main();
console.log("Finished");
```

To minimize the number of modifications to be done, this can be used:

```javascript
/**
This will actually work asynchronously, and 'setImmediate' will execute the codeblock after the rest of the I/O operations in the calling block have finished executing. The output will be this one:
2
Finished
1
*/
async function printLine1() {
    setImmediate(_ => console.log("1"));
}
async function printLine2() {
    console.log("2");
}
async function main() {
    printLine1();
    printLine2();
}
main();
console.log("Finished");
```

*  Iterators in JavaScript allow for the iteration through the properties of a data structure/object that has the exposed to the public. It is basically a pointer for traversing the elements of a data structure. It is a relatively new feature, which can be accessed starting with Node version 9.1, as an experimental feature. Starting with Node 10, it is widely available and it can be used without setting any flags upon launch.
*  Another feature which can make writing async/await code a much more concise and elegant job is the following code construct:

```javascript
(async () => {
    const util = require("util");
    const fs = require("fs");
    const readFile = util.promisify(fs.readFile);
    const files = ["./files/demofile.txt", "./files/demofile.other.txt"];
    const promises = files.map(name => {
        console.log(`The path to the file is: ${name}`);
        readFile(name, "utf8");
    });
    //'await' here has the role of making the construct blocking
    for await (let content of promises) {
        /**
        * the 'await after the 'for' is equivalent to the following code construct: await file;
        */
        console.log(content);
    }
})();
```

*  In JavaScript, arrays are considered to be iterators, meaning that they are objects which contain a property with the name ‘Symbol.iterator’, which points to an object with a ‘next()’ function that will return an object with ‘{done: false, value: ?}’ for each of the values of the object. To complete the iterator, just return ‘done: true’ instead of the object. Then, this can be used anywhere as an iterator, just like with the ‘for-of’ construct.

```javascript
/**
* This is a custom iterator – blocking construct
*/
const customIterator = () => ({ //declare the function. The round bracket ensures that the content within its scope gets returned as an object.
[Symbol.iterator]: () => ({ //the object has a special type of property: [Symbol.iterator].The round bracket after the fat arrow means it will return an object.
        x: 0, //the initial property
    next() { //the next function for being able to iterate through, and the logic inside of it
        if (this.x > 10) { //this references the current instance of the object
            return {
                done: true,
                value: this.x
            };
        }
        return {
            done: false,
            value: this.x++
        };
    }
})
});
//to use the custom iterator the 'for-of' construct is used here
for (let x of customIterator()) {
    console.log(x);
}
```

*  Async iterators are non-blocking asynchronous constructs, in which the ‘next()’ function will return a promise, instead of an object:

```javascript
/**
* This is a custom async iterator
*/
const customAsyncIterator = () => ({ //declare the function. The round
    bracket ensures that the content within its scope gets returned as an object.
[Symbol.asyncIterator]: () => ({ //the object has a special type of
    property: [Symbol.asyncIterator].The round bracket after the fat arrow means
it will return an object.
        x: 0, //the initial property
    next() { //the next function for being able to iterate through, and the
        logic inside of it
        let y = ++this.x;
        if (this.x > 10) {
            /**
            * notice that, instead of returning an object, a promise that
            resolves to that object is returned
            */
            return Promise.resolve({
                done: true,
                value: this.x
            });
        }
        return Promise.resolve({
            done: false,
            value: y
        });
    }
})
});
//to use the custom iterator the 'for-await-of' construct is used here
(async () => {
    for await (let x of customAsyncIterator()) {
        console.log(x);
    }
})();
```

*  Using custom async iterators in frequent Node operations involves error handling as well, when returning the promise constructs :

```javascript
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
const fileIterator = files => ({
    [Symbol.asyncIterator]: () => ({
        x: 0,
        next() {
            let y = this.x++;
            if (this.x > files.length) {
                return Promise.resolve({ //when reaching the opposite of the positive condition, end the execution of the custom async iterator
                    done: true
                });
            }
            return readFile(files[y]) // return the promise itself
                .then(data => ({ //with the resolve handler, return the agreed upon code construct, to allow for the continuation of the async iteration
                    done: false,
                    value: data
                }))
                .catch(err => ({ //with the error handler, return the agreed upon code construct, to allow for the continuation of the async iteration
                    done: false,
                    value: err
                }));
            /**
            * using .then and .catch means that the execution will handle eventual errors and provide output for the good parameters
            */
        }
    })
});
(async () => {
    for await (let x of fileIterator([
        "../codeStuff/files/demofile.txt",
        "../codeStuff/files/demofile.other.txt"
    ])) {
        console.log(x.toString());
    }
})();
```

*  In contrast to the usual way of running code (RUN-TO-COMPLETION), generators implement a new approach for running code, which involves stopping at a certain point in time, based on certain triggers (RUN-STOP-RUN). 
*  Generators will be able to stop during the execution and ‘yield’ the execution context to a certain block of code. The generator cannot be stopped by outside code. It can only be ‘paused’ by its own logic, with the ‘yield’ keyword. Only the code it yielded to can resume the execution of the generator.
*  Generators are marked with a star after the function name:

```javascript
function* demo() {
    console.log("1");
    yield;
    console.log("2");
}
console.log("start");
const it = demo(); // Doesn't execute the body of the function
console.log("before iteration");
console.log(it.next()); // Executes generator and goes on until the 'yield' line, and goes 'out' of the function
console.log(it.next()); // Goes back into the function, after the line where it yielded control of the execution context, and finishes the execution of the body, then comes back here
console.log(it.next()); // Won't go back into the function, because it has finished executing its body, but will finalize the execution
console.log("after iteration");
```

*  Data can be passed out from the generator, by adding the variable name after the ‘yield’. If the execution of the iterator is done, but we’re still calling it, the returned object will have the value: undefined and the done: true :

```javascript
function* demo() {
    for (let i = 0; i < 4; i++) {
        yield `Yielded this: ${i}`; //passed the value of the counter 'i'
    }
    yield "2";
    return "finished"; //in this context, ‘return’ is equivalent to ‘yield’
}
console.log("start");
const it = demo();
console.log("before iteration");
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log("after iteration");
```

*  We can iterate through all the yields of the generator, like this:

```javascript
function* demo() {
    for (let i = 0; i < 4; i++) {
        yield `Yielded this: ${i}`; //passed the value of the counter 'i'
    }
    yield "2";
    return 'finished'; //with this construct, 'return' isn't reached anymore. The execution ends at 'yield'.
}
console.log("start");
console.log("before iteration");
for (let x of demo()) {
    console.log(x); //this will call .next() until it reaches the end of the execution for the iterator
}
console.log("after iteration");
```

*  Since data can be passed from the generator, to an outside functionality, we can pass data back to the generator as well, using the ‘next()’ method calls:

```javascript
function* demoSay() {
    console.log(yield);
    console.log(" World");
}
const it = demoSay();
it.next(); //we have the yield, and the implicit pause that comes with it
it.next("Hello"); //we have the comeback to the function, that passes in the "Hello" value
```

* Generators can be combined with ‘for-await-of’ structures, to obtain asynchronous generators, like so:

```javascript
function* range() {
    for (let i = 0; i < 10; i++) {
        yield Promise.resolve(i);
    }
}
(async () => {
    for await (let x of range()) {
        console.log(x);
    }
})(); //needs the open & closed bracket to be executed as a function call. Otherwise, it's pointless
```

* Using promises with generators and error handling within them:

```javascript
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
function* fileLoader(files) {
    for (let i = 0; i < files.length; i++) {
        yield readFile(files[i]) //yield a promise, in order to be able to handle the errors as well
            .then(data => {
                return data; //return the data from the promise
            })
            .catch(err => {
                return err; //return the error from the promise
            })
    }
}
(async () => {
    for await (let contents of fileLoader([ //call the async generator, with the file paths as parameters
        "../codeStuff/files/demofile.txt",
        "../codeStuff/files/demofile3.other.txt"
    ])) {
        console.log(contents.toString());
    }
})();
```

* The event loop in NodeJS consists of microtasks and macrotasks. Unsurprisingly, the macrotasks have priority in the execution order, when compared to the microtasks.
*  The gross execution order in the event loop is:
1. setTimeout(), setInterval() kind of methods
1. I/O
1. setImmediate() kind of methods
1. close the execution for some blocks

Example:

```javascript
console.log("start"); //loop1
/**
* All callbacks are considered microtasks.
*/
const interval = setInterval(() => { //this is a macrotask (1) -> has priority
    debugger;
    console.log("setInterval"); //microtask 3 -> loop 2, 3, 4
}, 0);
setTimeout(() => { //yet another macrotask(2) -> loop 1
    debugger;
    console.log("setTimeout 1");
    Promise.resolve() //this will add 3 microtasks to the queue (starting at no 4)
        .then(() => { //microtask 4 -> loop 3
            debugger;
            console.log("promise 3");
        })
        .then(() => {//microtask 5 -> loop 3
            debugger;
            console.log("promise 4");
        })
        .then(() => {//microtask 6 -> loop 3
            setTimeout(() => {//macrotask 4 -> loop 4
                debugger;
                console.log("setTimeout 2");
                Promise.resolve()//macrotask 5 -> loop 5
                    .then(() => {
                        debugger;
                        console.log("promise 5");//microtask 7 -> loop 6
                    })
                    .then(() => {
                        debugger;
                        console.log("promise 6");//microtask 8 -> loop 7
                    })
                    .then(() => {
                        debugger;
                        clearInterval(interval);//microtask 9 -> loop 7
                    });
            });
        });
});
Promise.resolve() //macrotask 3 -> loop 1
    .then(() => {
        debugger;
        console.log("promise 1"); //microtask 1 -> loop 2
    })
    .then(() => {
        debugger;
        console.log("promise 2"); //microtask 2 -> loop 2
    });
console.log("end"); //loop 1
```