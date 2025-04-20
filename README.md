*********************************************************WEB SERVICES*************************************************

slip16a
Step 1: Create the Dynamic Web Project
Open Eclipse.
Go to File > New > Dynamic Web Project.
Project Name: WeightConverterService
Choose target runtime: Apache Tomcat
Click Finish


Step 2: Create a Web Service Java Class
-Right-click src > New > Package: com.service
-Inside that package, create a new class: WeightConverter.java

//WeightConverter.java

package com.service;

import javax.jws.WebMethod;
import javax.jws.WebService;

@WebService
public class WeightConverter {

@WebMethod
public double kgToGram(double kg) {
return kg * 1000;
}
}


Step 3: Convert to Web Service
Right-click on WeightConverter.java
Choose Web Services > Create Web Service
Move the slider to "Develop service"
Click Finish


Part 2: Create Web Service Client Project
Project Name: WeightConverterClient

//Step 1: Create the Dynamic Web Project
File > New > Dynamic Web Project
Name: WeightConverterClient
Use same runtime (Tomcat)
Finish

Step 2: Generate Client Stubs using WSDL
Right-click src > New > Package: com.client
Right-click WeightConverterClient project > New > Other > Web Service Client
Enter WSDL URL from above:
http://localhost:8080/WeightConverterService/services/WeightConverter?wsdl
Choose output folder: src > package: com.client
Finish


Step 3: Create a Servlet to Consume the Service
//Right-click src > New > Servlet: WeightTestServlet

package com.client;

import java.io.IOException;
import javax.servlet.*;
import javax.servlet.http.*;

public class WeightTestServlet extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

response.setContentType("text/html");

com.service.WeightConverterServiceLocator locator = new com.service.WeightConverterServiceLocator();
try {
com.service.WeightConverter port = locator.getWeightConverter();
double kg = 5.0;
double gram = port.kgToGram(kg);

response.getWriter().println("<h1>Converted " + kg + " kg to " + gram + " grams</h1>");
} catch (Exception e) {
e.printStackTrace(response.getWriter());
}
}
}


Step 4: Map the Servlet in web.xml
<servlet>
    <servlet-name>WeightTestServlet</servlet-name>
    <servlet-class>com.client.WeightTestServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>WeightTestServlet</servlet-name>
    <url-pattern>/testWeight</url-pattern>
</servlet-mapping>


//steps to start before running
1Start Tomcat Server in Eclipse.

2.Make sure both projects are added to the server.

3.Run the service project first and verify the WSDL URL in the browser.

4.Then run the client project and go to:

http://localhost:8080/WeightConverterClient/testWeight

--------------------------------------------------------------------------------------------------------------
slip16b

//steps
Step 1: Create Dynamic Web Project
1.Go to File > New > Dynamic Web Project
2.Name: StationeryPriceService
3.Target runtime: Apache Tomcat
 
//Step 2: Create Service Class
//Create package: com.service
//Create class: PriceService.java

package com.service;

import javax.jws.WebMethod;
import javax.jws.WebService;
import java.util.HashMap;

@WebService
public class PriceService {

@WebMethod
public String getPrice(String itemName) {
HashMap<String, String> priceList = new HashMap<>();
priceList.put("Pen", "10 INR");
priceList.put("Pencil", "5 INR");
priceList.put("Notebook", "40 INR");
priceList.put("Eraser", "3 INR");

return priceList.getOrDefault(itemName, "Item not found");
}
}


//Step 3: Convert to Web Service
//1.Right-click PriceService.java
//2.Select: Web Services > Create Web Service
//3.Move the slider to Develop service
//4.Finish

//Step 4: Verify WSDL
1.Visit this in your browser:
http://localhost:8080/StationeryPriceService/services/PriceService?wsdl


//Part 2: Client Project
📦 Project Name: StationeryClientApp
Step 1: Create Dynamic Web Project
File > New > Dynamic Web Project
//Name: StationeryClientApp
//Target runtime: Apache Tomcat
//Finish

Step 2: Generate Web Service Client
Right-click StationeryClientApp > New > Other > Web Service Client


Step 3: Create a Servlet
Package: com.client
Servlet name: PriceCheckServlet.java

package com.client;

import java.io.IOException;
import javax.servlet.*;
import javax.servlet.http.*;

public class PriceCheckServlet extends HttpServlet {

protected void doGet(HttpServletRequest request, HttpServletResponse response)
throws ServletException, IOException {

String itemName = request.getParameter("item");
response.setContentType("text/html");

try {
com.service.PriceServiceServiceLocator locator = new com.service.PriceServiceServiceLocator();
com.service.PriceService port = locator.getPriceService();

String price = port.getPrice(itemName);

response.getWriter().println("<h2>Item: " + itemName + "</h2>");
response.getWriter().println("<h3>Price: " + price + "</h3>");
} catch (Exception e) {
e.printStackTrace(response.getWriter());
}
}
}



//Step 4: Configure web.xml

<servlet>
    <servlet-name>PriceCheckServlet</servlet-name>
    <servlet-class>com.client.PriceCheckServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>PriceCheckServlet</servlet-name>
    <url-pattern>/getPrice</url-pattern>
</servlet-mapping>


Run on browser:
http://localhost:8080/StationeryClientApp/getPrice?item=Notebook



**********************************************************************************************************************

SLIP18a


 Part 1: Web Service Project
🌐 Project Name: VowelCounterService
Step 1: Create Dynamic Web Project
File > New > Dynamic Web Project
Name: VowelCounterService
Runtime: Apache Tomcat
Click Finish



Step 2: Create Java Class
Package: com.service
Class: VowelService.java


package com.service;

import javax.jws.WebMethod;
import javax.jws.WebService;

@WebService
public class VowelService {

@WebMethod
public int countVowels(String input) {
int count = 0;
String vowels = "aeiouAEIOU";

for (char c : input.toCharArray()) {
if (vowels.indexOf(c) != -1) {
count++;
}
}
return count;
}
}


//Step 3: Publish Web Service
Right-click VowelService.java
Choose Web Services > Create Web Service
Move slider to Develop service
Click Finish


Step 4: Test WSDL in browser
Go to:
http://localhost:8080/VowelCounterService/services/VowelService?wsdl


Part 2: Client Project
📦 Project Name: VowelCounterClient
Step 1: Create Dynamic Web Project
File > New > Dynamic Web Project
Name: VowelCounterClient
Runtime: Apache Tomcat
Finish


Step 2: Generate Web Service Client
1.Right-click VowelCounterClient > New > Other > Web Service Client
2.Enter WSDL URL:
http://localhost:8080/VowelCounterService/services/VowelService?wsdl
3.Set package to com.client
4.Click Finish


Step 3: Create Servlet to call service
Package: com.client
Servlet Name: VowelCheckServlet.java
package com.client;

import java.io.IOException;
import javax.servlet.*;
import javax.servlet.http.*;

public class VowelCheckServlet extends HttpServlet {

protected void doGet(HttpServletRequest request, HttpServletResponse response)throws ServletException, IOException {

String input = request.getParameter("text");
response.setContentType("text/html");

try {
com.service.VowelServiceServiceLocator locator = new com.service.VowelServiceServiceLocator();
com.service.VowelService port = locator.getVowelService();

int count = port.countVowels(input);

response.getWriter().println("<h2>Input: " + input + "</h2>");
response.getWriter().println("<h3>Vowel Count: " + count + "</h3>");
} catch (Exception e) {
e.printStackTrace(response.getWriter());
}
}
}


Step 4: Update web.xml
<servlet>
    <servlet-name>VowelCheckServlet</servlet-name>
    <servlet-class>com.client.VowelCheckServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>VowelCheckServlet</servlet-name>
    <url-pattern>/checkVowels</url-pattern>
</servlet-mapping>


Open this URL:
http://localhost:8080/VowelCounterClient/checkVowels?text=HelloWorld

----------------------------------------------------------------------------------------------------------------------

slip18b


Part 1: StudentPercentageService — Web Service Project
📌 Step 1: Create Dynamic Web Project
Name: StudentPercentageService
Runtime: Apache Tomcat

 Step 2: Create Java Class
Package: com.service
Class: PercentageService.java


package com.service;

import javax.jws.WebMethod;
import javax.jws.WebService;

@WebService
public class PercentageService {

@WebMethod
public float calculatePercentage(int m1, int m2, int m3, int m4, int m5) {
int total = m1 + m2 + m3 + m4 + m5;
return (total / 500.0f) * 100;
}
}


//Step 3: Publish as Web Service
Right-click PercentageService.java
Go to Web Services > Create Web Service
Move slider to "Develop Service"
Click Finish


Step 4: Confirm WSDL in browser
Visit:
http://localhost:8080/StudentPercentageService/services/PercentageService?wsdl


>>>Part 2: ActorInfoClientApp — Client Web Project
📌 Step 1: Create Dynamic Web Project
Name: ActorInfoClientApp

📌 Step 2: Generate Web Service Client (using WSDL from Part 1)
Right-click ActorInfoClientApp > New > Web Service Client
WSDL URL:
->http://localhost:8080/StudentPercentageService/services/PercentageService?wsdl
->Output package: com.client


 Step 3: Create Servlet for Actor + Marks Input
Package: com.client
Class: ActorStudentServlet.java

package com.client;

import java.io.IOException;
import javax.servlet.*;
import javax.servlet.http.*;

public class ActorStudentServlet extends HttpServlet {

protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

String actor = request.getParameter("actor");
int m1 = Integer.parseInt(request.getParameter("m1"));
int m2 = Integer.parseInt(request.getParameter("m2"));
int m3 = Integer.parseInt(request.getParameter("m3"));
int m4 = Integer.parseInt(request.getParameter("m4"));
int m5 = Integer.parseInt(request.getParameter("m5"));

response.setContentType("text/html");

try {
com.service.PercentageServiceServiceLocator locator = new com.service.PercentageServiceServiceLocator();
com.service.PercentageService service = locator.getPercentageService();

float percentage = service.calculatePercentage(m1, m2, m3, m4, m5);

response.getWriter().println("<h2>Actor: " + actor + "</h2>");
response.getWriter().println("<h3>Percentage: " + percentage + "%</h3>");

// Sample actor detail simulation
if (actor.equalsIgnoreCase("SRK")) {
response.getWriter().println("<p>Shah Rukh Khan - Bollywood Actor known for romantic roles.</p>");
} else if (actor.equalsIgnoreCase("Tom Cruise")) {
response.getWriter().println("<p>Tom Cruise - Hollywood actor known for action movies.</p>");
} else {
response.getWriter().println("<p>Actor details not found.</p>");
}

} catch (Exception e) {
e.printStackTrace(response.getWriter());
}
}
}


Step 4: Update web.xml

<servlet>
    <servlet-name>ActorStudentServlet</servlet-name>
    <servlet-class>com.client.ActorStudentServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>ActorStudentServlet</servlet-name>
    <url-pattern>/actorInfo</url-pattern>
</servlet-mapping>


Test the Project
🔗 URL Example:
http://localhost:8080/ActorInfoClientApp/actorInfo?actor=SRK&m1=78&m2=82&m3=85&m4=90&m5=80
