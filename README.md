# ee-queue

[![Greenkeeper badge](https://badges.greenkeeper.io/eventEmitter/ee-queue.svg)](https://greenkeeper.io/)

Queue with optional timing support

this may help you to implmenet rate limiting and timed retransmissions


	var Queue = require( "ee-queue" );

	// all options are optional
	var q = new Queue( {
		  maxItems:    	1000000 	// max items in the queue
		, maxAge:		3600 * 24 	// no item should be older than 1d
		, threads: 		100 		// execute 100 items in parallel
	} );



	// item from queue is ready to be executed
	q.on( "execute", function( item, next ){
		// next must be called to make sure the next item from the queue can be executed

	} );


	// items that expire due to the max age or max items options will be sent here
	// if the maxItems options is used always the oldest item exires
	q.on( "expire", function( itemId, item, age ){
		
	} );



	// you may queue an item of any type, if the type is function the function will be executed and the execute or expire event on the queue will not be emitted.
	// if the scheduledExecutionTime is passed the item will be executed n seconds in the future ( as soon there is a free slot )
	// if the maxAge argument is passed the scheduledExecutionTime must be set or null. maxAge is n seconds in the future.
	var itemId = q.add( function( err, next ){
		// err -> the item was not executed and expired due to the maxAge or item limit directive
		// next -> must be called to execute the next item. this argument is only present if there is no error
	}, [ scheduledExecutionTime ], [ maxAge ] );



	// you may remove items again
	var item = q.remove( itemId );