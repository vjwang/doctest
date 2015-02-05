<h1>Use the ${gateway.name.long} C AMQP Client Library ${enterprise.logo}</h1>
<p>In this procedure, you will learn how to use the ${gateway.name.long} C AMQP Client Library and the supported APIs:</p>

<ul>
  <li>Build the client using the Eclipse IDE as described in <a href="#proc">To Use the ${gateway.name.long} C AMQP Client Library</a>.</li>
  <li>See examples for producing and consuming messages, including OpenSSL: <a href="#examples">Examples</a>.</li>
</ul>

<h2><a name="_"></a>Before You Begin</h2>
<p>This procedure is part of <a href="o_dev_c_amqp.html">Checklist: Build C AMQP Clients</a>:</p>
<p>
        <ol>
        <li><a href="o_dev_c_amqp.html#keglibs">Overview of the ${gateway.name.long} AMQP Client Library</a></li>
        <li><strong>Use the ${gateway.name.long} C AMQP Client Library</strong></li>
        <li><a href="../security/p_tls_mutualauth.html">Require Clients to Provide Certificates to ${the.gateway}</a></li>
        </ol>
</p>

<h2>Components and Tools</h2>
<p>Before you get started, review the components and tools used to build the AMQP C client in this procedure. The AMQP broker used in this procedure can be switched with another broker that supports AMQP 0-9-1.</p>

<table border="0">
  <tr>
    <th scope="col">Component or Tool</th>
    <th scope="col">Description</th>
    <th scope="col">Location</th>
  </tr>
  <tr>
    <td>${gateway.name.long}</td>
    <td>For a detailed description of the ${gateway.name.long}, see <a href="http://kaazing.com/products/editions/kaazing-websocket-gateway-amqp">http://kaazing.com/products/editions/kaazing-websocket-gateway-amqp</a>.</td>
    <td><a href="http://developer.kaazing.com/downloads/amqp-edition-download/">http://developer.kaazing.com/downloads/amqp-edition-download/</a></td>
  </tr>
  <tr>
    <td>AMQP broker</td>
    <td>The full ${gateway.name.long} download (<strong>Gateway + Documentation + Demos</strong>) includes the Apache Qpid broker.</td>
    <td>Qpid_HOME: The installer creates the default Qpid_HOME destination in <span class="uri">C:\Program Files\Kaazing\amqp\version\qpid</span> (Windows) or <span class="uri">/usr/share/kaazing/amqp/version/</span> (Linux). The full ${gateway.name.long} download contains the Apache Qpid broker folder in its root directory.</td>
  </tr>
  <tr>
    <td>${gateway.name.long} AMQP C Client library files</td>
    <td>The C header files (.h) and the shared object files (.so) that make up the ${gateway.name.long} AMQP C Client library.</td>
    <td><p>The header and library files are contained in OS-specific gzip files located in <span class="uri"><em>GATEWAY_HOME</em>/lib/client/c</span>. Extract the gzip file for your operating system to see the header and library files.</p>
                        <p>AMQP header files (.h) and library (.so):</p>
<p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/AMQP-0-9-1/include</span></p>
<p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/AMQP-0-9-1/lib</span></p>

<p>WebSocket header files (.h) and library (.so):</p>
<p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/WebSocket/include</span></p>
<p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/WebSocket/lib</span></p>

<p><em>OS</em> represents the files for your operating system. For some operating systems, there might be subfolders, such as: <span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/Ubuntu-precise-64bit</span></p></td>
  </tr>
  <tr>
    <td>CDT (C/C++ Development Tooling) plugin for Eclipse or Eclipse IDE for C/C++ Developers</td>
    <td><p>The Eclipse IDE is used to develop, compile, and test the AMQP C client. You have two options when configuring Eclipse:</p>
        <ul>
          <li>If you have an installed Eclipse package (for example, Eclipse for Java Developers), you can install the CDT plug-in as described here: <a href="http://www.eclipse.org/cdt/downloads.php" title="CDT Downloads">http://www.eclipse.org/cdt/downloads.php</a>.</li>
          <li>If you do not have an Eclipse package, you can download Eclipse IDE for C/C++ Developers from <a href="http://www.eclipse.org/downloads" title="Eclipse Downloads">http://www.eclipse.org/downloads</a>, and install the downloaded files.</li>
        </ul>
    </td>
    <td><p>Eclipse IDE for C/C++ Developers: <a href="http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/keplersr2" title="Eclipse IDE for C/C++ Developers | Packages">http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/keplersr2</a></p>

    <p>CDT (C/C++ Development Tooling) plugin: <a href="http://www.eclipse.org/cdt/downloads.php" title="CDT Downloads">http://www.eclipse.org/cdt/downloads.php</a></p></td>
  </tr>
        <tr>
                <td>Security with Challenge Handlers</td>
                <td>Authenticating your C client involves implementing a challenge handler to respond to authentication challenges from ${the.gateway}. If your challenge handler is responsible for obtaining user credentials, then you will also need to implement a login handler.</td>
                <td>For examples, see <a href="#listen">amqp_listen_websocket.c</a> and <a href="#send">amqp_sendstring_websocket.c</a> and look at the code:<br>
                <pre class="auto-links: false; brush: js; toolbar: false;">
                        kaazing_challenge_handler_t* setup_demo_challenge_handler() {
                                return kaazing_basic_challenge_handler_new(&amp;getcredential);
                        }</pre>
                </td>
        </tr>
  <tr>
    <td>OpenSSL</td>
    <td>OpenSSL is required by the C client to connect with ${the.gateway} securely over TLS/SSL. See the OpenSSL examples: <a href="#sendssl">amqp_sendstring_websocket_ssl.c</a>, and <a href="#sslcallback">amqp_sendstring_websocket_ssl_callback.c</a>.</td>
    <td>OpenSSL is typically installed on most operating systems; however, if you are using a Linux distribution, you might need to install it. In most cases, you can simply update and upgrade your Linux distribution (<code>sudo apt-get update &amp;&amp; sudo apt-get upgrade</code>), or install the OpenSSL development package (<code>sudo apt-get install openssl libssl-dev</code>). For more information, see <span class="uri">http://www.openssl.org/support/faq.html</span> and <span class="uri">http://www.openssl.org/source/</span>.</td>
  </tr>
</table>

<!-- <p><span class="note"><b>Note:</b> Learn about supported browsers, operating systems, and platform versions in the ${certification.matrices.inline}.</span></p>   -->

<h2><a name="proc"></a>To Use the ${gateway.name.long} C AMQP Client Library</h2>
<p>Use the following steps to build and run an AMQP C client using the ${gateway.name.long} AMQP C library.</p>
<p>
<ol>
<li><a name="setup_dev_env"></a><p>Setting Up Your Development Environment.</p>
<p>To develop applications using the ${gateway.name.long} AMQP client libraries, you must configure ${the.gateway} to communicate with an AMQP broker.</p>
        <p><span class="note"><b>Note:</b><b></b> If you have the ${gateway.name.long} running on <span class="code_inline">localhost</span> and if you have an AMQP broker running on <span class="code_inline">localhost</span> at the default AMQP port <span class="code_inline">5672</span>, you do not have to configure anything to see the AMQP demos and the interactive AMQP guide.</span></p>
        <p>The following is an example of the default configuration element for the AMQP service in the ${gateway.name.long} bundle, as specified in the configuration file <span class="code_inline"><em>GATEWAY_HOME</em>/conf/gateway-config.xml</span>:</p>
<pre class="auto-links: false; brush: xml; toolbar: false;">
&lt;!-- Proxy service to AMQP server --&gt;
&lt;service&gt;
  &lt;accept&gt;ws://localhost:8001/amqp&lt;/accept&gt;
  &lt;connect&gt;tcp://localhost:5672&lt;/connect&gt;

  &lt;type&gt;amqp.proxy&lt;/type&gt;
              
  &lt;cross-site-constraint&gt;
    &lt;allow-origin&gt;http://localhost:8001&lt;/allow-origin&gt;
  &lt;/cross-site-constraint&gt;
&lt;/service&gt;</pre>

<p>In this case, the service is configured to accept WebSocket AMQP requests from the browser at <span class="code_inline">ws://localhost:8001/amqp</span> and proxy those requests to a locally installed AMQP broker (<span class="code_inline">localhost</span>) at port <span class="code_inline">5672</span>.</p>

<p>To configure ${the.gateway} to accept WebSocket requests at another URL or to connect to a different AMQP broker, you can edit <span class="code_inline"><em>GATEWAY_HOME</em>/conf/gateway-config.xml</span>, update the value for the <span class="code_inline">accept</span> and <span class="code_inline">connect</span> elements, and restart ${the.gateway}. For example, the following  configuration configures ${the.gateway} to accept WebSocket AMQP requests at <span class="code_inline">ws://www.example.com:80/amqp</span> and proxy those requests to an AMQP broker (<span class="code_inline">amqp.example.com</span>) on port <span class="code_inline">5672</span>.</p>

<pre class="auto-links: false; brush: xml; toolbar: false;">
&lt;!-- Proxy service to AMQP server --&gt;
&lt;service&gt;
        &lt;accept&gt;ws://www.example.com:80/amqp&lt;/accept&gt;
        &lt;connect&gt;tcp://amqp.example.com:5672&lt;/connect&gt;

        &lt;type&gt;amqp.proxy&lt;/type&gt;
&lt;/service&gt;</pre>

</li>
<li><a name="using_lib_javascript" id="using_lib_javascript"></a><p>Review the common C AMQP programming steps.</p>
  <p>You can either build a single application that both publishes and consumes messages, or create two different applications to handle each action. The demo located included in the ${gateway.name.long} distrubtion shows a single application that handles both actions. You can view the source code for this demo in <span class="uri"><em>GATEWAY_HOME</em>/demo/c</span>. Refer to the AMQP Client C API documentation for the complete list of all the AMQP command and callback functions.</p>
  <p>The common C AMQP programming steps are:</p>
        <ol type="a">
    <li>Download, install and configure the Eclipse IDE.</li>
    <li>Create an Eclipse C project.</li>
    <li>Import AMQP C client libraries.</li>
    <li>Create the source file for connecting to the AMQP broker via ${the.gateway} and for consuming messages.</li>
    <li>Create the source file for publishing.</li>
        </ol>
</li>

<li><p>Download and install the Eclipse IDE for C/C++ Developers. This topic uses the Kepler package of Eclipse.</p> 
<p>Download the Eclipse IDE for C/C++ Developers from here: <a href="https://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/keplersr2">https://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/keplersr2</a></p>
<p>If you have an installed Eclipse package (for example, Eclipse for Java Developers), you can install the CDT plug-in as described here: <a href="http://www.eclipse.org/cdt/downloads.php" title="CDT Downloads">http://www.eclipse.org/cdt/downloads.php</a></p></li>

<li><p>Create an Eclipse C project.</p>
  <ol type="a">
  <li>Launch Eclipse and select a workspace.</li>
  <li>In the welcome screen, click the Workbench icon.</li>
  <li>Right-click in the empty space in <strong>Project Explorer</strong>, click <strong>New</strong>, and then click <strong>C Project</strong>. The <strong>C Project</strong> dialog appears.</li>
  <li>In <strong>Project</strong> name, enter <strong>amqp-c-client-demo</strong>.</li>
  <li>In <strong>Project</strong> type, expand the <strong>Executable</strong> folder and select <strong>Empty Project</strong>.</li>
  <li>In <strong>Toolchains</strong>, select the compiler for your OS platform.</li>
  <li>Click <strong>Finish</strong>. The new project appears.</li>
  </ol>
</li>

<li><p>Add AMQP C header files.</p>
  <ol type="a">
  <li>Right-click the <strong>amqp-c-client-demo</strong> project in <strong>Project Explorer</strong> and click <strong>Properties</strong>.</li>
  <li>In the <strong>Properties</strong> window, click <strong>Settings</strong> under the <strong>C/C++ Build</strong> heading.</li>
  <li>In the <strong>Settings</strong> area, click <strong>Tool Settings</strong>.</li>
  <li><p>Click <strong>Includes</strong> under the compiler heading, for example <strong>GCC C Compiler</strong> (this heading might be different depending on the compiler you selected when creating the project).</p>
  
    <figure>
      <img src="../images/f-amqp-web-c-client-web-includes.png">
    <figcaption><strong>Figure: C Project Includes Compiler Heading</strong></figcaption>  
    </figure>
  </li>
  <li>In <strong>Include paths</strong>, click the <strong>Add</strong> icon. The <strong>Add directory path</strong> dialog appears.</li>
  <li><p>Click <strong>File System</strong>, and then navigate to the ${gateway.name.long} C header folders and click Open:</p>
<p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/AMQP-0-9-1/include</span></p>
<p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/WebSocket/include</span></p></li>
  <li>Click <strong>Apply</strong>. The libraries appear in the project.</li>
  </ol>
</li>

<li><p>Add AMQP C shared object files.</p>
  <ol type="a">
    <li><p>In the <strong>Properties</strong> window for the project, click <strong>Libraries</strong> under the linker heading, for example, <strong>GCC C Linker</strong>.</p>
        <figure>
          <img src="../images/f-amqp-web-c-client-web-libs.png">
        <figcaption><strong>Figure: C Project Libraries Settings</strong></figcaption>  
        </figure>
    </li>
    <li>In <strong>Libraries</strong>, click the <strong>Add</strong> icon. The <strong>Enter Value</strong> dialog appears.</li>
    <li><p>Enter the name of the libraries, <strong>websocket</strong> and <strong>rabbitmq</strong>, and click <strong>OK</strong>.</p>
    </li>
    <li>In <strong>Library search path</strong>, click the <strong>Add</strong> icon. The <strong>add directory path</strong> dialog appears.</li>
    <li><p>Click <strong>File system</strong>, navigate to the location of each of the library folders and click <strong>OK</strong>:</p>
    <p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/AMQP-0-9-1/lib</span></p>
    <p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/WebSocket/lib</span></p>
        </li>
    <li>Click <strong>OK</strong> to close the <strong>add directory path</strong> dialog.</li>
    <li>Click <strong>OK</strong> to close the <strong>Properties</strong> window.</li>
  </ol>
</li>

<li><p>Create the source file for the client to consume and publish messages.</p>
    <ol type="a">
      <li>In <strong>Project Explorer</strong>, right-click <strong>amqp-c-client-demo</strong>, click <strong>New</strong>, and then click <strong>Source File</strong>.</li>
      <li>In the <strong>Source</strong> file field, enter the name <strong>amqp_publish_consume_websocket.c</strong>.</li>
      <li>In <strong>Template</strong>, ensure that <strong>Default C source template</strong> is selected, and click <strong>Finish</strong>.</li>
    </ol>
</li>

<li><p>Add the C code in <a href="#publish_consume">amqp_publish_consume_websocket.c</a> to <strong>amqp_publish_consume_websocket.c</strong>. The code will create a connection to ${the.gateway}, open and bind to a channel with the AMQP broker and log in, publish and consume messages from the broker, and close the connection.</p>

<span class="note"><b>Note:</b> The example uses the files <strong>utils.c</strong> and <strong>utils.h</strong> located in the <span class="uri"><em>GATEWAY_HOME</em>/demo/c/src</span>. You should load those files into your project before building the project using the example files.</span>
</li>

<li>From the <strong>File</strong> menu, click <strong>Save All</strong>.</li>

<li><p>Build the Eclipse project.</p>
  <ol type="a">
    <li>Right-click the <strong>amqp-c-client-demo</strong> project, and then click <strong>Build Project</strong>.</li>
  </ol>
</li>

<li><p>Configure the launch configuration for the AMQP C client.</p>
  <ol type="a">
    <li>In the <strong>Properties</strong> window for the project, click the <strong>Run/Debug Settings</strong> heading.</li>
    <li>Click <strong>New</strong>.</li>
    <li><p>In the <strong>Select Configuration Type</strong> dialog, click <strong>C/C++ Application</strong>, and then click <strong>OK</strong>. The <strong>Edit Configuration</strong> window appears.</p>
      <figure>
        <img src="../images/f-amqp-web-c-client-web-config.png">
      <figcaption><strong>Figure: Environment Variables</strong></figcaption>  
      </figure>
    </li>
    <li>In the <strong>Name</strong> field, enter <strong>amqp-c-client-demo</strong>.</li>
    <li>Click the <strong>Environment</strong> tab.</li>
    <li>Click <strong>New</strong> to create a new variable. The <strong>New Environment Variable</strong> window appears.</li>
    <li>In <strong>Name</strong>, enter <strong>LD_LIBRARY_PATH</strong>.</li>
    <li><p>In <strong>Value</strong>, enter the paths to the shared object files (the two paths may be separated by a colon):</p>
    <p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/AMQP-0-9-1/lib</span></p>
    <p><span class="uri"><em>GATEWAY_HOME</em>/lib/client/c/<em>OS</em>/WebSocket/lib</span></p>
        </li>
    <li>Click <strong>OK</strong>.</li>
    <!-- <li>Click <strong>OK</strong> to exit the <strong>Edit Configuration</strong> window.</li> -->
    <li>Click the <strong>Arguments</strong> tab.</li>
    <li>Enter the following argument: <span class="uri">ws://localhost:8001/amqp amq.direct test</span><br>
      <figure>
        <img src="../images/f-amqp-web-c-client-web-program-args.png">
      <figcaption><strong>Figure: Program Arguments</strong></figcaption>  
      </figure>
    </li>
    <li>Click <strong>OK</strong>.</li>
    <li>Click <strong>OK</strong> to close the <strong>Properties</strong> window.</li>
  </ol>
</li>

<li>Start the AMQP broker as described in <strong>How do I start Apache Qpid?</strong> in ${setting.up.inline}.</li>

<li>Start ${the.gateway} as described in <strong>How do I start and stop the Gateway?</strong> in ${setting.up.inline}.</li>

<li>Run your AMQP C client program. In Eclipse, right-click the <strong>amqp-c-client-demo</strong> project, and click <strong>Run</strong>. The client will perform all of its functions in the Console.</li>

</ul>
 
<h2><a name="examples"></a>Examples</h2>
<p>The following examples demonstrate how to:</p>
<ol>
<li>Import the libraries.</li>
<li>Create the WebSocket connection to the Gateway.</li>
<li>Connect and log into an AMQP broker.</li>
<li>Create channels.</li>
<li>Declare an exchange.</li>
<li>Declare a queue.</li>
<li>Bind an exchange to a queue.</li>
<li>Consume messages.</li>
<li>Publish messages.</li>
<li>Handle Exceptions.</li>
<li>Use OpenSSL with your C client.</li>
</ol>

<p>Examples:</p>
<ul>
  <li><a href="#listen">amqp_listen_websocket.c</a> - demonstrates how to consume AMQP messages.</li>
  <li><a href="#send">amqp_sendstring_websocket.c</a> - demonstrates how to publish AMQP messages.</li>
  <li><a href="#publish_consume">amqp_publish_consume_websocket.c</a> - demonstrates how to publish and consume AMQP messages on a single connection.</li>
  <li><a href="#sendssl">amqp_sendstring_websocket_ssl.c</a> - demonstrates how to publish AMQP messages and verify an OpenSSL client certificate against a Certificate Authority (CA).</li>
  <li><a href="#sslcallback">amqp_sendstring_websocket_ssl_callback.c</a> - demonstrates how to publish AMQP messages and verify an OpenSSL client certificate against a local private key for client authentication.</li>
</ul>

<span class="note"><b>Note:</b> These examples use the files <strong>utils.c</strong> and <strong>utils.h</strong> located in the <span class="uri"><em>GATEWAY_HOME</em>/demo/c/src</span>. You should load those files into your project before building the project using one of these examples.</span>

<h3><a name="listen"></a>amqp_listen_websocket.c</h3>
<p>The following example demonstrates how to consume AMQP messages.</p>
<pre class="auto-links: false; brush: js; toolbar: false;">
#include &lt;stdlib.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

#include &lt;stdint.h&gt;
#include &lt;amqp_websocket.h&gt;
#include &lt;amqp.h&gt;
#include &lt;amqp_framing.h&gt;

#include &lt;assert.h&gt;

#include &quot;utils.h&quot;

/* The callback function which is invoked when authorization or 
   revalidation challenge is received from the gateway 
*/
credential_t* getcredential(const char* challenge) {
        char buf[80];
        credential_t* credential = (credential_t*)malloc(sizeof(credential_t));
        puts(&quot;Enter username:&quot;);
        scanf(&quot;%s&quot;, buf);
        credential-&gt;username = strdup(buf);
        puts(&quot;Enter password:&quot;);
        scanf(&quot;%s&quot;, buf);
        credential-&gt;password = strdup(buf);
        return credential;
}

kaazing_challenge_handler_t* setup_demo_challenge_handler() {
        return kaazing_basic_challenge_handler_new(&amp;getcredential);
}

int main(int argc, char const *const *argv)
{
  char const *url;
  int status;
  char const *exchange;
  char const *bindingkey;
  amqp_socket_t *socket = NULL;
  amqp_connection_state_t conn;

  amqp_bytes_t queuename;

  if (argc &lt; 4) {
    fprintf(stderr, &quot;Usage: amqp_listen_websocket url exchange bindingkey\n&quot;);
    fprintf(stderr, &quot;Example: amqp_sendstring_websocket ws://localhost:8001/amqp amq.direct test\n&quot;);
    return 1;
  }

  url = argv[1];
  exchange = argv[2];
  bindingkey = argv[3];

  /* Initialize AMQP connection object */
  conn = amqp_new_connection();

  /*
   * Initialize underlying transport object
   * We are using WebSocket as a transport protocol for AMQP messaging
   */
  socket = amqp_websocket_new(conn);
  if (!socket) {
    die(&quot;creating WebSocket&quot;);
  }

  /* Setup challenge handler to respond to authorization and revalidation challenge */
  /* This should be done before establishing the WebSocket connection */
  /* First retrieve the websocket object */
  websocket_t *ws = amqp_websocket_get(socket);
  kaazing_challenge_handler_t* challengeHandler = setup_demo_challenge_handler();
  
  /* Inject challenge handler to the websocket object*/
  websocket_set_challenge_handler(ws, challengeHandler);

  /* Establish WebSocket connection */
  status = amqp_websocket_open(socket, url);
  if (status) {
    die(&quot;opening WebSocket connection&quot;);
  }

  /* Establish AMQP connection against the backend broker */
  die_on_amqp_error(amqp_login(conn, &quot;/&quot;, 0, AMQP_DEFAULT_FRAME_SIZE, 0, AMQP_SASL_METHOD_PLAIN, &quot;guest&quot;, &quot;guest&quot;),
                    &quot;Logging in&quot;);

  /* Open Channel
   * To get the status of call to open channel, amqp_get_rpc_reply should be used
   * amqp_get_rpc_reply() returns the most recent amqp_rpc_reply_t instance corresponding
   * to such an API operation for the given connection.
   */
  amqp_channel_open(conn, 1);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Opening channel&quot;);

  {
        /* Declare Queue
         * In this case we are providing empty name that results in server creating a unique
         * queue name and sending it to the client
         */
    amqp_queue_declare_ok_t *r = amqp_queue_declare(conn, 1, amqp_empty_bytes, 0, 0, 0, 1,
                                 amqp_empty_table);
    die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Declaring queue&quot;);

    /* Get the queue name sent by the server */
    queuename = amqp_bytes_malloc_dup(r-&gt;queue);
    if (queuename.bytes == NULL) {
      fprintf(stderr, &quot;Out of memory while copying queue name&quot;);
      return 1;
    }
  }

  /* Bind queue to an exchange */
  amqp_queue_bind(conn, 1, queuename, amqp_cstring_bytes(exchange), amqp_cstring_bytes(bindingkey),
                  amqp_empty_table);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Binding queue&quot;);

  /* Start a queue consumer.
   * This method asks the server to start a &quot;consumer&quot;, which is a transient request
   * for messages from a specific queue.
   */
  amqp_basic_consume(conn, 1, queuename, amqp_empty_bytes, 0, 1, 0, amqp_empty_table);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Consuming&quot;);

  {
    while (1) {
      amqp_rpc_reply_t res;
      amqp_envelope_t envelope;

      amqp_maybe_release_buffers(conn);

      /* Wait for and consume a message
       * The function waits for a basic.deliver method on any channel, upon receipt of
       * basic.deliver it reads that message, and returns.
       */
      res = amqp_consume_message(conn, &amp;envelope, NULL, 0);

      if (AMQP_RESPONSE_NORMAL != res.reply_type) {
        break;
      }

      printf(&quot;Delivery %u, exchange %.*s routingkey %.*s\n&quot;,
             (unsigned) envelope.delivery_tag,
             (int) envelope.exchange.len, (char *) envelope.exchange.bytes,
             (int) envelope.routing_key.len, (char *) envelope.routing_key.bytes);

      if (envelope.message.properties._flags &amp; AMQP_BASIC_CONTENT_TYPE_FLAG) {
        printf(&quot;Content-type: %.*s\n&quot;,
               (int) envelope.message.properties.content_type.len,
               (char *) envelope.message.properties.content_type.bytes);
      }

      printf(&quot;Message: %.*s\n&quot;,
                     (int) envelope.message.body.len,
                     (char *) envelope.message.body.bytes);

      amqp_destroy_envelope(&amp;envelope);
    }
  }

  die_on_amqp_error(amqp_channel_close(conn, 1, AMQP_REPLY_SUCCESS), &quot;Closing channel&quot;);
  die_on_amqp_error(amqp_connection_close(conn, AMQP_REPLY_SUCCESS), &quot;Closing connection&quot;);
  die_on_error(amqp_destroy_connection(conn), &quot;Ending connection&quot;);

  return 0;
}
</pre>


<h3><a name="send"></a>amqp_sendstring_websocket.c</h3>
<p>The following example demonstrates how to publish AMQP messages.</p>

<pre class="auto-links: false; brush: js; toolbar: false;">
#include &lt;stdlib.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

#include &lt;stdint.h&gt;
#include &lt;amqp_websocket.h&gt;
#include &lt;amqp.h&gt;
#include &lt;amqp_framing.h&gt;

#include &quot;utils.h&quot;

/* The callback function which is invoked when authorization or 
   revalidation challenge is received from the gateway 
*/
credential_t* getcredential(const char* challenge) {
        char buf[80];
        credential_t* credential = (credential_t*)malloc(sizeof(credential_t));
        puts(&quot;Enter username:&quot;);
        scanf(&quot;%s&quot;, buf);
        credential-&gt;username = strdup(buf);
        puts(&quot;Enter password:&quot;);
        scanf(&quot;%s&quot;, buf);
        credential-&gt;password = strdup(buf);
        return credential;
}

kaazing_challenge_handler_t* setup_demo_challenge_handler() {
        return kaazing_basic_challenge_handler_new(&amp;getcredential);
}

int main(int argc, char const *const *argv)
{
  char const *url;
  int status;
  char const *exchange;
  char const *routingkey;
  char const *messagebody;
  amqp_socket_t *socket = NULL;
  amqp_connection_state_t conn;

  if (argc &lt; 5) {
    fprintf(stderr, &quot;Usage: amqp_sendstring_websocket url exchange routingkey messagebody\n&quot;);
    fprintf(stderr, &quot;Example: amqp_sendstring_websocket ws://localhost:8001/amqp amq.direct test \&quot;Hello World\&quot; \n&quot;);
    return 1;
  }

  url = argv[1];
  exchange = argv[2];
  routingkey = argv[3];
  messagebody = argv[4];
  
  /* Initialize AMQP connection object */
  conn = amqp_new_connection();
  
  /*
   * Initialize underlying transport object
   * We are using WebSocket as a transport protocol for AMQP messaging
   */
  socket = amqp_websocket_new(conn);
  if (!socket) {
    die(&quot;creating WebSocket&quot;);
  }
  
  /* Setup challenge handler to respond to authorization and revalidation challenge */
  /* This should be done before establishing the WebSocket connection */
  /* First retrieve the websocket object */
  websocket_t *ws = amqp_websocket_get(socket);
  kaazing_challenge_handler_t* challengeHandler = setup_demo_challenge_handler();
  
  /* Inject challenge handler to the websocket object*/
  websocket_set_challenge_handler(ws, challengeHandler);

  /* Establish WebSocket connection */  
  status = amqp_websocket_open(socket, url);
  if (status) {
    die(&quot;opening WebSocket connection&quot;);
  }
  
  /* Establish AMQP connection against the backend broker */
  die_on_amqp_error(amqp_login(conn, &quot;/&quot;, 0, 131072, 0, AMQP_SASL_METHOD_PLAIN, &quot;guest&quot;, &quot;guest&quot;),
  
  /* Open Channel
   * To get the status of call to open channel, amqp_get_rpc_reply should be used
   * amqp_get_rpc_reply() returns the most recent amqp_rpc_reply_t instance corresponding
   * to such an API operation for the given connection.
   */                  &quot;Logging in&quot;);
  amqp_channel_open(conn, 1);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Opening channel&quot;);

  {
    amqp_basic_properties_t props;
    props._flags = AMQP_BASIC_CONTENT_TYPE_FLAG | AMQP_BASIC_DELIVERY_MODE_FLAG;
    props.content_type = amqp_cstring_bytes(&quot;text/plain&quot;);
    props.delivery_mode = 2; /* persistent delivery mode */

    /* Publish a message to the broker on an exchange with a routing key. */
    die_on_error(amqp_basic_publish(conn,
                                    1,
                                    amqp_cstring_bytes(exchange),
                                    amqp_cstring_bytes(routingkey),
                                    0,
                                    0,
                                    &amp;props,
                                    amqp_cstring_bytes(messagebody)),
                 &quot;Publishing&quot;);
  }

  die_on_amqp_error(amqp_channel_close(conn, 1, AMQP_REPLY_SUCCESS), &quot;Closing channel&quot;);
  die_on_amqp_error(amqp_connection_close(conn, AMQP_REPLY_SUCCESS), &quot;Closing connection&quot;);
  die_on_error(amqp_destroy_connection(conn), &quot;Ending connection&quot;);
  return 0;
}
</pre>

<h3><a name="publish_consume"></a>amqp_publish_consume_websocket.c</h3>
<p>The following example demonstrates how to publish and consume using the same AMQP connection.</p>
<pre class="auto-links: false; brush: js; toolbar: false;">
#include &lt;stdlib.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;stdint.h&gt;
#include &lt;unistd.h&gt;
#include &lt;signal.h&gt;
#include &lt;assert.h&gt;
#include &lt;pthread.h&gt;

#include &lt;amqp_websocket.h&gt;
#include &lt;amqp.h&gt;
#include &lt;amqp_framing.h&gt;

#include &quot;utils.h&quot;

void* consume_messages(void* amqp_connection) {
        amqp_connection_state_t conn = (amqp_connection_state_t)amqp_connection;
        while (1) {
                amqp_rpc_reply_t res;
                amqp_envelope_t envelope;

                amqp_maybe_release_buffers(conn);

                /* Wait for and consume a message
                 * The function waits for a basic.deliver method on any channel, upon receipt of
                 * basic.deliver it reads that message, and returns.
                 */
                res = amqp_consume_message(conn, &amp;envelope, NULL, 0);

                if (AMQP_RESPONSE_NORMAL != res.reply_type) {
                        break;
                }

                printf(&quot;Delivery %u, exchange %.*s routingkey %.*s\n&quot;,
                                (unsigned) envelope.delivery_tag, (int) envelope.exchange.len,
                                (char *) envelope.exchange.bytes,
                                (int) envelope.routing_key.len,
                                (char *) envelope.routing_key.bytes);

                if (envelope.message.properties._flags &amp; AMQP_BASIC_CONTENT_TYPE_FLAG) {
                        printf(&quot;Content-type: %.*s\n&quot;,
                                        (int) envelope.message.properties.content_type.len,
                                        (char *) envelope.message.properties.content_type.bytes);
                }

                printf(&quot;Message: %.*s\n&quot;, (int) envelope.message.body.len, (char *) envelope.message.body.bytes);

                amqp_destroy_envelope(&amp;envelope);
        }

        return 0;
}



int main(int argc, char const *const *argv)
{
  char const *url;
  int status;
  char const *exchange;
  char const *bindingkey;
  amqp_socket_t *socket = NULL;
  amqp_connection_state_t conn;
  amqp_bytes_t queuename;

  pthread_t message_consumer_thread;

  if (argc &lt; 4) {
    fprintf(stderr, &quot;Usage: amqp_listen_websocket url exchange bindingkey\n&quot;);
    fprintf(stderr, &quot;Example: amqp_sendstring_websocket ws://localhost:8001/amqp amq.direct test\n&quot;);
    return 1;
  }

  url = argv[1];
  exchange = argv[2];
  bindingkey = argv[3];

  /* Initialize AMQP connection object */
  conn = amqp_new_connection();

  /*
   * Initialize underlying transport object
   * We are using WebSocket as a transport protocol for AMQP messaging
   */
  socket = amqp_websocket_new(conn);
  if (!socket) {
    die(&quot;creating WebSocket&quot;);
  }

  /* Establish WebSocket connection */
  status = amqp_websocket_open(socket, url);
  if (status) {
    die(&quot;opening WebSocket connection&quot;);
  }

  /* Establish AMQP connection against the backend broker */
  die_on_amqp_error(amqp_login(conn, &quot;/&quot;, 0, AMQP_DEFAULT_FRAME_SIZE, 0, AMQP_SASL_METHOD_PLAIN, &quot;guest&quot;, &quot;guest&quot;),
                    &quot;Logging in&quot;);

  /* Open Channel
   * To get the status of call to open channel, amqp_get_rpc_reply should be used
   * amqp_get_rpc_reply() returns the most recent amqp_rpc_reply_t instance corresponding
   * to such an API operation for the given connection.
   */
  amqp_channel_open(conn, 1);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Opening channel&quot;);

  {
        /* Declare Queue
         * In this case we are providing empty name that results in server creating a unique
         * queue name and sending it to the client
         */
    amqp_queue_declare_ok_t *r = amqp_queue_declare(conn, 1, amqp_empty_bytes, 0, 0, 0, 1,
                                 amqp_empty_table);
    die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Declaring queue&quot;);

    /* Get the queue name sent by the server */
    queuename = amqp_bytes_malloc_dup(r-&gt;queue);
    if (queuename.bytes == NULL) {
      fprintf(stderr, &quot;Out of memory while copying queue name&quot;);
      return 1;
    }
  }

  /* Bind queue to an exchange */
  amqp_queue_bind(conn, 1, queuename, amqp_cstring_bytes(exchange), amqp_cstring_bytes(bindingkey),
                  amqp_empty_table);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Binding queue&quot;);

  /* Start a queue consumer.
   * This method asks the server to start a &quot;consumer&quot;, which is a transient request
   * for messages from a specific queue.
   */
  amqp_basic_consume(conn, 1, queuename, amqp_empty_bytes, 0, 1, 0, amqp_empty_table);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Consuming&quot;);


  /* Create a new thread to consume messages. */
  pthread_create(&amp;message_consumer_thread, NULL, &amp;consume_messages, conn);
  
  /* Send messages */
  while(1) {
      amqp_basic_properties_t props;
      props._flags = AMQP_BASIC_CONTENT_TYPE_FLAG | AMQP_BASIC_DELIVERY_MODE_FLAG;
      props.content_type = amqp_cstring_bytes(&quot;text/plain&quot;);
      props.delivery_mode = 2; /* persistent delivery mode */

      /* Publish a message to the broker on an exchange with a routing key. */
      die_on_error(amqp_basic_publish(conn,
                                      1,
                                      amqp_cstring_bytes(exchange),
                                      amqp_cstring_bytes(bindingkey),
                                      0,
                                      0,
                                      &amp;props,
                                      amqp_cstring_bytes(&quot;Hello, World!&quot;)),
                   &quot;Publishing&quot;);
      sleep(1);
  }

  /* TODO: Additional logic needed to close connection and clean up resources */

  return 0;
}
</pre>

<h3><a name="sendssl"></a>amqp_sendstring_websocket_ssl.c</h3>
<p>The following example demonstrates how to publish AMQP messages and verify an OpenSSL client certificate against a Certificate Authority (CA).</p>

<pre class="auto-links: false; brush: js; toolbar: false;">
#include &lt;stdlib.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

#include &lt;stdint.h&gt;
#include &lt;amqp_websocket.h&gt;
#include &lt;amqp.h&gt;
#include &lt;amqp_framing.h&gt;

#include &quot;utils.h&quot;

int main(int argc, char const *const *argv)
{
  char const *url;
  int status;
  char const *exchange;
  char const *routingkey;
  char const *messagebody;
  amqp_socket_t *socket = NULL;
  amqp_connection_state_t conn;

  
  if (argc &lt; 5) {
    fprintf(stderr, &quot;Usage: amqp_sendstring_websocket url exchange routingkey messagebody\n&quot;);
    fprintf(stderr, &quot;Example: amqp_sendstring_websocket wss://localhost:9001/amqp amq.direct test \&quot;Hello World\&quot; \n&quot;);
    return 1;
  }

  url = argv[1];
  exchange = argv[2];
  routingkey = argv[3];
  messagebody = argv[4];
  


  /* Initialize AMQP connection object */
  conn = amqp_new_connection();
  
  /*
   * Initialize underlying transport object
   * We are using WebSocket as a transport protocol for AMQP messaging
   */
  socket = amqp_websocket_new(conn);
  if (!socket) {
    die(&quot;creating WebSocket&quot;);
  }
  
  
  
  websocket_t * ws = amqp_websocket_get(socket);
  
  // Set the CA Cert
  int set_ca_cert_result = websocket_ssl_set_cacert(ws, &quot;/home/user/cert/cacert.pem&quot;);
  if (set_ca_cert_result != 0) {
      die(&quot;Error while specifying CA certificate&quot;);
  }
  
  // Set Client key and certificate
  const char* cert = &quot;/home/user/cert/client.crt&quot;;
  const char* key = &quot;/home/user/cert/client.key&quot;;
  int set_client_key_result = websocket_ssl_set_clientkey(ws, cert, key);
  if (set_client_key_result != 0) {
      die(&quot;Error while specifying Client certificate and key&quot;);
  }
  
  /* Establish WebSocket connection */  
  status = amqp_websocket_open(socket, url);
  if (status) {
    die(&quot;opening WebSocket connection&quot;);
  }
  
  /* Establish AMQP connection against the backend broker */
  die_on_amqp_error(amqp_login(conn, &quot;/&quot;, 0, 131072, 0, AMQP_SASL_METHOD_PLAIN, &quot;guest&quot;, &quot;guest&quot;),
  
  /* Open Channel
   * To get the status of call to open channel, amqp_get_rpc_reply should be used
   * amqp_get_rpc_reply() returns the most recent amqp_rpc_reply_t instance corresponding
   * to such an API operation for the given connection.
   */                  &quot;Logging in&quot;);
  amqp_channel_open(conn, 1);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Opening channel&quot;);

  {
    amqp_basic_properties_t props;
    props._flags = AMQP_BASIC_CONTENT_TYPE_FLAG | AMQP_BASIC_DELIVERY_MODE_FLAG;
    props.content_type = amqp_cstring_bytes(&quot;text/plain&quot;);
    props.delivery_mode = 2; /* persistent delivery mode */

    /* Publish a message to the broker on an exchange with a routing key. */
    die_on_error(amqp_basic_publish(conn,
                                    1,
                                    amqp_cstring_bytes(exchange),
                                    amqp_cstring_bytes(routingkey),
                                    0,
                                    0,
                                    &amp;props,
                                    amqp_cstring_bytes(messagebody)),
                 &quot;Publishing&quot;);
  }

  die_on_amqp_error(amqp_channel_close(conn, 1, AMQP_REPLY_SUCCESS), &quot;Closing channel&quot;);
  die_on_amqp_error(amqp_connection_close(conn, AMQP_REPLY_SUCCESS), &quot;Closing connection&quot;);
  die_on_error(amqp_destroy_connection(conn), &quot;Ending connection&quot;);
  return 0;
}
</pre>

<h3><a name="sslcallback"></a>amqp_sendstring_websocket_ssl_callback.c</h3>
<p>The following example demonstrates how to publish AMQP messages and verify an OpenSSL client certificate against a local private key for client authentication.</p>

<pre class="auto-links: false; brush: js; toolbar: false;">
#include &lt;stdlib.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

#include &lt;stdint.h&gt;
#include &lt;amqp_websocket.h&gt;
#include &lt;amqp.h&gt;
#include &lt;amqp_framing.h&gt;

#include &quot;utils.h&quot;

static int verify_callback_client_cert(int preverify_ok, X509_STORE_CTX *ctx)
 {

    /*
     * Retrieve the pointer to the SSL of the connection
     */
    SSL* ssl = (SSL*)X509_STORE_CTX_get_ex_data(ctx, SSL_get_ex_data_X509_STORE_CTX_idx());
    //load client certification and private key for client authentication
    int ret;
    ret = SSL_use_certificate_file(ssl, &quot;/home/user/cert/client.crt&quot;, SSL_FILETYPE_PEM);
        ret = SSL_use_PrivateKey_file(ssl,&quot;/home/user/cert/client.key&quot;, SSL_FILETYPE_PEM);

    return 1;
 }



int main(int argc, char const *const *argv)
{
  char const *url;
  int status;
  char const *exchange;
  char const *routingkey;
  char const *messagebody;
  amqp_socket_t *socket = NULL;
  amqp_connection_state_t conn;

  
  if (argc &lt; 5) {
    fprintf(stderr, &quot;Usage: amqp_sendstring_websocket url exchange routingkey messagebody\n&quot;);
    fprintf(stderr, &quot;Example: amqp_sendstring_websocket wss://localhost:9001/amqp amq.direct test \&quot;Hello World\&quot; \n&quot;);
    return 1;
  }

  url = argv[1];
  exchange = argv[2];
  routingkey = argv[3];
  messagebody = argv[4];
  


  /* Initialize AMQP connection object */
  conn = amqp_new_connection();
  
  /*
   * Initialize underlying transport object
   * We are using WebSocket as a transport protocol for AMQP messaging
   */
  socket = amqp_websocket_new(conn);
  if (!socket) {
    die(&quot;creating WebSocket&quot;);
  }

  websocket_t * ws = amqp_websocket_get(socket);
  websocket_ssl_set_verify_callback(ws, &amp;verify_callback_client_cert);

  /* Establish WebSocket connection */  
  status = amqp_websocket_open(socket, url);
  if (status) {
    die(&quot;opening WebSocket connection&quot;);
  }
  
  /* Establish AMQP connection against the backend broker */
  die_on_amqp_error(amqp_login(conn, &quot;/&quot;, 0, 131072, 0, AMQP_SASL_METHOD_PLAIN, &quot;guest&quot;, &quot;guest&quot;),
  
  /* Open Channel
   * To get the status of call to open channel, amqp_get_rpc_reply should be used
   * amqp_get_rpc_reply() returns the most recent amqp_rpc_reply_t instance corresponding
   * to such an API operation for the given connection.
   */                  &quot;Logging in&quot;);
  amqp_channel_open(conn, 1);
  die_on_amqp_error(amqp_get_rpc_reply(conn), &quot;Opening channel&quot;);

  {
    amqp_basic_properties_t props;
    props._flags = AMQP_BASIC_CONTENT_TYPE_FLAG | AMQP_BASIC_DELIVERY_MODE_FLAG;
    props.content_type = amqp_cstring_bytes(&quot;text/plain&quot;);
    props.delivery_mode = 2; /* persistent delivery mode */

    /* Publish a message to the broker on an exchange with a routing key. */
    die_on_error(amqp_basic_publish(conn,
                                    1,
                                    amqp_cstring_bytes(exchange),
                                    amqp_cstring_bytes(routingkey),
                                    0,
                                    0,
                                    &amp;props,
                                    amqp_cstring_bytes(messagebody)),
                 &quot;Publishing&quot;);
  }

  die_on_amqp_error(amqp_channel_close(conn, 1, AMQP_REPLY_SUCCESS), &quot;Closing channel&quot;);
  die_on_amqp_error(amqp_connection_close(conn, AMQP_REPLY_SUCCESS), &quot;Closing connection&quot;);
  die_on_error(amqp_destroy_connection(conn), &quot;Ending connection&quot;);
  return 0;
}
</pre>

<span class="note"><b>Note:</b> If you select to use this method, then call the <code>kws_ssl_set_cacert()</code> function to ensure that you are connected to the correct server. Without setting the CA certificate, the client will connect to the server without any certificate verification. This might cause security problems. Here is the function:

<code>int kws_ssl_set_cacert(websocket_t *ws, const char *cacert);</code>

<p>The function takes a path to the CA certificate file as a second parameter. The CA certificate file should be of PEM format.</p>
</span>

<h2>Next Step</h2>
<p><a href="../security/p_tls_mutualauth.html">Require Clients to Provide Certificates to ${the.gateway}</a></p>

</html>
