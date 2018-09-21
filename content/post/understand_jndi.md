---
topics:
- java
- developer

date: 2018-09-20
weight: 12

project_url: https://tonybui1812.github.io
series:
- Developer
slug: java
tags:
- Java naming and Directory Interface
- JNDI
- Java
title: "JNDI - Understand"
---
<link rel="stylesheet" href="/css/w3css_4_w3.css">
<link rel="stylesheet" href="/highlight/styles/default.css">
<script src="/highlight/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>

What we have in this post:

- Overview
- Use cases
- Samples
- Source code

Let's begin



<h1>Overview</h1>
The <b>Java Naming and Directory Interface (JNDI)</b> is a Java API for a directory service that allows Java software clients to discover and look up data and resources (in the form of Java objects) via a name.

<img src="/img/basic_jndi_diagram.JPG" />

<b>SPI</b>: Service Provider Interface

<b>API</b>: Application Programing Interface

The information looked up via JNDI may be supplied by a server, a flat file, or a database; the choice is up to the implementation used.

<a href="https://en.wikipedia.org/wiki/Java_Naming_and_Directory_Interface" target="_blank">More on Wiki</a>

Sun Microsystems first released the JNDI specification on March 10, 1997. As of 2006, the current version is JNDI 1.2.

<img src="/img/sun_jw_jndi_orig.gif">

<i><b>JNDI architecture</b></i>
<a href="https://www.javaworld.com/article/2074186/jndi/j2ee-or-j2se--jndi-works-with-both.html" target="_blank">More on JavaWorld</a>


<h1>Use cases</h1>
Typical uses of JNDI include:

- connecting a Java application to an external directory service (such as an address database or an LDAP server)

- <b>WebServer JNDI</b>: allowing a Java Servlet to look up configuration information provided by the hosting web container
	- <a href="https://docs.oracle.com/cd/E13222_01/wls/docs100/jndi/over.html" target="_blank"><b>Oracle</b> WebLogic JNDI</a>
	- <a href="https://docs.jboss.org/author/display/AS71/JNDI+Reference" target="_blank"><b>Red Hat</b> JBoss JNDI</a>
	- <a href="https://www.ibm.com/support/knowledgecenter/en/SSAW57_9.0.0/com.ibm.websphere.nd.multiplatform.doc/ae/tnam_develop_naming.html" target="_blank"><b>IBM</b> WebSephere JNDI</a>
	
<h2>Samples</h2>	
<h3>WebServer JNDI</h3>

<div class="w3-bar w3-black">
  <button class="w3-bar-item w3-button tablink w3-red" onclick="openTab(event,'WebLogic','city')">WebLogic</button>
  <button class="w3-bar-item w3-button tablink" onclick="openTab(event,'JBoss','city')">JBoss</button>
  <button class="w3-bar-item w3-button tablink" onclick="openTab(event,'WebSephere','city')">WebSephere</button>
</div>

<div id="WebLogic" class="w3-container city">
  <pre>
  <code class="java">
  Context ctx = null;
  Hashtable ht = new Hashtable();
  ht.put(Context.INITIAL_CONTEXT_FACTORY,
         "weblogic.jndi.WLInitialContextFactory");
  ht.put(Context.PROVIDER_URL,
         "t3://localhost:7001");

  try {
    ctx = new InitialContext(ht);
    // Use the context in your program
	try { 
    ServiceBean bean = (ServiceBean)ctx.lookup("ejb.serviceBean");
	}catch (NameNotFoundException e) {
	// binding does not exist
	}catch (NamingException e) {
	// a failure occurred
	}
  }
  catch (NamingException e) {
    // a failure occurred
  }
  finally {
    try {ctx.close();}
    catch (Exception e) {
      // a failure occurred
    }
  }
  </code>
  </pre>
</div>

<div id="JBoss" class="w3-container city" style="display:none" >
 <pre> <code class="java">
final Properties env = new Properties();
env.put(Context.INITIAL_CONTEXT_FACTORY
	,org.jboss.naming.remote.client.InitialContextFactory.class.getName());
env.put(Context.PROVIDER_URL, "remote://localhost:4447");
remoteContext = new InitialContext(env);
MyBean myBean = remoteContext.lookup("java:global/MyBean");

</code></pre>
</div>

<div id="WebSephere" class="w3-container city" style="display:none">
  <pre><code class="java">
Hashtable env = new Hashtable();
env.put(Context.INITIAL_CONTEXT_FACTORY,
      "com.ibm.websphere.naming.WsnInitialContextFactory");
env.put(Context.PROVIDER_URL, "iiop://localhost:2809");
Context ctx = new InitialContext(env);
//look up
Object o = ctx.lookup("com/mycom/MyEJBHome");
</code>
</pre>
</div>
<h2>Source codes</h2>	
<script src="/scripts/tab_function.js" />
