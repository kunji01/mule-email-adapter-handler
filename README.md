# mule-email-adapter-handler

--Share email common flows in Mule projects in adapter-handler way.</br>

•	Catch exceptions globally in an application to send emails by simply adding a VM processor. </br>
•	Send an email in a flow with dynamic subject and content. </br>
•	Log integration activities to monitoring app and/or send emails. </br>
•	Centrally manage Email addresses for individual application without interrupting operations.</br>
•	Changes in the handler only and re-deploy it for all user applications</br>


--Test</br>
. Clone or download this repository to your Anypoint studio</br>
. Open adapter-handler-dv.properties to change the email related properties to yours</br>
. Run it. The default http.port is 8383</br>
. http://localhost:8383/email/address to add your email address for default on all "Email for".</br>
. http://localhost:8383/utilities/email/exception/test</br>
. http://localhost:8383/utilities/email/note/test</br>
. http://localhost:8383/utilities/email/log/test</br>
. check your email inbox</br><br/>

--Use flows in your projects</br>
. In mule-email-adapter-handler<br/>
. Open folder "target" -&gt; mule-email-adapter.jar. Copy this file mule-email-adapter.jar and then drop to your project "target" folder, select mule-email-adapter.jar -&gt; "Build Path" -&gt; "Add to Build Path" <br/>
. Add these lines in your flow "Configuration XML"
```html
<spring:beans>
        <spring:import resource="classpath:utilities-adapter.xml"/>
</spring:beans>
```
. Send exception to email<br/>
```html
<catch-exception-strategy doc:name="Catch Exception Strategy">
    <vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/exception" doc:name="VM" connector-ref="utilitiesAdapter_VM"/>
</catch-exception-strategy>
```

. Send payload to email<br/>
```html
<vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/payload" connector-ref="utilitiesAdapter_VM" doc:name="VM"/>
```

. Send your email<br/>
```html
<set-property propertyName="subject" value="Your Email subject" doc:name="subject"/>
<set-property propertyName="content" value="Your Email content" doc:name="content"/>
<vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/note" doc:name="VM" connector-ref="utilitiesAdapter_VM"/>
```

--How does it work<br>
. VM path is used for your project flow to invoke email adapters<br/>
. Adapter flows request HTTP in handler through http.port 8383<br/>
. Open utilities-test.xml to explore<br/>
 

--Make it yours<br/>
Checkout or download this repository to your Anypoint studio, open flows to make changes. If you need to change adapters, checkout project mule-email-adapter to your
Anypoint. After your changes, export the adapter as a zip without project files, then re-name it to be .jar
copy and drop to a user project and to project mule-email-adapter-handler, select the .jar to "add to build path".</br>



