---
layout: post
title: "Azure DevOps: #13 Table Storage"
date: 2017-05-22
category: Azure Storage

---

Table storage is a slightly misleading name.  It is actually a key value store where the "Key" is made up of the PartitionKey and RowKey and the "Value" is the associated attributes.

It can support 20,000 operations/s or >10 million operations/s with Cosmos DB Table API.

It is very easy to use.

The code snippet below creates a table called products and inserts a simple product into it, setting the PartitionKey to "insuranceproduct" and the RowKey to the ProductName.

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

If we pass a product into our function app we can see that it has created the table and saved the data.

![](/images/Table-Storage-Products-01.png)