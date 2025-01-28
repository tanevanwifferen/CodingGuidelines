- Check higher up if it's caught, or if it should bubble.

## What can happen?

In background processes, in loops, you might not want the exception to break your loop. When you used to return a boolean to see if a subroutine succeeded, and you replace that with throwing an exception (maybe to [Rollback a transaction](Database/Transactions)), you'll have to make sure the exception is caught at the correct time.