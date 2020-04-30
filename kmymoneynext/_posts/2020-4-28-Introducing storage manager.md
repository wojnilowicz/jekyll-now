---
layout: post
title: Introducing storage manager
---

Currently in KMyMoneyNEXT, **File->Open...**, and **File->Save As...** has to be clicked in order to open, or save, file based storages. Subsequently a [file dialog](https://en.wikipedia.org/wiki/File_dialog) prompts for a location.
![](/images/2020-04-28/file dialog.png)

Analogically, **File->Open database...**, and **File->Save As...** has to be clicked in order to open, or save, database based storages. Subsequently a dialog prompts for: host name, user name, database name, and password.

![](/images/2020-04-28/opendatabase.png)

As a side note: **File->Save As...** firstly invokes an intermediate dialog to choose XML or SQL, which has been omitted from this explanation.

From the above, it can be observed, that menu entries and dialogs are not visually [consistent](https://blog.prototypr.io/consistency-a-key-design-principle-5d125469da8e). Besides:

1. In order to save a file based storage as encrypted, a change of the default behaviour somewhere in the application's settings is needed;
2. In order to save a file based storage as anonymized, a special extension has to be entered manually;
3. If SQLCipher is available, but SQLite is desired, then SQLCipher has to be chosen, but a password must be not provided.

The conclusion is that there are some ergonomic issues. A storage manager has been introduced to improve it. It provides following features.

----

###### 1) Storage type selection
Having one way to handle different storage types is less ways for the user to remember.

The manager allows to explicitly choose the storage type for opening or saving under. Through that, it gives an overview of what types the manager has to offer. The manager with two different types set for opening is shown below.

<div id="imagesInRow" markdown="1">
![](/images/2020-04-28/file-open-gzip.png)
![](/images/2020-04-28/file-open-mysql.png)
</div>
In the image above, it can be observed, that vital inputs for given type changed, yet the rest of the dialog remains consistent.

At the present time, there are 14 types available. Such a long list can be unergonomic to choose from, so it's possible to shorten it in the plugin settings. In the figure below, only three most used ones have been chosen to be available.

![](/images/2020-04-28/storage-manager-options.png)

----

###### 2) Issues processor
The more feedback the user gets from the application, the higher chance he can understand and solve an issue he faces.

If during storage handling an issue occurs, or a user input is required, then an appropriate message box will appear. There are ca. 34 such occurrences handled, and it can be easily extended, when needed. An example occurrence is: a file is not readable or writeable, a server is not running. 

----

###### 3) Dialog for saving with GPG keys
Requiring all inputs from one place, in order to do an encrypted save, increases its chance of success for first time users. The dialog implementing it has been shown below.

![](/images/2020-04-28/file-save-gpg.png)

This approach also gives flexibility, because:
* The user still can save some files unencrypted, without changing application settings for that;
* The user can use different set of keys per file, instead of predefined set of keys for all files.

----

###### 4) Recovery key downloader
The less user has to do, to use a feature, the higher chance he will be using it.

The manager at the right moment detects that there is no [recovery key](https://kmymoney.org/recovery.html) present in user's keyring. Through the message box shown below, it offers to download, and make it seamlessly available for encryption.
![](/images/2020-04-28/missing-recovery-key.png)

If the fingerprint of the downloaded key doesn't match the expected one, then the user is informed, and asked for a permission to remove it for safety reasons.
The user has to explicitly enable encryption with recovery key in manager settings, for this feature to be available.

----

###### 5) Credentials dialog
If the application won't request an information, it doesn't need from the user, then the user won't waste his time.

The manager prompts the user for credentials, only when it cannot get access to a storage, instead of doing it preemptively. It's useful in cases of local MySQL or PostgreSQL storages, where the database allows logging in without a passphrase.

The manager will detect, that a bad passphrase has been given, and will rephrase the prompt accordingly, as shown below.

<div id="imagesInRow" markdown="1">
![](/images/2020-04-28/credentials.png)
![](/images/2020-04-28/bad-credentials.png)
</div>

----

###### 6) KWallet support
The manager presets credentials, if it'll find them in the system's wallet for all storage types that require it. If the credentials are wrong, then the user seamlessly gets asked for the right ones. If they are right, then they'll will be stored in the system's wallet for later use. The user has to explicitly enable this feature.

----

###### 7) Storage upgrader
If the user won't be forewarned about possible dangerous or limiting actions, he may lose his data not being aware of it.

This work extends the work on [xmlstorageupdater]({% post_url kmymoneynext/2019-3-11-Match transactions differently %}) in such a way, that it adds the support for database based storages.

![](/images/2020-04-28/kmymoney-upgrade-possible.png)

----

###### 8) Database creation
A database had to be created outside of the application in order to use it. The manager streamlines the workflow by offering to create it for the user, if it doesn't exist. In the case of failure, a message box similar to the one below is shown.

![](/images/2020-04-28/database-creation-error.png)

----

###### 9) Numbered backups for SQLite
File based storages of XML type already had this feature and now SQLite and SQLCipher types have it. It can be disabled independently from XML based types.

----

###### 10) Conversion to KMyMoney files
KMyMoneyNEXT can save files back to the KMyMoney format, and make them available to open from the later one.

----

Packages for Linux, macOS, and Windows are available in the [releases section](https://github.com/wojnilowicz/kmymoneynext/releases).<br>
Disclaimer:<br>
Availability of features vary across those packages due to technical issues. In order to get the best experience, it's recommended to [build from source](https://github.com/wojnilowicz/kmymoneynext/blob/master/README.cmake) on Linux.

