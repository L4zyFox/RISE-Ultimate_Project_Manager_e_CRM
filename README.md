# RISE-Ultimate_Project_Manager_e_CRM

Security vulnerability: IDOR (Insecure Direct Object Reference)<br>
Affected Component: profile image upload endpoint, team_members controller<br>
Software: RISE - Ultimate Project Manager & CRM <br>
Vendor: codecanyon<br>
Version: 3.8.2<br>

Describe the bug/issue:<br>
A vulnerability was discovered in RISE - Ultimate Project Manager & CRM that allows an authenticated user to change the profile picture of any other user by exploiting an Insecure Direct Object Reference (IDOR) in the /index.php/team_members/save_profile_image/[user_id] endpoint. The application does not properly validate whether the authenticated user is authorized to update the specified user ID’s profile image, allowing unauthorized modification of user data. This flaw impacts data integrity and may lead to impersonation or disruption of user experience.

To Reproduce:


🧑‍💼 1. Authenticated Access to Profile Settings

An authenticated user navigates to the "My Profile" section of the application, where they are provided with an option to upload or update their profile image.

<img src="https://github.com/L4zyFox/RISE-Ultimate_Project_Manager_e_CRM/blob/main/01-upload.png">

📤 2. Interception of the Upload Request

Upon uploading a profile image, the request can be intercepted using a proxy tool such as Burp Suite. The request is a POST to the following endpoint:

``POST /index.php/team_members/save_profile_image/36``

Here, 36 corresponds to the authenticated user's numeric ID.

<img src="https://github.com/L4zyFox/RISE-Ultimate_Project_Manager_e_CRM/blob/main/02-Interc.png">

🔁 3. Exploiting the IDOR via User ID Manipulation

By changing the ID in the URL path to that of another valid user, for example:

``POST /index.php/team_members/save_profile_image/44``

The application processes the request and updates the profile picture of user ID 44, without performing any access control checks.

<img src="https://github.com/L4zyFox/RISE-Ultimate_Project_Manager_e_CRM/blob/main/03-edited-Interc.png">

🔐 4. Lack of Authorization Validation

The server-side implementation fails to validate whether the authenticated user is authorized to perform the action on the targeted user ID. As a result, any authenticated user can change the profile images of other users, impacting the integrity of user data and potentially leading to impersonation or user confusion.
