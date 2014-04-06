##  E2E testing AngularJS links and routes using NCrunch, VisualStudio and FluentSharp.WatiN 

In order to have real TDD while developing AngularJS inside VisualStudio, I needed a way to write C# Unit Tests that could be executed in the background by [NCrunch](http://blog.diniscruz.com/search/label/NCrunch) (i.e. in real-time during coding).

Since I wanted to do E2E (End-to-End) testing of the AngularJS app, I needed either a good mocking environment (like the one provided by KarmaJS/AngularJS Mocks) or the real thing (i.e. actually running the app on a local IIS/Cassini server).

If I have the choice, I always prefer to run my tests without mocking (or with as least amount of Mocks as possible), since that allows for a much more realistic test environment, and promotes much better engineering and coding practices.

This post shows how I created such environment and provides a couple examples of C# tests written to check if links created by AngularJS directives and routes are being correctively set.  
  
The objective is to be able to run a test that looks like this:

[![image](images/image_thumb_25255B4_25255D1.png)](http://lh5.ggpht.com/-SguaBS6ytU8/UzCNafVHM6I/AAAAAAAARVA/K6j2WtLwPIo/s1600-h/image%25255B10%25255D.png)

This unit test (written in C# inside VisualStudio) is opening up an AngularJS powered page in an IE 11 browser, and checking to to see if the expected links+urls are there.

To see this in action we can setup a breakpoint in one of the tests (line 109 below) and run the test under the debugger (using NCrunch in this case, but I could also had used the ReSharper NUnit test runner):

[![image](images/image_thumb_25255B6_25255D1.png)](http://lh4.ggpht.com/-E-sNq8Z1q2M/UzCNbkdJAvI/AAAAAAAARVQ/Kcl5Ad2EsEM/s1600-h/image%25255B16%25255D.png)

Here is what is looks like during execution:

[![image](images/image_thumb_25255B7_25255D1.png)](http://lh3.ggpht.com/-p6AIYHo0Rd8/UzCNdCaSfNI/AAAAAAAARVg/r6SILSvJjO4/s1600-h/image%25255B19%25255D.png)   
**  
****Creating the IE TBot page**  
**  
**The '**_IE TBot'_** popup window that you can see at the background (image above),  was created by the **_IE_UnitTest_** class (below), which contains a static **_Current_** properly, and calls the extension method **_open_IE_** on its constructor:

[![image](images/image_thumb_25255B10_25255D1.png)](http://lh4.ggpht.com/-dMXTBA3dEFM/UzCNe2Hoc2I/AAAAAAAARVs/co5E7ri3pYs/s1600-h/image%25255B28%25255D.png)

The **_open_IE_** method is the one that creates the popup window, if there isn't already an instance created and stored in the **_Current_** property.

This really makes a massive difference in the speed of tests since there is no need (in most cases) to start a new instance of IE on a separate WinForm control (this is also compatible with NCrunch since there will be a level of reuse between executions)

[![image](images/image_thumb_25255B11_25255D1.png)](http://lh5.ggpht.com/-TCruyiVZpmM/UzCNgDX8g-I/AAAAAAAARV8/T5mS7dliUB4/s1600-h/image%25255B31%25255D.png)

The **_IE_UnitTest_** class is then extended by the **_API_IE_TBot_** which is the one that contains the **_TBot_** (TeamMentor Admin Bot) specific configuration:

[![image](images/image_thumb_25255B13_25255D1.png)](http://lh3.ggpht.com/-c0Qpm1ManuU/UzCNhgB8HHI/AAAAAAAARWQ/RMMHArKoyjU/s1600-h/image%25255B37%25255D.png)

... and methods:

[![image](images/image_thumb_25255B14_25255D1.png)](http://lh3.ggpht.com/-k9poDWlJNpY/UzCNi2Qnc1I/AAAAAAAARWc/-BKdG-rjZTU/s1600-h/image%25255B40%25255D.png)

Finally the **_API_IE_TBot_** is extended by the NUnit test class like the **_TBot_V2_MainPages_** (shown below) which is the one that also contains the **_Check_UsersMenu_Directive _**method shown in the beginning of this post:

[![image](images/image_thumb_25255B15_25255D1.png)](http://lh6.ggpht.com/-dx-tIustFTM/UzCNkeZUsbI/AAAAAAAARWw/jffCJrkg0Dc/s1600-h/image%25255B43%25255D.png)   
**  
****  
****Seeing Unit Tests in action**  
**  
**One way to run the tests is to use ReSharper NUnit test runner:

[![image](images/image_thumb_25255B17_25255D1.png)](http://lh3.ggpht.com/-mYO4aeKldxM/UzCNl707mzI/AAAAAAAARXA/Ikt6KQmO0LA/s1600-h/image%25255B49%25255D.png)

Looking at the **_Check_Root_Level_Pages_** test (couple screenshots above) you will notice that we are testing for the presence of the "**_TBot v2.0" _**text (which is from the **_main.html_** page, that is mapped as a **templateUrl**,** **by the **_$routeProvider_** config mapping, defined in the **_routes.js_** file):

Here is what the **_main.html_** page looks like at the moment:  
[![image](images/image_thumb_25255B16_25255D1.png)](http://lh6.ggpht.com/-L_suEhiiq3A/UzCNnErVnuI/AAAAAAAARXQ/-IGpjq3zOvY/s1600-h/image%25255B46%25255D.png)

If we change it to:

[![image](images/image_thumb_25255B18_25255D1.png)](http://lh4.ggpht.com/-MfllrBvB9Eo/UzCNokThCII/AAAAAAAARXc/wT-G-KtkXzk/s1600-h/image%25255B52%25255D.png)

... and run the tests again (using ReSharper NUnit test runner), we will get a failed test:

[![image](images/image_thumb_25255B19_25255D.png)](http://lh4.ggpht.com/-zTmpcQbrGYc/UzCNpgVBI2I/AAAAAAAARXs/rnveLDZtWiA/s1600-h/image%25255B55%25255D.png)

This is already quite nice, but since I'm using **_NCrunch_**, I will be able to see this even faster :)

To see this in action, I can run the test from the **_NCrunch_** UI (or make a small change in the **_Check_Root_Level_Pages_** method)

[![image](images/image_thumb_25255B21_25255D1.png)](http://lh5.ggpht.com/-KtcOVuxaUYA/UzCNqhFGGpI/AAAAAAAARYA/XfcYD_KnH8w/s1600-h/image%25255B61%25255D.png)

... and the failed test and location will be nicely visualised like this:

[![image](images/image_thumb_25255B20_25255D1.png)](http://lh4.ggpht.com/-IGU0YfCVUnc/UzCNr28AebI/AAAAAAAARYQ/TsVi-UCT4-A/s1600-h/image%25255B58%25255D.png)

Then as I edit the file, as soon as there is a change made, **_NCrunch_** (after about 1 sec of inactivity) will re-run the test:

[![image](images/image_thumb_25255B23_25255D1.png)](http://lh4.ggpht.com/-3wyinVqNeno/UzCNtt8JyMI/AAAAAAAARYg/iWOqy2-C-Uk/s1600-h/image%25255B67%25255D.png)

... and one I got the fix right, the test will be green again:

[![image](images/image_thumb_25255B22_25255D1.png)](http://lh4.ggpht.com/-E0VtebENUC0/UzCNu82Aj2I/AAAAAAAARYw/LIlR_Q8qwi8/s1600-h/image%25255B64%25255D.png)

It is hard to see/experience this type development environment in these screenshots, but it is really fast, and it really, really, really ...  improves my productivity and UnitTests coverage :)

**Another examples of similar AngularJS types of tests**

1) Check_Top_Links:

[![image](images/image_thumb_25255B2_25255D1.png)](http://lh3.ggpht.com/-gKBokraECis/UzCNwR9xRDI/AAAAAAAARZA/_syzWSlOY0o/s1600-h/image%25255B4%25255D.png)

2) Login into TBot:

[![image](images/image_thumb_25255B24_25255D1.png)](http://lh3.ggpht.com/-t5hbHS2g5NE/UzCNyChun2I/AAAAAAAARZQ/mVwh7UCfBJA/s1600-h/image%25255B70%25255D.png)   
**  
****3) Bug_IE_Out_of_Sync_afer_REST_Call___PoC**  
**  
**[![image](images/image_thumb_25255B25_25255D1.png)](http://lh4.ggpht.com/-G4V9xCrx7-Q/UzCNzocdmWI/AAAAAAAARZc/nLkCUqkf-sw/s1600-h/image%25255B73%25255D.png)
