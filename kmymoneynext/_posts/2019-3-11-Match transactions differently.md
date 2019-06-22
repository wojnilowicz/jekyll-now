---
layout: post
title: Match transactions differently
---

This patch allows to write and read information about matched transactions in a way suited well for SQL and XML storage.
Until now matched transactions worked only with XML storage.
In general, the new way entails referring to matched transactions by their IDs instead of embedding transaction in a transaction.

Changes:

###### 1) honor transaction IDs in XML storage, just like it is in SQL storage

Those IDs were the only ones that have no significance while loading storage, because at each loading, new transaction IDs were assigned.
We should use them now if we want to refer to transactions by their IDs.

###### 2) new transaction attribute called origin

It tells if the transaction is: imported, typed, matching input (transaction that served for matching process), matching output (transaction that is a result of matching process). It's an alternative for hiding a matched transaction (previously by embedding it in an another transaction) from balance calculation.

###### 3) no embedding of a matched transaction by a matched split

Transaction node as XML has been embedded in another transaction as a key-value pair. That's not well suited for SQL but also for easy undoing of operation. That's because matched split stored backup information not only about matched split but also about matched transaction in itself. That mix of backup values is not transparent, hard to develop further and error prone.
Now third transaction is created and the two that served for matching are hidden and the new transaction refers to the hidden ones by their IDs.

###### 4) information about storage version change in case of XML storage

User now gets an information window, that tells him that his storage gets updated. I think it's important to rise user's awareness in that situation instead of silently update his storage which always may go askew. SQL storage should also get this kind of information soon.

###### 5) xmlstorageupdater

Center place for all updates in XML storage. I think updater should update file and then give it to KMyMoney as the latest one. Later on it could be separate plugin or something like that. SQL storage should also get this kind of updater soon.

<br>
Below is a mockup to maybe show better what I've created

[![Mockup](/images/transaction-matching-mockup.png)](/images/transaction-matching-mockup.png)