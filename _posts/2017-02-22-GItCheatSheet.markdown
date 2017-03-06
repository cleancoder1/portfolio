---
layout: post
title:  "Git Cheat Sheet"
date:   2017-02-17 21:06:23 -0600
categories: Java
---
Git Deleting branches

{% highlight PowerShell %}

#Listing all branches
git branch -a
#Local Branch Deleting
git branch -D  branchName

#Deleting origin branches one go
git push origin :InvoiceSummaryDefectFix  :InvoiceSummaryForMultiInvoices

git fetch --all --prune

{% endhighlight %}
