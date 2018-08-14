---
layout: post
title: "Azure Functions"
date: 2017-05-25
category: Azure Function
tags: [Azure, Function App, C#, v1]
---
Azure Functions allow you to create small single function applications which can flex independently of one another.

As per the documentation:

*Azure Functions is a serverless compute service that enables you to run code on-demand without having to explicitly provision or manage infrastructure.*

As mentioned, it allows you to develop small single purpose functions in virtually any language and for each to be triggered in a variety of different ways that can be scaled independently of one another.  For example, you may have a place order function which listens on a queue and when a message appears to perform the PlaceOrder action.

Under the hood a function is an App Service.  One of the advantages of functions is that you can pay on a pure consumption model, therefore there is no need to keep multiple servers running during quiet periods.

I have pasted a very simple HTTP Azure Function below which takes a JSON and stores the data in an Azure Table.  An Azure Function can be created within Visual Studio: New / Projects / Cloud / Azure Functions.  A similar deployment is shown in my Getting Started article.

Our azure function looks as follows once deployed:

![](/images/Azure-Function-01.png)

We can look at the function responding whilst issuing a Postman request.

![](/images/Azure-Function-02.png)

Here is the code for our Azure Function mentioned above:

``` csharp
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using System;
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;

//This can be tested by posting the following body - via postman:
/*
{
    "ProductName": "Home Insurance XL",
    "ProductMarket": "Home",
    "Cover": "Full",
    "Price": "100.11",
}
*/

//A table called products will be created in the storage account specified.


namespace WeFinance
{
    public static class CreateProduct
    {

        private static HttpClient client = new HttpClient();

        [FunctionName("CreateProduct")]
        public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req, TraceWriter log)
        {
            //!!!For the sake of simplicity I have set the AuthorizationLevel to Anonymous.  This obviously completely insecure and should not be productionised.

            log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

            try
            {
                //Reads in the JSON.  See above format.
                dynamic body = await req.Content.ReadAsStringAsync();
                var e = JsonConvert.DeserializeObject<Product>(body as string);

                //Lets get the connection string for the storage.
                //We get the location for the secret within the keyvault using an environment variable. 
                //For App Services environment variables are set via Application settings.
                var azureServiceTokenProvider = new AzureServiceTokenProvider();
                var kvClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback), client);
                string cloudStorageString = (await kvClient.GetSecretAsync(Environment.GetEnvironmentVariable("KeyVaultLocation"), "wfecomprd2020-accesskey")).Value;

                // This sets the rowkey value
                string sRowKey = e.ProductName;

                // Create the Entity and set the PartitionKey to insuranceproduct.
                ProductEntity _product = new ProductEntity("insuranceproduct", sRowKey);

                //_product.Product_Name = e.ProductName;
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
                return req.CreateResponse(HttpStatusCode.OK, "Ok");
            }
            catch (Exception e)
            {
                log.Info(e.ToString());
                return req.CreateResponse(HttpStatusCode.InternalServerError, e.ToString());
            }

            
        }

        public class Product
        {
            public string ProductName { get; set; }
            public string ProductMarket { get; set; }
            public string Cover { get; set; }
            public double Price { get; set; }

        }



        public class ProductEntity : TableEntity
        {
            public ProductEntity(string skey, string srow)
            {
                this.PartitionKey = skey;
                this.RowKey = srow;
            }

            public ProductEntity() { }
            public string Product_Market { get; set; }
            public string Product_Cover { get; set; }
            public double Product_Price { get; set; }
        }
    }
}
```