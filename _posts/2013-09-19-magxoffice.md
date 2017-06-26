---
layout: post
title: "[Project] Magx Office (pdf, docx, odt viewer)"
categories: motorola magx
---

Magx Office Suite is a pdf, docx and odt viewer for the Motorola Magx Platform. The current version has been written for the Motorola Rokr E8 phone and can be easily ported to other phones running on the moto magx platform.

The Magx office suite includes two important components which are the PDF Viewer and DocViwer. Both of these components are re-usable and can be used as stand alone PDF / Doc viewers. These also supports MIME types, so you can associate the file types with PDF / Doc viewer accordingly. Both the viewer supports full screen modes and supports increasing or decreasing fonts and toggle font style bold.

PDF Viewer additionally supports Previous Page, Next Page, Goto Page, View PDF Information. It also can remember the last file opened. Since files are opened page by page, it can load any large pdf file. Please not that only support viewing texts from pdf files and not images.
PDF Support is implemented using the Xpdf library by porting it to the Magx Platform.

Here are some screenshots and do find the .mgx file and the source for the same at the end of this post.

TODO: img images/mgxOffice1.png
TODO: img images/mgxOffice2.png 

Mgx Package : [Download](http://dl.dropbox.com/u/12319078/magxoffice.mgx)

Source tarball : [Download](http://dl.dropbox.com/u/12319078/magxOffice.tar.gz)
