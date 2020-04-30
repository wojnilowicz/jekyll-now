---
layout: post
title: Qt Quick and Qt Widgets interchangeably
---

The need for new application frontend comes from the
goal to get the application working on Android.
Four observations have been made:

1. Qt's [user interface guide](https://doc.qt.io/qt-5/topics-ui.html#comparison), points at [Qt Quick](https://doc.qt.io/qt-5/qtquick-index.html) as the right choice for developing [frontend](https://en.wikipedia.org/wiki/Front_end_and_back_end) for touch devices, like e.g. [Android](https://www.android.com/);

2. The frontend of KMyMoneyNEXT is based on the [Qt Widgets](https://doc.qt.io/qt-5/qtwidgets-index.html), and the switch to the Qt Quick is not a [drop-in replacement](https://en.wikipedia.org/wiki/Drop-in_replacement);

3. [KTuberling](https://kde.org/applications/games/org.kde.ktuberling) uses Qt Widgets only, for running on Android.
Although its frontend is [usable](https://en.wikipedia.org/wiki/Usability), it may not be the same case for a complex KMyMoneyNEXT frontend due to insufficient [scalability](https://doc.qt.io/qt-5/scalability.html);

4. [KStars](https://edu.kde.org/kstars/) devised a separate part called [KStarsLite](https://youtu.be/l9ZmGmCP-8c), which purpose seems to be implementing desktop dialogs functionality in Qt Quick. Analysing two related dialogs: one [Widget](https://cgit.kde.org/kstars.git/tree/kstars/dialogs/locationdialog.ui) and one [Quick](https://cgit.kde.org/kstars.git/tree/kstars/kstarslite/qml/dialogs/LocationDialog.qml) version it can be seen, that they don't match visually, but both maintain [look and feel](https://en.wikipedia.org/wiki/Look_and_feel) of their [platform](https://en.wikipedia.org/wiki/Computing_platform).

----

It has been decided to develop a storage manager in Qt Quick and Widgets, with the ability to use them interchangeably. A system of [class]((https://en.wikipedia.org/wiki/C%2B%2B_classes)) [inheritance](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) has been designed to reduce a [maintenance work](https://en.wikipedia.org/wiki/Software_maintenance) of the two frontends. An example [class diagram](https://en.wikipedia.org/wiki/Class_diagram) is shown below for an explanation.

![](/images/2020-04-29/frontend design.png){: width="50%"}

A class **Open - Qt Widget** and **Open - Qt Quick** provide frontends for opening certain type of storage. They share some code, so it has been put in the [abstract](https://en.wikipedia.org/wiki/Abstract_type) **Open** class. As a result, both frontend classes contain the code specific to them only. That makes it feasible to extend the support by e.g. [DOS](https://en.wikipedia.org/wiki/DOS) frontend.

The frontend provided by **Open - Qt Widget** is:
1. partial in a sense, that it provides e.g. literally single [text field](https://material-ui.com/components/text-fields/);
2. specific to a given plugin;
3. specific to a given storage type;
4. specific to a given frontend type.

The manager manages these frontends, and present them to the user in a [consistent](https://blog.prototypr.io/consistency-a-key-design-principle-5d125469da8e) way. For that, it uses [interface](https://en.wikipedia.org/wiki/Application_programming_interface) **Dialog part**, which is a way of [standardizing](https://en.wikipedia.org/wiki/Software_standard) its partial frontends.

[Working principle](https://www.thefreedictionary.com/working+principle) for the rest part of manager's frontend is analogical, and thus not shown here.

----

The result of developing the storage manager with Qt Widgets and Qt Quick as frontend is:
1. 28 dialog combinations for opening and saving storages, from which 6 are unique;
2. 9 dialog combinations for entering credentials, from which 5 are unique;
3. 12 message box combinations for the issues processor, featured [here]({% post_url kmymoneynext/2020-4-28-Introducing storage manager %}).

The user can choose between used frontend through plugin settings, as shown below. The changes are applied immediately, and without an application restart.
![](/images/2020-04-29/storage-manager-options.png)

Two example dialogs in Qt Widgets (one the left), and Qt Quick (on the right) are shown below for comparison.

<div id="imagesInRow" markdown="1">
![](/images/2020-04-29/save-as-encrypted-widget.png)
![](/images/2020-04-29/save-as-encrypted-quick.png)
</div>

The same is done for message boxes.

<div id="imagesInRow" markdown="1">
![](/images/2020-04-29/messagebox-widget.png)
![](/images/2020-04-29/messagebox-quick.png)
</div>

It can be observed, that Qt Quick versions match the Qt Widgets versions visually, which may not be suitable for Android. They should be tested in that environment, and appropriate adjustments have to be made to the frontend, if needed.

