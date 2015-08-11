#Wrapper Classes

Wrapper classes allow developers to encapsulate sObject records with non-persistant attributes.  In this quick start, you will create a wrapper class that will bind a non-persistant checkbox field to the Task sObject.  The checkbox field will only exist during runtime and will capture user input, binding that input to each individual Task as if the checkbox was an actual field on the Task object.  You will start by creating the wrapper class and then you will create the Visualforce controller extension and page that will encapsulate all tasks related to an account.  You will finish up by creating a custom detail button on the account sObject to call the page.


##Step 1: Create Wrapper Class.

1. From Setup, click **Develop | Apex Classes**
2. Click New.
3. Paste the below code into the editor and click Save.
```java
public with sharing class TaskWrapper{

    public Task wrappedTask {get;set;}
    public Boolean selected {get;set;}

    public TaskWrapper(Task t){
        wrappedTask = t;
        selected = false;
    }
}
```
Login to verify step.

##Step 2: Create Controller Extension.

1. From Setup, click **Develop | Apex Classes**
2. Click New.
3. Paste the below code into the editor and click Save.

```java
public with sharing class wrapperExtension{

    private final Account acc;
    public List<TaskWrapper> wrappedTasks {get;set;}

    public wrapperExtension(ApexPages.StandardController sc){

        acc = (Account)sc.getRecord();
        wrappedTasks = new List<TaskWrapper>();

        for(Task tmp : [select id, subject from Task where whatId =: acc.id limit 1000]){
            wrappedTasks.add(new TaskWrapper(tmp));
        }
    }
}
```



Login to verify step.

##Step 3: Create Visualforce Page.

1. From Setup, click **Develop | Pages**.
2. Click New.
3. Type "Wrapped Tasks" in the Label field and hit the TAB key to autocomplete the name field.
3. Paste the below code into the editor and click Save.


```html
<apex:page standardController="Account" extensions="wrapperExtension">
<apex:form >

    <apex:sectionHeader title="Trailhead Quickstart" subtitle="Wrapper Classes" />
    <apex:pageBlock title="Account">
        <apex:pageBlockButtons location="top">
            <apex:commandButton value="Select"/>
        </apex:pageBlockButtons>

        <apex:pageBlockSection title="Tasks">
            <apex:pageBlockTable value="{!wrappedTasks}" var="t">

                <apex:column headerValue="Non-persistant Checkbox Field">
                    <apex:inputCheckbox value="{!t.selected}"/>
                </apex:column>
                <apex:column value="{!t.wrappedTask.id}"/>
                <apex:column value="{!t.wrappedTask.subject}"/>

            </apex:pageBlockTable>
        </apex:pageBlockSection>
    </apex:pageBlock>

</apex:form>
</apex:page>
```

Login to verify step.

##Step 4: Create Custom Button.

1. From Setup, click **Customize | Accounts | Buttons, Links, and Actions**
2. Click New Button or Link.
3. In the Label field, type "Wrap it up!" and press the TAB key to autocomplete the name field.
4. Click the "Detail Page Button" radio button.
5. Change the "Content Source" field to "Visualforce Page".
6. In the "Content" field, choose the new "Wrapped Tasks" visualforce page created in step 3 and clik save.
7. If a dialog box pops up, click ok.
8. Navigate to the "Accounts" tab and click on an account record.
9. In the top right hand side of the page, click the "Edit Layout" link.
10. Add the new "Wrap it up!" button to the page layout and click save.

Back on the account detail page, click the "Wrap it up!" detail button to launch the new page.
Notice that our non-persistant checkbox is displayed as a column like any other Task field.

Congratulations!  You have successfully used a wrapper class to encapsulate the Task sObject
giving it a non-persistant checkbox field!  Now it is your turn to come up with clever uses
for wrapper classes!



######Resources

Apex Developer's Guide: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/

Visualforce Developer's Guide: https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/

Success Community: https://success.salesforce.com/answers?id=90630000000hSoLAAU

DeveloperForce Wiki: https://developer.salesforce.com/page/Wrapper_Class
