# Access Control

By default, all Action classes are access controlled. It means that user will not be able to trigger an Action class if the user is not granted access to that Action. To make an Action accessible without granting any access, add the Action name in `AuthorizationInterceptor.java` in variable `directRunActionClass`.

To utilise the framework's access control, we need to configure Application and User Group.

## Application

1. Navigate to **System Administration > Module Setup**.

	Every application is tied to a module, so we need to make sure the module is configured first.
		
	> [!NOTE]
	> A module can also be configured as a sub module.
	
	Sample:	
	<img class="img-bordered" src="/sdsfw_docs/images/access-control/sample-module.png" alt="sample module" />
	
1. Navigate to **System Administration > Application Setup**.

	An application represents a page / function in the web application. Here, we need to configure information related to applications, such as URL, Action class, Applications Rights, and so on.

	Once granted access to an application, the Action class tied to the application can run these standard methods:
		
	<ul>
		<li>search2()</li>
		<li>loadViewPage()</li>
		<li>loadAddPage()</li>
		<li>loadEditPage()</li>
		<li>processSearch()</li>
		<li>processInsert()</li>
		<li>processUpdate()</li>
		<li>delete()</li>
		<li>cancel()</li>
		<li>cancelChanges()</li>
		<li>submitVerify()</li>
		<li>populateDynamicActionSetup()</li>
	</ul>
			
	Any methods other than these standard methods need to be defined in the Additional Rights.
	
	> [!TIP]
	> The `Invoking Methods` field accepts multiple methods, just separate the methods with a comma. E.g. `processAddRights, processDeleteRights`
		
	Sample:	
	<img class="img-bordered" src="/sdsfw_docs/images/access-control/sample-application.png" alt="sample application" />
		
## User Group

Navigate to **System Administration > User Group Setup**.

Create a user group by adding users and assigning applications to that group.

> [!NOTE]
> The User Group can also be configured as a workflow. To use `Workflow`, contact the framework developer.

Sample:	
<img class="img-bordered" src="/sdsfw_docs/images/access-control/sample-user-group.png" alt="sample user group" />

## Application Setup  
Usual application setup, but with methods of your API method name in the "Additional Rights" segment.  

## User Group Setup  
Add/Edit user group, and add the application_code for the API application.  

If you are using the older version of user setup, then you may need to switch the step between User Setup and User Group Setup. Then add the user to the User Group Setup.  

## Access Control Setup  
This step only apply if it is a new User Group, or the group cannot be found in User Setup screen. 

Search and edit base of the User Group code/name. It will be listed without the Access ID/Access Name. 

**Access ID -** Access ID is similar to group code. Any group code you want to group together under 1 Accordion Category in User Setup will be setup from here. You can select from lookup for existing Access ID to prevent typo or case sensitive error that will separate the grouping.   
**Access Name -** Access Name is the name of the Access ID. Naming it with proper understandable name will listed as the accordion name. If you are selecting from existing Access ID, the Access Name will be autofill.    
**Group Code -** Will be base on the group code you have created. Allow to edit just for in case you have some prefix or postfix logic you need to customize.  
**Group Name -** Will base on group name you have created. But sometime you may not want to display same name as the group name. Here you can ament the group name to be display diferrented in your Accordion Item.  
**Access Sequence -** This will be the sequence of the group items in your Accordion group. You can arrange your preferred sequence.   
**Group Sequence -** Group sequence will determine the current accordion group sequence. Example you have 3 groups, System Administration, Reports, Data Entry. You can set the System Administration as 1, Reports as 2, and Data Entry as 3 to display the group sequence accordingly. When you select from existing groups, the group sequence of your previous setting will be autofilled.   

## User Setup  
Create/Edit user from User Setup. Look for the user group, tick on the group.

## API Authentication
**Application Name:** API Authentication  
**Module:** System Administration  
**Table:** t_api_authentication  

**Steps to generate secret key and customer key**  
**Step 1:** Create new API Authentication  
**Step 2:** Input the required fields. Refer to API Authentication Screen details below.  
**Step 3:** Save and will receive the Customer Key and Secret Key required for generate access token  

**API Authentication Screen details.**  
**API Application -** Application Code.  
**API Description -** Description of what is your API do.  
**User -** User ID of your system. Must be exists in system.  
**Token Duration -** Duration of the generate token count in seconds.  
**Customer Key -** Will be auto-generated.  
**Secret Key -** Will be auto-generated.  