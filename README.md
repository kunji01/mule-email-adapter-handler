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
. In mule-email-adapter-handler, open folder "target" -&gt; mule-email-adapter.jar. Copy this file mule-email-adapter.jar and then drop to your project "target" folder, select mule-email-adapter.jar -&gt; "Build Path" -&gt; "Add to Build Path" <br/>
. Add these lines in your "Configuration XML" <br/>

<blockquote>
  <pre>
    <code>
		<spring:beans>
		        <spring:import resource="classpath:utilities-adapter.xml"/>
		</spring:beans>
	</code>
  </pre>
</blockquote>

<br/>

. Send exception to email<br/>
<blockquote>
  <pre>
    <code>
        <catch-exception-strategy doc:name="Catch Exception Strategy">
            <vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/exception" doc:name="VM" connector-ref="utilitiesAdapter_VM"/>
            <set-payload value="#['You test /utilities/email/exception/test/vm \n&lt;br&gt; Exception throwed, check your email to find exception content too. \n' + exception]" doc:name="Set Payload"/>
        </catch-exception-strategy>
	</code>
  </pre>
</blockquote>

. Send payload to email<br/>

<![CDATA[
<vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/payload" connector-ref="utilitiesAdapter_VM" doc:name="VM"/>
]]>


<![CDATA[
    <flow name="/utilities/email/exception/test">
        <http:listener config-ref="sharedHTTP_Listener" path="/utilities/email/exception/test" allowedMethods="GET" doc:name="/utilities/exception/test"/>
        <set-payload value="Kun Ji's test content as a payload." doc:name="Set test Payload"/>
        <scripting:transformer doc:name="throw exception">
            <scripting:script engine="JavaScript"><![CDATA[throw 'Throw a exception to test utilities-adapter VM \n';]]></scripting:script>
        </scripting:transformer>
        <catch-exception-strategy doc:name="Catch Exception Strategy">
            <set-property propertyName="appname" value="#[app.name]" doc:name="Property appname"/>
            <vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/exception" doc:name="VM" connector-ref="utilitiesAdapter_VM"/>
            <set-payload value="#['You test /utilities/email/exception/test/vm \n&lt;br&gt; Exception throwed, check your email to find exception content too. \n' + exception]" doc:name="Set Payload"/>
        </catch-exception-strategy>
    </flow>
    <flow name="/utilities/email/log/test">
        <http:listener config-ref="sharedHTTP_Listener" path="/utilities/email/log/test" doc:name="HTTP"/>
        <set-payload value="Test log payload to message email" doc:name="Set Payload"/>
        <vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/log" doc:name="VM" connector-ref="utilitiesAdapter_VM"/>
    </flow>
    <flow name="/utilities/log/test">
        <http:listener config-ref="sharedHTTP_Listener" path="/utilities/log/test" doc:name="HTTP"/>
        <set-payload value="Test log payload" doc:name="Set a Payload"/>
        <vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/log" doc:name="VM" connector-ref="utilitiesAdapter_VM"/>
    </flow>
    <flow name="/utilities/email/payload/test">
        <http:listener config-ref="sharedHTTP_Listener" path="/utilities/email/payload/test" doc:name="HTTP"/>
        <set-property propertyName="flowname" value="#[context:serviceName]" doc:name="Property flowname"/>
        <set-payload value="send payload to email - test

Remmeber adding the original flow name to content

context:serviceName = #[context:serviceName]" doc:name="Set Payload"/>
        <vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/payload" connector-ref="utilitiesAdapter_VM" doc:name="VM"/>
    </flow>
    <flow name="/utilities/email/note/test">
        <http:listener config-ref="sharedHTTP_Listener" path="/utilities/email/note/test" doc:name="HTTP"/>
        <set-property propertyName="subject" value="Email subject test" doc:name="subject"/>
        <set-payload value="#['subject =' + message.outboundProperties.subject +'\n\n' + payload]" doc:name="Set Payload"/>
        <vm:outbound-endpoint exchange-pattern="one-way" path="/utilities/email/note" doc:name="VM" connector-ref="utilitiesAdapter_VM"/>
    </flow>
 ]]

--Make it yours<br/>
Checkout or download this repository to your Anypoint studio, open flows to make changes. If you need to change adapters, checkout project mule-email-adapter to your
Anypoint. After your changes, export the adapter as a zip without project files, then re-name it to be .jar
copy and drop to a user project and mule-email-adapter-handler, select the .jar to "add to build path".</br>

--Connection between adapter and handler. Open test flows to find:</br>
Queue path</br>
Http request path and port</br>


