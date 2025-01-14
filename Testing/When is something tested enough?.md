# Introduction
When you commit a feature to production, we can assume it works locally. However, in production we find that untested logic appears and bugs too. How can we setup a framework so the undefined logic is found before pushing to production and before damage is done?

# Assumptions
- I assume there's a production environment, a dev environment and a testing environment

# Write new processes next to existing processes
When replacing logic, it's good practice to code the new version next to the existing process. This way you can test independently of the old process, and see results side by side.

When converting stored procedures to managed code, start off writing a wrapper around the old API that forwards the call verbatim. Then you can do something like this:

```C#
DoWrite(Message msg){
	ForwardToOldImplementation(msg);
	ForwardToNewImplementation(msg);

	if(!NewDataIsValidWithOldData()){
		NotifyDevs(msg);
	}
}

DoRead(Message msg){
	var old = ForwardRead(msg);
	var @new = ForwardRead(msg);
	if(!deepCompare(old, @new)){
		NotifyDevs(msg, old, @new);
	}
	return old;
}
```

Store the new results in a new database / table, and don't touch the old implementation. Only when you've gone a while without notifications can you remove the old call

# Reusing existing logic
> TODO: Reusing pure methods should be ok?

If you've re-used existing business logic, there might be side effects that produced unwanted situations. Race conditions, database getters and setters, service calls. 

You should map which externalities are used by the existing logic, and which assumptions are made about these inputs and outputs. Write unit tests formalising the assumptions of the unit tests. Once the new calls are implemented, the assumptions should still be the same.

> ❗️ When a side effect is produced, like a service call or a database insert, you should also map who uses this service or this table, and map the assumptions that are mad about the service. Are these assumptions invalidated? Then these indirect consumers should also be re-tested.