---
author: sjohner
comments: true
date: 2016-02-08 17:00:13+00:00
layout: post
slug: how-to-format-properties-on-custom-service-manager-forms
title: How to format properties on custom Service Manager forms
categories:
- Service Manager
tags:
- Custom Form
- ListView
- ListView control
---

When creating your own custom forms in Service Manager you may want to tweak your ListViews, escpecially when working with dates. For example you may want to hide time information for datetime properties. This is actually pretty simple by [using BindingBase.StringFormat property](https://msdn.microsoft.com/en-us/library/system.windows.data.bindingbase.stringformat%28v=vs.110%29.aspx)

The below example shows a ListView definition containing two datetime columns both formatted to show date in the format 'dd.MM.yyyy'
Properties do not necessary have to be bound to a ListVew column. Of course you can use StringFormat also to format text displayed in a TextBox for example.

    
    <ListView VerticalAlignment="Top" HorizontalAlignment="Stretch" x:Name="listViewInstances" MinWidth="450" MinHeight="250" ItemsSource="{Binding productinstance, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}">
    <ListView.View>
    <GridView>
    <GridViewColumn Header="Display Name"
    DisplayMemberBinding="{Binding DisplayName}"
    Width="Auto" />
    <GridViewColumn Header="Status"
    DisplayMemberBinding="{Binding status.DisplayName}"
    Width="Auto" />
    <GridViewColumn Header="Valid from"
    DisplayMemberBinding="{Binding validfrom, StringFormat='dd.MM.yyyy'}"
    Width="Auto" />
    <GridViewColumn Header="Valid until"
    DisplayMemberBinding="{Binding validuntil, StringFormat='dd.MM.yyyy'}"
    Width="Auto" />
    </GridView>
    </ListView.View>
    </ListView>
