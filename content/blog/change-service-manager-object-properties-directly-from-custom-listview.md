---
author: sjohner
comments: true
date: 2014-06-25 16:28:30+00:00
layout: post
slug: change-service-manager-object-properties-directly-from-custom-listview
title: Change Service Manager related object properties directly from custom ListView
categories:
- Service Manager
tags:
- Custom Form
- ListView
- ListView control
- Service Manager form
- TextBox
- XAML
---

Don't you think that sometimes it would be very handy if we could change some properties of a related object directly from a form? Normally you would have to close the active form and edit the related object to make some changes which can be pretty time consuming...
As it turns out, there is another way to simplify this procedure by using a custom ListView :-) All you need is a custom Service Manager form which contains a _ListView_ to display related objects. And as you can see below, it is possible to configure the ListView to display TextBoxes and even ListPickers to enable users to change properties of related objects shown in the list.

[![ListView with DataTemplate](/images/listviewdatatemplate.png?w=604)](/images/listviewdatatemplate.png)<!-- more -->

ListViews are used pretty often in Service Manager. For example think about the related items tab on a default Service Request form. A whole lot of ListViews are used on this tab to display all the relationships between the Service Request and other Work and Config Items.

The _ListView _control is an _ItemsControl _that is derived from [_ListBox_](http://msdn.microsoft.com/en-us/library/system.windows.controls.listbox.aspx) and is a pretty powerful control which provides the infrastructure to display a set of data items in different layouts or views. Especially when designing your own forms, ListViews are important to represent one-to-many relationships between different objects. I will not cover all the details about list views as you can also find more information about the this class on [MSDN.](http://msdn.microsoft.com/en-us/library/ms750972)


To add a _ListView_ to your form, no special references are needed. Just drag and drop the _ListView_ element from your Visual Studio Toolbox to one of your custom forms. Beside the common attributes like _Name_, you then have to define a View Mode for your ListView and a Binding to the TypeProjection. As a View Mode you probably want to use a _GridView_ like in my example below.




[![Add ListView to Visual Studio form](/images/addlistview.png?w=604)](/images/addlistview.png)




The _ItemsSource_ is bound to the name of the Alias for your TypeProjection. Furthermore the _GridViewColumn_ binds to a property of the related CI. And that's exactly where you have to add some more XAML code to make the properties of the related object available for editing.


 

    
    <ListView Grid.Row="0" Grid.Column="0" VerticalAlignment="Top" Name="listViewAccounts" Grid.ColumnSpan="2" MinHeight="200" ItemsSource="{Binding Path=EntityHasEntity_Identity, UpdateSourceTrigger=PropertyChanged, Mode=TwoWay}">
      <ListView.View>
        <GridView>
          <GridViewColumn Header="ID" DisplayMemberBinding="{Binding Path=itnetxid}" Width="100" />
          <GridViewColumn Header="Name" Width="200" >
            <GridViewColumn.CellTemplate>
              <DataTemplate>
                <TextBox Name="textBoxDisplayName" VerticalAlignment="Top" HorizontalAlignment="Stretch" Width="Auto" Text="{Binding Path=DisplayName}"/>
              </DataTemplate>
            </GridViewColumn.CellTemplate>
          </GridViewColumn>
          <GridViewColumn Header="Status" Width="200">
            <GridViewColumn.CellTemplate>
              <DataTemplate>
                <smcontrols:ListPicker Name="listPickerStatus" HorizontalAlignment="Stretch" Width="Auto" ParentCategoryId="{Binding Source=c1c6904f-8bf4-7da8-7ad2-13e3280fd672, Mode=OneWay}" SelectedItem="{Binding Path=itnetxstatus, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
              </DataTemplate>
            </GridViewColumn.CellTemplate>
          </GridViewColumn>
        </GridView>
      </ListView.View>
    </ListView>



Note that each element in the _DataTemplate _above is bound to a property of the related class. The default property _DisplayName_ and custom property _itnetxstatus_ are displayed as _TextBox_ and _ListPicker_ respectively.

But instead of binding a property to a _GridViewColumn_ (like it is done with the _itnetxid_ property) you have to create a _DataTemplate _inline which contains the appropriate control (e.g. _TextBox_ or _ListPicker_). The _DataTemplate _specifies how the property of the displayed data item appears in the _ListView_. Without specific instructions, a _ListBox _(which is inherited by the _ListView _class) displays the string representation of the objects in a collection. In that case, you can use a _DataTemplate _to define the appearance of your data objects.

The cool thing about this is, that now when looking at the _ListView_, you can change the properties and they will be updated in the related source object!

![Updated Related Object](/images/updatedrelatedobject.png?w=604)
