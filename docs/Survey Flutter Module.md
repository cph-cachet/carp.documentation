# Survey Flutter Module

## Overall requirements

We need a way to model, issue and collect data from a user survey (aka. questionnaire). This is similar to the Apple [Open Research Kit (ORK)](http://researchkit.org) and the [Research Stack (RS)](http://researchstack.org). 

We need one (or more) Flutter Plugin(s) that supports:

* domain model for a survey (see e.g. the [RS backbone](https://github.com/ResearchStack/ResearchStack/tree/master/backbone/src/main/java/org/researchstack/backbone) model)
* a UI component to render the survey
* a way to collect survey data and store is as [OMH `survey` schema](https://github.com/openmhealth/schemas/blob/feature/generic-survey/schema/omh/survey-0.1.json).

Note that the OMH domain model and the CARP domain model and communication with the web services are handled by other Flutter plugins. The Flutter OMH implementation now supports the [`survey` schema](https://github.com/cph-cachet/openmhealth_schemas/commit/aa2946283eac3822ad4c9c476a8c79bd7eb28071). 

## General Ideas & Inspiration & Notes

* Issue only part of a questionnaire a a time.
    * For example, in [1] they divide the 9 PHQ-9 questions up in 3x3x3 with 3 questions morning, afternoon, evening
* Support different wordings for the same questions [1].

## Example PROs

* PHQ-9
* SF-12, SF-20, SF-36
* WHO5
* Perceived Stress Scale (PSS)

## Inspiration

* [Apple Research Kit](http://researchkit.org/hig/index.html)


## References
1. Torous J, Staples P, Shanahan M, et al. Utilizing a personal smartphone custom app to assess the patient health questionnaire-9 (PHQ-9) depressive symptoms in patients with major depressive disorder. JMIR Ment Heal. 2015;2(1).
