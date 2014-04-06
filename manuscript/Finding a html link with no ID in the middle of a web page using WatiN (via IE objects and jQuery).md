##  Finding a html link with no ID in the middle of a web page using WatiN (via IE objects and jQuery) 

When coding web automation scripts,  a common problem is that the target HTML element that we want to access doesn't have any id or attribute that we can use to map it.

This posts shows a number of examples on how to to find those HTML elements using [O2 Platform](http://blog.diniscruz.com/p/owasp-o2-platform.html)'s WatiN/IE extension methods.

Lets start with this html code sample (see end of post for the actual code)

[![image](images/image_thumb.png)](http://lh6.ggpht.com/-K3X0yx4D9xg/UXEmh1Mp3XI/AAAAAAAABcA/uDAjLIsDo10/s1600-h/image%25255B2%25255D.png)

If you execute it on a O2 Platform's [C# REPL GUI](http://blog.diniscruz.com/p/c-repl-script-environment.html) you should get something like this:

[![image](images/image_thumb_25255B2_25255D.png)](http://lh4.ggpht.com/-DFou8NCo8ug/UXEmk45jqII/AAAAAAAABcQ/b2xSatRk1go/s1600-h/image%25255B8%25255D.png)

Note: our objective is to trigger the onClick event on the main link, which should pop up an alert:

[![image](images/image_thumb_25255B3_25255D1.png)](http://lh5.ggpht.com/-IA_naM1OdpA/UXEmnn1tX5I/AAAAAAAABcg/cNmnziT8eG0/s1600-h/image%25255B11%25255D.png)

**Example 1) CLICK THE LINK: Using list of links **  
**  
**The first way to click on the lists is to get a list of links:

[![image](images/image_thumb_25255B4_25255D.png)](http://lh6.ggpht.com/-3NjupdUqDHg/UXEmpho2bkI/AAAAAAAABcw/OFfLF26B6Qo/s1600-h/image%25255B14%25255D.png)

find the one we want ( **_ie.links()[1])_** , **_ie.links().value(1)_** or **_ie.links().item(1)_** would also work, with the last two returning null if the index provided is took big)

[![image](images/image_thumb_25255B5_25255D1.png)](http://lh4.ggpht.com/-KXCw8xJYk5c/UXEmr8lzIlI/AAAAAAAABdA/mYf1iQ3EogY/s1600-h/image%25255B17%25255D.png)

(note how you get a lot of details about the current element in the Output window)

Once we have the link we want we can just. **_click()_** it :)

[![image](images/image_thumb_25255B6_25255D1.png)](http://lh3.ggpht.com/-hVNfIHcFAss/UXEmuXD0iII/AAAAAAAABdQ/9zAmcnQGvw4/s1600-h/image%25255B20%25255D.png)

Tip: if you are doing a demo you might want to add a .flash() to highlight the element before clicking on it:

[![image](images/image_thumb_25255B7_25255D1.png)](http://lh3.ggpht.com/-mUgGbRnk2-s/UXEmw0jysaI/AAAAAAAABdg/jj1ufwZNWow/s1600-h/image%25255B23%25255D.png)

and use **_.disableFlashing()_** during development (so you don't have to wait for it):

[![image](images/image_thumb_25255B8_25255D.png)](http://lh6.ggpht.com/-DJErQ_D1S0M/UXEmzUavc1I/AAAAAAAABdw/0l6V_aods0k/s1600-h/image%25255B26%25255D.png)

**Example 2) CLICK THE LINK: Using innerHTML**  
**  
**Another way to find the link is to use the **_.innerHtml() _**value:

[![image](images/image_thumb_25255B9_25255D1.png)](http://lh6.ggpht.com/-SP5ceLFNnVk/UXEm1aHXENI/AAAAAAAABeA/4IVXPSmPPjE/s1600-h/image%25255B29%25255D.png)

which can be used on a foreach loop:

[![image](images/image_thumb_25255B10_25255D1.png)](http://lh6.ggpht.com/-8JUWPcWbkME/UXEm4K-AC8I/AAAAAAAABeQ/gfzJegQFQRo/s1600-h/image%25255B32%25255D.png)

to find and click it:

[![image](images/image_thumb_25255B11_25255D.png)](http://lh6.ggpht.com/-b6td0IMIdTM/UXEm6e63BGI/AAAAAAAABeg/RDfgO2raoOY/s1600-h/image%25255B35%25255D.png)

if you wanted to store the link in a strongly type variable, use the .**_getFullName()_** method to find the type:

[![image](images/image_thumb_25255B12_25255D.png)](http://lh4.ggpht.com/-A4DSxd5cGAA/UXEm8nIEKzI/AAAAAAAABew/SYJumcji8Mo/s1600-h/image%25255B38%25255D.png)

And then use a variable to hold its value before you click it:

[![image](images/image_thumb_25255B13_25255D.png)](http://lh3.ggpht.com/-SscrOyWm93Q/UXEm-iKnKRI/AAAAAAAABfA/UNWDNNHOQI0/s1600-h/image%25255B41%25255D.png)

We can refactor this code by moving the **_WatiN.Core _**reference into an **_using_** statement (which can be anywhere on the script, but are usually included at the end)

[![image](images/image_thumb_25255B14_25255D.png)](http://lh3.ggpht.com/-E36EV7PHEx0/UXEnAqOziVI/AAAAAAAABfQ/CYPmfuMqmkE/s1600-h/image%25255B44%25255D.png)

and replacing the foreach with a lambda method:

[![image](images/image_thumb_25255B15_25255D.png)](http://lh5.ggpht.com/-rhnjRcRdeC8/UXEnCxjwvzI/AAAAAAAABfg/XyqEfNRLTxE/s1600-h/image%25255B47%25255D.png)

and in this case we don't even need the **_foundLink_** variable:

[![image](images/image_thumb_25255B16_25255D.png)](http://lh3.ggpht.com/-44o3gWwogiE/UXEnE6TaA6I/AAAAAAAABfw/5s_nZS9td0o/s1600-h/image%25255B50%25255D.png)

or use a smaller text to search for the link:

[![image](images/image_thumb_25255B17_25255D.png)](http://lh3.ggpht.com/-CBO9qxfEEC0/UXEnG08614I/AAAAAAAABgA/QMIt1cor_3M/s1600-h/image%25255B53%25255D.png)

**Example 3) CLICK THE LINK: Via an Image**  
**  
**Another way to find a link is to search for something that we know is inside it (like an image)

[![image](images/image_thumb_25255B18_25255D.png)](http://lh5.ggpht.com/-G-FHnKLwEBk/UXEnJLQzn2I/AAAAAAAABgQ/Qd_hGhaG1IE/s1600-h/image%25255B56%25255D.png)

get its parent:

[![image](images/image_thumb_25255B20_25255D1.png)](http://lh4.ggpht.com/-nv7ky_PmUVY/UXEnLQ5d-AI/AAAAAAAABgg/gbOxfPtGi5I/s1600-h/image%25255B62%25255D.png)

cast it into a **_Link _**object and click it:

[![image](images/image_thumb_25255B21_25255D1.png)](http://lh4.ggpht.com/-QdaaEVKtEII/UXEnNldgXyI/AAAAAAAABgw/l7XQvvR2T5I/s1600-h/image%25255B65%25255D.png)

Tip: you can use an element/image attributes to find the image/element you want:

[![image](images/image_thumb_25255B22_25255D1.png)](http://lh5.ggpht.com/-Gfh9cZwOHxg/UXEnPwDs6tI/AAAAAAAABhA/RJHB90rPpAI/s1600-h/image%25255B68%25255D.png)

  
**Example 4) CLICK THE LINK: Using jQuery**  
**  
**If you are a jQuery fan, then you can also use it to click on the link.

Start by adding jQuery (if not there)

[![image](images/image_thumb_25255B23_25255D1.png)](http://lh5.ggpht.com/-pwpZ8sM2CL8/UXEnSMkjgfI/AAAAAAAABhQ/uiXU4HaA6F4/s1600-h/image%25255B71%25255D.png)

And here is how to use jQuery to find the link:

[![image](images/image_thumb_25255B24_25255D1.png)](http://lh3.ggpht.com/-cnXu3fGGZiU/UXEnUVGwATI/AAAAAAAABhg/lyY77FIqfPk/s1600-h/image%25255B74%25255D.png)

And how to click on the link (using a jQuery selector):

[![image](images/image_thumb_25255B25_25255D1.png)](http://lh3.ggpht.com/-b7w7yLq2bXA/UXEnWgqTgBI/AAAAAAAABhs/BolHuJmJeiQ/s1600-h/image%25255B77%25255D.png)

**Tip:** Inject FireBug Lite to make your life easier writing the scripts:

[![image](images/image_thumb_25255B26_25255D1.png)](http://lh3.ggpht.com/-QfGMq28tI-M/UXEnZDVRNJI/AAAAAAAABiA/VO_EJYnKHOU/s1600-h/image%25255B80%25255D.png)

which will allow to you try out your jQuery scripts:

[![image](images/image_thumb_25255B27_25255D1.png)](http://lh3.ggpht.com/-078YORICA5U/UXEncMPVPwI/AAAAAAAABiQ/G9qoQEltJPk/s1600-h/image%25255B83%25255D.png)

That's it, let me know if this helps and if you have other ways to find those annoying HTML elements with no id attribute :)

### **Code samples used on this post**

here are some of the code samples used to create the screenshots shown above:

**Loading test HTML in Web Browser:**  

    
    var ie = panel.clear().add_IE().silent(false);   
    //original HTML that would be found in the middle of a big web page  
    var targetHtml = @"<A TABINDEX=""1""  
                        onKeyDown=""KeyEnter(this)""   
                        onClick=""javascript:alert('I was clicked')""  
                        STYLE=""cursor:hand;""   
                        onMouseOut=""SetImage('TaskList','');""  
                        onMouseDown=""SetImage('TaskList','pressed');""   
                        TARGET=""main"">  
                      <IMG NAME=""TaskList"" SRC=""images/TaskList.jpg"" ALT=""Show Your Task List"" BORDER=""0""/></A>";

//adding a couple links before and after to make the sample a bit more realistic  
var codeSample = "<a href=''>a link before</a><br/>{0} <br/><a href=''>a link after</a><br/>".format(targetHtml);

//load the html into the current IE object (you could also save the text as an HTML page and open that)  
ie.set_Html(codeSample);

return ie.IE.Html;  
  
**Clicking using innerHtml via foreach loop:**  

    
    var ie = panel.clear().add_IE().silent(false);   
    //original HTML that would be found in the middle of a big web page  
    var targetHtml = @"<A TABINDEX=""1""  
                        onKeyDown=""KeyEnter(this)""   
                        onClick=""javascript:alert('I was clicked')""  
                        STYLE=""cursor:hand;""   
                        onMouseOut=""SetImage('TaskList','');""  
                        onMouseDown=""SetImage('TaskList','pressed');""   
                        TARGET=""main"">  
                      <IMG NAME=""TaskList"" SRC=""images/TaskList.jpg"" ALT=""Show Your Task List"" BORDER=""0""/></A>";

//adding a couple links before and after to make the sample a bit more realistic  
var codeSample = "<a href=''>a link before</a><br/>{0} <br/><a href=''>a link after</a><br/>".format(targetHtml);

//load the html into the current IE object (you could also save the text as an HTML page and open that)

ie.set_Html(codeSample);

var innerHtmlToFind = "<IMG border=0 name=TaskList alt=\"Show Your Task List\" src=\"images/TaskList.jpg\">";

WatiN.Core.Link foundLink = null;

foreach(var link in ie.links())  
if (link.innerHtml() == innerHtmlToFind)  
{  
foundLink = link;  
break;  
}  
if (foundLink.notNull())  
foundLink.click();

return foundLink;

//return ie.links().second().innerHtml();

  
//O2File:WatiN_IE_ExtensionMethods.cs   
//O2Ref:WatiN.Core.1x.dll  
  
**Clicking using innerHtml via Lambda method:**  

    
    var ie = panel.clear().add_IE().silent(false);   
    //original HTML that would be found in the middle of a big web page  
    var targetHtml = @"<A TABINDEX=""1""  
                        onKeyDown=""KeyEnter(this)""   
                        onClick=""javascript:alert('I was clicked')""  
                        STYLE=""cursor:hand;""   
                        onMouseOut=""SetImage('TaskList','');""  
                        onMouseDown=""SetImage('TaskList','pressed');""   
                        TARGET=""main"">  
                      <IMG NAME=""TaskList"" SRC=""images/TaskList.jpg"" ALT=""Show Your Task List"" BORDER=""0""/></A>";

//adding a couple links before and after to make the sample a bit more realistic  
var codeSample = "<a href=''>a link before</a><br/>{0} <br/><a href=''>a link after</a><br/>".format(targetHtml);

//load the html into the current IE object (you could also save the text as an HTML page and open that)

ie.set_Html(codeSample);

var innerHtmlToFind = "name=TaskList alt=\"Show Your Task List\" src=\"images/TaskList.jpg\">";

  
return ie.links()  
.where((link)=> link.innerHtml() == innerHtmlToFind)  
.first()  
.click();

  
//using WatiN.Core  
//O2File:WatiN_IE_ExtensionMethods.cs   
//O2Ref:WatiN.Core.1x.dll  
  
**Click using an Image's parent:**  

    
    var ie = panel.clear().add_IE().silent(false);   
    //original HTML that would be found in the middle of a big web page  
    var targetHtml = @"<A TABINDEX=""1""  
                        onKeyDown=""KeyEnter(this)""   
                        onClick=""javascript:alert('I was clicked')""  
                        STYLE=""cursor:hand;""   
                        onMouseOut=""SetImage('TaskList','');""  
                        onMouseDown=""SetImage('TaskList','pressed');""   
                        TARGET=""main"">  
                      <IMG NAME=""TaskList"" SRC=""images/TaskList.jpg"" ALT=""Show Your Task List"" BORDER=""0""/></A>";

//adding a couple links before and after to make the sample a bit more realistic  
var codeSample = "<a href=''>a link before</a><br/>{0} <br/><a href=''>a link after</a><br/>".format(targetHtml);

//load the html into the current IE object (you could also save the text as an HTML page and open that)

ie.set_Html(codeSample);

var link = (Link)ie.images().first().Parent;  
link.click();  
return "link clicked";

//using WatiN.Core  
//O2File:WatiN_IE_ExtensionMethods.cs   
//O2Ref:WatiN.Core.1x.dll  
  
**Click using jQuery**  

    
    var ie = panel.clear().add_IE().silent(false);   
    //original HTML that would be found in the middle of a big web page  
    var targetHtml = @"<A TABINDEX=""1""  
                        onKeyDown=""KeyEnter(this)""   
                        onClick=""javascript:alert('I was clicked')""  
                        STYLE=""cursor:hand;""   
                        onMouseOut=""SetImage('TaskList','');""  
                        onMouseDown=""SetImage('TaskList','pressed');""   
                        TARGET=""main"">  
                      <IMG NAME=""TaskList"" SRC=""images/TaskList.jpg"" ALT=""Show Your Task List"" BORDER=""0""/></A>";

//adding a couple links before and after to make the sample a bit more realistic  
var codeSample = "<a href=''>a link before</a><br/>{0} <br/><a href=''>a link after</a><br/>".format(targetHtml);

//load the html into the current IE object (you could also save the text as an HTML page and open that)

ie.set_Html(codeSample);

ie.inject_jQuery();

ie.eval("$('a').eq(1).click();");

//using WatiN.Core  
//O2File:WatiN_IE_ExtensionMethods.cs   
//O2Ref:WatiN.Core.1x.dll  

