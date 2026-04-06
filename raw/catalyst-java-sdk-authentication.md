# Catalyst Java SDK — Authentication (Cloud Scale)

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/cloud-scale/authentication/
> Sub-pages: add-new-user, get-org-id, add-new-user-to-existing-org, get-users-in-org, reset-password, third-party-server-token, custom-user-validation, get-user-details, update-user-details, enable-disable-user, delete-user
> Captured: 2026-04-05

Catalyst Authentication features enable you to add end-users to your Catalyst serverless applications, configure their user accounts and roles, and manage user sign-in and authentication. The Authentication component is part of the Cloud Scale service category.

---

## 1. Add New User

When a user signs up to a Catalyst application, unique identification values like ZUID and userID are created for them. The user is also assigned to an organization by Catalyst.

**Key classes:** `ZCSignUpData`, `ZCUser`, `ZCMailTemplateDetails`, `PlatformType`

**Mandatory fields:** EmailId, FirstName

**Notes:**
- Max 25 users in development environment; unlimited after deploying to production
- RoleId can be obtained from the Roles section in Authentication console
- Email sender address must be configured and verified in Catalyst Mail Component

```java
import com.zc.component.users.PlatformType;
import com.zc.component.users.ZCSignUpData;
import com.zc.component.users.ZCUser;
import com.zc.component.ZCMailTemplateDetails;

ZCSignUpData signUpdetails = ZCSignUpData.getInstance();
ZCMailTemplateDetails mailData = signUpdetails.mailTemplateInstance();
mailData.setSendersMail("docofoh552@lukaat.com");
mailData.setSubject("Welcome to %APP_NAME%");
mailData.setMessage("Hello, Follow this link to join %APP_NAME%. %LINK% If you didn't ask to join, ignore this email. Thanks, Your %APP_NAME% team");
signUpdetails.setTemplateDetails(mailData);
signUpdetails.setPlatformType(PlatformType.WEB);
signUpdetails.userDetail.setEmailId("p.boyle@zylker.com");
signUpdetails.userDetail.setLastName("Boyle");
signUpdetails.userDetail.setRoleId(1256000000228024L);
signUpdetails = ZCUser.getInstance().registerUser(signUpdetails);
```

---

## 2. Get All Org IDs

Org ID (ZAAID) is the unique identification of the organization that an end-user belongs to. Generated when a user signs up via authentication types, Add User API, or console.

```java
import com.zc.component.users.ZCUser;

ZCUser user = ZCUser.getInstance();
user.getAllOrgs();
```

---

## 3. Add User to Existing Organization

Register a user to an existing organization without creating a new one.

**Mandatory fields:** FirstName, EmailId, OrgID

```java
import com.zc.component.users.PlatformType;
import com.zc.component.users.ZCSignUpData;
import com.zc.component.users.ZCUser;
import com.zc.component.ZCMailTemplateDetails;

ZCSignUpData signUpdetails = ZCSignUpData.getInstance();
ZCMailTemplateDetails mailData = signUpdetails.mailTemplateInstance();
mailData.setSendersMail("docofoh552@lukaat.com");
mailData.setSubject("Welcome to %APP_NAME%");
mailData.setMessage("Hello, Follow this link to join %APP_NAME%. %LINK% ...");
signUpdetails.setTemplateDetails(mailData);
signUpdetails.setPlatformType(PlatformType.WEB);
signUpdetails.userDetail.setEmailId("amelia.burrows@zylker.com");
signUpdetails.userDetail.setLastName("Amelia");
signUpdetails.userDetail.setOrgId("35712181");
signUpdetails = ZCUser.getInstance().addUser(signUpdetails);
```

---

## 4. Get All Users in an Organization

Fetch the list of all users of an organization using `getAllUsers()`.

```java
import com.zc.component.users.ZCUser;

ZCUser user = ZCUser.getInstance();
user.getAllUser(10062701096); // Enter your Org ID here
```

---

## 5. Reset Password

Sends a reset password link to the user's email via `resetPassword()`.

**Note:** Sender email must be verified in Catalyst Mail Component.

```java
import com.zc.component.users.PlatformType;
import com.zc.component.users.ZCSignUpData;
import com.zc.component.users.ZCUser;
import com.zc.component.ZCMailTemplateDetails;

ZCSignUpData signUpdetails = ZCSignUpData.getInstance();
ZCMailTemplateDetails mailData = signUpdetails.mailTemplateInstance();
mailData.setSendersMail("docofoh552@lukaat.com");
mailData.setSubject("Welcome to %APP_NAME%");
mailData.setMessage("Hello, Follow this link...");
signUpdetails.setTemplateDetails(mailData);
signUpdetails.setPlatformType(PlatformType.WEB);
signUpdetails.userDetail.setEmailId("amelia.burrows@zylker.com");
signUpdetails.userDetail.setLastName("Burrows");
ZCUser.getInstance().resetPassword(signUpdetails);
```

---

## 6. Generate a Custom Server Token

For third-party authentication: after a user is redirected from a third-party service, generate a custom server token to pass to the Web SDK.

**Key classes:** `ZCCustomTokenDetails`, `ZCCustomTokenUserDetails`, `ZCCustomTokenResponse`

**Notes:**
- Must enable Public Signup
- Security depends on the third-party auth service
- Token must be generated every time the user logs in via third-party auth

```java
ZCCustomTokenDetails customTokenDetails = ZCCustomTokenDetails.getInstance();
ZCCustomTokenUserDetails tokenUserDetails = ZCCustomTokenUserDetails.getInstance();
tokenUserDetails.setEmailId("emma@zylker.com");
tokenUserDetails.setFirstName("Amelia");
tokenUserDetails.setLastName("Burrows");
tokenUserDetails.setRoleName("App Admin");
customTokenDetails.setUserDetails(tokenUserDetails);
ZCCustomTokenResponse customTokenResp = ZCUser.getInstance().generateCustomToken(customTokenDetails);
```

---

## 7. Custom User Validation

Authorize and validate end-users using a custom Basic I/O function on signup. Write custom logic to process credentials and grant/deny access.

**Key classes:** `ZCSignupUserValidationRequest`, `ZCSignupUserValidationResponse`, `ZCSignupResponseUserDetails`, `ZCSignupUserService`, `ZCSignupValidationStatus`

```java
import com.catalyst.Context;
import com.catalyst.basic.BasicIO;
import com.catalyst.basic.ZCFunction;
import com.zc.api.APIConstants.ZCSignupValidationStatus;
import com.zc.common.ZCProject;
import com.zc.component.auth.ZCSignupResponseUserDetails;
import com.zc.component.auth.ZCSignupUserValidationRequest;
import com.zc.component.auth.ZCSignupUserValidationResponse;
import com.zc.component.users.ZCSignupUserService;

public class MainClass implements ZCFunction {
    @Override
    public void runner(Context context, BasicIO basicIO) throws Exception {
        try {
            ZCProject.initProject();
            ZCSignupUserValidationRequest requestDetails = ZCSignupUserService.getSignupValidationRequest(basicIO);
            if (requestDetails != null) {
                ZCSignupUserValidationResponse validationResponse = ZCSignupUserValidationResponse.getInstance();
                if (requestDetails.getUserDetails().getEmailId().contains("@notallowedmail")) {
                    validationResponse.setStatus(ZCSignupValidationStatus.FAILURE);
                } else {
                    validationResponse.setStatus(ZCSignupValidationStatus.SUCCESS);
                    ZCSignupResponseUserDetails respUserDetails = ZCSignupResponseUserDetails.getInstance();
                    respUserDetails.setFirstName("Patricial");
                    respUserDetails.setLastName("Boyle");
                    respUserDetails.setRoleIdentifier("App User");
                    respUserDetails.setOrgId("1241113");
                    validationResponse.setUserDetails(respUserDetails);
                }
                basicIO.write(validationResponse);
            }
        } catch (Exception e) {
            basicIO.write(e);
            basicIO.setStatus(500);
        }
    }
}
```

**Test JSON payload:**
```json
{
    "request_type": "add_user",
    "request_details": {
        "user_details": {
            "email_id": "emmy@zylker.com",
            "first_name": "Emma",
            "last_name": "Thompson",
            "org_id": "65**************",
            "role_details": {
                "role_name": "Moderator",
                "role_id": "10*****"
            }
        },
        "auth_type": "web"
    }
}
```

---

## 8. Get User Details

Three variants:

### Get Current User Details
```java
import com.zc.component.ZCUserDetail;
import com.zc.component.users.ZCUser;

ZCUserDetail details = ZCUser.getInstance().getCurrentUser();
```

### Get All User Details
```java
List<ZCUserDetail> details = ZCUser.getInstance().getAllUser();
```

### Get User Details by User ID
```java
ZCUserDetail details = ZCUser.getInstance().getUser(1510000000113214L);
```

---

## 9. Update Details of a User

Modifiable fields: First Name, Last Name, ZAAID (Org ID), RoleID

```java
import com.zc.component.ZCUserDetail;
import com.zc.component.users.ZCUser;

ZCUser user = ZCUser.getInstance();
ZCUserDetail userDetail = user.getCurrentUser();
userDetail.setFirstName("Josh");
user.updateUser(userDetail.getUserId(), userDetail);
```

---

## 10. Enable or Disable a User

A disabled user remains listed but cannot access the application. Uses `updateUserStatus()`.

### Enable
```java
ZCUser user = ZCUser.getInstance();
user.updateUserStatus(USER_ID, USER_STATUS.ENABLE);
```

### Disable
```java
ZCUser user = ZCUser.getInstance();
user.updateUserStatus(USER_ID, USER_STATUS.DISABLE);
```

---

## 11. Delete User

Permanently remove a user's access using `deleteUser()` with their UserID.

```java
ZCUser.getInstance().deleteUser(1510000000109587L);
```
