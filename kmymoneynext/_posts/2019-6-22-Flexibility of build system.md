---
layout: post
title: Flexibility of build system
---

KDE runs build system called [Craft](https://community.kde.org/Craft). There you have dependencies already built according to [one-size-fits-all](https://en.wikipedia.org/wiki/One_size_fits_all) rule because they have to fulfil needs of all KDE software. In own build system one has to build dependencies by oneself but that allows flexibility. Areas where the flexibility gave the most spectacular results have been presented below.

----

###### 1) Icons

Lean [BreezeIcons](https://api.kde.org/frameworks/breeze-icons/html/index.html) is packaged which weights way less than the stock one.

<table width="40%">
  <thead>
    <tr class="header">
      <th>—</th>
      <th>BreezeIcons (stock)</th>
      <th>BreezeIcons (lean)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>size</td>
      <td>19,3 MB</td>
      <td>340 KB</td>
    </tr>
  </tbody>
</table>

Stuff that is left out here is e.g. an Adobe icon in three different sizes.

----

###### 2) HTML rendering engine

Old [KHtml](https://api.kde.org/frameworks/khtml/html/index.html) is packaged instead of modern [QtWebEngine](https://doc.qt.io/qt-5/qtwebengine-index.html) or slightly outdated [KDEWebKit](https://api.kde.org/frameworks/kdewebkit/html/index.html). In the table below KHtml is compared with unoptimized libraries but that's enough to get a general feeling about the size differences.

<table width="40%">
  <thead>
    <tr class="header">
      <th>—</th>
      <th>QtWebEngine</th>
      <th>KDEWebKit</th>
      <th>KHtml</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>size</td>
      <td>108 MB</td>
      <td>44 MB</td>
      <td>8 MB</td>
    </tr>
  </tbody>
</table>

Looking from the security perspective, KMyMoneyNEXT renders its own HTML content and not potentially unsafe content from the web, so it should pose no threat.

----

###### 3) Internationalization

[ICU](http://site.icu-project.org/home) (and more precisely [ICU data](https://github.com/unicode-org/icu/blob/master/docs/userguide/icu_data/buildtool.md)) is a third library where much has been saved.

<table width="40%">
  <thead>
    <tr class="header">
      <th>—</th>
      <th>ICU data (stock)</th>
      <th>ICU data (lean)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>size</td>
      <td>25,9 MB</td>
      <td>5,6 MB</td>
    </tr>
  </tbody>
</table>

Now it consists mostly from character conversion tables. It may grow if there would be support for more than English only.

----

###### 4) Build flags

Compiler for KMyMoneyNEXT and its dependencies is instructed to take size as its priority - at a cost of non-optimal performance and debugging capabilities. Goal here is to showcase how far one can reduce package sizes, so it may change in the future. Here the saving is ca. 15 % on size of the package for Windows.

----

###### 5) Packages compression

For distributing software, exe installer is used on Windows, dmg image on MacOS, and AppImage on Linux - just like in Craft. The difference is in their [compression ratio](https://en.wikipedia.org/wiki/Data_compression_ratio).

Craft uses [NSIS](https://sourceforge.net/projects/nsis/) with custom 1,1 MB lzma2 decompresser to create exe installer. Built-in lzma decompress should be [no worse](https://en.wikipedia.org/wiki/Lempel%E2%80%93Ziv%E2%80%93Markov_chain_algorithm) and saves that 1,1 MB, so it has been used.

Craft uses [create-dmg](https://github.com/andreyvit/create-dmg) with the fastest gzip compression to create dmg image. It's compatible with all MacOS versions, but the latest and packaged Qt is [not](https://doc.qt.io/qt-5/macos.html). [lzfse](https://en.wikipedia.org/wiki/LZFSE) supports MacOSes that Qt supports and yields the best compression ratio, so it has been used.

Craft uses [AppImageKit](https://github.com/AppImage/AppImageKit) with the best gzip compression to create AppImage. Switching to lzma and setting larger block size yields better result. Goal here is, as before, to showcase how far one can reduce package size. This configuration noticeably lengthens application's start-up time, so it may change in the future.

Size differences for compressed packages and after their decompression are presented in the table below.

<table width="40%">
  <thead>
    <tr class="header">
      <th rowspan="2" style="vertical-align:middle">Build environment</th>
      <th colspan="2">Windows</th>
      <th colspan="2">MacOS</th>
      <th colspan="2">Linux</th>
    </tr>
    <tr class="header">
      <th>compressed</th>
      <th>uncompressed</th>
      <th>compressed</th>
      <th>uncompressed</th>
      <th>compressed</th>
      <th>uncompressed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>BinaryFactory</td>
      <td>69,7 MB</td>
      <td>318,8 MB</td>
      <td>130,0 MB</td>
      <td>423,8 MB</td>
      <td>183,9 MB</td>
      <td>507,4 MB</td>
    </tr>
    <tr>
      <td>TravisCI&nbsp;and&nbsp;AppVeyor (compressed&nbsp;as&nbsp;in&nbsp;BinaryFactory)</td>
      <td>23,6 MB</td>
      <td>94,7 MB</td>
      <td>39,0 MB</td>
      <td>99,3 MB</td>
      <td>38,1 MB</td>
      <td>107,7 MB</td>
    </tr>
    <tr>
      <td>TravisCI and AppVeyor</td>
      <td>23,7 MB</td>
      <td>94,7 MB</td>
      <td>34,9 MB</td>
      <td>99,3 MB</td>
      <td>28,9 MB</td>
      <td>107,7 MB</td>
    </tr>
  </tbody>
</table>

As one can observe, size of packages after decompression varies across OSes and is 13 % for own build system and 45 % for BinaryFactory. This should be as lowest as possible because that would mean no redundant bytes and no missing functionality across packages for different OSes.
BinaryFactory's compression ratio for exe installer is surprisingly, slightly better even with additional lzma2 decompresser.

Size gains for compressed packages relative to packages compressed as in BinaryFactory are presented in the table below.

<table width="40%">
  <thead>
    <tr class="header">
      <th>—</th>
      <th>Windows</th>
      <th>MacOS</th>
      <th>Linux</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>size gain</td>
      <td>-0,42 %</td>
      <td>10,51 %</td>
      <td>24,15 %</td>
    </tr>
  </tbody>
</table>

As one can observe, gain is not manyfold but differences are measurable in case of MacOS and Linux.