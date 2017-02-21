---
title: The 40,000 Dollar Checklist
date: 2015-04-16
published: true
---

The other day my wife and I were talking to a friend of hers and she told us how
launching a campaign with a tiny mistake cost her company around $40K. She hated
the fact that it could have been prevented if only she had "double checked".
I told her how I use checklists for deploying code to production and she
couldn't believe there was such a simple solution.

READMORE

I also learned this lesson the hard way. One time I was asked to generate
a report. I wrote some SQL and hit enter. "What an idiot!" I said to myself as
I realized I had left a TRUNCATE TABLE at the top of my script (If you are not
familiar with SQL this means delete all the information).

And of course I had no backup. Luckily I was able to regenerate the information
from different sources but not without wasting a LOT of time! After that
I decided to use a checklist before putting anything in production, so I made
sure I had an updated backup.

## When to use checklists

Going through a checklist is a simple but brilliant solution used to reduce
mistakes in Lean manufacturing, the aviation industry, some areas of medicine,
etc. However unless you are some sort of cyborg you don't want to systematize
every single thing you do during your day.

The way I decide what goes into a checklist and what doesn't is by making myself
the following question

> How high is the risk of getting this wrong?

Not every mistake will be a matter of life and death, nor every bug cost you
thousands of dollars, but if there is a chance it will, make sure you get things
right the first time.

## How to do a checklist

 - [ X ] Done Item
 - [ \_ ] Pending Item

I'm serious! You can use pen and paper, a text document, a spreadsheet or some
app like [Wunderlist][1]. Just make sure it is simple enough that you'll want to
use it.

[1]: https://www.wunderlist.com/
