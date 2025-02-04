
<h1>Creating & Implementing Account Lockout Policy/Password Policy for Client Side</h1>
<p>
  This is a demonstration on creating and implementing an account lockout policy / password policy in a previously configured Windows Server. We will interact with the newly created GPO as a user on the client side to finish this demonstration.
</p>
<p>
<h2>Environments and Technologies used</h2>
  
  - Microsoft Azure (Virtual Machines/Compute)
  - Remote Desktop
  - Active Directory Domain Services
  - CMD Prompt
</p>

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Opening and Observing Group Policy Management</h2>

First, log on to DC-1 as our administrator user account and type "run" in the search bar upon entry. Type in "gpmc.msc", this command will open up our Group Policy Management. Next expand Forest -> expand Domains -> expand mydomain.com -> observe Default Domain Policy. We can see what kind of policy it is enforcing by clicking "Settings", on inspection this default policy is already enforcing an account policy and local policy. After observing the policy, right click Default Domain Policy and click Edit.

  ![1](https://github.com/user-attachments/assets/0472b355-55b1-49e9-a18e-841e9681ef0d)
  ![2](https://github.com/user-attachments/assets/c37da3a5-073a-4969-8a3b-8031d34ab8ec)
  ![3](https://github.com/user-attachments/assets/197cf42c-9e43-4864-baa2-6d095f9adae3)

<h2>Editing Password Policy</h2>

We want to let this policy affect all computers rather than configuring for a single user, under "Computer Configuration" expand Policies -> expand Windows Settings -> expand Security Settings -> expand Account Policies -> select Password Policy. Set the Minimum Password Age as 0 so we can change our user's password with no wait time. Set the Length of the password to 10 just to add more security. And enable Password must meet complexity requirements, this will also give better security when creating a password.

![4](https://github.com/user-attachments/assets/633018cc-164c-4d0d-9b03-f2b0caa486fb)

<h2>Editing Account Lockout Policy</h2>

Right under the Password Policy will be the Account Lockout Policy. In "Account Lockout Threshold" set the amount of invalid login attempts to 5. This means that if a user fails to log in 5 times, the account will automatically lock itself for the duration which is 10 minutes in this case. Click Apply and OK, the Account Lockout Policy will automatically define the other policies through suggestion, click OK. This Account Lockout Policy will lockout the administrator in failing 5 login attempts, lockout users in failing 5 login attempts, the lockout time will be 10 minutes and the reset will take place for the user to attempt signing in again. Exit out of the GPO Editor and go back to GPO Management.

![6](https://github.com/user-attachments/assets/2d7ecab3-e72c-41c7-8b91-101cdf398525)
![5](https://github.com/user-attachments/assets/bd8cac95-a792-433b-951b-f088aaa35a31)

In Group Policy Management, right click Default Domain Policy and click Rename. Rename the policy to a better fitting description for organizational purposes.

![7](https://github.com/user-attachments/assets/205e06a5-e295-47f4-819c-bdf07b0cecb3)

<h2>Adding Security Groups to Domain Policy</h2>

To add Security Groups in our new policy, select Add under "Security Filtering". GPM will prompt you to input an object name, here we can type "Domain Users" and select OK. Add "Domain Admins" security group right after and select OK. Now the settings in our "AccountLockoutPolicy" will apply to both Domain Users and Admins.

![9](https://github.com/user-attachments/assets/45361c5b-ad19-44e1-8df1-8b0502032196)
![10](https://github.com/user-attachments/assets/9bfa9c5a-3880-4975-81ae-f15f50cd9345)

<h2>Linking GPO to an OU</h2>

In order for our GPO to work on another computer, we need to link the group policy to our organizational unit. Right click the OU "_CLIENTS" and select "Link an existing GPO", select AccountLockoutPolicy and click OK. We have successfully added our AccountLockoutPolicy to the OU that has Client1 in it. Any user signing on with their account through Client1 now has a lockout policy and password policy enforced.

![11](https://github.com/user-attachments/assets/8991be51-2167-4172-8962-24500b51fa27)

<h2>Finalizing GPO & Best Practices</h2>

The GPO will automatically update in due time, but we can manually force the group policy to update. (Note that you have to update both client side's GPO and server side GPO.) In our domain controller run command prompt and type "gpupdate /force". Wait for both User and Computer policies to finish updating, then log in to Client1 as a local admin user. In Client1, run command prompt and type "gpupdate /force". Before testing your policies, it's important to always finalize with a "gpupdate /force" on both client side and server side. (It takes 90 minutes for the GPO to update by itself).

![12Admin](https://github.com/user-attachments/assets/f3c3d317-5215-4f66-bb62-8f6a83be0cc1)
![13Client](https://github.com/user-attachments/assets/33ffb879-0f8f-48fa-8690-725aad7c2f5b)

<h2>Testing AccountLockOut Policy</h2>

Attempt to log in Client1 5 times with one of the users using the wrong password. A pop up will occur and tell you that the accout has now been locked from too many failed attempts. We can observe the user account in our AD in the domain controller and unlock the account. In the Domain Controller open up Active Directory Users and Computers, locate the user that we locked out, right click the user and click Properties. In our user's properties click Account and check "Unlock Account", click Apply and click OK. We have successfully tested the AccountLockOut Policy and unlocked the user's account for them to attempt sign-in again.

![14](https://github.com/user-attachments/assets/c38e5fca-0c42-40f7-8d68-5eaf7b7be26d)
![15](https://github.com/user-attachments/assets/e6f39a87-bd08-40a6-9d4a-ae5ea38feb8d)
![16](https://github.com/user-attachments/assets/0cb463b2-e498-4cf5-bcdc-f7258cdd39c5)

This concludes the demonstration on creating and implementing an Account Lockout Policy / Password Policy.




</p>
