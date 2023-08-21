---
layout: page
title: "AD Login"
permalink: /ad-login/
layout: default
nav_order: 6
---

# AD (SAML) Login

## Setup steps

### Step 1: Connect your Active Directory with your Skyfish

1. Visit [Azure Portal](https://portal.azure.com/). Login.
2. Go to **Azure Active Directory**.
3. In the side menu, click **App registrations**.
4. Click **New registration**.
5. In the textbox titled *Name*, enter a fitting name, e.g. *Skyfish Login*.
6. At the bottom of the form, fill in the *Redirect URI* textbox with: `https://auth.colourbox.com/saml2/idpresponse`, leave the drop-down box to the left set as *Web*.
7. Click **Register** to save the registration.
8. Click **Add an Application ID URI**.
9. Click **Set**.
10.  Fill in the textbox that appears titled *Application ID URI* with: `urn:amazon:cognito:sp:eu-west-1_95snzYZYv`
11. Click **Save**.
12. In the side menu, click **Token configuration**.
13. Click **Add groups** claim. A submenu appears.
14. Checkmark **Security groups**.
15. Click **Add**.
16. In the side menu, click **Overview**.
17. Click **Endpoints**. A submenu appears.
18. **Copy** the URL titled: **Federation Metadata Document**.
19. Visit Skyfish’s **Settings** page for **AD Integration**.
20. **Paste** the **Federation Metadata Document URL** into the input field titled *Federation Metadata Document URL*.

Step 1 is now complete. Continue with Step 2.

### Step 2: Align your Active Directory groups with your Skyfish Groups.

1. Visit Skyfish’s [Settings page for AD Integration](https://www.skyfish.com/account/ad-integration).
2. Go to **Groups in Skyfish**.
3. Click Add new **AD Security Group**.
4. Visit [Azure Portal](https://portal.azure.com/). Login.
5. Go to **Groups**.
6. Select a group in your Active Directory that you want to align with a Skyfish Group.
7. **Copy** the group’s **Object ID**.
8. Go back to Skyfish’s **Groups in Skyfish** settings page.
9. In the textbox titled *AD Security Group ID*, **paste** the **Object ID**.
10. Select the **Skyfish User Role** the group’s users will have.
11. Assign the group to one or more **Skyfish Groups**: create and/or use existing ones.
12. Click **Save access rights**, to complete aligning the Active Directory group and
    Skyfish Group.
13. Repeat this **Step 2** process for each of your Active Directory’s groups that you wish
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
