---
title: Ledger Tutorial part 1
date: 2015-01-15
published: true
featured: 3
---

[Ledger](http://www.ledger-cli.org/) is a great tool to keep track of your
finances. It's simple, fast and most important it doesn't limit you like most
other tools.

The following post is a small introduction on how to start traking your finances
this 2015.

READMORE

## Installation

Installing ledger in OS X is as simple as calling a brew command:

```shell
brew install ledger
```

[Here](http://www.ledger-cli.org/download.html) you can find information for
other systems.

## Define a starting point

In order to keep track of your finances you need to start somewhere. If you are
doing this at the beginning of the year or in the middle of it, it doesn't
matter. The important thing is that you have the sources of your different
accounts and credit cards so you can migrate them to ledger.

I will suggest you try with your current balance today. Don't try to get old
data now. Picking up a new tool is already hard work, specially if you need to
build a habit to use it. Plus once you dominate it you will be able to automate
things and you might be able to import that data in a snap.

My starting point for this tutorial will be January 1st 2015. So now I go into
every imaginary account I have and verify what is my balance up to that day.
Once I have collected all that information I will write it down into my journal
file from which ledger will read and generate reports.

Create a file called journal.dat and add the following

```
; Testing ledger CLI
2015/01/01  Opening Balance
  Assets:BankOne:Checking                $100.00
  Assets:BankTwo:Savings               $2,000.00
  Liabilities:BankOne:Mastercard        $-250.00
  Liabilities:BankThree:Visa             $-30.00
  Equity:Opening Balances
```

**Line 1:** starts with a semicolon and it's just a comment so you don't really need
it but it's very useful when you need to keep notes about a particular expense.

_You will find other ways to put comments in the
[documentation](http://www.ledger-cli.org/docs.html). Feel free to use the one
you prefer. For consistency purposes I will use the semicolon._

**Line 2:** starts with a date. This is the date from the registry we are adding.
_Everything between this date and the next empty line will be our registry._ The
format is **YYYY/MM/DD**. _Banks don't always follow the same format for dates
so be careful with this_

After the date we see the payee. This is the entity to whom the transaction is
being referenced. If you bought something from Amazon.com here you would put
"Amazon". Was the case that your employer paid your salary, here you would write
your employer's name. In this case we will call it "Opening Balance" since there
is no real entity.

**Lines 3-4:** Correspond to assets. In this case we have two banks. One with
a Checking account and one with a Savings account.

**Line 5-6:** Represents liabilites, a Mastercard from the same bank
than our Checkings account and a Visa from another bank.

**Line 7:** is the last line of the registry. You can see that it has no value
assigned to it. When you do this, Ledger knows that you expect him to add
everything in the previous lines and balance it to the given account. In this
case our account is "Equity:Openin Balances"

This special account is necesary because Ledger is a double-entry accounting
system. That means that for every transaction there has to be a transaction of
equal negative value so that everything balances.

## Knowing your Net Worth

Perfect now that we have written down all the values of our accounts down.
Run in the terminal

```shell
ledger -f journal.dat balance Assets Liabilities
```

What this command says is:

ledger from the file journal.dat balance my Assets and Liabilities. The result
will be as follows.

```
           $2,100.00  Assets
             $100.00    BankOne:Checking
           $2,000.00    BankTwo:Savings
            $-280.00  Liabilities
            $-250.00    BankOne:Mastercard
             $-30.00    BankThree:Visa
--------------------
           $1,820.00
```

**Line 1:** is the total of your Assets. _The sum of BankOne:Checking
and BankTwo:Savings._

**Line 4:** is the total of your Liabilities. _The sum of BankOne:Mastercard and
BankThree:Visa_

**Line 8:** is your net worth. _The sum of your Assets and your Liabilities_

With this information now you know that even if you see $2,100.00 in your banks
that you actually only have $1,820.00 that are yours because you have $280 in
credit that you need to pay.

_If you liked this post please let me know in the comments so I write more about
this topic._

