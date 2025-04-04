---
title: Simulating risk detections in Azure AD Identity Protection
description: Learn how to simulate risk detections in Identity Protection

services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020

ms.author: joflore
author: MicrosoftGuyJFlo
manager: amycolannino
ms.reviewer: sahandle

ms.collection: M365-identity-device-management
---
# Simulating risk detections in Identity Protection

Administrators may want to simulate risk in their environment in order to accomplish the following items:

- Populate data in the Identity Protection environment by simulating risk detections and vulnerabilities.
- Set up risk-based Conditional Access policies and test the impact of these policies.

This article provides you with steps for simulating the following risk detection types:

- Anonymous IP address (easy)
- Unfamiliar sign-in properties (moderate)
- Atypical travel (difficult)
- Leaked credentials in GitHub for workload identities (moderate)

Other risk detections cannot be simulated in a secure manner.

More information about each risk detection can be found in the article, What is risk for [user](concept-identity-protection-risks.md) and [workload identity](concept-workload-identity-risk.md).

## Anonymous IP address

Completing the following procedure requires you to use:

- The [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) to simulate anonymous IP addresses. You might need to use a virtual machine if your organization restricts using the Tor browser.
- A test account that is not yet registered for Azure AD Multi-Factor Authentication.

**To simulate a sign-in from an anonymous IP, perform the following steps**:

1. Using the [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en), navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).   
2. Enter the credentials of the account you want to appear in the **Sign-ins from anonymous IP addresses** report.

The sign-in shows up on the Identity Protection dashboard within 10 - 15 minutes. 

## Unfamiliar sign-in properties

To simulate unfamiliar locations, you have to sign in from a location and device your test account has not signed in from before.

The procedure below uses a newly created:

- VPN connection, to simulate new location.
- Virtual machine, to simulate a new device.

Completing the following procedure requires you to use a user account that has:

- At least a 30-day sign-in history.
- Azure AD Multi-Factor Authentication enabled.

**To simulate a sign-in from an unfamiliar location, perform the following steps**:

1. When signing in with your test account, fail the multi-factor authentication (MFA) challenge by not passing the MFA challenge.
2. Using your new VPN, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com) and enter the credentials of your test account.

The sign-in shows up on the Identity Protection dashboard within 10 - 15 minutes.

## Atypical travel

Simulating the atypical travel condition is difficult because the algorithm uses machine learning to weed out false-positives such as atypical travel from familiar devices, or sign-ins from VPNs that are used by other users in the directory. Additionally, the algorithm requires a sign-in history of 14 days and 10 logins of the user before it begins generating risk detections. Because of the complex machine learning models and above rules, there is a chance that the following steps will not lead to a risk detection. You might want to replicate these steps for multiple Azure AD accounts to simulate this detection.

**To simulate an atypical travel risk detection, perform the following steps**:

1. Using your standard browser, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Enter the credentials of the account you want to generate an atypical travel risk detection for.
3. Change your user agent. You can change user agent in Microsoft Edge from Developer Tools (F12).
4. Change your IP address. You can change your IP address by using a VPN, a Tor add-on, or creating a new virtual machine in Azure in a different data center.
5. Sign-in to [https://myapps.microsoft.com](https://myapps.microsoft.com) using the same credentials as before and within a few minutes after the previous sign-in.

The sign-in shows up in the Identity Protection dashboard within 2-4 hours.

## Leaked Credentials for Workload Identities

This risk detection indicates that the application's valid credentials have been leaked. This leak can occur when someone checks in the credentials in a public code artifact on GitHub. Therefore, to simulate this detection, you need a GitHub account and can [sign up a GitHub account](https://docs.github.com/get-started/signing-up-for-github) if you don't have one already.

**To simulate Leaked Credentials in GitHub for Workload Identities, perform the following steps**:
1. Navigate to the [Azure portal](https://portal.azure.com).
2. Browse to **Azure Active Directory** > **App registrations**.
3. Select **New registration** to register a new application or reuse an exsiting stale application.
4. Select **Certificates & Secrets** > **New client Secret** , add a description of your client secret and set an expiration for the secret or specify a custom lifetime and click **Add**. Record the secret's value for later use for your GitHub Commit.

   > [!Note]
   > **You can not retrieve the secret again after you leave this page**.
   
5. Get the TenantID and Application(Client)ID in the **Overview** page.
6. Ensure you disable the application via **Azure Active Directory** > **Enterprise Application** > **Properties** > Set **Enabled for users to sign-in** to **No**.
7. Create a **public** GitHub Repository, add the following config and commit the change.
   ```GitHub file
     "AadClientId": "XXXX-2dd4-4645-98c2-960cf76a4357",
     "AadSecret": "p3n7Q~XXXX",
     "AadTenantDomain": "XXXX.onmicrosoft.com",
     "AadTenantId": "99d4947b-XXX-XXXX-9ace-abceab54bcd4",
   ```
7. In about 8 hours, you will be able to view a leaked credentail detection under **Azure Active Directory** > **Security** > **Risk Detection** > **Workload identity detections** where the additional info will contain your the URL of your GitHub commit.

## Testing risk policies

This section provides you with steps for testing the user and the sign-in risk policies created in the article, [How To: Configure and enable risk policies](howto-identity-protection-configure-risk-policies.md).

### User risk policy

To test a user risk security policy, perform the following steps:

1. Navigate to the [Azure portal](https://portal.azure.com).
1. Browse to **Azure Active Directory** > **Security** > **Identity Protection** > **Overview**.
1. Select **Configure user risk policy**.
   1. Under **Assignments**
      1. **Users** - Choose **All users** or **Select individuals and groups** if limiting your rollout.
         1. Optionally you can choose to exclude users from the policy.
      1. **Conditions** - **User risk** Microsoft's recommendation is to set this option to **High**.
   1. Under **Controls**
      1. **Access** - Microsoft's recommendation is to **Allow access** and **Require password change**.
   1. **Enforce Policy** - **Off**
   1. **Save** - This action will return you to the **Overview** page.
1. Elevate the user risk of a test account by, for example, simulating one of the risk detections a few times.
1. Wait a few minutes, and then verify that risk has elevated for your user. If not, simulate more risk detections for the user.
1. Return to your risk policy and set **Enforce Policy** to **On** and **Save** your policy change.
1. You can now test user risk-based Conditional Access by signing in using a user with an elevated risk level.

### Sign-in risk security policy

To test a sign in risk policy, perform the following steps:

1. Navigate to the [Azure portal](https://portal.azure.com).
1. Browse to **Azure Active Directory** > **Security** > **Identity Protection** > **Overview**.
1. Select **Configure sign-in risk policy**.
   1. Under **Assignments**
      1. **Users** - Choose **All users** or **Select individuals and groups** if limiting your rollout.
         1. Optionally you can choose to exclude users from the policy.
      1. **Conditions** - **Sign-in risk** Microsoft's recommendation is to set this option to **Medium and above**.
   1. Under **Controls**
      1. **Access** - Microsoft's recommendation is to **Allow access** and **Require multi-factor authentication**.
   1. **Enforce Policy** - **On**
   1. **Save** - This action will return you to the **Overview** page.
1. You can now test Sign-in Risk-based Conditional Access by signing in using a risky session (for example, by using the Tor browser). 

## Next steps

- [What is risk?](concept-identity-protection-risks.md)

- [Securing workload identities with Identity](concept-workload-identity-risk.md)

- [How To: Configure and enable risk policies](howto-identity-protection-configure-risk-policies.md)

- [Azure Active Directory Identity Protection](overview-identity-protection.md)
