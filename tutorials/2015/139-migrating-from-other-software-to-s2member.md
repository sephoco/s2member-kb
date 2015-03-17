---
title: Migrating from Other Software to s2Member
categories: tutorials
tags: import-export-tools, member-migration
author: jaswsinc
github-issue: https://github.com/websharks/s2member-kb/issues/139
---

This article will explain how to migrate to s2Member Pro from another software application, while retaining existing users and their payment information.

_The s2Member plugin contains additional inline documentation that should be reviewed carefully before following the steps in this article. See: **Dashboard → s2Member® → Import/Export**. Import/Export features are only available as part of the s2Member Pro add-on. If you’re using the free s2Member Framework, you’ll need to [upgrade to s2Member Pro](http://www.s2member.com/pro/) to follow along._

---

## Step 1: Collecting existing membership data {#step1}

The first step is to collect the membership data from your existing software. If your existing software supports exporting the data to CSV (Comma Separated Values), you can simply export the data, open the file in a spreadsheet program such as Excel, and then reformat the data for s2Member's Import Tool (more on formatting in [Step 2](#step2)).

Or, if the other membership software does _not_ export data to CSV, but uses a different export format, you’ll need to convert the existing data to s2Member’s CSV format using software such as Excel. As a last resort, you can also build the import file manually by collecting the necessary data and creating a CSV file. This file can be created as a simple text file and saved with the `.csv` extension, or you can use a spreadsheet program such as Excel.

### Minimum Required Data

The minimum required data for s2Member importation is as follows:

-   Username
-   Email Address

However, you’ll want to collect at least three more pieces of information to retain subscription information and define the users Membership Level within s2Member Pro. The additional fields are as follows:

-   Paid Subscr. ID (this is the subscription ID, e.g., `I-EUAVMMPD04HF`)
-   Paid Subscr. Gateway (the payment gateway associated with this subscription ID, e.g., `paypal`)
-   s2Member Level (e.g., `2` for s2Member Level 2, or `0` for Free Subscriber)

_The format of this data is important and you must format the data using the s2Member import format (see [Step 2](#step2)). If you’re building this file manually and you’re only inserting some of the information, be sure to include the blank fields in your CSV file._

---

## Step 2: Formatting Existing Data for Importation {#step2}

Here’s a basic example of the s2Member import format:

```text
"ID","Username","Password","First Name","Last Name","Display Name","Email"
```

If you fill the ID field, the Import routine will update an account matching the ID you specify (so long as the account ID does _not_ belong to an Administrator, this is for security). When importing new Users/Members, you can leave the ID field empty; don’t remove it, just leave it empty (e.g. `""`).

Example: `"","Username","First Name","Last Name","Display Name","Email"`

If you fill the Password field, the Password that you specify (in plain text) will be used. Otherwise, a Password will be auto-generated by s2Member. The Password field can also be used with an ID update (to update/change the current Password)—so long as the ID field is also filled in, and it matches an account already in the system.

If you fill the ID field to update an existing account and the leave the Password field empty; the existing Password (for the account matching the ID you specify) will remain unchanged.

Here is an extended example that includes all possible fields for an s2Member import file:

```text
"ID","Username","Password","First Name","Last Name","Display Name","Email","Website","Level[0-9]+ or Role ID","Custom Capabilities","Registration Date ( mm/dd/yyyy )","First Payment Date ( mm/dd/yyyy )","Last Payment Date ( mm/dd/yyyy )","Auto-EOT Date ( mm/dd/yyyy )","Custom Value ( starts w/domain )","Paid Subscr. ID","Paid Subscr. Gateway","Custom Field ID #1","Custom Field Value #1","Custom Field ID #2","Custom Field Value #2", ...
```

When assembling the data it’s important to leave the empty fields present. For example, using the minimum required data explained in [Step 1](#step1), and including the First Name, Last Name, and Full Name fields; a CSV file that contains three users might look like this:

![](http://cdn.websharks-inc.com/s2member/uploads/spreadsheet-screenshot.png)

```text
"", "johnsmith22", "", "John", "Smith", "John Smith", "john.smith@example.com", "", "2", "", "", "", "", "", "", "I-2342934SSER243", "paypal", "", "", "", ""
"", "janedoe11", "", "Jane", "Doe", "Jane Doe", "jane.doe@example.com", "", "3", "", "", "", "", "", "", "I-2426974EEFR216", "paypal", "", "", "", ""
"", "tombradley84", "", "Tom", "Bradley", "Tom Bradley", "tom.bradley@example.com", "", "0", "", "", "", "", "", "", "", "", "", "", "", ""
```

In this example, we have three users along with their subscription information and their s2Member Levels defined. The third user, Tom Bradley, has an s2Member Level of 0 defined, which means that he will be imported as a Free Subscriber. You’ll also notice that Tom doesn’t have any subscription data associated with his account.

Since the Password fields in the above example were left blank, s2Member will automatically generate a password for each of the imported users. However, no emails will be sent during the importation process. You may want to send an email to your members informing them that they must reset their password. You can then point them to `/wp-login.php?action=lostpassword` where they can enter their username or email address and reset their password.

If you’re uncomfortable formatting the import data manually, you can create a sample import file by exporting your existing s2Member information (see: **Dashboard → s2Member® → Import/Export → User/Member Exportation**). You can download the resulting CSV file and then open it with a spreadsheet program such as Excel. From there, you can insert new rows for each of your existing members and follow the format of the existing data.

---

## Step 3: Importing Membership Data {#step3}

Once your existing membership data has been formatted in the s2Member import format, navigate to: **Dashboard → s2Member® → Import/Export → User/Member Importation**. From here, you can either copy and paste the formatted data into the import text field or click the "Choose File" button to select the CSV file for upload. When you’re ready, click "Import Now".

![](https://www.filepicker.io/api/file/0vO3e2yYTBGmgS8hLX74#.png)

_**1000 Each Time (Max):** To ensure the server does not hang when importing new users, you are limited to importing 1000 users at a time. If you have more than 1000 lines to import, please split the file into groups of 1000 prior to importing._

_**No Email Notification:** This import routine works silently. Users/Members will NOT be contacted by s2Member; that is, unless you have another plugin installed that conflicts with s2Member’s ability to perform the Import properly. You should always test one or two accounts before importing a large number of Users all at once. If you want Users/Members to be contacted, you can add them manually, by going to **WordPress® → Users → Add New** and selecting one of the s2Member Roles from the drop-down menu._

---

## Common Member Migration Questions {#common-questions}

---

#### My existing members are charged on a recurring basis. Will they continue to be billed?

This depends. If your previous membership platform is like s2Member, and it creates Recurring Billing Profiles that live on the payment gateway side for best security (and portability), then all of your existing members should continue to be billed without interruption. Moving from one membership platform to another is very easy when this is the case, because the information related to billing cycles and the relevant payment details are actually stored in a remote location; i.e., within your Stripe, PayPal, or Authorize.Net account.

However, if your previous membership platform handled this all by itself; e.g., if it stored credit card details in your own database, and it ran its own scheduling system that determined when/if members should continue to be billed, then no. Moving away from a platform like this is very difficult to do, because recurring billing is actually dependent upon the platform itself.

#### Does s2Member bill my existing customers?

No. s2Member does not function in this way. s2Member creates Recurring Billing Profiles that live on the payment gateway side for best security (and portability), so that all of your existing members can continue to be billed without interruption—even if you change membership platforms.

Understanding this, we can see that s2Member only bills customers when they first complete checkout (i.e., if there is an initial charge) and then s2Member creates a remote Recurring Billing Profile that takes care of any future billing that needs to occur. This remote Recurring Billing Profile will live at Stripe, PayPal, or Authorize.Net. In this way, you can move the content/services that you provide to your members (e.g., you could change publishing platforms or membership management systems) without issue; i.e., without changing the way you collect payments from your customers.

Thus, when you bring existing members from an old membership platform into an s2Member-powered site, you can grant them membership access, set up an EOT (End of Term), control what data is associated with the membership, and much much more. However, if the customer is going to be billing on a recurring basis, this needs to have already been set up ahead of time. Otherwise, you will need to ask each customer to re-register and complete checkout again at your new site; i.e., to create a remote Recurring Billing Profile that is compatible with s2Member Pro.

#### Will s2Member know when to cancel access to my existing members?

Yes, but _only_ if you tell it to. Whenever you import your existing members, please set an EOT (End of Term) Time so s2Member will know when (or if) it should terminate access. More on this below; see: [Understanding Automatic EOT Times](#eot-times)

_**Note:** s2Member is not capable of determining this on its own, and it does not respond to IPN data received for members that you imported from another membership platform. If you want to terminate access automatically on a given date in the future, please set an EOT Time for each of your existing customers._

_Moving forward with s2Member Pro, an EOT Time is determined automatically for customers you acquire with the s2Member software itself. In other words, this limitation applies only to members imported from other membership platforms. Not to those you acquire with the s2Member software._

## Understanding Automatic EOT Times {#eot-times}

An EOT (End of Term) Time defines when (or if) a particular customer should lose access at some point in the future. For instance, if you import a member that originally purchased a one year membership, and they have been paying you for 6 months; you should set their EOT Time to 6 months from today. This way s2Member will terminate their access at the correct point in time automatically.

How s2Member handles EOT Times is determined by the settings you configure in the Automatic EOT Behavior settings for your payment gateway. See: **Dashboard → s2Member → [Payment Gateway] Options → Automatic EOT Behavior**

Configuring an EOT Time is an easy way to deal with member migration. It allows you to setup an EOT Time with s2Member Pro (during importation or later—manually) so that no matter how billing was handled by your previous membership platform, you can always tell s2Member when access should be terminated. It this ideal in every case? No, but it provides many site owners with an easy way to deal with limitations in other/existing membership platforms. EOT Times are your friend when importing existing customers into s2Member Pro :-)

### Importing a Customer w/ a Custom EOT Time

```text
"ID","Username","Password","First Name","Last Name","Display Name","Email","Website","Level[0-9]+ or Role ID","Custom Capabilities","Registration Date ( mm/dd/yyyy )","First Payment Date ( mm/dd/yyyy )","Last Payment Date ( mm/dd/yyyy )","Auto-EOT Date ( mm/dd/yyyy )"
"","johndoe","9934jxxeddf!","John","Doe","John Doe","johndoe@example.com","","s2member_level1","music,videos","","","","12/31/2030"
```

_See end of the line ↑ In this example, user `johndoe` will automatically lose access on Dec 31st, 2030._