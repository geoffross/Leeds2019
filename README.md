# Lab Guide

## Table of Contents
1. [ Applying a Basic Theme to The Cireson Service Manager Portal ](#themer)
2. [ More Specific Styling ](#css)
3. [ Basic JavaScript Customisation ](#js)
4. [ Creating a Custom Task ](#task)
5. [ Advanced Task Integrating with Remote Support ](#advancedtask)

## Lab Details

### Server
RDP to sandpit1377857395000.southcentralus.cloudapp.azure.com:64624  

### Credentials
Username: JohnLeeds  
Password: Cireson123  

<a name="themer"></a>
## 1. Applying a Basic Theme to The Cireson Service Manager Portal

We're going to apply a Portal theme in order to set our basic colour scheme in the Portal.

1. Load Portal at http://cset-a-2016smms/ using Chrome. Note the Out of Box colors

1. Head to http://portalthemer.cireson.com/
1. Select the latest version from the selector at the top of the page.
1. Select same basic colours.
1. Click _Download Theme CSS_ to drop out your CSS file.
1. Copy it into your _CustomSpace_ folder at _C:\inetpub\CiresonPortal\CustomSapce_
1. Edit _custom.css_ from _CustomSpace_.
1. Add the following at the top.
   ```css
   @import _custom.theme.min.css_;
   ```
1. Refresh your Portal to see your new theme.
1. Experiment with different themes.

<a name="css"></a>
## 2. More Specific Styling

We're going to change the appearance of a particular element, that cannot specifically be done using the themer tool. In this lab were going to change the color of the header bar, discover its unique selector and add a CSS rule.

1. Right click on the element you want to customise and choose Inspect.

   ![Inspect Element][inspect]

1. Find the element you want to style in the HTML. We're looking for a unique class on the element.
1. In this case we can use the _navbar_ class.
1. Edit _custom.css_ from _CustomSpace_.
1. Add a ccs rule for the _navbar_ class
   ```css
    .navbar {

    }
   ```
1. Add your styling into the rule. Don't forget the US spelling of color.
   ```css
    .navbar {
        background-color: red
    }
   ```
1. You can define your color in words or rgb in decimal or hex.
   ```css
    .navbar {
        background-color: rgb(255, 0, 0)
    }
   ```
   ```css
    .navbar {
        background-color: #ff0000
    }
   ```
1. Experiment with different colours. You can try different properties too. _color_ will change the font colour, _font-size_ will change the font size.
<a name="js"></a>
## 3. Basic JavaScript Customisation

CSS Customisation is all about changing the look and feel of the portal. To change the behaviour, we need to use JavaScript.

We're going to add a custom message to the Portal Homepage.

1. We'll develop our code in the Browsers console. Open the Portal the Homepage and press <kbd>F12</kbd> Go to the Console tab.
1. We'll use jQuery to get the element we want to manipulate.
   ```javascript
   $("h1[class='page_title']")
   ```
1. Pressing <kdb>Enter</kbd> will return an array of one object. Expand it and hover over the first (0th) object and the area on the page will be highlighted.

   ![Element][element]
1. Now we have the element we wish to manipulate, we can do so. Run this to add our custom message.
   ```javascript
   $("h1[class='page_title']").after('<h4>Hello. Welcome to the Leeds Event Portal.</h4>');
   ```
1. We could even pull meta data from the current logged on user and add that into our message to personalise it. _session.user_ contains the logged on user's information. Refresh the page to start again from scratch.
   ```javascript
   $("h1[class='page_title']").after('<h4>Hello ' + session.user.FirstName + '. Welcome to the Leeds Event Portal.</h4>');
   ```
    Once we have it working as we want, we need to make this permanent. We need the portal to run this extra code each time the Homepage loads. We use the _custom.js_ file to do this.

    The _custom.js_ file is a JavaScript file that the portal will execute last on every page so we can use it to override the Portal's default behaviour.

    We don't want to place our final code directly in the _custom.js_ file directly. Doing so would mean it will run on every page, potentially slowing the Portal down and will make the _custom.js_ file difficult to manage over time.

    Instead we will place our code in a separate js file and use the Cireson ScriptLoader to import it.

1. Inside the _CustomSpace_ folder create a new subfolder called _WelcomeMessage_
1. Inside _WelcomeMessage_ create a new file called _WelcomeMessage.js_ and paste our working code into it.
1. Copy and Paste the Cireson ScriptLoader code from [here](./Snippets/ScriptLoader.js)

   The ScriptLoader efficiently loads external js files into the Portal. It takes two Parameters. The path to the external js file and a list of strings to look for in the URL of a page to determine whether to run the code here or not.

1. Add our call to the ScriptLoader at the bottom of _custom.js_. This will load our external file, but only when we are on the Homepage _/View/02efdc70-55c7-4ba8-9804-ca01631c1a54_
   ```javascript
   loadScript("/CustomSpace/WelcomeMessage/WelcomeMessage.js", ["View/02efdc70-55c7-4ba8-9804-ca01631c1a54"]);
   ```
1. Refresh your Portal. You will need to disable the cache in the Network Tab to ensure the browser is pulling the latest files. You should see the file being loaded in the Console Tab.
1. However, the custom welcome message is not showing. We have to handle a few timing issues to avoid our custom code being executed before the page is ready to process it.
   ```javascript
   $(document).ready(function () {
     setTimeout(function () {
       $("h1[class='page_title']").after('<h4>Hello ' + session.user.FirstName + '. Welcome to the Leeds Event Portal.</h4>');
     },3000)
   });
   ```
1. One more refresh should show the custom message.
1. The whole customisation can be toggles off and on but commenting and uncommenting the line where we call the ScriptLoader. This is down with a double backslash.
   ```javascript
   //loadScript("/CustomSpace/WelcomeMessage/WelcomeMessage.js", ["View/02efdc70-55c7-4ba8-9804-ca01631c1a54"]);
   ```
1. Experiment toggling the customisation off and on and with different messages.
<a name="task"></a>
## 4. Creating a Custom Task

A very useful area of customisation is custom tasks. These can be added to the form of any type of SCSM Object to perform pre-defined repeatable functions. In this lab we're going to create a custom task to run on an Incident.

1. Once again, rather than add our code directly to _custom.js_ we'll add a subfolder and external js file and load it using the ScriptLoader. Create a Subfolder and file for our custom task, both called _IncidentTask_.

1. Add the base code for the task into _IncidentTask.js_ We'll add the _DoSomething_ later.
   ```javascript
   app.custom.formTasks.add('Incident', "Custom Task", function (formObj, viewModel) {
     //Do Something
   });
   ```
1. Use the ScriptLoader to ensure this code runs on every Incident page but nowhere else.
1. Refresh the Portal and open any Incident from Active Work. We should see our custom task on the right.
1. Now we can replace the _//Do Something_ with some more JavaScript to make our task do something.
   ```javascript
   app.custom.formTasks.add('Incident', "Custom Task", function (formObj, viewModel) {
     alert("Hello World");
   });
   ```
1. Because we have access to the Incident data in the _viewModel_ object, we can also make it do something useful.
   ```javascript
   app.custom.formTasks.add('Incident', "Custom Task", function (formObj, viewModel) {
     viewModel.set("Title","This is a new Title");
   });
   ```
1. Experiment with other JavaScript snippets to make more custom tasks
1. Experiment making custom tasks for other type of object, eg Service Request, Change Request, Hardware Asset, Shared Mailbox.




[inspect]: https://github.com/geoffross/Leeds2019/raw/master/Images/Inspect.png "Inspect Element"
[element]: https://github.com/geoffross/Leeds2019/raw/master/Images/Element.png "Element"