---
layout: post
title: Attributes of fields in a Extended Model Class
categories: [Odoo]
tags: [Odoo, inheritance]
date: 2016-03-08 09:41:00 +5:45
#date-modified: 2016-03-04 10:08:00 +5:45
description: Modify attributes of an inherited field in an extended model class by adding a new one or replacing existing one
fullview: False
comments: True
---

[Odoo][odoo] lets you define attributes of a **Field** in-place during declaration. Though optional attributes can be defined in **Views**, it can not possibly be the best place since the attributes defined in one view can not be reused in another, unless you want to change the attributes value specific to a particular view.

Attributes of fields defined in python can be overridden by redefining  them in respective view's xml.

{% highlight python %}
class goal_task(models.Model):

    _name = "goal.task"
    # Lets give the optional attribute `required` a value
    review_members = fields.Many2one(comodel_name='res.users', required=True)
    ....
    ....

{% endhighlight %}

{% highlight xml %}
<record id="goal_task_form_view" model="ir.ui.view">
   <field name="name">goal.task.form</field>
   <field name="model">goal.task</field>
   <field name="arch" type="xml">
      <form string="Tasks">
         <!-- Here an extra attribute `string` is defined and the value of attribute `required` is redefined  -->
         <field name="review_members" string="Review Panel Members" required="0">
         .....
         .....
      </field>
   </field>
</record>
{% endhighlight %}

In the case of inherited fields (fields inherited via [extending][odoo-class-extension] a model class) if an attribute value needs to be redefined globally irrespective of a particular view then redefining it in **view** is not the right way to do it, since it will involve duplicating the attribute value in every possible view there were and will be in future.

The existing attributes of an inherited field can be overridden and extra attributes can be added via [Incremental definition][odoo-incremental-field-definition], just like the normal field definition but instead of passing all the required attributes as parameter you pass only the attributes you wish to change/add.

{% highlight python %}
# Extending Model Class `goal.task`
class work_task(models.Model):

    _inherit = "goal.task"
    # change the value of existing attribute `required`
    # add an extra attribute 'string'
    review_members = fields.One2many(required=False, string="Review Panel Members")

{% endhighlight %}
[odoo]: <https://www.odoo.com>
[odoo-incremental-field-definition]: <https://www.odoo.com/documentation/9.0/reference/orm.html#field-incremental-definition>
[odoo-class-extension]: <https://www.odoo.com/documentation/9.0/reference/orm.html#extension>
