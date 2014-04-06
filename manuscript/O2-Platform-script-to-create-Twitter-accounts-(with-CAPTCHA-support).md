##  O2 Platform script to create Twitter accounts (with CAPTCHA support) 

Part of the challenge of automating/scripting web application security vulnerabilities is the need to handle multi-stage data inputs.

A good example (and PoC) are account creation wizards, which these days (for example) include a captcha question.

To show how this can be done in O2, I've just coded a script that shows this in action:  


  * [Tool_-_Twitter_Account_Creation.h2](http://www.o2platform.com/wiki/O2_Script/Tool_-_Twitter_Account_Creation.h2) - Wiki page with technical details and screenshots
  * [O2 Platform - Tool - Create Twitter Accounts.avi](http://www.youtube.com/watch?v=Gu8Hg02Zv5c) - YouTube Video 

Here is the code snippet that automates (except for the captcha question) the Twitter account creation using O2's Browser Automation API (WatiN based). The key design-goal of the O2's Browser Automation API  was to make it easier to read (and code):

  
_// ensure that there isn't an logged in session  
ie.open("http://www.twitter.com");  
if (ie.hasLink("Sign out"))  
    ie.link("Sign out").click();  
//open account creation page and populate the fields      
ie.open("http://api.twitter.com/signup");     
ie.textField("user[name]").value(userName);               
ie.textField("user[screen_name]").value(twitterId);   
ie.textField("user[user_password]").value(password);   
ie.textField("user[email]").value(userEmail);  
ie.checkBoxes()[0].uncheck();  
ie.checkBoxes()[1].uncheck();  
ie.buttons()[1].click();   
// ask user to resolve captcha  
var captchaUrl = ie.images()[4].src();  
var captchaAnswer = ascx_CaptchaQuestion.askQuestion(captchaUrl);  
ie.field("recaptcha_response_field").value(captchaAnswer);  
ie.button("Finish").click();_

  * What do you think? 
  * Can you read the script? 
  * Does it make sense? 
  * Can you describe what is going to happen at each line of code?

  
One of the tasks that I want to complete in the short/medium term is to figure out how to execute scripst/workflows like this under other OWASP tools (WebScarab, ZAP, ...) and Commercial 3rd party BlackBox tools (Burp, WebInspect, AppScan, ...) 
