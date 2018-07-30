---
date: 2013-01-10 23:14:17+00:00
slug: how-to-add-a-button-to-edit-listview-item-on-a-custom-scsm-form
title: How to add a button to edit ListView item on a custom SCSM form
categories:
- Service Manager
tags:
- Service Manager
- Custom Form
- Visual Studio
---

I recently had to work with ListViews on custom forms to represent related CIs. While working on this, I was asked if it would be possible to edit objects in this ListView directly from the form by just selecting the object and then clicking on a button to open the form for this object. The result should look something like this.

[![EntityForm](/images/entityform.png?w=460)](/images/entityform.png)

A click on the _Edit_ button should open the form for the selected object

[![Entity_IdentityForm](/images/entity_identityform.png?w=460)](/images/entity_identityform.png)

I will first demonstrate how to create a ListView in your forms XAML code (see [http://msdn.microsoft.com/en-us/library/ms750972.aspx](http://msdn.microsoft.com/en-us/library/ms750972.aspx) for more information about the ListView Class) and afterwards show you what Code-Behind you have to add to your form to get this working.

To add a ListView to your form, no special references are needed. Just drag and drop the ListView element from your Visual Studio Toolbox to your custom form. Beside the common attributes like _Name_, you then have to define a View Mode for your ListView and a Binding to the TypeProjection defined for the relationship between your classes. As a View Mode you will probably use a GridView like in my example below.

The _ItemsSource_ is bound to the name of the Alias for your TypeProjection. Furthermore the _GridViewColumn_ binds to a property of the related CI.

```xml    
    <ListView Grid.Row="0" Grid.Column="0" Margin="10,20,85,10" VerticalAlignment="Top" Name="listViewAccounts" Grid.ColumnSpan="2" MinHeight="200" ItemsSource="{Binding Path=EntityHasEntity_Identity, UpdateSourceTrigger=PropertyChanged, Mode=TwoWay}">
        <ListView.View>
            <GridView>
                <GridViewColumn Header="ID"
                    DisplayMemberBinding="{Binding Path=entityidentityid}"
                    Width="Auto" />
                <GridViewColumn Header="Display Name"
                    DisplayMemberBinding="{Binding Path=DisplayName}"
                    Width="Auto" />
                <GridViewColumn Header="Valid from"
                    DisplayMemberBinding="{Binding Path=entityidentityvalidfrom}"
                    Width="Auto" />
                <GridViewColumn Header="Valid until"
                    DisplayMemberBinding="{Binding Path=entityidentityvaliduntil}"
                    Width="Auto" />
            </GridView>
        </ListView.View>
    </ListView>
```

This should do it for the ListView. Now the form yet lacks of a button to edit the selected ListView item. Add the following XAML code to your form to place a button beside your ListView. Notice the _Click_ attribute which will reference to the button event handler we will create later on.

```xml
    <Button Grid.Row="0" Grid.Column="0" Margin="0,20,10,0" VerticalAlignment="Top" HorizontalAlignment="Right" Name="buttonAccountEdit" Grid.ColumnSpan="2" Height="20" Width="65" Click="buttonAccountEdit_Click" >Edit</Button>
```

Your form should now basically look like this:

[![ListView with Edit button](/images/entityformdraft.png?w=460)](/images/entityformdraft.png)

Now we have to give life to that button :-)

First you have to add some references to your Visual Studio project:

* Microsoft.EnterpriseManagement.ServiceManager.Application.Common.dll (Contains ConsoleContextHelper)
* Microsoft.EnterpriseManagement.UI.SdkDataAccess.dll (Contains BindableDataItems)
* Microsoft.EnterpriseManagement.UI.Foundation.dll (Contains IDataItem)

Open your forms .cs file in Visual Studio and add the following namespaces:

```csharp
    //Contains IDataItem
    using Microsoft.EnterpriseManagement.UI.DataModel;
    //Contains ConsoleContextHelper
    using Microsoft.EnterpriseManagement.ServiceManager.Application.Common;
    //Contains BindableDataItems
    using Microsoft.EnterpriseManagement.UI.SdkDataAccess.DataAdapters;
```

Now create a button handler together with the corresponding method, which contains the action. Notice that the button event handler passes your ListView object to the method.

```csharp
    // account edit button event handler
    private void buttonAccountEdit_Click(object sender, RoutedEventArgs e)
            {
                EditIdentityFromListView(this.listViewAccounts);
            }
    
    internal static void EditIdentityFromListView(ListView listView)
            {
                if (listView.ItemsSource == null || listView.SelectedItems == null || listView.SelectedItems.Count == 0)
                {
                    MessageBox.Show("Select Identity to edit", "No Identity selected", MessageBoxButton.OK, MessageBoxImage.Information);
                }
                else
                {
                    BindableCachedDataItem<IComposableProjection> bcdItem = (BindableCachedDataItem<IComposableProjection>)listView.SelectedItem;
                    IDataItem item = (IDataItem)bcdItem;
                    ConsoleContextHelper.Instance.PopoutForm(item);
                }
            }
```

Build your form and deploy it to Service Manager to see if it works.

Hope this helps!








