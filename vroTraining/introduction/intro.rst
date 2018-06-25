
Introduction
=================

Welcome to the vRO Plug-in for BIG-IP by Blue Medora lab guide.  We hope you find this document valuable.  This plug-in is built, supported and maintained by Blue Medora – a company with expertise in third party integrations into VMware vRealize Suite.  F5 and Blue Medora have collaborated very closely on the design, construction and testing of this plugin with reviews from VMware.  
 
As background, F5 produced an unsupported, DevCentral-distributed plug-in for LTM and GTM with vRO v6 as early as 2014.  This was an attempt to assess the market demand for orchestrating BIG-IP with VMware vRealize Orchestrator.  The response was very strong – customers wanted the plug-in and began deploying it in their test labs.  The challenge was that when it came to deploying it in production, many of the larger customers had concerns that it wasn’t supported officially by F5; and thus began a two year effort to find a way to support it.  The end result was leveraging a very close technology partner of ours, Blue Medora, to rebuild the plug-in using all the feedback that F5 had collected over the past two 
years, enhance and expand it accordingly. 
 
In case you didn’t know, vRO is a popular orchestration engine that comes with every vSphere license.  It’s free to all vSphere customers. It’s a powerful tool that more and more customers are leveraging – from the very big to the medium-sized or smaller.  Not only that, but anything we do with vRO is automatically bubbled up into the higher-order vRealize Automation product.  vRA contains the self-service portal that enables users to request applications, and this trickles down to vRO, which then trickles down to all of the relevant services necessary to provision and deploy that application (including BIG-IP).   
 
The deployment process is relatively quick once a customer has vRO up and running. This plug-in is a bit like a lego set.  We’re giving customers a toolkit of lego blocks – from which they can build an almost infinite set of workflows by combining individual atomic actions together.  Does it require some effort to build the workflows you really want?  Yes.  But this is not necessarily something that needs to be outsourced – most IT shops can assemble these workflows on their own, and we encourage them to do so.  We have provided a number of pre-built workflows in their plug-in, however the sky really is the limit.

KEY CONCEPTS OF WORKFLOWS
=========================
Workflows consist of a schema, attributes, and parameters. The workflow schema is the main component of a workflow as it defines all the workflow elements and the logical connections between them. The workflow attributes and parameters are the variables that workflows use 
to transfer data. Orchestrator saves a workflow token every time a workflow runs, recording the details of that specific run of the workflow.  

Workflow Parameters
-------------------
 -	Workflows receive input parameters and generate output parameters when they run.  

Input Parameters
----------------
 -	Most workflows require a certain set of input parameters to run. An input parameter is an argument that the workflow processes when it starts. The user, an application, another workflow, or an action passes input parameters to a workflow for the workflow to process 
when it starts.  
 -	For example, if a workflow resets a virtual machine, the workflow requires as an input parameter the name of the virtual machine.  

Output Parameters
-----------------
 -	A workflow's output parameters represent the result from the workflow run. Output parameters can change when a workflow or a workflow element runs. While workflows run, they can receive the output parameters of other workflows as input parameters.  
For example, if a workflow creates a snapshot of a virtual machine, the output parameter for the workflow is the resulting snapshot.  

Workflow Attributes  
-------------------
 -	Workflow elements process data that they receive as input parameters, and set the resulting data as workflow attributes or output parameters.  
 -	Read-only workflow attributes act as global constants for a workflow. Writable attributes act 
as a workflow’s global variables.  
 -	You can use attributes to transfer data between the elements of a workflow. You can obtain 
attributes in the following ways:  
  - Define attributes when you create a workflow 
  -	 Set the output parameter of a workflow element as a workflow attribute  
  -	Inherit attributes from a configuration element  

Workflow Schema
---------------
 -	A workflow schema is a graphical representation that shows the workflow as a flow diagram of interconnected workflow elements. The workflow schema is the most important element of a workflow as it determines its logic.  

Workflow Presentation  
---------------------
 -	When users run a workflow, they provide the values for the input parameters of the workflow in the workflow presentation. When you organize the workflow presentation, consider the type and number of input parameters of the workflow.  

Workflow Tokens  
---------------
 -	A workflow token represents a workflow that is running or has run.  
 -	A workflow is an abstract description of a process that defines a generic sequence of steps and a generic set of required input parameters. When you run a workflow with a set of real input parameters, you receive an instance of this abstract workflow that behaves according to the specific input parameters you give it. This specific instance of a completed or a running workflow is called a workflow token.  

 Workflow Token Attributes  
--------------------------
 -	Workflow token attributes are the specific parameters with which a workflow token runs. 
The workflow token attributes are an aggregation of the workflow's global attributes and the specific input and output parameters with which you run the workflow token.  
 
