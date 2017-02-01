---
# required metadata

title: Retail online store publishing architecture | Microsoft Docs
description: This topic contains conceptual information to help developers and system administrators understand how channels and catalogs are published from the retail module to an online store in Microsoft SharePoint 2013 Products. Understanding the publishing process can help you develop, manage, and troubleshoot your Retail online store.
author: kfend
manager: AnnBe
ms.date: 2016-03-29 18:43:48
ms.topic: article
ms.prod: 
ms.service: Dynamics365Operations
ms.technology: 

# optional metadata

# keywords: 
# ROBOTS: 
audience: Developer
# ms.devlang: 
ms.reviewer: 61
ms.suite: Released- Dynamics AX 7.0.0
# ms.tgt_pltfrm: 
ms.custom: 72124
ms.assetid: becdaaec-5206-4a85-9ff6-23650069fd11
ms.region: Global
# ms.industry: 
ms.author: meeram

---

# Retail online store publishing architecture

This topic contains conceptual information to help developers and system administrators understand how channels and catalogs are published from the retail module to an online store in Microsoft SharePoint 2013 Products. Understanding the publishing process can help you develop, manage, and troubleshoot your Retail online store.

Publish a Retail online store channel
-------------------------------------

When you publish a Retail online store channel, you replicate the basic structure of your online store between Microsoft Dynamics 365 for Operations and Microsoft SharePoint. You create the basic structure of your online store channel in the **Retail** module in Dynamics 365 for Operations. Before you can publish an online store channel, you must complete the following setup tasks:

1.  Add the online store to the organization hierarchy.
2.  Create the online store and configure properties in Dynamics 365 for Operations.
3.  Configure the category hierarchy of your site.

After you've completed these steps, you're ready to publish the product schema to the online store.

1.  You create the online store in Dynamics 365 for Operationsand publish it from the **Online stores** page. The status is changed from **Draft** to **In progress**.
2.  Dynamics 365 for Operations takes a snapshot of the category hierarchies (the Retail hierarchy) and properties.
3.  Commerce Data Exchange: Async Server reads information about the online store, hierarchies, and properties in the Retail store database, and sends that information to the commerce runtime (CRT).
4.  Async Server synchronizes the tables in the channel database.
5.  The Retail publishing job runs from the CRT application programming interface (API) and creates hierarchies for the site that you created in the online store.
6.  Commerce Data Exchange: Real-time Service receives the status of the Retail publishing job actions from the CRT API and publishes that status in Dynamics 365 for Operations. The status is either **Published** or **Error**.

For the specific procedures to publish a channel, see [Set up an online store](https://msdn.microsoft.com/en-us/jj682095). After you've published the channel, you can publish a catalog.

## Publish a Retail online store catalog
A retail product catalog lets you identify the products that you want to offer in your online stores. When you create a catalog, you identify the online stores where the products will be offered, add products, and enhance the product offerings by adding merchandising details. After the catalog is approved, you publish it to make products available in the online store. Before you can publish a retail product catalog, you must complete the following setup tasks:

1.  Set up retail products, and configure hierarchies, assortments, and variants.
2.  Set up retail product catalogs, and configure attribute groups and workflow.

After you've completed these steps, you're ready to publish the Retail online store catalog.

1.  Dynamics 365 for Operationsreads the product tables in the Retail database.
2.  Async Server synchronizes all products in the channel database.
3.  The CRT/Publishing Connector creates a *listing*. A listing is an instance of a product for a channel at a given point in time. For example, you have a product that is named “jeans”, and this product has a variant that is named “red”. In this case, the system creates a listing for “red jeans”.
4.  The system determines whether any new attributes were added for the listing. If new attribute were added (for example, if the “red jeans” listing includes a new attribute that is named **texture**, and this attribute is marked as **Included** at the channel level), the system creates a custom site column for that attribute. The system also creates a new rule for the list item and completes the process in SharePoint by creating a new row for the “red jeans” listing.
5.  The CRT records the publishing status for the listing.
6.  Async Server synchronizes the publishing status of the listing with all other publishing statuses. The status is either **Published** or **Error**.

