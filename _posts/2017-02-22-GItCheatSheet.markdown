---
layout: post
title:  "Git Cheat Sheet"
date:   2017-02-17 21:06:23 -0600
categories: Java
---
Git Deleting branches all except develop and master
{% highlight PowerShell %}
git branch | grep -v "develop" | grep -v "master"| xargs git branch -D
{% endhighlight %}

{% highlight PowerShell %}

#Listing all branches
git branch -a
#Local Branch Deleting
git branch -D  branchName

#Deleting origin branches one go
git push origin :InvoiceSummaryDefectFix  :InvoiceSummaryForMultiInvoices

git fetch --all --prune

{% endhighlight %}
