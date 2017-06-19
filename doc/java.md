# Create and deploy Java application on Cloud Foundry
## Technology involved:
* Latest Java JDK
* Eclipse IDE for Java EE Developers Luna
* IBM Eclipse Tools for Bluemix. 
* The Liberty profile V8.5.5.5 (or later) runtime. You can either install it directly or download a Liberty profile installation from within Eclipse. If given a choice, select web profile.
## Steps:
## 1. Create And Modify your java application <a name="1">
The command that you just ran will: 

### 1.1 Import the app into your Eclipse workspace  <a name="1.1">
Starting with this step, you'll begin to examine and modify the code. The Eclipse IDE makes it easy to work with the code and navigate the big project directories tree when you develop Java web applications.
* Start your Eclipse IDE and select **File > Import**. Then select **General > Existing Projects into Workspace**:

<img src="../img/3.1.png" align="left" width="40%"  >
<br clear="all" />

* Click the **Select archive file** option: 
<img src="../img/3.2.png" align="left" width="40%"  >
<br clear="all" />

* Browse to and select the lllproject.zip file.
Click **Finish**.   
he LaurenLandscapesJava project is now imported into your workspace. You can see its structure in the Enterprise Explorer pane on the left. The next step familiarizes you with the project and the code.   

### 1.2 Modify the code  <a name="1.2">
With your project open in Eclipse, take a look at the Enterprise Explorer pane on the left:
<img src="../img/4.1.png" align="left" width="40%"  >
<br clear="all" />


Expand Java Resources to see the Java source code files. Expand WebContent to see the four JSP files that make up the website:

This diagram shows how the app works:

<img src="../img/4.2.png" align="left" width="60%"  >
<br clear="all" />


Web requests for a page of the Lauren's Lovely Landscapes store first go through the DispatchServlet and then are forwarded to one of the JSP pages. The DispatchServlet attaches an instance of the WebsiteTitle POJO (Plain Old Java Object) to the request. The request is passed on to the JSP page. The JSP page uses the WebsiteTitle to set the title to Lauren's Lovely Landscapes.

If you examine the DispatchServlet source code, you can see the servlet path mapping specified with the @WebServlet annotation:

```
@WebServlet({ "/home", "/antarctica", "/alaska", "/arctic", "/australia"})
public class DispatchServlet extends HttpServlet {
...
```
In this case, all four paths —/home, /antarctica, /alaska, and /australia— map to DispatchServlet. The Liberty profile hands the request for these paths first to the DispatchServlet.

In DispatchServlet, you can also see the code that attaches an instance of WebsiteTitle as an attribute of the request:   
```
request.setAttribute("myapp", myapp);
```
If you examine one of the JSP files — alaska.jsp, for instance — you can see the Expression Language (EL) code that fetches the title:
```
<head>
    <title> ${myapp.title} </title>
...
```

### 1.3 Create and bind a service to the application
You're now ready to run the app locally on an instance of the Liberty profile that's managed by Eclipse:

Select the project in the Enterprise Explorer, right-click, and select **RunAs > Run on Server...** to open a server-selection dialog box.
    
Expand the localhost folder and select the local Liberty profile server: 

<img src="../img/5.1.png" align="left" width="40%"  >
<br clear="all" />


The selection you just made starts the local instance of the Liberty profile, loads the app, and points the Eclipse internal browser to the running application: 

<img src="../img/5.2.png" align="left" width="60%"  >
<br clear="all" />


Try out this instance of the application and see if you notice any difference from the Bluemix-hosted one. Because you're looking at the same app, produced with the same code, there should be no noticeable differences between the two.

### 1.4 Run JUnit tests
It's good Java coding practice to write unit tests for your classes.

The WebsiteTitle class comes with two unit tests. To run the tests, follow this sequence:
* Stop the app by clicking the square red button in the server pane.
* Select the project in the Enterprise Explorer.
* Right-click and select **Run As > JUnit Tests**.

You can see both tests being run. Green status indicates that all unit tests were successful:

<img src="../img/6.1.png" align="left" width="60%"  >
<br clear="all" />

# Deploy your Java Application 
### 2.1 As an app
In this step, you'll modify the price of a print and see it updated on the locally running website right away.
* In the Enterprise Explorer in Eclipse, click the antarctica.jsp file and look for the price in the source code.
* Change the price from 100.00 to 99.99 and save the file. The changed code should look like: 
```
<div id="price">99.99</div>
```
* Run the app on the local Liberty Profile server again by selecting the project and right-clicking **Run as > Run on server**.
* Use the built-in browser in Eclipse to browse to the app.
* Select the Antarctica print and note the print's changed price.

### 2.2 Using Diego
To ensure that your code changes don't break anything, get into the habit of running unit tests every time an app is modified.

To rerun the unit tests:

* Make sure the server has stopped running the app.
* Select the project in the Enterprise Explorer.
* Right-click and select **Run As... > JUnit Tests**.

Once again, you see the green status, indicating that all unit tests were successful.

### 9. Deploy the changed code to Bluemix
To let everyone on the Internet know about the Antarctica print's new price, you'll deploy the changed app to Bluemix. In this step, you see an even easier way to deploy the project to Bluemix than using a WAR file — namely, using the IBM Eclipse Tools for Bluemix to package up your Liberty profile server instance and deploy it on Bluemix:

* In Eclipse, stop the local Liberty profile server instance by clicking the square red button. Now you can see the stopped status associated with the server: 

<img src="../img/9.1.png" align="left" width="60%"  >
<br clear="all" />


* Right-click the Liberty profile server in the Servers pane and select **Utilities > Package Server to IBM Bluemix**. You may need to add the Bluemix server: Click the **Add Server... **link in the **Select IBM Bluemix Server** dialog and follow the instructions.
* Log in to your Bluemix account when prompted.
* Provide a name for the application. You can either reuse the existing app name or create a new one: 

<img src="../img/9.2.png" align="left" width="40%"  >
<br clear="all" />


* In the Launch deployment dialog box, you can increase the memory limit if you like, but for this app 512MB is sufficient: 

<img src="../img/9.3.png" align="left" width="40%"  >
<br clear="all" />


* Click **Finish** to start the deployment. You see a series of status messages, and the server is deployed and started on Bluemix.

After successful deployment, try out the app by pointing any web browser to:
**https://<b><i>app-name</i></b>.mybluemix.net/*
