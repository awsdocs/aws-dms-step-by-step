# Working with the Sample Database for Migration<a name="chap-on-premoracle2aurora.appendix.sampledatabase"></a>

We recommend working through the preceding outline and guide by using the sample Oracle database provided by Amazon\. This database mimics a simple sporting event ticketing system\. The scripts to generate the sample database can be found at [https://github\.com/aws\-samples/aws\-database\-migration\-samples/tree/master/oracle/sampledb/v1](https://github.com/aws-samples/aws-database-migration-samples/tree/master/oracle/sampledb/v1)\.

To build the sample database, go to the oracle/sampledb/v1 folder and follow the instructions in the README\.md file\.

The sample creates approximately 8\-10 GB of data\. The sample database also includes a *ticketManagment* package, which you can use to generate some transactions\. To generate transactions, log into SQL\*Plus or SQL Developer and run the following as ** *dms\_sample* **:

```
SQL>call generateTicketActivity(1000,0.01);
```

The first parameter is the transaction delay in seconds, the second is the number of transactions to generate\. The procedure preceding simply "sells tickets" to people\. You’ll see updates to the tables: sporting\_event\_ticket, and ticket\_purchase\_history\.

Once you’ve "sold" some tickets, you can transfer them using the command following:

```
SQL>call generateTransferActivity(100,0.1);
```

The first parameter is the transaction delay in seconds, the second is the number of transactions to generate\. This procedure also updates sporting\_event\_ticket and ticket\_purchase\_history\.