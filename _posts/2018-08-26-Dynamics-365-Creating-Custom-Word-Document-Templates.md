---
title: "Creating Custom Word Document Templates in Dynamics 365"
last_modified_at: 2018-08-26T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Word
  - Templates
header:
  teaser: /assets/images/posts/creating-word-template/0_teaser.png
classes: wide
comments: true
---
The Document Template functionality in Dynamics 365 is useful as it allows you to create business branded documents using information from the Dynamics 365 system. It does have some limitations which I feel let it down and prevent it from being able to do more complex documents, such as limiting the number of entities you can drill through, however, it can still be useful in some scenarios.

To create a new Document Template, go to Settings -> Templates -> Document Templates. In this post I will be making a new Word document template for the Quote entity.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/1_NewWordTemplate.png"
  alt="Creating a new Quote Word Template"
  caption="Creating a new Quote Word Template"
%}

Once you have selected the entity to work from, you will be prompted to choose the relationship details to include, for this example I have included the Quote Line (quote_details) 1:N relationship and the User (system_user_quotes) relationship.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/2_Relationships.png"
  alt="Selecting the relationship details to include in the document"
  caption="Selecting the relationship details to include in the document"
%}

After selecting the relationships that should be included in the document, click on the ‘Download Template’ button and a new Word document will be downloaded.

Whilst within Word the XML Mapping Pane is used to insert the fields into the document. This option is available under the Developer tab, which may need to be enabled if you have not used it before.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/3_XMLMappingPane.png"
  alt="Displaying the XML Mapping Pane"
  caption="Displaying the XML Mapping Pane"
%}

The XML Mapping Pane needs to be changed to show the fields from Dynamics. To do this, click on the drop down box for the Custom XML Part and change it to the CRM record.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/4_ChangingXMLMappingPane.png"
  alt="Changing the Custom XML Part"
  caption="Changing the Custom XML Part"
%}

Once the XML Mapping Pane is using the correct Custom XML Part you will be able to see the fields from the Quote record. To insert these fields into the Word document, click where you would like the field to be entered and then right click on the field in the XML Mapping Pane, select Insert Content Control and then select the field type (I usually use Plain Text as I have found that Rich Text can cause Word to crash).

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/5_InsertField.png"
  alt="Adding field to the Word Document"
  caption="Adding field to the Word Document"
%}

This step can then be repeated as many times as needed to include all the information you want in the Word document.

As I am working with a Quote in this example, I then wanted to add the line items to the document. This is in done by first creating a normal table in Word.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/6_InsertTable.png"
  alt="Adding fields & a normal table"
  caption="Adding fields & a normal table"
%}

After the table is inserted, select the row which should repeat and then go to the XML Mapping Pane and find the repeating section, right click on it and then select Insert Content Control and Repeating. This will then make that row a repeating section, you can tell it has worked correctly because the row changes slightly to include an outer box.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/7_InsertRepeatingTable.png"
  alt="Setting the row to repeat"
  caption="Setting the row to repeat"
%}

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/8_RepeatingTableInserted.png"
  alt="Row display after being made a repeating row"
  caption="Row display after being made a repeating row"
%}

You can then add the fields to the individual cells in the same way that you would add the field to the document. In this example I produced the table below.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/9_RepeatingTableWithFields.png"
  alt="Repeating row with fields from Dynamics"
  caption="Repeating row with fields from Dynamics"
%}

Once the template has been finished, it can be uploaded to the system and made available to the users by going to Settings -> Templates -> Document Templates and then selecting Upload. Once the document template has been uploaded the system will take you to the details page.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/10_UploadingTemplate.png"
  alt="Details for the Document Template"
  caption="Details for the Document Template"
%}

To then run the document template, go to a quote and select the 3 dots in the menu bar, then go to Word Templates and select the appropriate template. The Word document will then be downloaded with the information from the record.

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/11_UsingTemplate.png"
  alt="Using the Document Template"
  caption="Using the Document Template"
%}

### Finished Word Document

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/12_FinalDocument.png"
  alt="Generated Document"
  caption="Generated Document"
%}

### Original Quote & Quote Details

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/13_OriginalQuote.png"
  alt="Original Quote that has been used"
  caption="Original Quote that has been used"
%}

{%
  include figure
  image_path="/assets/images/posts/creating-word-template/14_originalQuoteDetails.png"
  alt="Original Quote Line items that have been used"
  caption="Original Quote Line items that have been used"
%}

## My frustrations with Document Templates

Whilst the custom document templates are useful there are some frustrations I find when using them.

### Updates to template

After the Document Template has been created, it seems to be difficult to update the relationship with Dynamics. For example, if a new field is added to Dynamics after the template has been generated, it appears that the only way to update the field list is to go into the XML of the document and manually add in the required fields. This could be daunting if the person updating the document does not understand XML.

Alternatively, a new document would need to be generated and the content of the template copied over manually, this would be prone to errors and cost more time each time an update is needed.

### Direct Relationships Only

There seems to be a limit to the depth of relationship that can be reached, it looks like you can only reach the direct relationships that are presented when generating a new document. I have tried customizing the XML to bring through extra relationships but the updates are ignored. In most cases this may not be an issue, but it can be frustrating when you need to. To get around this, it is possible to create a custom field on the entity you can reach and use JavaScript to pull through information from the entities you cannot reach when the user is modifying the records.
