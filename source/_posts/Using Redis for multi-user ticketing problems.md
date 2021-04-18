---
title: Using Redis for multi-user ticketing problems
date: 2020-11-07 21:09:32
tags: Redis
category: Redis
---
## Redis Transaction Processing Mechanisms
There are four ACIDs that transactions in an RDBMS satisfy, representing atomicity, consistency, isolation, and persistence.

There are a few differences between Redis transactions and RDBMS transactions.

First of all, Redis does not support a transaction rollback mechanism, which means that when an error occurs in a transaction, the entire transaction continues to execute until all commands in the transaction queue have been executed. The official Redis documentation explains why Redis does not support transaction rollbacks.

Redis command execution fails only if there is a programming syntax error. This error usually occurs in development environments, but rarely in production environments, and there is no need to develop a transaction rollback feature.

In addition, Redis is an in-memory database and, unlike file-based RDBMSs, typically only performs in-memory calculations and operations and does not guarantee persistence. However, Redis also provides two persistence modes, the RDB and AOF modes.

RDB persistence generates a snapshot of the current process data and saves it to disk, and RDB persistence can be triggered manually or automatically. Because the persistence operation is not synchronized with the command operation, the persistence of the transaction is not guaranteed.

AOF persistence uses logging of each write operation, which makes up for the lack of data consistency in RDB, but using AOF mode means that each executed command needs to be written to a file, which will greatly reduce Redis access performance. There are three different ways to enable AOF mode, and the default is everysec, which is synchronized once per second. This is followed by always and no modes, which mean that data is written to the AOF file whenever it is modified, and the operating system decides when it is logged to the AOF file.

Although Redis provides two persistence mechanisms, persistence is not its strong suit as an in-memory database.

Redis is a single-threaded program that does not interrupt transactions while they are executing, and various operations submitted by other clients cannot be performed, so you can understand that Redis transaction processing is serialized in a way that is always isolated.

## Transactional Commands for Redis
Once we understand Redis' transaction processing mechanism, let's look at what commands are included in Redis' transactions.

MULTI: Open a transaction.
EXEC: transaction execution, which will execute all commands within the transaction at once.
DISCARD: Cancellation of a transaction.
WATCH: monitors one or more keys and also interrupts a transaction if a key is changed before the transaction is executed.
UNWATCH: cancels the WATCH command's monitoring of all keys.
It should be noted that Redis implements transactions based on the COMMAND queue. If Redis does not open a transaction, any COMMAND will be executed immediately and return the result. If Redis opens a transaction, the COMMAND command is placed in the queue and returns the queued state QUEUED, and commands in the COMMAND queue are executed only when EXEC is called.

For example, we use a transaction to store information about the heroes selected by 5 players, with the following code.

```sql
MULTI
hmset user:001 hero 'zhangfei' hp_max 8341 mp_max 100
hmset user:002 hero 'guanyu' hp_max 7107 mp_max 10
hmset user:003 hero 'liubei' hp_max 6900 mp_max 1742
hmset user:004 hero 'dianwei' hp_max 7516 mp_max 1774
hmset user:005 hero 'diaochan' hp_max 5611 mp_max 1960
EXEC
```
You can see that all COMMAND commands between MULTI and EXEC are placed in the COMMAND queue and returned to the queued state, only to be executed all at once when called by EXEC.

![](1.jpg)

We often use Redis' WATCH and MULTI commands to handle concurrent operations on shared resources, such as spikes, ticket grabbing, etc. What WATCH+MULTI actually implements is optimistic locking. Let's use two Redis clients to simulate the ticket grabbing process.

![](2.jpg)

We start Redis client 1, execute the above statement, and then wait for client 2 to complete the above execution before executing EXEC, with the following result for client 2.
![](3.jpg)
Client 1 then executes EXEC with the following result.

![](4.jpg)

You can see that the last ticket was actually grabbed by client 2, because the variable for client 1WATCH's ticket changed before EXEC, and the entire transaction was interrupted, returning a null reply (nil).

It should be noted that the WATCH command can no longer be executed after MULTI, otherwise the WATCH inside MULTI is not allowed error is returned (because WATCH stands for observe the variable to see if it has changed before executing the transaction, and does not interrupt the transaction if the variable has changed, so the transaction is not interrupted before the transaction is also executed). (that is, before MULTI, using WATCH). Also, if there is a syntax error during the execution of a command, Redis will report the error and the entire transaction will not be executed, and Redis will ignore the error at runtime without affecting the execution later.

## Multi-user ticket grabbing simulation
We've just explained the Redis transaction command and simulated the process of two users grabbing tickets using the Redis client. Let's continue the simulation in Python, with three caveats.

In Python, Redis transactions are wrapped in pipeline, so after you create a Redis connection, you need to get the pipeline, and then use the WATCH, MULTI, and EXEC commands through the pipeline.

Secondly, the user is concurrent, so you need to use Python's multithreading, which you create using the threading library.

For the user's ticket grab, you set up the sell function to simulate the user i's ticket grab. Before executing MULTI, you need to use pipe.watch(KEY) to monitor the number of tickets, if the number of tickets is not greater than 0, it means the tickets are sold out and the user has failed to grab a ticket; if the number of tickets is greater than 0, it means the ticket can be grabbed, then execute MULTI again, subtract the number of tickets by 1 and commit. If the number of tickets is greater than 0, it proves that we can snatch the tickets, and then execute MULTI again to subtract the number of tickets by 1 and submit. However, the transaction may fail when it is submitted because an exception will be generated if the monitored KEY is changed. If the transaction is executed successfully, the user is prompted for a successful ticket grab and the current number of remaining tickets is displayed.

```python
import redis
import threading
pool = redis.ConnectionPool(host = '127.0.0.1', port=6379, db=0)
r = redis.StrictRedis(connection_pool = pool)
 
KEY="ticket_count"
def sell(i):
    pipe = r.pipeline()
    while True:
        try:
            pipe.watch(KEY)
            c = int(pipe.get(KEY))      
            if c > 0:
                pipe.multi()            
                c = c - 1
                pipe.set(KEY, c)        
                pipe.execute()
                print('User {} Successful snatching, current number of votes {}'.format(i, c))
                break
            else:
                print('User {} Failed to grab a ticket, tickets are sold out!'.format(i))
                break
        except Exception as e:
            print('User {} failed to grab a ticket, try again'.format(i))
            continue
        finally:
            pipe.unwatch()
 
if __name__ == "__main__":
    r.set(KEY, 5)  
    for i in range(8):
        t = threading.Thread(target=sell, args=(i,))
        t.start()
```

```
User 0 Successful snatching, current number of votes 4
User 4 failed to grab a ticket, try again.
User 1 has successfully snatched tickets, current number of tickets 3
User 2 successfully snatched tickets, current number of tickets 2
User 4 failed to grab a ticket, try again.
User 5 failed to grab a ticket, try again.
User 6 successfully snatched tickets, current number of tickets 1
User 4 snatched the ticket successfully, current number of votes 0
User 5 failed to grab a ticket, try again.
User 3 failed to grab a ticket, try again.
User 7 Failed to grab a ticket, tickets are sold out!
User 5 failed to snag a ticket, tickets are sold out!
User 3 failed to grab a ticket, the ticket is sold out!
```
There is no pessimistic locking in Redis, transaction processing should take into account concurrent requests, we need to implement optimistic locking by WATCH+MULTI, if the monitored KEY does not change then the transaction can be executed smoothly, otherwise it means that the security of the transaction has been compromised, the server will give up on executing the transaction and directly return a null reply (nil) to the client, after the transaction execution fails, we can try again.

## Sum up
A Redis transaction is a collection of Redis commands, and all commands in the transaction are executed in order and without interference from other clients during the execution. However, during the execution of a transaction, Redis may encounter the following two types of errors.

The first is a syntax error, which is the one that occurs when a Redis command is queued.Redis does not allow syntax errors before transaction execution, and if they occur, they will cause transaction execution to fail. As the official documentation says, this usually happens very rarely in production environments, usually in development environments, and if this syntax error is encountered, it is up to the developer to correct the error.

The second is an execution time error, which is an error that occurs during transaction execution, such as handling the wrong type of key, etc. This error is not a syntax error and Redis can only determine it during actual execution. However, Redis does not provide a rollback mechanism, so when this type of error occurs, Redis will continue to execute to ensure that other commands are executed properly.

In transaction processing, we need a locking mechanism to resolve the situation of concurrent access to shared resources. The WATCH+MULTI approach to optimistic locking is provided in Redis. We have previously understood that optimistic locking is the idea that a locking mechanism is implemented by a program to determine when data updates are made, execute if it succeeds, fail if it doesn't, and there is no need to wait for another transaction to release the lock. In fact, this optimistic, simple design philosophy is reflected everywhere in the design of Redis.
