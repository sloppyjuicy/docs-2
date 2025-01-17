---
Order: 30
xref: ccm-api
Title: API
Description: Information on using the CCM API
RedirectFrom: docs/central-management-api
---

## Description

As of CCM v0.4.0, the API for Chocolatey Central Management is exposed via Swagger.
The API documentation and examples can be reached from your CCM dashboard by selecting the :gear: **API** option on the left sidebar.
The information contained there is referenced here, but you can additionally try out the various API endpoints directly from the Swagger API page for your own CCM environment.

> :warning: **WARNING**
>
> The Swagger UI does not work in Internet Explorer. Please use another browser.

## ChocoCCM

The [ChocoCCM](xref:chococcm) PowerShell module is available from the PowerShell Gallery for use with CCM v0.4.0 and later.
This module contains PowerShell commands designed for interfacing with the CCM API directly.

See the [ChocoCCM Function Reference](xref:chococcm-functions) page for a full list of commands and their associated documentation.

## Authentication

Before being able to utilise the CCM API, you will need to create an authenticated session.
This can be done by targeting the `/Account/Login` endpoint and submitting the username and password.
You will need to use the authenticated session variable for any API calls.

```powershell
# Replace 'chocoserver' with the hostname of your CCM server in your environment
$CcmServerHostname = 'chocoserver'
$Credential = Get-Credential
$body = @{
    usernameOrEmailAddress = $Credential.UserName
    password = $Credential.GetNetworkCredential().Password
}
$Result = Invoke-WebRequest -Uri "https://$CcmServerHostname/Account/Login" -Method POST -ContentType 'application/x-www-form-urlencoded' -Body $body -SessionVariable Session -ErrorAction Stop
```

While authenticated, you can target API endpoints like this:

```powershell
# The $Session variable name must match the string given to -SessionVariable when authenticating.
Invoke-WebRequest -Uri "https://$CcmServerHostname/$endpointUrl" -Method $Method -Body $Parameters -Session $Session
```

## Endpoints

Below is a list of the API endpoints available for CCM as of v0.10.0.
For more detailed information on the API endpoints and their parameters for your version of CCM, select the :gear: **API** option from the sidebar on the Central Management dashboard, or navigate to `https://CCM_SERVER_HOSTNAME/swagger/`.

### AuditLog

| Method | EndpointUrl                             |
| :----: | :-------------------------------------- |
|  GET   | /api/services/app/AuditLog/GetAuditLogs |

### ChocolateyLicenseInformation

| Method | EndpointUrl                                                           |
| :----: | :-------------------------------------------------------------------- |
|  GET   | /api/services/app/ChocolateyLicenseInformation/GetLicenseCount        |
|  GET   | /api/services/app/ChocolateyLicenseInformation/GetLicenseExpiration   |
|  GET   | /api/services/app/ChocolateyLicenseInformation/GetIsTrialLicense      |
|  GET   | /api/services/app/ChocolateyLicenseInformation/GetIsLicenseCountValid |

### ComputerGroup

| Method | EndpointUrl                                        |
| :----: | :------------------------------------------------- |
|  GET   | /api/services/app/ComputerGroup/GetAllByComputerId |
|  GET   | /api/services/app/ComputerGroup/GetAllByGroupId    |
|  POST  | /api/services/app/ComputerGroup/CreateOrEdit       |
| DELETE | /api/services/app/ComputerGroup/Delete             |

### Computers

| Method | EndpointUrl                                                  |
| :----: | :----------------------------------------------------------- |
|  GET   | /api/services/app/Computers/GetComputerCount                 |
|  GET   | /api/services/app/Computers/GetAllPaged                      |
|  GET   | /api/services/app/Computers/GetAll                           |
|  GET   | /api/services/app/Computers/GetComputerForView               |
|  GET   | /api/services/app/Computers/GetComputerForEditByComputerGuid |
|  GET   | /api/services/app/Computers/GetComputerForEdit               |
|  POST  | /api/services/app/Computers/Edit                             |
| DELETE | /api/services/app/Computers/Delete                           |

### ComputerSoftware

Methods for querying or managing mappings between Computer objects and Software entries

| Method | EndpointUrl                                                |
| :----: | :--------------------------------------------------------- |
|  GET   | /api/services/app/ComputerSoftware/GetAllByComputerId      |
|  GET   | /api/services/app/ComputerSoftware/GetAllPagedByComputerId |
|  GET   | /api/services/app/ComputerSoftware/GetAllBySoftwareId      |
|  GET   | /api/services/app/ComputerSoftware/GetAllPagedBySoftwareId |

### DeploymentPlans

| Method | EndpointUrl                                                                  |
| :----: | :--------------------------------------------------------------------------- |
|  GET   | /api/services/app/DeploymentPlans/GetAllPaged                                |
|  GET   | /api/services/app/DeploymentPlans/GetAll                                     |
|  GET   | /api/services/app/DeploymentPlans/GetDeploymentPlanForView                   |
|  GET   | /api/services/app/DeploymentPlans/GetAllScheduledDeploymentPlansReadyToStart |
|  GET   | /api/services/app/DeploymentPlans/GetDeploymentPlanForEdit                   |
|  POST  | /api/services/app/DeploymentPlans/CreateOrEdit                               |
| DELETE | /api/services/app/DeploymentPlans/Delete                                     |
| DELETE | /api/services/app/DeploymentPLans/DeleteNewDraftDeploymentPlan               |
|  POST  | /api/services/app/DeploymentPlans/Archive                                    |
|  POST  | /api/services/app/DeploymentPlans/Duplicate                                  |
|  POST  | /api/services/app/DeploymentPlans/Start                                      |
|  POST  | /api/services/app/DeploymentPlans/Cancel                                     |
|  POST  | /api/services/app/DeploymentPlans/MoveToReady                                |

### DeploymentStepComputers

| Method | EndpointUrl                                                                           |
| :----: | :------------------------------------------------------------------------------------ |
|  GET   | /api/services/app/DeploymentStepComputers/GetAllPagedByDeploymentStepId               |
|  GET   | /api/services/app/DeploymentStepComputers/GetAllByDeploymentStepId                    |
|  GET   | /api/services/app/DeploymentStepComputers/GetDeploymentStepComputerForView            |
|  GET   | /api/services/app/DeploymentStepComputers/GetDeploymentStepComputerForEdit            |
|  GET   | /api/services/app/DeploymentStepComputers/GetReadyDeploymentStepComputerByMachineGuid |
|  GET   | /api/services/app/DeploymentStepComputers/GetUnreachableDeploymentStepComputers       |
|  GET   | /api/services/app/DeploymentStepComputers/GetStaleActiveDeploymentStepComputers       |
|  POST  | /api/services/app/DeploymentStepComputers/CreateOrEdit                                |
| DELETE | /api/services/app/DeploymentStepComputers/Delete                                      |
|  GET   | /api/services/app/DeploymentStepComputers/GetDeploymentStepDetailsToZip               |

### DeploymentStepGroups

| Method | EndpointUrl                                                          |
| :----: | :------------------------------------------------------------------- |
|  GET   | /api/services/app/DeploymentStepGroups/GetDeploymentStepGroupForView |
|  GET   | /api/services/app/DeploymentStepGroups/GetDeploymentStepGroupForEdit |
|  POST  | /api/services/app/DeploymentStepGroups/CreateOrEdit                  |
| DELETE | /api/services/app/DeploymentStepGroups/Delete                        |

### DeploymentSteps

| Method | EndpointUrl                                                     |
| :----: | :-------------------------------------------------------------- |
|  GET   | /api/services/app/DeploymentSteps/GetAllPagedByDeploymentPlanId |
|  GET   | /api/services/app/DeploymentSteps/GetAllByDeploymentPlanId      |
|  GET   | /api/services/app/DeploymentSteps/GetDeploymentStepForView      |
|  GET   | /api/services/app/DeploymentSteps/GetDeploymentStepForEdit      |
|  POST  | /api/services/app/DeploymentSteps/CreateOrEdit                  |
|  POST  | /api/services/app/DeploymentSteps/CreateOrEditPrivileged        |
| DELETE | /api/services/app/DeploymentSteps/Delete                        |
| DELETE | /api/services/app/DeploymentSteps/DeletePrivileged              |

### Groups

| Method | EndpointUrl                              |
| :----: | :--------------------------------------- |
|  GET   | /api/services/app/Groups/GetAllPaged     |
|  GET   | /api/services/app/Groups/GetAll          |
|  GET   | /api/services/app/Groups/GetGroupForEdit |
|  POST  | /api/services/app/Groups/CreateOrEdit    |
| DELETE | /api/services/app/Groups/Delete          |

### OutdatedReports

| Method | EndpointUrl                                        |
| :----: | :------------------------------------------------- |
|  GET   | /api/services/app/OutdatedReports/GetAllPaged      |
|  GET   | /api/services/app/OutdatedReports/GetAll           |
|  GET   | /api/services/app/OutdatedReports/GetAllByReportId |
|  POST  | /api/services/app/OutdatedReports/Create           |
| DELETE | /api/services/app/OutdatedReports/Delete           |

### Permission

| Method | EndpointUrl                                    |
| :----: | :--------------------------------------------- |
|  GET   | /api/services/app/Permission/GetAllPermissions |

### Role

| Method | EndpointUrl                               |
| :----: | :---------------------------------------- |
|  GET   | /api/services/app/Role/GetRoles           |
|  GET   | /api/services/app/Role/GetRoleForEdit     |
|  POST  | /api/services/app/Role/CreateOrUpdateRole |
| DELETE | /api/services/app/Role/DeleteRole         |

### SensitiveVariables

| Method | EndpointUrl                                                      |
| :----: | :--------------------------------------------------------------- |
|  GET   | /api/services/app/SensitiveVariables/GetAllPaged                 |
|  GET   | /api/services/app/SensitiveVariables/GetAll                      |
|  GET   | /api/services/app/SensitiveVariables/GetSensitiveVariableForView |
|  POST  | /api/services/app/SensitiveVariables/Create                      |
| DELETE | /api/services/app/SensitiveVariables/Delete                      |

### Software

| Method | EndpointUrl                                                        |
| :----: | :----------------------------------------------------------------- |
|  GET   | /api/services/app/Software/GetAll                                  |
|  GET   | /api/services/app/Software/GetAllCurrentlyInstalled                |
|  GET   | /api/services/app/Software/GetAllWithoutFilter                     |
|  GET   | /api/services/app/Software/GetSoftwareByPackageId                  |
|  GET   | /api/services/app/Software/GetSoftwareByPackageIdAndVersion        |
|  GET   | /api/services/app/Software/GetSoftwareForView                      |
|  GET   | /api/services/app/Software/GetSoftwareForEdit                      |
|  GET   | /api/services/app/Software/GetSoftwareForEditByPackageIdAndVersion |

### TokenAuth

| Method | EndpointUrl                                       |
| :----: | :------------------------------------------------ |
|  POST  | /api/TokenAuth/Authenticate                       |
|  POST  | /api/TokenAuth/RefreshToken                       |
|  GET   | /api/TokenAuth/LogOut                             |
|  POST  | /api/TokenAuth/SendTwoFactorAuthCode              |
|  POST  | /api/TokenAuth/ImpersonatedAuthenticate           |
|  POST  | /api/TokenAuth/LinkedAccountAuthenticate          |
|  GET   | /api/TokenAuth/GetExternalAuthenticationProviders |
|  POST  | /api/TokenAuth/ExternalAuthenticate               |
|  GET   | /api/TokenAuth/TestNotification                   |

### WebLog

| Method | EndpointUrl                               |
| :----: | :---------------------------------------- |
|  GET   | /api/services/app/WebLog/GetLatestWebLogs |
|  POST  | /api/services/app/WebLog/DownloadWebLogs  |
