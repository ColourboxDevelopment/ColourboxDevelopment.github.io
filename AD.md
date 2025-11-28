---
layout: page
title: "AD Login"
permalink: /ad-login/
layout: default
nav_order: 6
---

By default AD will be using Controlled Group version, where assigned groups is used to define which folders permissions exist in Skyfish.

If you only need AD to permit access via one default group, that permits a set of folder permissions, refer to the section here: https://developer.skyfish.com/ad-login/#setup-a-default-ad-security-group




# AD (SAML) Login

## Setup steps

### Step 1: Connect your Active Directory with your Skyfish

1. Visit [Azure Portal](https://portal.azure.com/). Login.
2. Go to **Entra ID**.
3. In the top menu, click Add and choose **App registration**.
5. In the textbox titled *Name*, enter a fitting name, e.g. *Skyfish Login*.
6. Leave the option as **Accounts in this organizational directory only (Default Directory only - Single tenant)**.
7. At the bottom of the form, fill in the *Redirect URI* textbox with: `https://auth.colourbox.com/saml2/idpresponse`, leave the drop-down box to the left set as *Web*.
8. Click **Register** to save the registration.
9. Navigate to **Manifest** and look into the editor.
10. Navigate to where it says **"knownClientApplications": [],**  add this line of code on the next row: **"requestedAccessTokenVersion": 2,**. (If this line already exist, instead change its value from **Null** to **2**).
11. Now save the changes.
12. Navigate back to **Overview** in the left menu.
13. Click **Add an Application ID URI**.
14. Click **Add** next to Application ID URI.
15.  Fill in the textbox that appears titled *Application ID URI* with: `urn:amazon:cognito:sp:eu-west-1_95snzYZYv`.
16. Click **Save**.
17. In the side menu, click **Token configuration**.
18. Click **Add groups** claim. A submenu appears.
19. Checkmark **Security groups** and **Groups assigned to the application**.
20. Click **Save**.
21. In the side menu, click **Overview**.
22. Click **Endpoints**. A submenu appears.
23. **Copy** the URL titled: **Federation Metadata Document**.
24. Visit Skyfish’s **Settings** page for **AD Integration**.
25. **Paste** the **Federation Metadata Document URL** into the input field titled *Federation Metadata Document URL*.


________________________________________________________________________________________

#### If you are using an Enterprise app remember to assign the needed Skyfish groups to the application.

* Enter Enterprise apps
* Select the Skyfish application
* Select users and groups
* Choose add group 
* Now select the Skyfish affiliated groups you want assigned to the Skyfish application

**Step 1 is now complete. Continue with Step 2.**

### Step 2: Align your Active Directory groups with your Skyfish Groups.

1. Visit Skyfish’s [Settings page for AD Integration](https://www.skyfish.com/account/ad-integration).
2. Go to **Groups in Skyfish**.
3. Click Add new **AD Security Group**.
4. Visit [Entra ID Groups](https://portal.azure.com/#view/Microsoft_AAD_IAM/GroupsManagementMenuBlade/~/Overview). (If the link does not work, navigate to Entra ID in Azure Portal and open groups).
6. Select a group in your Active Directory that you want to align with a Skyfish Group.(Usually security groups).
7. **Copy** the group’s **Object ID**.
8. Go back to Skyfish’s **Groups in Skyfish** settings page.
9. In the textbox titled *AD Security Group ID*, **paste** the **Object ID**.
10. Select the **Skyfish User Role** the group’s users will have.
11. Select the wanted ekstra Skyfish features the user should have access to.
12. If the user has Colourbox access, the user role can be set here(Optional).
13. Assign the group to one or more **Skyfish Groups**: create and/or use existing ones.
14. Click **Save access rights**, to complete aligning the Active Directory group and
    Skyfish Group.
15. Repeat this **Step 2** process for each of your Active Directory’s groups that you wish
    to integrate with Skyfish.

## Troubleshooting the integration

### Invalid SAML response received: The value of the attribute custom:cbx_groups must have a length less than or equal to 2048 characters

#### Limit the groups being sent to Skyfish

The best option is to **limit the groups you send to Skyfish**, so you only expose the groups relevant to the Skyfish integration. This should bring the groups field under 2048 characters.

#### Setup a default AD Security Group

If limiting the amount of groups sent to Skyfish is not an option, we have the option to give your users a default AD Security Group when they login via your AD.

This will change the AD login slightly. The default behavior for AD login is to strip the user from its rights first, then give them the access according to the AD Security Groups you setup in Step 2, we call this strict mode. When using a default AD Security Group we relax this by not stripping the user of its right before giving the rights the default AD Security Group allows the user.

To setup a default AD Security Group:

1. In step 2 you have aligned one or more AD Security Groups with Skyfish. **Find** the **AD Security Group** you want to be the default.
2. **Send an email** to info@colourbox.com with the **AD Security Group** and ask them to set it as the default AD Security Group in your integration.

### Got groups [] but no groups map to access in Skyfish.

This error comes in one of two forms:

#### Got groups []

If the square brackets are empty, **we did not receive any security groups from your AD**. If we don't receive any AD security groups, we cannot give any access to the user and the login attempt is thus rejected. Make sure you have gone through **Step 1** to correctly setup your AD.

#### Got groups [7ea05961-86f4-45cd-8134-e3ffb9b4c22b]

If you see groups in the brackets, **the AD security groups we are getting does not map to any access in Skyfish**. Make sure you go through **Step 2** to correctly map your AD security groups to access in Skyfish.
