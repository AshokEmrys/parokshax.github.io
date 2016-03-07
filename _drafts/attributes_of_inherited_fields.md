---
layout: post
title: Attributes of Inherited fields
categories: [Odoo]
tags: [Odoo]
#date: 2016-03-04 10:08:00 +5:45
date-modified: 2016-03-04 10:08:00 +5:45
description: Modify attributes of an inherited field by adding a new one or replacing existing one 
fullview: False
comment: True
---

Odoo lets you define attributes of a **Field** in-place during declaration. Though attributes can essentially be redefined in **Views**, it cannot possibly be the best place since the attributes defined in one view can not be reused in another, unless inherited. 

Atrributes of fields defined in python can be overriden by redefining them in respective view's' xml.

{% highlight python %}
# file: task.py

class project_task(models.Model):

    _inherit = "project.task"
    review_members = fields.One2many(comodel_name='res.users', string="Reviewers" )

{% endhighlight %}

{% highlight xml %}
<field name="review_members" attrs="

They can essentialy be overriden in views
