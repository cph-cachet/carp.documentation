# Data application service endpoint and data point DTO

## DataApplication service

Name of service == `dk.cachet.carp.data.DataUploadService`

### Endpoints

#### Open mHealth DataPoint

`POST http://{{resourceServer.host}}:{{resourceServer.port}}/v{{apiVersion}}/dataPoints`

#### CARP DataPoint Endpoint

- Java: `+addDataPoint( data: JsonDataPoint )`
- URL: `POST htpp://hostname:port/api/datapoint`

## JsonDataPoint

The following is an example `JsonDataPoint` used during data upload using `addDataPoint`.

### Version 1.2

On a meeting on May 7th 2020 (and based on [this note](https://docs.google.com/document/u/1/d/1lp-_hyC3VWxgVVwCmIBNpL5z9yxUNvTvp08fHpR8otI/edit)) the header `study_id` is changed to `study_deployment_id` (which is a UUID generated from the server). Hence, the json looks like this:

```
{
  "carp_header":
  {
    "study_deployment_id": "ace89584-..",          // UUID defined by server
    "device_role_name": "Patient's phone",
    "trigger_id": "Sensor Task",
    "user_id": "'user@cachet.dk",
    "data_format": {
      "namespace": "omh",
      "name": "acceleration",
    }
    "upload_time": "2018-08-27T21:40:29.693587",   // Set by server
    "start_time": "2018-08-27T21:40:29.693587",    // Start time of measure in the body
    "end_time": "2018-08-27T21:40:29.693587",      // Optional

  },
  "carp_body":
  {
    // e.g., Open mHealth header and body
  }
}
```


### Version 1.1 

On the meeting at Aug. 30th it was decided to change the header to;
 * add more details to the "data_format" header field
 * remove the `start_time` and `end_time` from the header -- this is specific to the data point and should be part of the body.

```
{
  "carp_header":
  {
    "study_id": "DF#4dD",
    "device_role_name": "Patient's phone",
    "trigger_id": "Sensor Task",
    "user_id": "'user@cachet.dk",
    "data_format": {
      "namespace": "omh",
      "name": "acceleration",
    }
    "upload_time": "2018-08-27T21:40:29.693587",   // Set by server
    "start_time": "2018-08-27T21:40:29.693587",    // Start time of measure in the body
    "end_time": "2018-08-27T21:40:29.693587",      // Optional

  },
  "carp_body":
  {
    // e.g., Open mHealth header and body
  }
}
```

In terms of namespace, CARP currently support the following two namespaces:
 * `omh` : Open mHealth
 * `carp` : CARP 

We need to define name space or data-format name....

### Version 1.0 format

```
{
  "carp_header":
  {
    "study_id": "i.e., GUID",
    "device_role_name": "e.g., Patient's phone",
    "trigger_id": "e.g., task1",
    "user_id": "i.e., defined by user service",
    "data_format": "e.g., org.openmhealth",
    
    "upload_time": "i.e., JSON datetime, set by server",
    "start_time": "i.e., JSON datetime",
    "end_time": "i.e., JSON datetime; optional"
  },
  "carp_body":
  {
    // e.g., Open mHealth header and body
  }
}
```
