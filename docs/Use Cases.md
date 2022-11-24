This page contains a set of loosely formulated use cases to be used in the user-oriented design of CARP. This includes use cases for the back-end as well as use cases for end-user applications. This documentation is made in an overall 'sketchy' manner, and once the systems are designed and developed, more detailed information and documentation can be found in the different git repositories containing each implementation.

## DTU DigitalStress Sensing

This use case is for a (simple) stress monitoring app containing three main parts:

* mobile sensing of noise, physical activity, etc.
* a survey ([perceived stress scale](http://www.mindgarden.com/documents/PerceivedStressScale.pdf))
* fitbit (sleep, physical activity, HR) using the [Fitbit Web API](https://dev.fitbit.com/build/reference/web-api/).

### Protocol
An example protocol :: `digital_stress`

```yaml
 mobile_sensing
    noise         :: polling : f=10 min, d=10 sec
    light         :: polling : f=10 min, d=0,1 sec
    bluetooth     :: polling : f=10 min
    screen        :: listening        
    location      :: listening        
    app_usage     :: polling : f=1 hour
    activity      :: listening     
    phone_log     :: polling : daily
    notifications :: listening        
survey
    pss :: triggered once a day
fitbit
    physical_activity :: daily summary
    heart_rate        :: daily summary
    sleep             :: daily summary
```

### Study

We could imagine the following studies to be run using the `digital_stress` protocol:

* `test_study` >> a study run by the developer during development and testing.
   * one researcher 
   * 2-3 test users
* `pilot_dtu_UX_course` >> a study run in the UX class involving all the students
   * one main researcher (e.g. Jakob) and 4 TAs
   * 60 test users 
* `dtu_2018_1_deployment` >> the first deployment of the study at DTU in the spring semester of 2018.
   * 4 main researchers and 17 TAs
   * 350 test users 

### Deployment

Users sign up using their own mobile phone and those that have Fitbit, add this to their study.






## ICAT Study Setup

This use case is intended to provide requirements for the design of the CARP Study and Protocol Manager sub-system. The aim is to support the design and deployment of the [ICAT Study](http://www.cachet.dk/research/PhD-Projects/Computer-and-Smartphone-based-Assessment-of-Cognitive-Functioning-in-Affective-Disorders-in-Young-Pe).

### General Flow of Actions

1. The `PrimaryInvestigator` (PI) logs into the **Study Manager Portal**.
2. On her personal **Dashboard**, she can see a list of her ongoing studies, their status, and get an overview of the number of participants and how much data has been collected.
3. She create a new `Study`. This study is using an already existing `Protocol` called "_ICAT Web Assessment Protocol_" which defines the details of the study protocol. She fills in basic details like name, etc. Since this is an ICAT study aimed at doing a feasbility study with psychology students, she name the study "_ICAT Feasibility Study w. KU Psychology Students_".
4. She reviews the `InformedConsent` to the study (a default is associated with the protocol) and is able to revise and edit different section of it.
4. She starts to add `Participants` to the study. This is by done entering an the basic data (listed below). As a PI, she is automatically added to the study, so she can test it later.
4. Basic mandatory data are:
    * patient identification as outlined by the [HL7 Patient Resource](https://www.hl7.org/fhir/patient.html) (social security number (cpr.nr.), name, address, email, picture, ...)
    * patient health data (diagnosis, birthday, gender, height, weight, ...)
    * patient demographic data (employment, ...)
4. The participant receive an "invitation to the ICAT Study" email, in which s/he can click a link to verify participation. This takes him/her to the sign-up page, where s/he enters a password, and other details (see above) as well as initial data for this particular study (e.g. initial questionnaire).
4. She tick the box `Open Study` which allow any participant to self-register to the study.
5. She adds a research assistent as another `Researcher` to the study.
6. She "deploys" or start the study. This means that participants -- including herself -- can access the study. Participants cannot log in or upload data before a study is started (or, more precisely; user can access and upload data to a study only if it is in the `Started` state. See the life cycle in the [state diagram](https://github.com/cph-cachet/carp.protocols.core/wiki) of a study).
7. She goes to the ICAT web app, logs in, sign the informed consent, and runs the assessment tasks.
8. The research assistent (RA) is monitoring the study and have access to the Study Manager Portal. On the dashboard he can see that some participants have filled in some data for this specific ICAT study. He clicks on the study and get a detailed list of participants and their status, such as whether they have logged in for the first time, have signed the consent, and what tasks they have performed. By clicking on each participant, he can see additional data from each participant from this test.
9. The RA can click on a "send email" button for each participant, which allow him to send a message to the participant -- e.g. to remind them to do the assessment. 
10. The RA can also see a list of studies that this participant is part of. He cannot, however, see the details of these studies, if he is not associated with a study.
10. At some point, the PI need to pause the study and hits the "Pause study" button. This brings the study in a `Paused` state, in which no participants can log in or do the assessment. Later she can "restart" the study and it goes back to a `started` state.
10. Once all (or most) participants have done the assessment, the RA hit the "end study" button and marks the study as `Ended`. In this state it can be "restarted", but this will seldom happen. 
11. At some point, the PI mark the study as `Archived` in which case it disappears from the default view on the Dashboard. All studies - including archived ones - can however be shown.
