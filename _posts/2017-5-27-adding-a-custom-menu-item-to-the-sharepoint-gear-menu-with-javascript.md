---
layout: post
title: Adding a custom menu item to the SharePoint gear menu with JavaScript
comments: true
---

Adding a custom menu item to the SharePoint gear menu using Javascript is easy, but there isn’t a lot of documentation on how to do this in practice. Here is a script you can insert wherever you are adding any custom Javascript, whether in a custom script file or in the script editor. As soon as the page with the script is loaded, it will check if the menu item exists. If not, it will add it. Lines 30-35 is where you can set the properties of the custom menu item.

```javascript
SP.SOD.executeFunc('sp.js', 'SP.ClientContext', GetCustomActions);
    var clientContext, 
        oWeb, 
        collUserCustomAction;
    function GetCustomActions () {
        //Get the client context and web object 
        clientContext = new SP.ClientContext();
        oWeb = clientContext.get_web();
        //Get the custom user action collection and add the new custom action to it   
        collUserCustomAction = oWeb.get_userCustomActions(); 
        clientContext.load(collUserCustomAction);  
        clientContext.executeQueryAsync(AddCustomAction, QueryFailure); 
    }
    function AddCustomAction () {
        var customActionEnumerator = collUserCustomAction.getEnumerator();
        var isDashboard = false;

        while (customActionEnumerator.moveNext()) 
        {
            var oUserCustomAction = customActionEnumerator.get_current();

            if (oUserCustomAction.get_title() == 'Dashboard') 
            {
                isDashboard = true;
            }
        }
        if(isPresNetAdmin === false) {
            var oUserCustomAction = collUserCustomAction.add();  
            //Specify the location and properties for the new custom action   
            oUserCustomAction.set_location('Microsoft.SharePoint.StandardMenu');  
            oUserCustomAction.set_sequence(101);  
            oUserCustomAction.set_group('SiteActions');  
            oUserCustomAction.set_title("Dashboard");  
            oUserCustomAction.set_url("/Pages/dashboard.aspx");
            oUserCustomAction.update(); 
            //Load the client context and execute the batch 
            clientContext.load(oUserCustomAction);
            clientContext.executeQueryAsync(QuerySuccess,QueryFailure);
        }
    }

    function DeleteCustomAction()
    { 
        var customActionEnumerator = collUserCustomAction.getEnumerator();

        while (customActionEnumerator.moveNext()) 
        {
            var oUserCustomAction = customActionEnumerator.get_current();

            if (oUserCustomAction.get_title() == 'Dashboard') 
            {
            oUserCustomAction.deleteObject();
            clientContext.load(oUserCustomAction);
            clientContext.executeQueryAsync(QueryDeleteSuccess,QueryFailure);
            }
        }
    }
  
    function QuerySuccess() {  
        console.log("New Custom User Action has been added to site settings");  
    } 

    function QueryDeleteSuccess() {  
        console.log("Custom action has been deleted");  
    }  
  
    function QueryFailure() {   
        console.error('Error')
    }
```

### Additional Reading
- [SharePoint 2013: Create And Delete A User Custom Action Using CSOM](http://www.c-sharpcorner.com/article/sharepoint-2013-create-and-delete-a-user-custom-action-usin/)
- [SP.UserCustomActionCollection.add Method (sp.js)](https://msdn.microsoft.com/en-us/library/office/jj246884.aspx)
- [Create SharePoint Custom User Action In Site Actions Menu Using JavaScript Object Model](http://www.c-sharpcorner.com/article/create-sharepoint-custom-user-action-in-site-actions-menu-using-javascript-objec/)
