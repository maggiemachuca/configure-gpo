In this tutorial 

Dealing with Account Lockouts
Enabling and Disabling Accounts
Observing Logs

# Step 1: Configure Group Policy settings as Domain Admin
- We are going to login to our domain controller machine **DC-1** using **jane_admin** since she is the Domain Admin
- Right click the start menu -> Click Run
- Type in **gpmc.msc** (screenshot below)

<img width="394" height="203" alt="Screenshot 2025-09-24 at 10 06 25 PM" src="https://github.com/user-attachments/assets/7538a449-2f00-41d0-9840-c56ef633ddd4" />

- This should open up the **Group Policy Management** window

<img width="751" height="527" alt="Screenshot 2025-09-24 at 10 07 15 PM" src="https://github.com/user-attachments/assets/c5db7a15-624e-4229-bf76-8aaa98e7a5d9" />

- Expand **Forest: mydomain.com** -> Expand **Domains**
- Expand **mydomain.com**
- Right click on **Default Domain Policy** -> Click edit (screenshot below)

<img width="414" height="342" alt="Screenshot 2025-09-24 at 10 12 11 PM" src="https://github.com/user-attachments/assets/4cfee64e-3057-4803-8e64-8bd1ec3f0872" />

- This should open up a new window named **Group Policy Management Editor**

<img width="785" height="564" alt="Screenshot 2025-09-24 at 10 12 50 PM" src="https://github.com/user-attachments/assets/ce28a467-01ff-4efa-8cd5-59f261b3d616" />

- Expand Computer Configuration -> Expand Policies
- Expand Windows Settings -> Expand Security Settings
- Expand Account Policies -> Click **Account Lockout Policy**
- This is what your screen should look like (screenshot below)

<img width="1096" height="271" alt="Screenshot 2025-09-24 at 10 18 34 PM" src="https://github.com/user-attachments/assets/d07dbb32-6ea3-4875-a153-b3316076809d" />

- Let's set **Account Lockout Duration** to 30 minutes.
- Click the "Define this policy setting" option
- Click OK
- After clicking OK, Windows will tell you that it is changing the values on some of the policy settings within our **Account Lockout Policy**. (screenshot below)
- Click OK
  
<img width="512" height="265" alt="Screenshot 2025-09-24 at 10 20 17 PM" src="https://github.com/user-attachments/assets/eaa34354-4646-4edb-9e30-eea4e2193049" />

- Double-click the "Allow Administrator account lockout" policy setting
- Click "Enabled"
- Click OK

<img width="415" height="506" alt="Screenshot 2025-09-24 at 10 26 38 PM" src="https://github.com/user-attachments/assets/313f4181-6250-483f-be24-0dfe0124c92b" />

- Feel free to set the policy settings as you wish.
- In my example we want it set like this (screenshot below)

<img width="552" height="113" alt="Screenshot 2025-09-24 at 10 26 57 PM" src="https://github.com/user-attachments/assets/6cbe9fb0-34b8-4724-a097-f48490b7a40c" />

- Navigate back to the **Group Policy Management** window
- On **Default Domain Policy** click the Settings tab
- Here you can look at all the GPO policy settings currently configured within the Default Domain Policy GPO.

<img width="935" height="588" alt="Screenshot 2025-09-24 at 10 36 46 PM" src="https://github.com/user-attachments/assets/90375233-8a35-4a64-abc9-02638e510f66" />

- Since we modified the Default Domain Policy GPO linked to mydomain.com, the settings we just changed will apply to all users and computers within the domain and the OUs that we created earlier (_ADMINS, _CLIENTS, _EMPLOYEES)
- Note that I said the Default Domain Policy GPO is already linked. This happened when we first set up Active Directory.
- If it was a Custom GPO, however, we would have to link it. This is not the case here.


# Step 2: Force Update Group Policy

- Let’s force the client machine to apply the latest Group Policy updates immediately.
- Alternatively, you could wait 90 minutes for Group Policy to update automatically.
- In our example we will do this manually since we want to force changes right now.
  
- We are going to login as our Domain Admin onto our **CLIENT-1** machine.
- Open up Command Prompt and type in **gpupdate /force** and hit enter (screenshot below)

<img width="453" height="173" alt="Screenshot 2025-09-24 at 10 52 57 PM" src="https://github.com/user-attachments/assets/3ad6340f-c95c-477b-9aa4-3ceae1fb2ef6" />

Optional: 
- Open Command Prompt **as an Administrator**
- Type **gpresult /r** and hit enter
- You can see the Group Policy successfully updated
- You can also see which GPO was applied, which was **Default Domain Policy**

<img width="564" height="208" alt="Screenshot 2025-09-24 at 10 58 36 PM" src="https://github.com/user-attachments/assets/567ad9f8-6149-46e1-9cf7-d604e587ad46" />

# Step 3: Domain User Locks Themselves ouf of Account

- We will be acting as the domain user **cal.did**
- Next, you are going to **purposely fail to log into the account**
- Fail your login (aka input any incorrect password) at least 5 times for good measure. (screenshot below)
- This should make our user cal.did get locked out from their account.
  
- <img width="429" height="224" alt="Screenshot 2025-09-24 at 9 54 20 PM" src="https://github.com/user-attachments/assets/65e2ea3a-df1f-4efb-ae76-2c2d04f1d4b0" />

- After doing so, you should get a popup similar to this one. (screenshot below)
- Yours may look different than mine since I am using WindowsApp on macOS to remotely login.

<img width="254" height="306" alt="Screenshot 2025-09-24 at 11 15 20 PM 1" src="https://github.com/user-attachments/assets/e4db80c3-d299-48c6-be07-e3cf594c21b7" />

- This means our Group Policy worked!
- Let's go back to being a Domain Admin on our Domain Controller.

# Step 4: Domain Admin 

- As the Domain Admin we will go back to the **Active Directory Users and Computers** window to view **cal.did**'s account.
- Since we have so many user accounts, right-click mydomain.com -> click Find
<img width="350" height="66" alt="Screenshot 2025-09-24 at 11 19 28 PM" src="https://github.com/user-attachments/assets/f3daddd9-43b2-4e0f-baa0-013a1636ead9" />

- Type in the username you are looking for
- Click Find Now
- Double-click the user once you found it

<img width="510" height="369" alt="Screenshot 2025-09-24 at 11 20 40 PM" src="https://github.com/user-attachments/assets/10ffe005-8398-4b07-80fb-741ebb6414c6" />

- Click on the **Account** tab
- Click on **Unlock Account**
- Click Apply -> Click OK

<img width="413" height="534" alt="Screenshot 2025-09-24 at 11 21 56 PM" src="https://github.com/user-attachments/assets/44f4101d-75a4-481b-abe2-d32b2385ab6a" />


# Step 5: Domain User Attempts to Login Again

- Let's try to log into our **CLIENT-1** machine again as cal.did
- Enter your credentials correctly this time.
  
<img width="431" height="222" alt="Screenshot 2025-09-24 at 11 24 05 PM" src="https://github.com/user-attachments/assets/e3c15e28-3dce-4060-8c6a-b8a1298e3f60" />

- Success! **cal.did**'s account was successfully unlocked after the Domain Admin unlocked the account through Active Directory. (screenshot below)

<img width="354" height="427" alt="Screenshot 2025-09-24 at 11 24 19 PM" src="https://github.com/user-attachments/assets/25db734d-4eca-4635-9f3b-637e6242017e" />


---

work in-progress (stopped at 17:30)

