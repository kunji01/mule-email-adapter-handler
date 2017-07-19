# mule-email-adapter-handler

--Share email common flows in Mule projects in adapter-handler way.</br>

•	Catch exceptions globally in an application to send emails by simply adding a VM processor. </br>
•	Email a notification to emails by dynamically giving subject and content. </br>
•	Log integration activities to monitoring app and/or send emails. </br>
•	Centrally manage Email addresses for individual application without interrupting operations.</br>

--Test it</br>
. Check this repository to your Anypoint studio</br>
. Open adapter-handler-dv.properties to change email related properties to yours</br>
. Run it. The default http.port is 8383</br>
. http://localhost:8383/email/address to add your email address for default on all "Email for".</br>
. http://localhost:8383/utilities/email/exception/test</br>
. http://localhost:8383/utilities/email/note/test</br>
. http://localhost:8383/utilities/email/log/test</br>
. check your email inbox</br>

--Make it yours
Open the flows to make changes. If you need to change adapters, checkout mule-email-adapter to your
Anypoint. After your changes, export the adapter as a zip without project files, then re-name it to be .jar
copy and drop to a user project and mule-email-adapter-handler, select the .jar to "add to build path".</br>v

--Connection between adapter and handler. Open test flows to find:</br>
Queue path</br>
Http request path and port</br>


