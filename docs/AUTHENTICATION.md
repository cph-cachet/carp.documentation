# Authentication in CARP

## Roles

Roles in CARP cores is described [here](CARP-Roles).

* `Study Owner` - the account(s) (user(s)) who can access studies (sometimes referred to as "Researcher")
* `Participant` - the user/patient/... who is part of a study deployment. 


## Authentication for Study Owners

Two basic scenarios

### Self-signup 

A `study owner` want to join CARP and start using it.

 * create user / account (username & password)
 * provide meta-data (email, name, picture, etc.)
 * email verification
 * reset password

### Invitation

A `study owner` can invite another `study owner` to join a group of study owners for a `study`.

 * receive an email with a sign-up link, linked email with username
 * provide password
 * reset password

> **Note** - all study owners of a study have equal access / privileges. Right now we don't support different types of study owners.

> **Note** - email verification is not needed in this scenario since we assume that the study owner has the valid email address. 

## Authentication for Participants

Basically we have a 2x2 matrix dependent on whether CARP knows the user or not, and knows the study or not.

| user\study | known       | unknown   |
|------------|-------------|-----------|
| **known**  | invitation  | register  |
| **unknown**| sign-up     | volunteer |



### Invitation

The user and the study is known to CARP, i.e. entered into the system somehow.

1. Send email to user with a QR code or deep link with
   * username
   * one time password
   * study id
   * deployment id
2. The user scan QR code / click link and
   * provide a new password
3. The password is changed and the user authenticated

If the user clicks the deep link / QR code after the password has changed (and the one-time password is hence invalid), the app should ask for the **current password** (and not for a new password). This can happen if the user, for example, re-install the app.

The format of the deeplink is

```
carp://diafocus.cachet.dk/username=bardram&password=1234&study_id=67&deployment_id=45678
```

with the username, password, etc. section encrypted.

**References:**

 * Flutter[`uni_links`](https://pub.dev/packages/uni_links)
 * [iOS deep linking is stripped out in Gmail](https://stackoverflow.com/questions/23575553/ios-deep-linking-is-stripped-out-in-gmail)
 * [iOS Universal Links](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html)
 * [Android App Links](https://simonmarquis.github.io/Android-App-Linking/#app-links)

### Sign-up

The study is know to CARP (defined in the portal e.g.), but the user is unknown - i.e. any user can sign up to a study.

1. Provide link or QR code with 
   * study id
   * deployment id
2. The user scan QR code / click link and register with
   * a username 
   * a password
3. Then
   * the user is created in CARP 
   * associated with the study 
   * the user is authenticated

### Register

The user is know but not the study - i.e. a user is being added to CARP

1. Provide user name & one-time password in CARP
2. Provide link for user to 
    * change / set password
    * provide meta data (name, ...)

### Volunteer

A unknown user want to enrol to the CARP platform for participation in studies.

1. Provide a link to the user where they can specify
    * username
    * password
    * meta data (name, ...)

## Endpoints

This results in the need for the following endpoints:

 * get invitation link 
 * get invitation QR code
 * change password
 * create user (username & password)
 * associate a user with a study
 * provide user meta data
 * get user data / profile
 * authenticate


## Anonymous Authentication

Should we support anonymous authentication like in [Firebase](https://www.tutorialspoint.com/firebase/firebase_anonymous_authentication.htm)?

This would support a scenario like:

 * download a CARP app, and start using it with an anonymous user id registred in CARP/CANS.
 * store the user credentials locally on the phone
 * keep using the app, uploading data to this anonymous id
 * if the app is deleted, the local id is deleted
 * if the app is re-installed, a new id is generated 
     * this id cannot be linked to the old one
     * unless we maybe also send a hardware id in the registration?
     * but then, a hardware id isn't really anonymous is it?



