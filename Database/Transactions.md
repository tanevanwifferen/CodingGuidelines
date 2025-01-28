- Use try-catch to see when a transaction should be rolled back

## Why?

For code cleanliness, it is better to use transactions than returning booleans.

Example:

```js
db.startTransaction();
var shouldCommit = true;
for(let item of items){
	shouldCommit &= addItem(item);
}
if(shouldCommit){
	db.commitTransaction();
} else {
	db.rollBackTransaction();
}
```

This could be a lot more readable, robust, and as side effect it wouldn't finisht the loop when we use try-catch

```js
db.startTransaction()
try {
	for(let item of items){
		addItem(item);
	}	
	db.commitTransaction();
} catch(e){
	db.rollbackTransaction();
}
```