# Regulatory

## GDPR

The data subject has the right to:
* access
* rectification
* object
* erasure
* restriction of processing
* data portability

### Duty to provide information

As a data controller, you have the "duty to provide information" [2]. See [Consent](Consent) for details.

### Research

Recital 159 states that for the purposes of the Regulation, the processing of personal data for scientific research purposes should be interpreted in a broad manner

* Including for example technological development and demonstration, fundamental research, applied research and privately funded research
* Scientific research purposes should also include studies conducted in the public interest in the area of public health

# DTU

## DTU Regulations

According to the [DTU guideline on Inside](https://www.inside.dtu.dk/da/medarbejder/it-og-telefoni/informationssikkerhed/gdpr/kategorier-af-personoplysninger-og-hjemmel-til-behandling/foelsomme-oplysninger-hjemmel-til-behandling), there is NO need for obtaining consent for research purposes:

* "Personoplysninger kan lovligt behandles til videnskabelige forskningsformål, uanset om der er indhentet et samtykke fra den registrerede person. "

# Firebase

According to [Firebase Data Processing and Security Terms](https://firebase.google.com/terms/data-processing-terms/) Google/Firebase that they are a data processor - section 5 states:

* "Google is a processor of that Customer Personal Data under the Data Protection Legislation"
* "Customer is a controller or processor, as applicable, of that Customer Personal Data under Data Protection Legislation"

## Google Terms and Privacy Policy

Google have elaborate descriptions of their regulatory framework. Here are some pointers:

* [Terms of Service](https://policies.google.com/terms?hl=en) -- these apply from 2019 and for the EU/EØS area.
* [Privacy Policy](https://policies.google.com/privacy?hl=en)
* [Anonymization Techniques](https://policies.google.com/technologies/anonymization?hl=en)

# (Danish) Social Security Numbers

# Location Data


According to the [Irish Data Protection Commission](https://www.dataprotection.ie/docs/Guidance-Note-for-Data-Controllers-on-Location-Data/1587.htm), "you should treat information about the location of a device which can be tracked or located electronically as **personal data**, and comply with the Data Protection Acts".

Location data may be "**sensitive personal data**" if it is possible to discover any of the defined sensitive traits about the data subject from the data.  



# (Informed) Consent


The following information must be part of the informed consent (or 'notice') to the user:

* The **identity** and the **contact details** of the controller and, where applicable the contact details of the data protection officer
* The **purposes** of the processing for which the personal data are intended as well as the **legal basis** for the processing
* The **categories** of the personal data
* The **recipients** of the personal data
* The **period** for which the personal data will be stored, and
* The existence of the right to **request** from the controller:
   * _access to_ and _rectification_ or _erasure_ of personal data or 
   * _restriction of processing_ concerning the data subject or 
   * to _object to processing_ as well as 
   * the right to _data portability_.

The consent must be;

* voluntary, 
* specific,
* informed 
* explicit. 

In addition, it should be as easy to withdraw consent as it is to give it.

## Consent Programming Models / APIs

* [ResearchStack](https://github.com/ResearchStack/ResearchStack/tree/master/backbone/src/main/java/org/researchstack/backbone/model)
* [ResearchKit](http://researchkit.org/docs/docs/InformedConsent/InformedConsent.html) 

with the following main java classes:

* `ConsentDocument` with `ConsentType`
* `ConsentSection`
* `ConsentSignature`
* `DocumentProperties`


# Privacy

## Encryption

## Coding Practices


# References

Here is a (more or less) random list of useful resources and references:

* [EU GDPR info](https://gdpr-info.eu)
* [GDPR on DTU Inside](https://www.inside.dtu.dk/da/medarbejder/it-og-telefoni/informationssikkerhed/gdpr)
    * [DTU Instruks for databeskyttelse](https://www.inside.dtu.dk/-/media/DTU-Inside/Medarbejder/IT-og-telefoni/Informationssikkerhed/GDPR/Instruks-for-databeskyttelse.ashx)
* [DTU Forskningsdata](https://www.inside.dtu.dk/da/medarbejder/forskning-innovation-og-raadgivning/forskningsdata)
* [Irish Data Protection Commission](https://www.dataprotection.ie/docs/Guidance-Note-for-Data-Controllers-on-Location-Data/1587.htm)
* Louise Olesen. GDPR in Denmark .Kammeradvokaten. Slides 19. April 2018.
