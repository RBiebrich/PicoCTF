# SQL Direct
Web Exploitation, 200 points
### Description:
> Connect to this PostgreSQL server and find the flag! Additional details will be available after launching your challenge instance.

First, we launch the instance and we are provided with the necessary information to connect to a Postgres Server. Implementing this information in terminal, we are greeted by a query prompt and told only how to access a help page. To start, I think it would be wise for us to get a birds-eye view of the database we are in. We can query the database to show us all the tables it contains using the command **\dt**:

![schema](https://github.com/RBiebrich/PicoCTF/blob/main/assets/schema.png)

From here we can see that the database contains a table called flags! We can write a query to show us the contents of this table:

```SQL
SELECT * FROM flags;
```

![table](https://github.com/RBiebrich/PicoCTF/blob/main/assets/table.png)

We found the flag!

**picoCTF{L3arN_S0m3_5qL_t0d4Y_31fd14c0}**
