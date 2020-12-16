---
title: "Dynamics 365 – Enabling SharePoint Document Management"
last_modified_at: 2019-02-14T18:00:00-00:00
categories:
  - Dynamics
tags:
  - SharePoint
header:
  teaser: /assets/images/posts/enable-sharepoint-document-management/6_DocumentsShown.png
classes: wide
comments: true
---
Dynamics 365 is able to store attachments on records. This is obviously already useful but was made even more useful when Microsoft added the ability to search within documents ([more info](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/basics/relevance-search-results){:target="_blank"}).

Unfortunately, the amount of free storage you get within Dynamics is quite low, and the cost per GB is expensive. This means that storing documents such as quotes & invoices quickly becomes expensive.

One of the methods that can be used to reduce the cost is to store the documents in Microsoft SharePoint. SharePoint is a collaborative platform and has a vastly reduced cost per GB than Dynamics.

A downside is that the search functionality for Dynamics cannot check within documents stored on SharePoint. Due to this, it may be worth having some documents stored in SharePoint & some in Dynamics.

## Security

An important thing to keep in mind is that security roles do not get copied automatically. There are some 3rd party tools that can do it, and it may be possible to create your own solution, but out of the box, you will need to manage this manually.

Due to the structure of the SharePoint folders, it will be easier for people to navigate around the folders. This means that someone may be able to easily see documents for an opportunity that they do not have permission to see in Dynamics.

This means that you will want to take care when enabling the integration and make sure that you’re happy with the permissions. I would recommend testing the permissions from a range of user permissions.

## Setup

The environment during this demo is a Dynamics 365 Free 30 day trial. The trial also comes with a trial version of SharePoint.

The first step to enable Dynamics 365 SharePoint Integration is to go to **Settings -> Document Management** and from within there select **Document Management Settings**.

![Document Management Settings](/assets/images/posts/enable-sharepoint-document-management/1_DocumentManagementSettings.png)

A new window will open. In this window, you will be able to select the entities to enable Document Management on. You will also be able to enter the SharePoint URL (in my example I have made a site specifically for CRM documents).

![First Window showing the entities that can be enabled & the site url](/assets/images/posts/enable-sharepoint-document-management/2_chooseentities.png)

The second page will then allow you to confirm the URL entered & select the folder structure. In my case I have selected to base it on the account entity. This means all the files will be stored under the account folder – each entity will get its own subfolder.

![Selecting the folder structure](/assets/images/posts/enable-sharepoint-document-management/3_FolderStructure.png)

Selecting next will allow the system to create the top-level folders in the SharePoint site.

Interestingly, even though I have selected to have a folder structure based on an entity, each entity will still get an individual top-level folder. The folder structure will still be honored though, so any opportunity documents are added under */account/AccountName/opportunity/*

![Folders are created on SharePoint site](/assets/images/posts/enable-sharepoint-document-management/4_CreateFolders.png)

## Usage

The documents are then accessible from within a record using the navigation menu at the top for related entities using the Documents link.

![Navigation Menu shows Documents under Related Items](/assets/images/posts/enable-sharepoint-document-management/5_NavigateToDocuments.png)

It is not possible to add a sub-grid for SharePoint Documents to the page out of the box. I have created another [post](/dynamics/dynamics-365-show-sharepoint-documents-on-form) which shows how you can use an IFrame to achieve this.

![The view from within Dynamics](/assets/images/posts/enable-sharepoint-document-management/6_DocumentsShown.png)

![The view from within SharePoint](/assets/images/posts/enable-sharepoint-document-management/7_DocumentShownInSharePoint.png)

## Summary

The Dynamics 365 SharePoint Integration is easy to setup & can help reduce costs. You will need to consider how you implement the security, but this should be manageable.

You will also receive the added benefit of version control & SharePoint workflows, which can help with document management.

The ability to search within documents in Dynamics will be lost, but it is possible to search with SharePoint. This is something that you will have to decide if it is worth it or not. It is possible to have some documents in Dynamics & some in SharePoint.
