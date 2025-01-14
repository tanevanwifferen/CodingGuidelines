# Summary
1. Check all usages of the enum type
2. Check all usages of the containing class
# Introduction and  terminology
We have a class `Account`, which contains a property `AccountType`
The account type is `Credit` or `Debit`, and we want to add a type `Mortgage`
# 1. Check existing behaviour
## Inclusive filters
When we want to search for **debt** a person has, we now return `Accounts` where `Type` is `Credit`. 
```C#
var debt = accounts.Where(acc => acc.AccountType == AccountType.Credit)
```
This logic breaks when we add `Mortgage` type, because a mortgage is also a form of debt.

To resolve this:

1. Search for all usages of the `AccountType` enum and see where logic needs to be updated

## Exclusive filters
There's also the option where the query could be setup like this:
```C#
var debt = accounts.Where(acc => acc.AccountType != AccountType.Debit)
```
> Note the != here

1.  Check all usages of the `Account` class (all classes that use `AccountType`), and see where new logic needs to be implemented

# 2. Introduce new behaviour
Example: to query the balance of the account, we query all Accounts for a person. However, we only want to see liquid funds, so we should add an introduction of a filter to only return Accounts where Type is `Credit` or `Debit`

Imagine this code:
```C#
var spendableBalance = accounts.Sum(x => x.Balance);
```

> There is no usage of the AccountType here, so using #1 doesn't apply here, as it doesn't show up in our search results. The way to find where the logic needs to be introduced is to search for all usages of the Account class.

2. Find where the enum is used, and query all usages of those classes. Where logic needs to be introduced, introduce it.

# TODO
1. When introducing logic, you can add equality and inequality checks. 
2. What about libraries? You don't know the ways a consumer uses your code.