# E-Forms Reference Guide
**Foundation 25.1**

## Table of Contents
- [Overview](#overview)
- [Installation Guide](#installation-guide)
- [Administration Guide](#administration-guide)
- [E-Forms Best Practices](#e-forms-best-practices)
- [Offline E-Form Design](#offline-e-form-design)
- [LoginFormProc](#loginformproc)
- [FormPop](#formpop)
- [Enterprise Integration Server E-Form Design](#enterprise-integration-server-e-form-design)
- [User Guide](#user-guide)

---

## Overview

### Introduction
The E-Forms module gives users the ability to complete and submit Hyper Text Markup Language (HTML)-based E-Forms into the OnBase document repository. When the form is submitted, OnBase automatically indexes the document by pulling Keyword Values from the E-Form's Keyword fields.

E-Form templates are created outside of OnBase and imported into the Client module. This HTML template is assigned to the E-Form's Document Type and displays all the individual E-Form documents.

### File Formats
Two file formats are used for E-Forms:

1. **Virtual Electronic Form** - Stores only associated Keyword Values in the database; nothing is stored in any Disk Group. All fields must be keywords; otherwise, values will be lost.

2. **Electronic Form** - Saves Keyword Values to the database AND saves a file containing all values to the Document Type's default Disk Group.

### Applications
- Requesting purchase orders
- Ordering office supplies
- Reporting software bugs and requesting software enhancements
- Submitting online questionnaires
- Creating shipping requests

### Licensing

#### Simplified Licensing
The Standard User or Premier User license is required (for new customers as of OnBase Foundation EP5 or greater).

#### Legacy Licensing
E-Forms requires the E-Forms license (for customers upgrading from versions prior to OnBase Foundation EP5).

---

## Installation Guide

### Requirements

#### General Requirements
For general requirement information, see the Installation Requirements manual sections on:
- Databases Supported
- Database/File Servers
- Supported Desktop Operating Systems
- Microsoft .NET Framework Requirements
- Web Client Browser Requirements
- Unity Client Browser Requirements
- Hardware Requirements

#### Third-Party Software Requirements
The ability to create and edit HTML documents is required. An HTML editor such as Microsoft Expression Web or a text editor can be used.

#### FormPop and PDFPop Browser Requirements
Supported browsers:
- Internet Explorer 11
- Edge (EdgeHTML 14 and higher)
- Firefox 28 and higher
- Safari 9.1 and higher for OS X and macOS
- Google Chrome 29 and higher

### Installation
There is no installation process for E-Forms.

### Backup/Recovery

#### Backup
**Configuration:**
- E-Form configuration is stored in the database
- E-Form templates are stored in the System document disk group
- E-Form data is stored in the E-Form's Document Type disk group

**External Files:**
- Backup the OnBase.ini file

#### Recovery
**Configuration:**
- Restore all disk groups and the database from backup

**External Files:**
- Restore the OnBase.ini file

---

## Administration Guide

### Configuration Overview

Configuring an E-Form is a 4-step process:
1. Create an HTML form with an external editor
2. Import the HTML Form into the SYS HTML Forms Document Type
3. Create or use a Document Type with Default File Format of Electronic Form
4. Configure the form as an E-Form for the Document Type

### Creating an HTML Form with an External Editor

E-Form templates are written in HTML. HTML code can be written directly in programs like Notepad or in HTML editors like Microsoft Expression Web.

#### Form Tags
All information on the HTML E-Form must be contained within form tags:
```html
<form method="POST">
</form>
```

**Note:** The form tag must include `method="POST"` for the E-Form to submit correctly.

### Mapping Form Fields to Keyword Types

There are two ways to map form fields to Keyword Types:

#### 1. Keyword Type Number Format
**Format:** `OBKey__KeywordNumber_#`

Example: `OBKey__110_1`

- `OBKey__` - Required prefix (two underscores)
- `KeywordNumber` - The Keyword Number assigned in OnBase
- `#` - The occurrence number on the form (1, 2, 3, etc.)

#### 2. Keyword Type Name Format
**Format:** `OBKey_Keyword_Type_Name_#`

Example: `OBKey_PO_Number_1`

- `OBKey_` - Required prefix (one underscore)
- `Keyword_Type_Name` - Keyword Type name with underscores instead of spaces
- `#` - The occurrence number on the form

**Recommendation:** Mapping to Keyword Type Number is recommended in most cases.

### Special Buttons

#### OBBtn_Yes or OBBtn_Save
Saves information to the database and creates a new document.
```html
<input type="submit" value="Submit" name="OBBtn_Yes">
```

#### OBBtn_SaveAndClose
Saves information and closes the E-Form.
```html
<input type="submit" value="Save and Close" name="OBBtn_SaveAndClose">
```

#### OBBtn_Cancel
Closes the E-Form without saving data.
```html
<input type="submit" value="Cancel" name="OBBtn_Cancel">
```

#### OBBtn_SaveNoClose
Saves information without closing the form.
```html
<input type="submit" value="Save" name="OBBtn_SaveNoClose">
```

### System Properties

#### OBDocumentDate
Used by HTML Form Custom Queries to restrict search by document date.

#### OBFromDate / OBToDate
Used to restrict date range of a search.

### Document Properties

#### OBProperty_DocumentDate
The document date assigned at import.

#### OBProperty_DateStored
The date when the document was imported.

#### OBProperty_UserName
The user who brought the document into the system.

#### OBProperty_ItemNum
The document handle (unique identifier).

### User Properties

#### OBProperty_CurrentUserName
Displays the OnBase user name of the currently logged in user.

#### OBProperty_CurrentUserEmailAddress
Displays the currently logged in user's email address.

#### OBProperty_CurrentUserID
Displays the User ID of the currently logged in user.

### Keyword Data Sets

A Keyword Data Set can automatically populate drop-down fields with Data Set values.

**Format:** `OBDataset_Keyword_Type_Name_#`

Example:
```html
<select size="1" name="OBDataset_Request_Type_1"></select>
```

### Auto-Incrementing Keywords

Auto-incrementing Keywords automatically increment upon opening a new E-Form.

**Format:** `OBKey_KeyTypeName_1` or `OBKey__KeyTypeID__1`

### Using Multi-Instance Keyword Type Groups

To use Multi-Instance Keyword Type Groups, specify the instance number for each Keyword Type Group instance.

Example:
```html
<input type="text" name="OBKey_Name_1" size="20">
<input type="text" name="OBKey_Address_1" size="20">
<input type="text" name="OBKey_Phone_Number_1" size="20">

<input type="text" name="OBKey_Name_2" size="20">
<input type="text" name="OBKey_Address_2" size="20">
<input type="text" name="OBKey_Phone_Number_2" size="20">
```

### Importing the HTML E-Form Template

To use an HTML E-Form template in OnBase:
1. Import through one of the Client modules
2. Store into the system SYS HTML Forms Document Type
3. In the Keywords section, type a description in the Description text box
4. Best practice: Include the name of the Document Type that will use the form

### Creating a Document Type

1. Create a Document Type with Default File Format of Electronic Form (or Virtual Electronic Form)
2. Associate an HTML form by clicking E-Form from the Document Type configuration dialog
3. Choose an HTML form from the drop-down menu

---

## E-Forms Best Practices

### General
- Perform data validation in E-Forms
- Keep forms as simple as possible
- Re-use code and configurations
- Add comments to E-Form code
- Document what the form should do and how it should be maintained
- Use a text editor like Notepad or HTML editor like Microsoft Expression Web
- Validate HTML using W3C Markup Validation Service
- Test E-Forms in all browsers users will use
- Limit the number of fields on an E-Form
- Use Virtual E-Forms when non-keyword data storage is not needed
- Use Document Type revisions when making form template changes

### Keywords
- Map to Keyword Type number (recommended) rather than Keyword Type name
- When using dates, use drop-down lists or date select controls
- Limit total number of data set values to 5000
- Create all hidden keywords in one location

### Scripting
- Don't script something OnBase already has built-in functionality for
- Separate HTML and JavaScript completely
- Store style sheets and scripting in separate files referenced from the E-Form

### Workflow Interaction
- Limit the number of HTML fields on the driving document
- Perform data validation on the form rather than with Workflow logic
- Always appropriately configure buttons

---

## Offline E-Form Design

### Special Considerations
- Virtual E-Forms are supported but not recommended
- Signatures using Signature Pad Interface or Digital Signatures are not supported
- Forms referencing external resources may not have access when offline
- Embed CSS and JavaScript in the E-Form for offline integrity
- Use Base64 encoding for images to make them available offline
- Auto-increment keywords are assigned when synchronized, not when created offline
- External Keyword Data Sets are not supported
- Several button types are not supported (OBBtn_AutoSave, OBBtn_CrossReference, OBBtn_xRefItemnum, OBBtn_CQ##)

---

## LoginFormProc

### Overview
LoginFormProc presents users with a custom HTML form that they can complete and submit to OnBase as an E-Form.

### Configuration
Login settings are configured in the Web Server's Web.config file within the `Hyland.Web.LoginFormProc` element.

#### Required Settings
- `username` - User name for creating forms
- `password` - Password for the user
- `datasource` - Name of the data source

### Features

#### Submitting a New Form
Required fields:
- `OBDocumentType` - Contains the Document Type number
- E-Form fields
- `OBBtn_Save` or `OBBtn_Yes` button

Action parameter example:
```html
<form method="post" action="http://[server name]/appnet/loginformproc.aspx">
```

#### Redirecting Users
Use `OBWeb_Redirect` field to specify the URL of the custom HTML form:
```html
<INPUT type="hidden" name="OBWeb_Redirect" value="http://servername/appnet/file.htm">
```

---

## FormPop

### Overview
FormPop allows users to view and edit E-Forms and Unity Forms using a simplified Web Client viewer interface.

### Usage

#### Retrieving Forms
Access forms by clicking a link from a website or email. Example URL:
```
http://WebServer/AppNet/docpop/FormPop.aspx?doctypeid=139
```

#### Editing Forms
After accessing a FormPop link, edit necessary fields and save or submit the form.

### Configuration
Configuration settings are in the Web Server's Web.config file within the `Hyland.Web.FormPop` element.

#### Key Settings
- `username` - Default login username
- `password` - Default login password
- `datasource` - Data source name (required)
- `embedded` - Set to true when embedding FormPop results in a custom Web page
- `enableDefaultLogin` - Set to true to use specified username/password
- `enableChecksum` - Set to true to add checksum value to URL query string

---

## Enterprise Integration Server E-Form Design

### Non-Keyword Fields

#### Format
`OBNONKey__REPEATED__<group name>__<field name>_<sequence #>`

Example:
```html
<input type="text" name="OBNONKey__REPEATED__Tests__TestName_1">
<input type="text" name="OBNONKey__REPEATED__Tests__TestScore_1">
```

### Keyword Fields
All data from the line-of-business application must be properly formatted to successfully create the E-Form.

### AutoFill Keyword Sets
When using `AllowAutoFillExpansion` binding property, ensure:
- All Keyword Types in the AutoFill Keyword Set are part of a single group
- AutoFill Keyword Sets don't contain overlapping values

### Unsupported Features
- AutoFill Keyword Sets with more than one primary Keyword Value
- Keyword Data Sets
- Dynamically growing tables
- Data validation of non-keyword fields

---

## User Guide

### Creating E-Forms in the Client

1. Open the New Form Document dialog:
   - Click the Create New Form Toolbar Button
   - Select File | New | Forms
   - Press Ctrl + E

2. Select the desired form and click Create

3. Fill in the field entries on the form

4. Click the save/submit button

5. Choose whether to create another form

### Creating E-Forms in the Web Client

1. Select the Main Menu button

2. Select New Form from the Document section

3. Select the E-Form to complete and submit

4. Complete the form

5. Submit the form

6. Choose whether to create another form

### Creating E-Forms in the Unity Client

1. In the Create group, click Forms

2. Enter text in the Find field to narrow forms displayed

3. Click on the E-Form you want to create

4. Enter data and submit

5. Choose whether to create another form

### Printing E-Forms

E-Forms print using either the OnBase print dialog box or the print dialog box associated with Microsoft Internet Explorer, depending on configuration.

**Print Preview:** Right-click the open E-Form and select Print Preview.

### Notes on E-Forms

**Creating Notes:**
- Select Document | Create Note
- Right-click on Notes status indicator and select Add Note

**Deleting Notes:**
- Right-click on the Note Type and select Delete Note
- Double-click on Notes status indicator, right-click on Note Type and select Delete Note

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Home, Ctrl + Home | Move to beginning of E-Form |
| End, Ctrl + End | Move to end of E-Form |
| Page Up | Scroll up to previous page |
| Page Down | Scroll down to next page |
| Ctrl + X | Cut |
| Ctrl + C | Copy |
| Ctrl + V | Paste |

---

## Documentation Notice

© 2025 Hyland Software, Inc. and its affiliates.

Information in this document is subject to change without notice. The software described in this document is furnished only under a separate license agreement and may only be used or copied according to the terms of such agreement.

Hyland, Hyland Experience, OnBase, Alfresco, Nuxeo, and product names are registered and/or unregistered trademarks of Hyland Software, Inc. and its affiliates in the United States and other countries.

For support, contact your solution provider with:
- OnBase module where issue was encountered
- OnBase version and build
- Database type and version
- Operating system and Service Pack
- Complete description of the problem
- Screenshots of error messages
