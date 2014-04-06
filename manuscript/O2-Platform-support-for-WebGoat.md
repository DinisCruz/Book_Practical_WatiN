##  O2 Platform support for WebGoat 

_(just sent to the OWASP WebGoat mailing list)_

Hi WebGoat Crowd

  


I finally started adding support for WebGoat to the [OWASP O2 Platform](http://www.owasp.org/index.php/OWASP_O2_Platform) by creating an API for Web Goat and showing how to automate a number of WebGoat's tasks and lessons. I've started to documentat how to use it and how it works, which you can read it (or watch the video) here: [http://o2platform.com/wiki/WebGoat/First_Example_of_O2_WebGoat_API](http://o2platform.com/wiki/WebGoat/First_Example_of_O2_WebGoat_API) 

  


At the moment there is a first pass at an API for WebGoat (see [API_WebGoat.cs](http://www.blogger.com/API_WebGoat.cs)) and a number of Unit Tests (see [WebGoat_BlackBox_Exploits.cs](http://www.blogger.com/WebGoat_BlackBox_Exploits.cs)). Both these files are available locally when you[ install the O2 Platform](http://o2platform.googlecode.com/files/OWASP%20O2%20Platform%20%28v1.0%20Beta%29.msi) and the [WebGoat_BlackBox_Exploits.cs](http://www.blogger.com/WebGoat_BlackBox_Exploits.cs) can be executed by double-clicking on it or dragging'n'dropping it into a loaded O2 instance (see video for an example)

  
So far the following browser-driven UnitTests/WebAutomation workflows have been implemented:

  * Download the latest version of WebGoat (starting in the OWASP website and ending with the download 'save as' pop-up from Google Code)
  * Start a local copy of WebGoat
  * Open the WebGoat HomePage (with support for auto submitting the WebGoat's Basic Auth requirement (using guest:guest), and clicking on the 'Start WebGoat' button)
  * Complete the Lab: "Cross-Site Scripting (XSS) --> LAB: Cross Site Scripting --> Stage 1: Stored XSS " using two injection points: the "address1" field (which is vulnerable to XSS) and the "description" field (which is NOT vulnerable to XSS)

The Stored XSS lesson is a good example of O2's powerful web automation capabilities (using WatiN's engine on top of IE) since it shows how complex web workflows can be easily created, tested and packaged:

  


  


  

    
    [Test]  
      public string Exploit_Stage_1_Stored_XSS_OK()  
      {  
                return Exploit_Stage_1_Stored_XSS("address1");  
      }
    
      [Test]  
      public string Exploit_Stage_1_Stored_XSS_Fail()  
      {  
       return Exploit_Stage_1_Stored_XSS("description");  
      }

private string Exploit_Stage_1_Stored_XSS(string fieldToInsertPayload)  
{  
setup();  
var payload = "[Over me to see xss](http://www.blogger.com/%22/%22)";  
webGoat.openMainPage();   
ie.link("Cross-Site Scripting (XSS)").flash().click();  
ie.link("LAB: Cross Site Scripting").flash().click();   
ie.link("Stage 1: Stored XSS").flash();  
ie.field("password").flash().value("larry");  
ie.button("Login").flash().click();  
ie.selectLists()[1].options()[0].select().flash();  
ie.button("ViewProfile").flash().click();  
ie.button("EditProfile").flash().click();   
ie.field(fieldToInsertPayload).value(payload).flash();   
ie.button("UpdateProfile").flash().click();   
Assert.That(ie.html().contains("onmouseover=\"javascript:alert('xss')\""), "Payload was not inserted into page");  
return "ok";  
}

  


The sample code above (from [WebGoat_BlackBox_Exploits.cs](http://www.blogger.com/WebGoat_BlackBox_Exploits.cs) is designed to be easy to read and write/test (using one of O2's development/test environments).  


  


For example:  
  

    
    **_ie.link("LAB: Cross Site Scripting").flash().click();_**
    
    will:

  

* in the current page, find the link called "Lab: Cross Site Scripting" 
  
  

* flash the link 3 times (i.e. scroll the browser to the current page location of the link (which could be outside the user's view) and set its background color to yellow for a couple seconds)
  

* click on the link
  

* wait for the linked page to load completely before continuing execution 
  
  
  

    
    **_ie.field("password").flash().value("larry");_**
    
    will:

  

* find the field called "password" in the current page
  

* flash it 3 times
  

* set the value of the field to "larry"
  
  


  

    
    _**ie.selectLists()[1].options()[0].select().flash();**_
    
    will:
    
      
    
    * get a list of 'Selection Lists' from the current page (these are also called DropDownList/ComboBox)
      
    
    * select the 2nd SelectionList (we couldn't get the reference using the name since its HtmlElement didn't seem to have a value that could be used to find it)
      
    
    * get the current Options of the chosen SelectionList (this is the list of the DropDownList options/values)
      
    
    * chose the first option and selected it
      
    
    * Flash the selected option so the user can see which one we chose
      
      
    
    
      
      
    
    
      
    
    
      
      
    
    
      
    
    
      
    Hopefully this all makes sense, and you (webGoat community) can now continue this from here.
    
      
    
    
      
      
    
    
      
    
    
      
    Ideally we should have all WebGoat Lessons mapped using these Security UnitTests, and once that is done, create a 'secure version of WebGoat' that passes all those UnitTests (assuming the UnitTests are designed to succeed when the vulnerability was exploited)
    
      
    
    
      
      
    
    
      
    
    
      
    Please have a go and let me know if you have any problems or ideas on how to improve it (see the video on [http://o2platform.com/wiki/WebGoat/First_Example_of_O2_WebGoat_API](http://o2platform.com/wiki/WebGoat/First_Example_of_O2_WebGoat_API) if you have questions on how to use the [WebGoat_BlackBox_Exploits.cs](http://www.blogger.com/WebGoat_BlackBox_Exploits.cs) script)
    
      
    
    
      
      
    
    
      
    
    
      
    Dinis Cruz  
    
    
      
    
