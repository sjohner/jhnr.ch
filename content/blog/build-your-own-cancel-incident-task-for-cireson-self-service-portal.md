---
author: sjohner
comments: true
date: 2015-05-13 19:34:52+00:00
slug: build-your-own-cancel-incident-task-for-cireson-self-service-portal
title: Build your own "Cancel Incident" task for Cireson self-service portal
categories:
- Service Manager
tags:
- Cireson
- HTML
- JavaScript
- Self-Service Portal
- Task
---

Most of our customers replaced the out of the box, Silverlight based Service Manager portal with a more lightwight, more flexible, HTML5 based 3rd party solution. Some of them are using the [Cireson Self-Service Portal](http://cireson.com/apps/self-service-portal/) to enhance their Service Manager environments. The Cireson Portal is a complete replacement of the Microsoft Self-Service Portal for Service Manager. But Cireson is not the only company offering a replacement for the Service Manager portal. Other Self-Service portal solutions are offered by [Syliance](http://itnetx.ch) as well as [Cased Dimensions](http://www.caseddimensions.com/scsm_self_service_portal/).

But for this post I will concentrate on the Cireson portal, escpecially on the functionality which allows you to create custom tasks for work item forms. Based on a Cireson [support article](https://support.cireson.com/KnowledgeBase/View/52?selectedtab=enduser) I created a custom task which enables endusers to cancel an active Incident.

![Cancel Incident Task](/images/cancelincidenttask.png?w=604)

Basically the following steps are necessary to create a custom task for the Cireson Self-Service portal:

* Define custom task in _CustomSpace/custom.js_
* Add any custom HTML templates your _CustomSpace_ directory

The second step is optional but in order to give your custom task a richer UI you can define HTML templates that your custom task can use. Just add a new HTML file in your _CustomSpace_ directory or in a subdirectory of _CustomSpace. _You will later have to specify the exact path in your _custom.js_ file.

Creating a custom task for the Cireson portal requires some JavaScript and HTML knowledge. But Cireson did a good job in explaining the details on how to create a custom task which makes it pretty easy to create your own tasks. Even if you don't have a deep knowledge in JavaScript :-) And if you have any questions about how to create a task or if you are stuck somewhere with your code, I am pretty sure the Cireson guys are happy to help!

The task is designed to support multiple languages. When using localized strings, you can referr to predefined localization strings or of course create your own in the localization settings of the Cireson portal. For my task to work properly you will have to define a new localization string with Key _CancelIncident _and translate it for each language supported by your portal installation.

![Add Localization Cancel Incident](/images/addlocalizationcancelincident.png?w=604)

Furthermore, when cancelling an Incident the task it will do the following for the selected Incident:

* Change Status to _Resolved_
* Change Resolution category to _Cancelled _and set corresponding resolution text
* Create appropriate Action Log entry

Please find the custom.js file below followed by the corresponding HTML definition. Have fun building your very own custom portal tasks!

**Important Update: **Since the below version of the script does not work properly anymore with Cireson Portal v6 and newer ('actionLogModel.push()' has unfortunately never really been a supported method, and stopped working), I recommend using the solution provided by Cireson consultant [Nicholas Velich](https://community.cireson.com/profile/Nicholas_Velich) which can be downloaded from [Cireson Community](https://us.v-cdn.net/6026663/uploads/editor/yq/sh9ruf4v6s79.zip)

```javascript
    app.custom.formTasks.add('Incident', "Cancel Incident", function (formObj, viewModel) {
    	console.log(formObj);
    	//use requirejs to load the template first 
    	require(["text!/CustomSpace/changeStatusComment.html"],
    	function (template) { 
    		//make a jQuery obj 
    		cont = $(template);
    		//Create a view model to handle the UX 
    		var _vmWindow = new kendo.observable({
    			resolutionDescription: "",
    			okEnabled: false,
    			charactersRemaining: "4000",
    			charactersRemainingText: localization.CharactersRemaining,
    			textCounter: function () { 
    				var maximumLength = 4000;
    				var val = this.resolutionDescription.length; 
    				(val > maximumLength) ? this.resolutionDescription.substring(0, maximumLength) : this.set("charactersRemaining", maximumLength - val);
    				(val > 0) ? this.set("okEnabled", true) : this.set("okEnabled", false); 
    			}, 
    			saveStatus: function () { 
    				//Set Incident Status to "Resolved"
    				viewModel.set("Status", {Name: "Resolved", Id: "2b8830b6-59f0-f574-9c2a-f4b4682f1681"});
    				//Set ResolutionCategory to "Cancelled"
    				viewModel.set("ResolutionCategory", {Name: "Cancelled", Id: "c72b161a-c065-f391-ab4d-5794308bd495"}); 
    				//Set Resolution Description entered by the user
    				viewModel.set("ResolutionDescription", this.get("resolutionDescription"));
    				//Create action log entry
    				var newActionLog = {
    					EnteredBy: session.user.Name,
    					//Set localized title. Other values could be localization.Comment, localization.Analyst or localization.EndUser
    					//e.g. Title: localization.Analyst + " " + localization.Comment
    					Title: localization.RecordResolved,
    					EnteredDate: new Date().toISOString().split(".")[0],
    					LastModified: new Date().toISOString().split(".")[0],
    					//Set description entered by the user
    					Description: this.get("resolutionDescription"),
    					//Set image for action log entry. Other values could be "comment" or "privateComment"
    					//e.g. Image: (isPrivate) ? app.config.iconPath + app.config.icons["privateComment"] : app.config.iconPath + app.config.icons["comment"],
    					Image: app.config.iconPath + app.config.icons["Record Resolved"],
    					//Set action type. Other values could be "AnalystComment" or "EndUserComment"
    					//e.g. ActionType: "AnalystComment"
    					ActionType: {
    						Id: "5ca2cfee-6740-1576-540B-ce17222840b8",
    						Name: "Record Resolved"
                        }
    				} 
    				//Add entry to action log
    				actionLogModel.push(newActionLog); 
    			},
    			//Do changes when user clicks "Save" and close window 			
    			okClick: function () { 
    				this.saveStatus(); customWindow.close(); 
    			},
    			//Do nothing when user clicks "Cancel" and close window
    			cancelClick: function () { 
    				customWindow.close(); 
    			} 
    		}); 
    		//Create the window, set window title and size
    		customWindow = cont.kendoWindow({ 
    			//title: "Cancel Incident", 
    			title: localization.CancelIncident,
    			resizable: false, 
    			modal: true, 
    			viewable: false, 
    			width: 500, 
    			height: 300, 
    			close: function () { }, 
    			activate: function () { 
    				//On window activate bind the view model to the loaded template content 
    				kendo.bind(cont, _vmWindow); 
    			} 
    		}).data("kendoWindow"); 
    		//Open the window 
    		customWindow.open().center(); 
    	}); 
    });
```

```html
    <div>
      <!--Make sure you have an outer div if you are putting this in a kendo window-->
      <div class="defined-form">
      <!--<div class="defined-form hide" id="commentHTML">-->
        <div>
          <div class="col-group">
            <div class="editor-label">
              <label class="editor-label" data-bind="localize: Comment">
            </div>
            <div class="editor-field">
              <textarea class="k-textbox" data-bind="value: resolutionDescription, events: {keyup:textCounter}" data-value-update="keyup" rows="7"></textarea>
              <div class="inline-spacing">
    			<span data-bind="html: charactersRemaining"></span>
                <span data-bind="html: charactersRemainingText"></span>
              </div>
            </div>
          </div>
          <div class="window-buttons">
            <button data-role="button" class="btn btn-primary" 
    			data-bind="enabled: okEnabled, events: { click: okClick }">
                <span data-bind="localize: OK"></span>
            </button>
    	    <button data-role="button" class="btn btn-default" 
    			data-bind="events: { click: cancelClick }">
                <span data-bind="localize: Cancel"></span>
    		</button>
          </div>
        </div>
      </div>
    </div>
```