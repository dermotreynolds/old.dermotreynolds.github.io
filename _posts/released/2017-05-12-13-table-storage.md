---
layout: post
title: "Azure Table Storage"
date: 2017-05-22
category: Azure Storage
tags: [Azure, C#, Table Storage, v1]
---
Table storage is a NoSQL key-value store that is very easy to use and supports high throughput.

The "Key" is made up of the PartitionKey and RowKey and the "Value" is the associated attributes.

It can support 20,000 operations/s or >10 million operations/s with Cosmos DB Table API.

It is limited in functionality in comparison to other database technologies.  As an example, it does not support joins - or the vast majority of functionality that you will get in other databases - so you will generally need to de-normalise your data in order to support efficient querying.

To show you how easy it is to use, the code snippet below creates a table called products and inserts a simple product into it, setting the PartitionKey to "insuranceproduct" and the RowKey to the ProductName.

~~~~~~
// This sets the rowkey value
string sRowKey = e.ProductName;

// Create the Entity and set the PartitionKey to insuranceproduct.
ProductEntity _product = new ProductEntity("insuranceproduct", sRowKey);

_product.Product_Market = e.ProductMarket;
_product.Product_Cover = e.Cover;
_product.Product_Price = e.Price;

// Connect to the Storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(cloudStorageString);

CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("products");

table.CreateIfNotExists();

TableOperation insertOperation = TableOperation.Insert(_product);

table.Execute(insertOperation); 
~~~~~~

We have used the above snippet in the function app example.

If we pass a product into our function app we can see that it has created the table and saved the data.

![](/images/Table-Storage-Products-01.png)

The tool used for managing Azure Storage is __[Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/){:target="_blank"}__.
