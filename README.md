# ServiceNow Coding Test
It is always fun to stretch my legs in a new environment. I come from a *very* mature environment, so I always appreciate having the opportunity to play around with a fresh environment. This gave me some great direction to do that. I brought a little flair to it, I hope thats fine. :)

[ServiceNow environment](https://dev101047.service-now.com/)

## Items

### 1) Branding
I decided to keep the additional branding to a minimum. I see no sense in creating crazy color combinations or changing anything out of the box too much, especially when it isn't broken. I did go with a slight White Sox theme though.

### 2) Incident Producer
I am a strong supporter of using standard English when engaging with a user, so while the record producer itself is called "Report an Incident", the main button on the homepage reads "Report an Issue". ITSM terminology is great when both people speak that language, but we could have any given employee reporting an incident on this form. I also wanted to make the input as easy as possible to understand and quickly fill out, but still provide the Service Desk with crucial information. Many users don't know to report the steps they took to get to their issue, so I ask them. Hopefully this helps eliminate a few initial discovery emails and gets the incident resolved quicker.

The above record producer can also be reached by clicking on Self-Service > Report an Issue on platform view. A new window opens in the Service Portal.

I also added a widget to the bottom of every Service Portal page, so now a user doesn't need to be on the homepage to report an incident. The widget opens a modal where the user can describe the issue. When the modal is submitted, an incident is created for them that references the page from which they reported the incident.

### 3) Service Catalog Item
I chose to make a requestable mechanical keyboard. The user has an option of a full size keyboard, TKL (without number pad), or 60% (only main part of the keyboard), a choice of ANSI or ISO layout, and a variety of switches to choose from, some of which have an additional cost associated with them. The flow that I envisioned involves actually assembling these keyboards from scratch, soldering switches to circuit boards, and then delivering them straight to the employee.

For a little extra flair, I imagined a company that employs *extremely* staunch supporters of their own preferred switch type who refuse to work with other types. For this reason, we had to create a decision table so that the request is routed to the correct team for keyboard assembly. There are three teams, Kailh Keyboard Assemblers, Gateron Keyboard Assemblers, and Cherry Keyboard Assemblers. This was done through a decision table because there is hope that in the future, some of these groups may resolve their differences and come together. If that happens, the decisions on the table can be updated and we won't need to modify the flow. It also allows us to more easily expand the flow if a certain switch type requires additional unique tasks or other unique flow activites.

### 4) Knowledge Articles
I am a big fan of movies, so I just copied some wikipedia entries for some of my recent favorite films. These can be found under the Leisure Time > Movies category in the Knowledge Base.

### 5) New Task type (Extra Special Task)
**On change of Change Request field and On change of Impact to 1**
I was slightly unclear if these were supposed to be done client-side or server-side. I went the client-side route, but I also created inactive business rules to accomplish the same thing if that was the true intention. In the client script for CI's attributes, just retrieving the CI record would only provide a sys_id for Owned by and a numeric value for Operational Status, so I had to create a client-callable script include to retrieve the display data that I needed. Appending the Change Request's Short Description was as simple as retrieving the Change Request on change.

**Restrict Configuration Item to Application CIs**
Again, I was a little unclear if we wanted exclusively Application CIs or all of its inherited types too. I went with the latter, but restricting to *only* Application CIs would be as simple as changing the reference qualifier in the dictionary override to `sys_class_name=cmdb_ci_appl`;

**Hide Impact field for non admins**
I went the ACL route with this instead of a UI policy, since I assumed we never want non admins to view that data. This prevents them from adding the column to their list view and being able to still see the data. I also assumed if we don't want them to see the data, we certainly don't want them editing it, so I created a write ACL for the Impact field as well.

### 6) Understanding of APIs

I used Postman for these tasks. The Postman collection in this repository can be imported and has two REST calls. New user (int_postman) has REST access. Credentials are removed from the collection but will be provided separately.

**External tool to pull Incident data by number**
The GET request retrieves an incident by number. Changing the number after `number=` within the `sysparm_query` parameter will return the desired incident.

**External tool to create Incident**
The POST request creates an incident. Sample data is in the body of the request. `caller_id` is the sys_id of the user int_postman which is stored in the Postman collection's variables.

**Application CI Import Set**
The excel file in this repository contains 10 Application CIs (I used some musicians as Application names). I imported the data to `u_cmdb_ci_application_import`, created a transform map, and transformed to the Application CI table.
