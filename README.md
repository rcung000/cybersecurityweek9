# cybersecurityweek9
Time spent total: 5 Hours
## Challenge Goals:
The following goals **must** be completed:
1. **Username Enumeration:** A careless developer mistake has created a username enumeration vulnerability. Determine which color has the vulnerability. You can use the existing username "jmonroe99" as a test. Next, figure out what mistake the developer made.

2. **Insecure Direct Object Reference:** One of the three sites is missing code which would prevent some sensitive information from being made public. Determine which color has the vulnerability. Then, figure out what the other two sites did correctly to prevent the information leak.

3. **SQL Injection:** Most of the data input received by these websites is being sanitized properly. However, one of the three sites has one place where the input is not being sanitized before being used in an SQL query. Determine which color has the vulnerability.

4. **Cross-Site Scripting:** All three sites do a good job of protecting against a reflected XSS attack. However, one of the sites has a mistake which leaves the site vulnerable to a stored XSS attack. A reflected XSS attack would be easy to reveal, while a stored XSS does not provide instant feedback. You will need to log into the admin area and look through the CMS in order to "spring the trap" and find out if your attack succeeded. Determine which color has the vulnerability. Remember, others will be attacking these sites alongside you. Use your name in the XSS so that your results won't be confused with anyone else's (example: 
`<script>alert('Mallory found the XSS!');</script>).`

5. **Cross-Site Request Forgery:** One of the three sites does not have CSRF protections on the admin area. A clever attacker could design a form which would automatically submit form data to the staff area and take advantage of a logged in user's access permissions. Be the attacker and design a form which will make a change to the spelling of some database content. (For example, change the first user from "James" to "Jim", or change "Alabama" to "Alabamaaaa".) Then point the form action at each of the three sites to find out which color has the vulnerability. Do not neglect to be stealthy with your form—your unsuspecting, logged-in admin should neither see the form nor the results of the form submission.

6. **Session Hijacking/Fixation:** Two of the three websites expire their active sessions and require users to re-login every 30 minutes. That is probably too aggressive for the real world, but it is better than the third site which allows sessions to be a year old, and never regenerates the session ID, even when the user agent string changes. This makes it vulnerable to both session hijacking and session fixation attacks.
There are many ways an attacker could get or set the session ID in the real world (XSS, sniffing WiFi packets, etc.). For this exercise, a PHP script has been provided to help you, "public/hacktools/change_session_id.php". You can load this script to find out the current session ID or to set it to a new one.

Use two different web browsers (e.g., Firefox and Chrome). Let one browser be the attacker and the other the target. Choose if you want to attempt a session hijacking or session fixation. For hijacking, log the target in first, then give the logged-in session ID to the attacker. For fixation, get a session ID for the attacker, give it to the target, then the target will log in. In both cases, the attacker should now have access to the staff area.

Determine which color has the vulnerability.

>Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

## Blue
Vulnerability #1: SQL Injection
Inject the following instead of the id, this makes the database to wait 5 seconds while querying the data.
`%27%20OR%20SLEEP(5)=0--%27`

![GIF](https://github.com/rcung000/cybersecurityweek9/blob/master/SQLI%20Exploit.gif)

Vulnerability #2: Session Hijacking/Fixation
Using the tool given to us, we can grab the SessionID and paste it into a different browser which isn't logged in. Alternatively we can use burp or packet sniffer to get the SessionID.

This allows anyone who is able to grab the SessionID to take over the account.

![GIF](https://github.com/rcung000/cybersecurityweek9/blob/master/Session%20Hijack.gif)

## Red
Vulnerability #1: IDOR

By changing the id parameter in the URL, we can get access to two hidden employees, one who was fired and one who goes public at a later date.

![GIF](https://github.com/rcung000/cybersecurityweek9/blob/master/IDOR.gif)

Vulnerability #2: CSRF

By sending a malicious html page, when they go to the link it'll submit a request to edit one of the employees and the admin will be none the wiser.

![GIF](https://github.com/rcung000/cybersecurityweek9/blob/master/CSRF.gif)

## Green
Vulnerability #1: Username Enumeration

By logging in with a known good username, the error message is bolded. However when logging in with a bad username, the error message remains the same but not bolded. See the below images where the HTML code is inspected. The Existing Username has it named Failure and the non-existent username has it named Failed.

Existing Username:
![PNG](https://github.com/rcung000/cybersecurityweek9/blob/master/UsernameEnumerationFailure.png)

Non-existent Username:
![PNG](https://github.com/rcung000/cybersecurityweek9/blob/master/UsernameEnumerationFailed.png)

Vulnerability #2: XSS

The attacker can inject using the feedback form.
`<script>alert('Richard found the XSS!');</script>`

This runs once the admin checks the feedback page.

![GIF](https://github.com/rcung000/cybersecurityweek9/blob/master/XSS.gif)

## Notes
