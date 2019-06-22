---
layout: post
title: Framework for testing storage version changes
---
The KMyMoneyNEXT's storage layout can change over time due to new features or design changes. As a result, none of previously created storages should become corrupted or incompatible.

A testing framework has been created to ensure that. It's purpose is to check whether storages created with different KMyMoneyNEXT version get loaded as expected.

Previously it has been done through snippets of data from that storage, like the one shown below
```
  QString ref_ok = QString(
                     "<!DOCTYPE TEST>\n"
                     "<TRANSACTION-CONTAINER>\n"
                     " <TRANSACTION postdate=\"2001-12-28\" memo=\"Wohnung:Miete\" id=\"T000000000000000001\" commodity=\"EUR\" entrydate=\"2003-09-29\" >\n"
                     "  <SPLITS>\n"
                     "   <SPLIT payee=\"P000001\" reconciledate=\"\" shares=\"96379/100\" action=\"Withdrawal\" bankid=\"SPID\" number=\"\" reconcileflag=\"2\" memo=\"\" value=\"96379/100\" account=\"A000076\" />\n"
                     "   <TAG id=\"G000001\"/>\n"
                     "  </SPLITS>\n"
                     "  <KEYVALUEPAIRS>\n"
                     "   <PAIR key=\"key\" value=\"value\" />\n"
                     "  </KEYVALUEPAIRS>\n"
                     " </TRANSACTION>\n"
                     "</TRANSACTION-CONTAINER>\n"
                   );
```
Advantages of the new approach are:

###### 1) no manually/semi-automatically written snippets of storage

There is no need to do time intensive but not value adding tasks like:
* escaping characters,
* indenting rows,
* adding new line characters.

###### 2) tests payload is external

It's easily extensible and manageable that way. Test payload is divided in many separate parts. If a user gets loaded something else than expected, then his storage can be put directly into this testing framework. Code only loads this file and checks whether it's loaded as expected.

###### 3) tests can be complex

Storages to test are created in the application through GUI. One can create, edit and modify complex scenarios without too much hassle.

<br>
The work can be observed in [this](https://github.com/wojnilowicz/kmymoneynext/commit/0e9a9d494ec1fbadd554f1b0a6ab281b266fa1da) patch.