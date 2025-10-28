# The General Structure of Penetration Testing Notes

Example of a Testing Note.
>``` shell
>Testing for Cross-Site Scripting 
>
>Testing Target: 192.168.1.52 
>Application:    XSSBlog
>Date Started:   31 March 2022
>
>1. Navigated to the application
>    http://192.168.1.52/XSSBlog.html
>    Result: Blog page displayed as expected
>    
>2. Entered our standard XSS test data: 
>    You will rejoice to hear that no disaster has accompanied the
>    commencement of an enterprise which you have regarded with such
>    evil forebodings.<script>alert("Your computer is infected!");</script> 
>    I arrived here yesterday, and my first task is to assure my dear
>    sister of my welfare and increasing confidence in the success of
>    my undertaking. 
>
>3. Clicked Submit to post the blog entry.
>    Result: Blog entry appeared to save correctly.
>
>4. Navigated to read the blog post
>    http://192.168.1.52/XSSRead.php
>    Result: The blog started to display and then the expected alert popped up.
>
>5. Test indicated the site is vulnerable to XSS.
>
>PoC payload: <script>alert(‘Your computer is infected!')</script>
>```
