<!--

    Copyright (c) 2007-2014, Kaazing Corporation. All rights reserved.

-->

<!DOCTYPE html>

   <!--#include virtual="/_header.html"-->

        <div class="main-container">
            <div class="main wrapper clearfix">
            
                <div class="powered-wrapper">
                    <div class="powered-header"></div>
                </div>
                
                <ul class="breadcrumbs clearfix">
                   <li><a href="../../index.html">Home</a></li>
                   <li><a href="../index.html">Documentation</a></li>
           <!-- Use li to add more breadcrumbs -->
                   <li>Checklist: Build C AMQP Clients</li>
                </ul>

                <article>
                  <section>

          <!-- CONTENT GOES HERE -->  

<h1>Checklist: Build C AMQP Clients  ${enterprise.logo}</h1>
<p>This checklist provides the steps necessary to build AMQP clients to communicate with ${gateway.name.long}:</p>

<table width="789" border="0">
  <tr>
    <th scope="col">#</th>
    <th scope="col" width="400">Step</th>
    <th scope="col">Topic or Reference</th>
  </tr>
  <!-- <tr>
  <td>1</td>
    <td>Learn about supported browsers, operating systems, and platform versions.</td>
    <td><a href="../release-notes.html">Release Notes</a></td>
  </tr> -->
  <tr>
        <td>1</td>
    <td>Learn about the ${gateway.name.long} C AMQP client.</td>
    <td><a href="#keglibs">Overview of the ${gateway.name.long} C AMQP Client Library</a></td>
  </tr>
  <tr>
        <td>2</td>
  <td>Learn how to use ${the.gateway} C Client Library and the supported APIs.</td>
  <td><a href="p_dev_c_client_amqp.html">Use the ${gateway.name.long} C AMQP Client Library</a></td>
  </tr>
  <tr>
        <td>3</td>
    <td>Learn how to establish trust between your C client and ${the.gateway} by using client-side TLS/SSL digital certificates.</td>
    <td><a href="../security/p_tls_mutualauth.html">Require Clients to Provide Certificates to ${the.gateway}</a></td>
  </tr>
</table>     

<h2><a name="overview" id="overview"></a>Overview of AMQP 0-9-1</h2>
<p>Advanced Message Queuing Protocol (AMQP) is an open standard for messaging middleware that was originally designed by the financial services industry to provide an interoperable protocol for managing the flow of enterprise messages. To guarantee messaging interoperability, AMQP 0-9-1 defines both a wire-level protocol and a model&mdash;the AMQP Model&mdash;of messaging capabilities.</p>
<p>The AMQP Model defines three main components:</p>
<ol>
  <li><em>Exchange</em>: clients publish messages to an exchange</li>
  <li><em>Queue</em>: clients read messages from a queue</li>
  <li><em>Binding</em>: a mapping from an exchange to a queue</li>
</ol>

<p>An AMQP client connects to an AMQP broker and opens a <em>channel</em>. Once the channel is established, the client can send messages to an exchange and receive messages from a queue. To learn more about AMQP functionality, take a look at the <a href="../guide-amqp.html">Real-Time Interactive Guide to AMQP</a>, an interactive guide that takes you step-by-step through the main features of AMQP version 0-9-1.</p>
<p>For more information about AMQP, visit <a href="http://www.amqp.org" class="code_inline">http://www.amqp.org</a>.</p>


<h2><a name="websocket" id="websocket"></a>WebSocket and AMQP</h2>
<p>${gateway.name.long} enables direct WebSocket communication from the AMQP client to ${the.gateway} and then onto an AMQP broker over TCP. ${the.gateway.cap} radically simplifies C application design by providing the AMQP client libraries for C client technologies.</p>

<p>Using the AMQP client libraries, you can take advantage of the AMQP features, making the C client a first-class citizen in AMQP systems.</p>

<h2><a name="keglibs"></a>Overview of the ${gateway.name.long} C AMQP Client Library</h2>
<p>${gateway.name.long} includes AMQP client libraries that allow clients to subscribe and publish messages to a message broker using AMQP. With the ${gateway.name.long} AMQP client libraries, you can leverage WebSocket in your C client. This WebSocket-enabled C client then communicates between your application and the message broker, as shown in the following figure:</p><br>

        <figure>
                <img src="../images/f-amqp-web-c-client-web.png" width="729" height="207" alt="${gateway.name.long} C AMQP Client">
        <figcaption><strong>Figure: ${gateway.name.long} C AMQP Client Connection</strong></figcaption> 
        </figure>

<h2><a name="setup_server"></a>Starting an AMQP Broker</h2>
 
<p>There are a wide variety of AMQP brokers available that implement different AMQP versions. For example, RabbitMQ, Apache Qpid, OpenAMQ, Red Hat Enterprise MRG, UMQ, and Zyre. If you do not have an AMQP broker installed yet, you can use the Apache Qpid AMQP broker included in ${the.gateway} download package, that supports AMQP version 0-9-1.</p>

<p>To set up and start the Apache Qpid broker on your system, perform the steps described in <a href="../about/setup-guide.html">Setting Up ${gateway.name.long}</a>.</p>
<p><span class="note"><strong>Note</strong>: The AMQP client libraries are compatible with AMQP version 0-9-1. Refer your AMQP broker documentation for information about certified AMQP versions.</span></p>

<p>For information on integrating with RabbitMQ, see <a href="../integration-amqp/p_amqp_integrate_rabbitmq.html">Integrate RabbitMQ Messaging</a>.</p>

<h2><a name="demo" id="demo"></a>Taking a Look at the AMQP Demo Files</h2>
<p>Take a look at demonstration files built with the C version of the AMQP client library: <span class="uri"><em>GATEWAY_HOME</em>/demo/c/src/amqp</span></p>
<!-- <ol>
<li>Start the ${gateway.name.long} and Apache Qpid as described in <a href="../about/setup-guide.html">Setting Up Kaazing WebSocket Gateway</a>.</li>
<li>In a browser, navigate to <a href="http://localhost:8001/demo/amqp/c/?d=amqp-c" class="code_inline">http://localhost:8001/demo/amqp/c/?d=amqp-c</a>.
From this page, you can navigate to additional AMQP demos for different client technologies (Java, Silverlight, .NET Framework, C, and Adobe Flash and Flex).</li>
</ol> -->

<h2>Next Step</h2>
<p><a href="p_dev_c_client_amqp.html">Use the ${gateway.name.long} C AMQP Client Library</a></p>

                  </section>
                </article>

            </div> <!-- #main -->
        </div> <!-- #main-container -->

 <!--#include virtual="/_footer.html"-->

</html>
