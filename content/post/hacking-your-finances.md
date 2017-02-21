---
title: Hacking your finances
date: 2015-01-08
category: finances
published: true
---
Being a good developer is not only about knowing how to code. There are many
other skills like communication, management, decision-making, etc. One of those
very important skills is keeping track of your personal finances.

READMORE

## Before you begin

There are two important things to learn if you want to keep your finances under
control.

 1. You need a tool that makes it **EASY** to keep track of your income and expenses
 2. You need to develop the **HABIT** of using that tool constantly
 3. You need to generate the right reports in order to make **SMART** decisions

## Finding the right tool

Finding the right tool is the easy part. All you need to do is to find something
that **a)** covers all your requirements and **b)** makes you feel comfortable when
using it.

By covering all your requirements what I mean is that the tool must be able to
adapt to the kind of finances you manage. This will be very specific to each
individual so you will have to find out what you need but here is a list of the
important things I require from my tool

 - connect or import my non US bank statements
 - handle multiple currencies including Guatemalan Quetzales
 - access it while traveling
 - be able to track virtual accounts within my accounts (ie: funds)
 - track reimbursements from multiple companies
 - present reports of net flow, monthly expenses, etc.
 - track budget

Now don't think that I came to have all these needs since I started keeping
track of my finances. The first tool I used was Microsoft Money, it did all
I needed and I could make reports easily. Later on I switched to using Google
Sheets since it was so easy to access it from any computer and I wasn't using
Windows any more. After that I tried GNUCash because it did all that my
spreadsheet did and more, plus thanks to Dropbox I could have it updated in
every machine I used. My last and current tool is
[Ledger](http://ledger-cli.org/) because it allows me to stay withing the
confines of the command line.

Which brings me to the fact that you have to use a tool that you enjoy using.
The truth is I will always prefer to use a tool over the command line than over
the GUI because it makes me feel more productive.

## Developing a Habit

Developing a habit is more complicated and this is why your tool has to make it
easy for you because habits are more likely to stick if it doesn't feel like
a pain doing them. This is why you want to automate as much as you can of the
process and then make a routine about the rest.

There are tools like mint that can make pretty much everything for you but if
you care about privacy I wouldn't recommend it. In my case I don't use it
because they limit usage only to their list of associated banks. Being a CLI
tool, Ledger is fully automatable but be warned that this might take a while.

Initially you might want to do all the process by hand because this will allow
you to see which are the painful parts in the process and also will make you
feel more comfortable with the tool. Doing the process manually also allows you
to build the rules on which your automation will be based.

Depending on how many transactions you make every month you can do your finances
every day/week/two weeks/month. I wouldn't recommend doing them less than every
month and more than once a day would be definitely too much. For me biweekly is
the best option. I do it once every 5th of the month because it is when I have
to pay my cards and I do it once every 20th because it is when I get paid.

Now the trick to create a routine is **1)** having a trigger **2)** doing the process
without interruptions **3)** having a reward. This is why I chose those specific
dates because there are specific triggers and rewards as you can see:

|        | 5th                             | 20th                             |
|--------|---------------------------------|----------------------------------|
|Trigger |Credit Card payment due          | I get paid                       |
|Process |Pay Credit Cards & update ledger | Update ledger & generate Reports |
|Reward  |I don't pay interests            | I'm confident on where I stand   |

Another little hack I have is that I use the card for everything. If I take cash
out I keep it as a miscellaneous expense and I don't record any detail of it. Some
people like to use their smart-phone to keep track of those expenses but I find
it tedious.

Once you have defined your habit you just have to stick to it. No matter what,
you stick to it! Believe me after a while it will become habit.

## Generating the right reports

What kind of decisions you need to make will depend on your current financial
situation. Whether you should spend some money or not will be completely
different if you are in debt or not. So first you need to determine what are
your priorities.

Once you know your priorities you need to create a budget. I'm not going to go
through that right now but I will in a future post. Creating a budget is like
setting up your Key Performance Indicators. If you stick to it you will reach
your goal.

Let's say you know you need to replace your computer every 3 years and you are
willing to spend no more than $2000 on it. Then you know you need to put $55
aside each month in order to accomplish your goal.

There are many reports you may want to have and with enough data to play with
you may even come up with new ones but I believe having the following three is
a good place to start.

- **Net Worth:** You want to know how much money do you have and how much you
owe.
- **Monthly Expenses:** You want to know where your money is going.
- **Funds Balance:** You want to measure where you stand with your goals.

## Final thoughts

Keeping track of your finances is not harder than coding or managing a project.
If you gamify it you may even have lot's of fun while doing it.

I'm working on a couple of posts in order to show in detail how I use Ledger. In
the mean time feel free to contact me with any questions you may have.

