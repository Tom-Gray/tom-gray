---
layout: post
title:  "Labelling IIS sites with Logstash
date:   2013-11-10 10:18:00
categories: ELK IIS Logstash
---

Ingesting logs from IIS with into Logstash is easy enough, there are plenty of examples to get started with. However if you run mutliple sites on the same IIS host, you're going to run into two problems:

## You need to consolodate logging into a single file

I am running many sites on each IIS host. By default you get a seperate log file for each site, each in a folder according to the service ID W3SVC1, W3SVC2 etc. You could be dealing with dozens or hundreds of folders. Each site created or removed from the host would need to be updated in the filebeat configuration. I've also seen reports that filebeat has some trouble processing mutliple IIS files although I have not seen this behavour myself.

Changing to server-level logging gives you a single file to target, which logstash can easily part back out to individual sites using Logstash filters which I'll describe below.

To enable server-level logging:

The log will appear in a foder %loglocation%\W3SVC\W3SVC.log




## You need to translate the W3SVC* site codes into something readable

Nobody wants a dashboard which a bunch of unknown codes, but there is nothing in the log file that can reliably tell you what the name of the site is. For this, you need to give logstash a little help, in two parts:

## [The Logstash transalte plugin] The Translate plugin]: https://www.elastic.co/guide/en/logstash/2.4/plugins-filters-translate.html 
Installing the plugin is straight forward unless you've installed logstash via a package manager. In that case you install the plugin 




- a dictionary file to transalte W3SVC codes into friendly site names. This can be embedded in your logstash filter or as a seperate .yml file.



