###事件循环

当我们执行 JS 代码的时候其实就是往执行栈中放入函数，其实当遇到异步的代码时，会被挂起并在需要执行的时候加入到 Task（有多种 Task） 队列中。一旦执行栈为空，Event Loop 就会从 Task 队列中拿出需要执行的代码并放入执行栈中执行

不同的任务源会被分配到不同的 Task 队列中，任务源可以分为 微任务（microtask） 和 宏任务（macrotask）。在 ES6 规范中，microtask 称为 jobs，macrotask 称为 task

微任务包括 process.nextTick ，promise ，MutationObserver，其中 process.nextTick 为 Node 独有。

宏任务包括 script ， setTimeout ，setInterval ，setImmediate ，I/O ，UI rendering。

Event Loop 执行顺序

首先执行同步代码，这属于宏任务，当执行完所有同步代码后，执行栈为空，查询是否有异步代码需要执行，执行所有微任务，当执行完所有微任务后，如有必要会渲染页面，然后开始下一轮 Event Loop，执行宏任务中的异步代码，也就是 setTimeout 中的回调函数


		setTimeout(function() {
    		console.log('setTimeout');
		})

		new Promise(function(resolve) {
		    console.log('promise');
		}).then(function() {
		    console.log('then');
		})

		console.log('console');
		//promise，console，then，setTimeout，



		console.log('1');

		setTimeout(function() {
		    console.log('2');
		   
		    new Promise(function(resolve) {
		        console.log('4');
		        resolve();
		    }).then(function() {
		        console.log('5')
		    })
		})
		
		new Promise(function(resolve) {
		    console.log('7');
		    resolve();
		}).then(function() {
		    console.log('8')
		})

		setTimeout(function() {
		    console.log('9');
		   
		    new Promise(function(resolve) {
		        console.log('11');
		        resolve();
		    }).then(function() {
		        console.log('12')
		    })
		})

		//1,7,8,2,4,5,9,11,12


