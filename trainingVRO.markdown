F5 Lab Guide:

VMware vRealize Orchestrator Plug-In for F5 BIG-IP™by BlueMedora UDF\
\
\
Lab Guide for using the VMware vRO plug-in for F5 to automate and
orchestrate the deployment/attachment of F5 BIG-IPs, and the use of
workflows to create F5 BIG-IP ADC service configurations.

![](media/image1.png){width="6.5in" height="5.047916666666667in"}

[I. Introduction 2](#introduction)

[II. Useful IP and Password info System Information Win 7 Jump Box
3](#useful-ip-and-password-info)

[III. Task 1—Access the vRO environment and verify that the Blue Medora
Plug in is licensed
5](#task-1access-the-vro-environment-and-verify-that-the-blue-medora-plug-in-is-licensed)

[IV. TASK 2—Tour the vRO Interface 8](#task-2tour-the-vro-interface)

[V. Custom Workflow—Advanced Operations
10](#custom-workflowadvanced-operations)

[1. Key Concepts of Workflows 10](#key-concepts-of-workflows)

[2. Create a CustomWorkflows Folder
13](#create-a-customworkflows-folder)

[3. Create a Custom Workflow 14](#create-a-custom-workflow)

[4. Add Create Pool to Custom Workflow
14](#add-create-pool-to-custom-workflow)

[5. Add the Create Pool Member workflow to the custom workflow
25](#add-the-create-pool-member-workflow-to-the-custom-workflow)

[6. Add the Create Virtual Server workflow to the custom workflow
27](#add-the-create-virtual-server-workflow-to-the-custom-workflow)

[7. Add the Set Virtual Server SNAT to the custom workflow
34](#add-the-set-virtual-server-snat-to-the-custom-workflow)

[8. Run the CustomWF1workflow 38](#run-the-customwf1workflow)

Introduction
============

Welcome to the vRO Plug-in for BIG-IP by Blue Medora lab guide. We hope
you find this document valuable. This plug-in is built, supported and
maintained by Blue Medora – a company with expertise in third party
integrations into VMware vRealize Suite. F5 and Blue Medora have
collaborated very closely on the design, construction and testing of
this plugin with reviews from VMware.

As background, F5 produced an unsupported, DevCentral-distributed
plug-in for LTM and GTM with vRO v6 as early as 2014. This was an
attempt to assess the market demand for orchestrating BIG-IP with VMware
vRealize Orchestrator. The response was very strong – customers wanted
the plug-in and began deploying it in their test labs. The challenge was
that when it came to deploying it in production, many of the larger
customers had concerns that it wasn’t supported officially by F5; and
thus began a two year effort to find a way to support it. The end result
was leveraging a very close technology partner of ours, Blue Medora, to
rebuild the plug-in using all the feedback that F5 had collected over
the past two

years, enhance and expand it accordingly.

In case you didn’t know, vRO is a popular orchestration engine that
comes with every vSphere license. It’s free to all vSphere customers.
It’s a powerful tool that more and more customers are leveraging – from
the very big to the medium-sized or smaller. Not only that, but anything
we do with vRO is automatically bubbled up into the higher-order
vRealize Automation product. vRA contains the self-service portal that
enables users to request applications, and this trickles down to vRO,
which then trickles down to all of the relevant services necessary to
provision and deploy that application (including BIG-IP).

The deployment process is relatively quick once a customer has vRO up
and running. This plug-in is a bit like a lego set. We’re giving
customers a toolkit of lego blocks – from which they can build an almost
infinite set of workflows by combining individual atomic actions
together. Does it require some effort to build the workflows you really
want? Yes. But this is not necessarily something that needs to be
outsourced – most IT shops can assemble these workflows on their own,
and we encourage them to do so. We have provided a number of pre-built
workflows in their plug-in, however the sky really is the limit.

Useful IP and Password info 
============================

\
System Information\
\
Win 7 Jump Box {#system-information-win-7-jump-box .ListParagraph}
===================

External RDP address –Provided by Instructor\
Management IP: 10.128.1.50

vRO Appliance\
Hostname vro.fdemo5.com\
Management IP: 10.128.1.235\
vRO Main Page: https:// vro.fdemo5.com /vco/\
SSH Credentials: root/default\
vRO Control Center: https://vro.f5demo:8283/vco-controlcenter\
vRO Control Center Credentials: root/default\
vRO Client Credentials: vcoadmin/vcoadmin\
\
BIG-IP Information\
BIG-IP SSH Credentials: root/default\
BIG-IP UI Credentials: admin/admin\
\
BIG-IP 1\
Hostname bigip1.f5demo.com\
ManagementIP 10.128.1.245\
External IP 10.128.10.245/24\
Internal IP 10.128.20.245/24\
External Float 10.128.20.247/24\
Internal\
HA IP 10.128.30.245/24\
\
\
BIG-IP 2\
Hostname bigip2.f5demo.com\
ManagementIP 10.128.1.246\
External IP 10.128.10.246/24\
Internal IP 10.128.20.246/24

External Float 10.128.10.247/24\
Internal Self\
HA IP 10.128.30.246/24\
\
Lamp Server—Web services\
10.128.20.11

10.128.20.12

10.128.20.13

10.128.20.14

10.128.20.15

Task 1—Access the vRO environment and verify that the Blue Medora Plug in is licensed
=====================================================================================

-   <span id="OLE_LINK52" class="anchor"><span id="OLE_LINK51"
    class="anchor"></span></span>Use the RDP function on your laptop to
    connect to the “WIN7“ RDP server/workstation

    -   Instructor to provide IP address

-   Username: Student

-   Password: F5Agility-vro

    ![](media/image2.png){width="3.8541666666666665in"
    height="2.9284547244094488in"}

-   Connect to the vRO instance/client and check the license

    -   http://vro.f5demo.com which will redirect you to\
        https://vro,f5demo.com:8281/vco

-   Click the Start Orchestrator Client link.\
    ![](media/image3.png){width="3.53125in"
    height="2.1354166666666665in"}

-   This will open a warning in the toolbar, Click Keep

![](media/image4.png){width="3.8958333333333335in"
height="0.7931113298337708in"}.

-   Launch The Java Client bt clicking it

![](media/image5.png){width="2.8125in" height="0.6700142169728784in"}

-   If you are asked to proceed to an untrusted website, Click
    Continue.\
    ![](media/image6.png){width="5.010416666666667in"
    height="2.6458333333333335in"}

-   Do you want to run this application. Click Run

    ![](media/image7.png){width="5.49992125984252in"
    height="2.8958333333333335in"}

-   You will be presented with the actual vRO login screen. Verify or\
    choose from the dropdown vro.f5demo.com:8281

    -   User Name vcoadmin

    -   Password vcoadmin

    -   click Login.

        ![](media/image8.png){width="4.40625in"
        height="3.1644028871391074in"}

<!-- -->

-   If you receive a Certificate Warning If you do make sure to put the
    checkmark in\
    the Install this certificate… checkbox and click Ignore

    ![](media/image9.png){width="4.34375in"
    height="1.7604166666666667in"}

I.  \
    TASK 2—Tour the vRO Interface 
    ==============================

    Before we get started in earnest it would be good to take a quick
    tour of the vRO client\
    interface. Here are a few of the important screens you will
    encounter while working with this\
    plug-in.

    In the screen capture below you are seeing the Schema of the License
    Plugin workflow,\
    which is a quite simple workflow. Specifically, the View Details of
    the Action “License\
    Plugin”. You can explore the bindings of input and outputs to the
    actual APIs.

    -   Select the design from the pulldown

    -   Select the Schma Tab

    -   Expand
        [Admin@vro.f5demo.com&gt;&gt;F5&gt;&gt;Basic](mailto:Admin@vro.f5demo.com%3e%3eF5%3e%3eBasic)

    -   Select License Plug in from the left panel

    -   Right click the mouse over the licensePlugin and select the
        small icon above and to the right\
        ![](media/image10.png){width="5.302083333333333in"
        height="2.9583333333333335in"}

    -   Review the, Out and Scripting tabs on the new window In
        parameters mapped to the variables for this particular action.\
        ![](media/image11.png){width="3.6979166666666665in"
        height="1.8020833333333333in"}

    -   Use the pulldown to access the Adminiatrator View

    -   ![](media/image12.png){width="5.302083333333333in"
        height="4.534527559055118in"}

II. Custom Workflow—Advanced Operations
    ===================================

While the workflows included in this Plug-in cover a large majority of
the use cases for Day\
0, Day 1, and Day 2 ADC operations. One of the many great features of
vRealize\
Orchestrator is that you can combine workflows to extend the
capabilities of the Plug-in to\
more complex customized use cases.\
\
You will learn how to create a custom Workflow to complete the
following:

-   1\. Create a Pool

-   Add members to the new Pool

-   Create a VIP

-   Enable SNAT the VIP

Key Concepts of Workflows
-------------------------

Workflows consist of a schema, attributes, and parameters. The workflow
schema is the main\
component of a workflow as it defines all the workflow elements and the
logical connections\
between them. The workflow attributes and parameters are the variables
that workflows use\
to transfer data. Orchestrator saves a workflow token every time a
workflow runs, recording\
the details of that specific run of the workflow.\
\
Workflow Parameters

-   Workflows receive input parameters and generate output parameters
    when they run.

Input Parameters

-   Most workflows require a certain set of input parameters to run. An
    input parameter is an\
    argument that the workflow processes when it starts. The user, an
    application, another\
    workflow, or an action passes input parameters to a workflow for the
    workflow to process\
    when it starts.

-   For example, if a workflow resets a virtual machine, the workflow
    requires as an input\
    parameter the name of the virtual machine.

Output Parameters

-   A workflow's output parameters represent the result from the
    workflow run. Output\
    parameters can change when a workflow or a workflow element runs.
    While workflows run,\
    they can receive the output parameters of other workflows as input
    parameters.\
    For example, if a workflow creates a snapshot of a virtual machine,
    the output parameter for\
    the workflow is the resulting snapshot.

Workflow Attributes

-   Workflow elements process data that they receive as input
    parameters, and set the resulting data as workflow attributes or
    output parameters.

-   Read-only workflow attributes act as global constants for
    a workflow. Writable attributes act\
    as a workflow’s global variables.

-   You can use attributes to transfer data between the elements of
    a workflow. You can obtain\
    attributes in the following ways:

    -   Define attributes when you create a workflow

    -   Set the output parameter of a workflow element as a workflow
        attribute

    -   Inherit attributes from a configuration element

Workflow Schema

-   A workflow schema is a graphical representation that shows the
    workflow as a flow diagram\
    of interconnected workflow elements. The workflow schema is the most
    important element\
    of a workflow as it determines its logic.

Workflow Presentation

-   When users run a workflow, they provide the values for the input
    parameters of the workflow\
    in the workflow presentation. When you organize the workflow
    presentation, consider the\
    type and number of input parameters of the workflow.

Workflow Tokens

-   A workflow token represents a workflow that is running or has run.

-   A workflow is an abstract description of a process that defines a
    generic sequence of steps\
    and a generic set of required input parameters. When you run a
    workflow with a set of real\
    input parameters, you receive an instance of this abstract workflow
    that behaves according to\
    the specific input parameters you give it. This specific instance of
    a completed or a running\
    workflow is called a workflow token.

Workflow Token Attributes

-   Workflow token attributes are the specific parameters with which a
    workflow token runs.\
    The workflow token attributes are an aggregation of the workflow's
    global attributes and the\
    specific input and output parameters with which you run the
    workflow token.

Process Tip

During the following workflow you may save and exit at some point . The
Create Workflow in this exercise assumes you stay in the Edit mode for
the entire process. In the event that you exit the edit mode it is a
simple task to get back to it

-   Select the Workflow you wish to edit

-   Select the schema tab

-   Select the pencil icon

-   Expand the tabs as needed to return to your work

![](media/image13.png){width="6.5in" height="3.270138888888889in"}

Create a CustomWorkflows Folder
-------------------------------

Note: This step has been completed. Sample workflows are in the folder\
\
Right click on the admin@vro.f5demo.com container.

-   Choose Add Folder.

-   Enter CustomWorkflows in the Name field

-   click Ok.

> ![](media/image14.png){width="4.004400699912511in"
> height="2.4920636482939633in"}

Create a Custom Workflow 
-------------------------

Right click on the CustomWorkflows folder

-   Choose New Workflow

-   Enter CustomWF1

-   Choose OK

-   Select Save

    ![](media/image15.png){width="4.354166666666667in"
    height="2.5104166666666665in"}

Add Create Pool to Custom Workflow\
-----------------------------------

Select the Schema Tab

Expand the All Workflows heading on the left and navigate to F5/Ltm/Pool

![](media/image16.png){width="4.4966655730533684in"
height="2.8299989063867015in"}\
\
Drag and drop Create Pool to the right of the Green Arrow

Choose the Setup button in the upper right(This will bring up the
Promote Workflow\
Input Parameters)

![](media/image17.png){width="4.4966655730533684in"
height="2.1366666666666667in"}

Choose Skip in the Rest all binding to: section

![](media/image18.png){width="4.5in" height="5.25in"}

Choose Value for loadBalancingMode and enter **round-robin *(****This
will be the\
default load balancing algorithm used)*

Choose Value for partition and enter **Common** *(This will force all
objects created by\
CustomWF1 to be put in the Common partition)*\
\
Select Input for monitor, name and bigip *(scroll down for the last 2) *

![](media/image19.png){width="4.4966655730533684in" height="3.88in"}

Choose Promote\
Choose Save

Modify presentation layer of custom workflow

Choose the Green Run triangle to start your workflow

![](media/image20.png){width="6.5in" height="3.0770833333333334in"}

You see the 3 parameters that we set to input appear as input
parameters.\
\
![](media/image21.png){width="4.4966655730533684in" height="2.88in"}

Choose Cancel

Choose the General tab

You see the loadBalancingmode and partition set to the values we defined
define. *(view the full screen)*\
![](media/image22.png){width="4.5in" height="1.5499989063867017in"}\
\
\
Choose the Presentation tab(Each variable is shown with 3 parameters
(type)name\
description)

Choose monitor and enter **Monitor to be used by LTM Pool** in the
description.

Choose name and enter **Name for LTM objects created by this WF** name
for the description.\
\
Choose bigip and enter **BigIP to be configured** in the description
field.\
![](media/image23.png){width="4.5in" height="2.8499989063867015in"}

Reorder the variables so they appear in this order: bigip, name, and
monitor(You can\
rearrange the variable order by dragging and dropping the variable
names.)\
\
![](media/image23.png){width="4.996225940507436in"
height="2.6666666666666665in"}\
\
\
Choose the Schema tab

Mouse over the Create Pool workflow on the Schema tab\
\
Choose the Pencil icon above the Create Pool workflow

![](media/image24.png){width="0.9580511811023622in"
height="0.7916666666666666in"}\
\
![](media/image25.png){width="6.5in" height="2.4166666666666665in"}

Choose the OUT tab\
\
Double Click the actionResult \[out-parameter\] under Source parameter\
![](media/image26.png){width="4.4966655730533684in" height="0.98in"}
Figure 62\
\
\
Choose the Create parameter/attribute in workflow link

![](media/image27.png){width="5.332452974628172in" height="3.09375in"}\
\
\
\
Enter F5Pool in the Name field *(This will create an F5Pool output
variable that\
references the pool that has just been created)*\
![](media/image28.png){width="4.4966655730533684in" height="4.67in"}\
\
Choose Ok\
\
Choose Close\
\
Choose Save

Choose the Green Run triangle

![](media/image29.png){width="6.5in" height="2.9472222222222224in"}

The order and description of the input parameters has been updated.\
![](media/image30.png){width="4.9966655730533684in"
height="1.6799989063867016in"}\
Choose Cancel

\
Add the Create Pool Member workflow to the custom workflow
----------------------------------------------------------

Expand the All Workflows heading on the left and navigate to
F5/Ltm/Pool\
\
Drag and drop the Create Pool Member workflow to the right of the Create
Pool\
workflow

Choose Setup in the upper right\
![](media/image31.png){width="4.4966655730533684in" height="1.97in"}

Choose Skip in the Reset all binding to: section\
\
Chose Input for **address** and **port**(This will add the address and
port fields to the\
presentation layer)\
\
Enter **lamp11** as the value for node *(This is the pool member object
you created\
as part of the Create Pool Member workflow earlier)*

![](media/image32.png){width="4.65625in" height="6.802083333333333in"}\
Choose Promote

Mouse over the Create Pool Member workflow\
\
Choose the Pencil icon to edit\
![](media/image33.png){width="1.21875in" height="0.7291666666666666in"}\
\
\
Choose the IN tab\
\
Double click the NULL Source parameter next to the bigip Local
Parameter\
\
Choose the bigip in-parameter\
\
Choose Select\
\
Repeat steps for the following parameters:\
pool:F5Pool\
partition:partition\
\
\
![](media/image34.png){width="5.0in" height="3.9in"}\
Choose Close\
\
Choose Save

\
Add the Create Virtual Server workflow to the custom workflow 
--------------------------------------------------------------

Be sure you are in the Schema tab\
\
Expand the All Workflows heading on the left and navigate to
F5/Ltm/Virtual Server\
\
Drag and drop the Create Vitual Server workflow to the right of Create
Pool Member on the Schema tab\
![](media/image35.png){width="4.9966655730533684in"
height="1.5299989063867017in"}\
Choose Setup in the upper right

Choose Skip in the Reset all binding to: section\
\
Set destination to Input(This will add the destination variable to the
presentation\
layer of the workflow)\
\
![](media/image36.png){width="4.4966655730533684in"
height="6.3699989063867015in"}\
\
Step 6: Choose Promote

Mouse over the Create Virtual Server workflow\
\
Choose the Pencil to edit\
![](media/image37.png){width="0.8854166666666666in"
height="0.7708333333333334in"}

Verify that you are on the IN tab

Double click the NULL Source parameter next to the bigip Local
Parameter\
\
Choose the bigip in-parameter\
\
Choose Select\
\
Repeat steps 9-11 for the following parameters:\
name:name\
partition:partition\
pool:name\
\
![](media/image38.png){width="4.5in" height="3.15625in"}

Scroll down until you see ipProtocol

Click NULL next to ipProtocol\
\
Choose the Create parameter/attribute in workflow link

![](media/image39.png){width="4.426562773403324in" height="2.6875in"}\
\
Verify ipProtocol in the Name field\
\
Enter tcp as the Value\
\
![](media/image40.png){width="4.496527777777778in" height="3.65625in"}

Choose Ok\
\
Choose the OUT tab

![](media/image41.png){width="6.5in" height="1.1805555555555556in"}\
Click the virtual Source Parameter\
\
Choose the Create parameter/attribute in workflow link\
\
Enter F5Vip in the Name field *(This will create an output variable
called F5Vip that\
references the new F5 virtual)*\
\
![](media/image42.png){width="4.5in" height="4.699998906386702in"}

-   Choose Ok

-   Choose Close

-   Choose Save

Choose Validate (This will throw an error showing the the parameter
virtual is\
never used. We change this in step 20 when we changed the name to F5Vip)

![](media/image43.png){width="6.5in" height="1.9770833333333333in"}

![](media/image44.png){width="6.5in" height="3.509027777777778in"}

-   Choose Delete parameter under the Quick fix action for both results
    (we replaced virtual with F5Vip)

-   Choose Close

-   Choose Save

\
Add the Set Virtual Server SNAT to the custom workflow 
-------------------------------------------------------

Expand the All Workflows heading on the left and navigate to
F5/Ltm/Virtual\
Server\
\
Drag and drop the Set Virtual Server SNAT workflow to the right of
Create\
Virtual Server on the Schema tab

Choose Setup in the upper right\
\
![](media/image45.png){width="5.916666666666667in"
height="2.0208333333333335in"}

Verify that bigip is set to input

Choose F5Vip in the virtual drop down and check Value\
\
Set type to Value and enter automap\
\
Set pool to Skip

Original\
![](media/image46.png){width="4.4966655730533684in"
height="3.1199989063867015in"}\
\
Choose Promote

Mouse over the Set Virtual Server SNAT workflow\
\
Choose the Pencil to edit\
![](media/image37.png){width="0.8854166666666666in"
height="0.7708333333333334in"}

Click the bigip parameter

![](media/image47.png){width="5.480722878390202in"
height="3.6145833333333335in"}

Double click the NULL Source parameter next to the bigip Local Parameter

Choose the bigip in-parameter\
\
Choose Select

![](media/image48.png){width="6.5in" height="4.913194444444445in"}

Select Close

Select save

Choose the Presentation tab\
\
Set the following descriptions for the variables:\
Address: **IP Address for the pool member**\
Port: **IP Port for the pool member**\
Destination: **VIP IP address and port &lt; ipaddress:port &gt;**\
\
Choose Save and Close

\
Run the CustomWF1workflow
-------------------------

Right Click on the CustomWorkflows&gt;CustomWF1 workflow

-   Start Workflow.

-   Click the Not set link in the BIG-IP field. This will open the
    Inventory Browser\
    dialog.

-   Select the F5 Networks &gt; 10.128.1.245 inventory object.

-   Click Select.

-   Enter **CustWF1-102** in the LTM object name field.

-   Enter **http** in the Monitor to be used by LTM Pool field

-   Enter **10.128.20.11** in the Address for the pool member field

-   Enter **80** for the Port for the pool member field

-   Enter **10.128.10.102:80** for the Destination ipaddress:port field

-   Click Submit.

    ![](media/image49.png){width="6.5in" height="3.3854166666666665in"}

Verify your new virtual and pool on your BIG-IP GUI

-   <https://10.128.1.245>

    -   Username: admin

    -   Password: admin

-   Browse through the LTM components (Virtual Server, Pool, etc)

Verify your new virtual from the Client GUI

-   Select Administer From Pulldown

-   Note: A significant amount of data is pulled into the client. This
    is a scheduled activity at 5 minute intervals. Changes may not
    appear immediately.

-   Expand the LTM items (Pool, Virtual, Virtual Address)

![](media/image50.png){width="6.0125in" height="5.864583333333333in"}
