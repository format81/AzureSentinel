# Azure Sentinel - SOAR Demo - ITASEC21
authors: Antonio Formato & Rebecca Travasi

Developed for ITASEC21 conference April 8th 2021: www.itasec.it

This playbook will enable you to automatically pursue a few remediation, enrichment and custom actions given a specific incident trigger in Sentinel:
The playbook can be run automatically or manually.
Starting from a responde triggered by our alert in Azure Sentinel, we initialize a string variable related to VirusTotal profile to enable the service. In the next step we specificy our Azure subscription and other data to Get our incident.
The flow continues on the bottom left hand-side of the playbook, we connected our dev ServiceNow profile to automatically open a ticket in the third party service. Moving from left to right, we get the entity (and thus the account) related to the incident and for each account in the incident we post a message to our internal Teams channel notifying the SOC team that a new alert has been triggered. The flow then sends an approval email to manager to block the sign-in of the user involved in the incident. It waits until the manager approves blocking and if the request is approved it changes the value to "No" related to Account Enabled value. If the request is rejected nothing happens to the user.
Moving to the right, we uploaded a csv file in the Watchlist section of Azure Sentinel with a column for VIP user (Y,N) and the flow checks that file for VIP users. If this set is not empty in the query (namely, if the user related to the incident is also a VIP user), the severity of the incident gets upgraded to High. On the last section of our workflow in the bottom right hand-side we enrich the alert by connecting our VirusTotal service and GreyNoise service (3rd party open source services). We get our unknown IP and call out for these services to enrich the alert posting comments if they have further info on the given IP address.
The results coming out of this playbook are multiple: SOC team has an iipen ticket in ServiceNow, it is notified of the alert on Teams and the manager is aware of a risky situation. Furthermore, the alert is enriched with useful info (country of unknown IP, malicious/or not, etc..) and Severity is upgraded to High thus can be prioritized by SOC analysts. Moreover, Roger our user, is not able to access enterprise resources until SOC team has completed operations.

To upload this playbook in Azure Sentinel please follow the below instructions:
- From the Azure portal menu, in the search box, type Template, and then select Templates
- Select +Add and give it a Name and Description
- copy and paste the json file in the previous folder in the ARM template layout.
- click add
- your logic app is created and ready to be deployed
- From the Azure portal menu, in the search box, type Sentinel 
- click on Azure Sentinel and then in the left hand-side menu click automation 
- click on Add
- Add new playbook
- on the bottom select download custom template
- fill in the requested fields
- at completion you'll see it available among the playbooks

Thank you!

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fformat81%2FAzureSentinel%2Fmaster%2FPlaybooks%2FITASEC21%2FITASEC21_SOAR_template.json" target="_blank">
    <img src="https://aka.ms/deploytoazurebutton"/>
</a>
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fformat81%2FAzureSentinel%2Fmaster%2FPlaybooks%2FITASEC21%2FITASEC21_SOAR_template.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazuregov.png"/>
</a>

**Additional Post Install Notes:**
