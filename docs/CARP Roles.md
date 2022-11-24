# CARP Roles

## Main Roles

### Administrative / Study Roles

* `System Administrator` - can do everything; superuser.
* `CARP Administrator` - can allow a user to create studies by making them a `Study Owner`. Cannot modify or access studies.
* `Study Owner` - can create studies and invite participants. Can access data for a study.

### Participant Roles

* `Participant` - the participant/user/patient uploading and accessing his/her own data.

### Permissions 

 `System Administrator` always has permission to access all functions. These are not documented in the table below since this applies to all functions.
As for a study, there can be one owner (the `Study Owner`, who created the study) and multiple additional `Study owners`. The additional owners can be invited through the API and they have the same access level as the original creator. 

| **Package**    | **Function** | **Permission** | **Condition** |
| ------     | ------   | ------      | ------      |
| **Collection** | **CREATE**   | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study.<br/> `Participant` is allowed if he is a participant in the study. |
|            | **VIEW**, **UPDATE**, **DELETE** | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study.<br/> `Participant` is allowed if he is the creator of the resource. |
|    |    |    |    |
| **Consent** | **CREATE**  | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study deployment.<br/> `Participant` is allowed if he is a participant in the study deployment. |
|             | **VIEW**, **DELETE**  | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study deployment.<br/> `Participant` is allowed if he is the creator of the resource. |
|             | **VIEW ALL**    | `Study Owner` | `Study Owner` is allowed if he has access to the study deployment. |
|    |    |    |    |
| **Data Point** | **CREATE**  | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study deployment.<br/> `Participant` is allowed if he is a participant in the study deployment. |
|                | **VIEW**    | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study deployment.<br/> `Participant` is allowed if he is the creator of the resource. |
|                | **VIEW ALL**  | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study deployment.<br/> `Participant` is allowed if he has access to the study deployment. |
|                | **DELETE**  | `Study Owner`, `Participant`  | `Study Owner` is allowed if he has access to the study deployment.<br/> `Participant` is allowed if he is the creator of the resource. |
|                | **COUNT**  | `Study Owner`, `Participant`  | `Study Owner` is allowed if he has access to the study deployment.<br/> `Participant` is allowed if he has access to the study deployment. |
|    |    |    |    |
| **Document**   | **CREATE**, **VIEW**, **VIEW ALL**, **UPDATE**, **DELETE**  | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study.<br/> `Participant` is allowed if he is a participant in the study. |
|    |    |    |    |
| **File**       | **CREATE**  | `Study Owner`, `Participant` | `Study Owner` is allowed if he has access to the study.<br/> `Participant` is allowed if he is a participant in the study. |
|                | **DOWNLOAD**, **VIEW**, **DELETE**  | `Study Owner`, `Participant` |  `Study Owner` is allowed if he has access to the study.<br/> `Participant` is allowed if he is the creator of the resource. |
|                | **VIEW ALL**  | `Study Owner` | `Study Owner` is allowed if he has access to the study.|
|    |    |    |    |
|  **Protocol**  | **CREATE**  | `Study Owner` | `Study Owner` is always allowed. |
|                | **VIEW**  | `Study Owner` | `Study Owner` is always allowed. |
|                | **VIEW ALL FOR AN OWNER**  |  `Study Owner` | `Study Owner` is always allowed. |
|                | **VIEW VERSION HISTORY**  |  `Study Owner` | `Study Owner` is always allowed. | 
|                | **UPDATE**  | `Study Owner`  | `Study Owner` is only allowed if he created the resource. |
|    |    |    |    |
|  **Study**     | **CREATE**  | `Study Owner` | `Study Owner` is always allowed. |
|                | **VIEW STATUS**  | `Study Owner`  | `Study Owner` is only allowed if he has access to the study. |
|                | **VIEW OVERVIEW FOR OWNER**  | `Study Owner`  | `Study Owner` is always allowed. |
|                | **VIEW PARTICIPANTS**  | `Study Owner` | `Study Owner` is only allowed if he has access to the study. |
|                | **SET PROTOCOL**  | `Study Owner` | `Study Owner` is only allowed if he has access to the study. |
|                | **GO LIVE**  | `Study Owner` | `Study Owner` is only allowed if he has access to the study. |
|                | **DEPLOY**  | `Study Owner` | `Study Owner` is only allowed if he has access to the study. |
|                | **ADD PARTICIPANT**  | `Study Owner` | `Study Owner` is only allowed if he has access to the study. |
|                | **ADD RESEARCHER**  | `Study Owner`  | `Study Owner` is only allowed if he has access to the study. |
|                | **SET INTERNAL DESCRIPTION**  | `Study Owner`  | `Study Owner` is only allowed if he has access to the study. |
|                | **SET INVITATION**  | `Study Owner`  | `Study Owner` is only allowed if he has access to the study. |
|                | **GET STUDY DETAILS**  | `Study Owner`  | `Study Owner` is only allowed if he has access to the study. |
|                | **GET PARTICIPANT GROUP STATUS LIST**  | `Study Owner`  | `Study Owner` is only allowed if he has access to the study. |
|                | **STOP PARTICIPANT GROUP**  | `Study Owner`  | `Study Owner` is only allowed if he has access to the study. |
|    |    |    |    |
|  **Deployment**     | **CREATE**  | `Study Owner` | `Study Owner` is always allowed. |
|                     | **GET DEPLOYMENT STATUS**  | `Study Owner`, `Participant` | `Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **REGISTER DEVICE**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **GET DEVICE DEPLOYMENT**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **ADD PARTICIPATION**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **VIEW PARTICIPATION INVITATIONS**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **GET DEPLOYMENT SUCCESS**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **UNREGISTER DEVICE**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **STOP**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **ADD PARTICIPANT DATA**, **GET PARTICIPANT DATA**, **GET PARTICIPANT DATA LIST**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **SET PARTICIPANT DATA**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **GET STUDY DEPLOYMENT STATUS LIST**  | `Study Owner`, `Participant` | Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from). `Participant` is always allowed if they have access to the deployment. |
|                     | **STATISTICS**  | `Study Owner`| Study Owner` is only allowed if they have access to the deployment (part of the study the deployment was made from).|
|    |    |    |    |
|  **Account/User**     | **GET CURRENT USER**  | OPEN  |
|                       | **REGISTER**  | OPEN  |
|                       | **REQUEST FORGOTTEN PASSWORD EMAIL**  | OPEN  | If the user is not blocked. |
|                       | **SAVE NEW PASSWORD**  | OPEN  | If the user is not blocked. |
|                       | **INVITE STUDY OWNER**  | `Study Owner`, `CARP Administrator` | `CARP Administrator` is always allowed.<br/> `Study Owner` is always allowed.<br/> |
|                       | **INVITE SYSTEM ADMIN**  | `System Administrator`  | `System Administrator` is always allowed. |
|                       | **INVITE CARP ADMIN**  | `System Administrator`  | `System Administrator` is always allowed. |
|                       | **UNLOCK BANNED ACCOUNT**  | `System Administrator`  | `System Administrator` is always allowed. |
|    |    |    |    |
|  **Summaries**     | **CREATE**  | `Study owner` | If they have access to the study. |
|                    | **DOWNLOAD**, **GET**, **DELETE**  | `Study owner` | If they are the creator of the resource. |
|                    | **GET ALL**  | `Study owner` | Only returns the ones that are created by the user. |









