Getting Started
========================

**Subscriber Consent Workflow**

Access Tokens are required in order to navigate and do transactions on IVES.
To get the access token, the subscriber needs to login via **POST request**:

### Get an Access token

Use <span class="method">POST</span> method on this URI:
```
/api/v1/auth/login
```

###### Representation Formats

For the IVES API, it is implemented
using application/json.

###### Request Body or Payload Parameters

| Parameter | Usage |
| ----------|-------|
| _string_ **email** refers to the email of your account on IVES | Required |
| _string_ **password** refers to the password of your account on IVES | Required |

###### Sample POST Request using Postmant

```
curl -X POST \
  https://ives.ph/api/v1/auth/login \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: 298' \
  -H 'Content-Type: multipart/form-data; boundary=--------------------------681062962588880388344671' \
  -H 'Cookie: _ives_session=ZG9aNUZnWTJjY2VIVkdaMVg0Z3U1RWFSVFpibFBadE1oTDJnUlNNeGc5ZzNuc2w3ck5ORHpadExxbjU5eDlDUFc5Um1TWTlpLzgwWXJyY1Uvbjd2cHZnS1VRSmh0KzJ2dXlaQi9xNDAyNzZLQVoybjJsamFCUFFFeHdockNwUlhEQmNTdUJEVU8rS1M3a2FpV3k3bkZBPT0tLUw5ejJsT1g3RS9rcnphTERXQlR4VFE9PQ%3D%3D--a06be212c01929d70b6f9ce3207b36d521217ac9' \
  -H 'Host: ives.ph' \
  -H 'Postman-Token: 3221bd41-6882-43a4-9727-75c463ffc151,03b8c470-c5fd-4ee4-9446-c91bd238b16d' \
  -H 'User-Agent: PostmanRuntime/7.19.0' \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F email=yourivesaccountemail@email.com \
  -F 'password=yourivesaccountpassword'
```

###### Sample Successful POST Response

```javascript
{
    "token": "your_token_to_be_use_for_navigating_ives",
    "exp": "11-12-2019 15:29",
    "email": "yourivesaccountemail@email.com"
}
```

###### Response Parameters

| Parameter 			 | Usage    |
| -----------------------|----------|
| **token** Token to be used on your Authorization header. | Required |
| **exp** Expiration date of your token. | |
| **email** Refers to the email of your account on IVES. | |

###### Error Response

```javascript
{
    "error": "unauthorized"
}
```

###### Response Parameters

| Parameter 			 | Usage    |
| -----------------------|----------|
| **error** Invalid account credential. | |


INPROGRESS
========================



Campaigns
========================

**Overview**

Short Message Service (SMS) enables your application or service to send and
receive secure, targeted text messages and alerts to your Globe / TM and other telco subscribers.

Note: All API calls must include the access_token as one of the
Universal Resource Identifier (URI) parameters.

### Sending SMS (SMS-MT)

Send an SMS message to one or more mobile terminals:

(Mobile Terminating - Application to Subscriber)

Use <span class="method">POST</span> method on this URI:
```
https://devapi.globelabs.com.ph/smsmessaging/v1/outbound/{senderAddress}/requests?access_token={access_token}
```


###### Representation Formats

For the Globe Labs API SMS, it is implemented
using application/json.

###### Resource Parameters

| Parameter | Usage |
| ----------|-------|
| _string_ **senderAddress** refers to the application short code suffix (last 4 digits) | Required |
| _string_ **access_token** which contains security information for transacting with a subscriber. Subscriber needs to grant an app first via SMS or Web Form Subscriber Consent Workflow. | Required |

###### Request Body or Payload Parameters

| Parameter        | Usage |
| -----------------|-------|
| _string_ **address** is the subscriber MSISDN (mobile number), including the ‘tel:’ identifier. Parameter format can include the ‘+’ followed by country code  +639xxxxxxxxx or 09xxxxxxxxx | Required |
| _string_ **message** must be provided within the ```outboundSMSTextMessage``` element. Currently, the API implementation is limited a maximum of 160 characters. Also make sure that your language or framework's editor is encoding the HTTP parameters as UTF-8 | Required |
| _string_ **clientCorrelator** uniquely identifies this create SMS request. If there is a communication failure during the request, using the same clientCorrelator when retrying the request allows the operator to avoid sending the same SMS twice. | Optional |

###### Sample POST Request

```
curl -X POST
"https://devapi.globelabs.com.ph/smsmessaging/v1/outbound/1234/requests?access_token=3YM8xurK_IPdhvX4OUWXQljcHTIPgQDdTESLXDIes4g" -H "Content-Type: application/json" -d
{"outboundSMSMessageRequest": {
   "clientCorrelator": "123456",
   "senderAddress": "1234",
   "outboundSMSTextMessage": {"message": "Hello World"},
   "address": "9171234567"
 }
}
```

###### Sample Successful POST Response

```javascript
{
 "outboundSMSMessageRequest": {
   "address": "tel:+639175595283",
   "deliveryInfoList": {
     "deliveryInfo": [],
     "resourceURL": "https://devapi.globelabs.com.ph/smsmessaging/v1/outbound/8011/requests?access_token=3YM8xurK_IPdhvX4OUWXQljcHTIPgQDdTESLXDIes4g"
   },
   "senderAddress": "8011",
   "outboundSMSTextMessage": {
     "message": "Hello World"
   },
   "receiptRequest": {
     "notifyURL": "http://test-sms1.herokuapp.com/callback",
     "callbackData": null,
     "senderName": null,
     "resourceURL": "https://devapi.globelabs.com.ph/smsmessaging/v1/outbound/8011/requests?access_token=3YM8xurK_IPdhvX4OUWXQljcHTIPgQDdTESLXDIes4g"
   }
 }
}
```

###### Response Parameters


| Parameter 			 | Usage    |
| -----------------------|----------|
| **outboundSMSResponse** Specifies the body of the response. | Required |
| **address** Subscriber MSISDN (mobile number) whom the SMS was sent to. | |
| **senderAddress** Refers to the application short code suffix (last 4 digits). | |
| **outboundSMSTextMessage** The string message sent to the subscriber’s number (address). | |
| **resourceURL** Specifies the URI that provides network delivery status of the sent message. The API Endpoint. | Required |
| **notifyURL** App call back URL defined at the App Info. | Optional |

__Note:__ Response parameters deliveryInfo, callbackData, senderName are optional parameters that are not currently supported by the Globe Labs SMS API. 
Error response with 400 series will deduct 0.50 from your wallet balance.

### Receiving SMS (SMS-MO)

In receiving SMS, globe will send a data(POST) to your Notify URL (that you provided when you created your app) when the subscriber sends an SMS or replied to your short code number.

(Mobile Originating - Subscriber to Application)


```javascript
{
  "inboundSMSMessageList":{
      "inboundSMSMessage":[
         {
            "dateTime":"Fri Nov 22 2013 12:12:13 GMT+0000 (UTC)",
            "destinationAddress":"tel:21581234",
            "messageId":null,
            "message":"Hello",
            "resourceURL":null,
            "senderAddress":"tel:+639171234567"
         }
       ],
       "numberOfMessagesInThisBatch":1,
       "resourceURL":null,
       "totalNumberOfPendingMessages":null
   }
}
```

In your Notify URL, create a script that will catch and save these data to a file or to the database.


###Multi-Part SMS

**Sending Multi-Part SMS**

You can send multi-part sms like you would in sending a normal sms, just keep in mind that you will be charged per 160 characters. 


###### Sample POST Request

```
curl -X POST
"https://devapi.globelabs.com.ph/smsmessaging/v1/outbound/1234/requests?access_token=3YM8xurK_IPdhvX4OUWXQljcHTIPgQDdTESLXDIes4g" -H "Content-Type: application/json" -d
{"outboundSMSMessageRequest": {
   "clientCorrelator": "123456",
   "senderAddress": "1234",
   "outboundSMSTextMessage": {"message": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis commodo posuere dui vitae feugiat. Cras vehicula, elit eget commodo tristique, mi magna placerat turpis, quis ultrices massa odio vitae sapien. Proin elit diam, malesuada sit amet blandit id, fermentum eu orci."},
   "address": "9171234567"]
 }
}
```
 
**Receiving Multi-Part SMS**

There will be additional parameters whenever you receive an sms content exceeding 160 characters, this would be:

Parameter | Description |
----------|-------------|
`multipartRefId` | ID of multi-part message
`multipartSeqNum` | Sequence of multi-part message

sample of expected format of multi-part content below:

```json
{
   "inboundSMSMessageList":{
      "inboundSMSMessage":[
         {
            "dateTime":"Fri Nov 22 2013 12:12:13 GMT+0000 (UTC)",
            "destinationAddress":"tel:21581234",
            "messageId":"57cd07d61a28d80100a26bc7",
            "message":"AlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrigh",
            "resourceURL":null,
            "senderAddress":"tel:+639171234567",
            "multipartRefId":"5476ddbba4e7d20c527c0f83422bf938",
            "multipartSeqNum":"1"
         }
      ],
      "numberOfMessagesInThisBatch":"2",
      "resourceURL":null,
      "totalNumberOfPendingMessages":0
   }
}

{
   "inboundSMSMessageList":{
      "inboundSMSMessage":[
         {
            "dateTime":"Fri Nov 22 2013 12:12:13 GMT+0000 (UTC)",
            "destinationAddress":"tel:21581234",
            "messageId":"57cd07dab9d3660100cee8d2",
            "message":"tAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightAlrightu",
            "resourceURL":null,
            "senderAddress":"tel:+639171234567",
            "multipartRefId":"5476ddbba4e7d20c527c0f83422bf938",
            "multipartSeqNum":"2"
         }
      ],
      "numberOfMessagesInThisBatch":"2",
      "resourceURL":null,
      "totalNumberOfPendingMessages":0
   }
}
```

Batch of messages could be identified if they have the same `multipartRefId`.

###Binary SMS

Binary Short Messaging interface allows an application to send any generic binary object attachments to the network using SMS.

Use <span class="method">POST</span> method on this URI:
```
https://devapi.globelabs.com.ph/binarymessaging/v1/outbound/{senderAddress}/requests?access_token={access_token}
```
#### Parameters

Parameter | Description | Required
----------|-------------|----------
`userDataHeader` | UDH of the message | true
`dataCodingScheme` | data coding of the message | true
`address` | MSISDN of the recipient | true
`outboundBinaryMessage.message` | message to be sent | true
`senderAddress` | shortcode of the app | true
`access_token` | access token of the subscriber | true

Data Coding Value | Description
------------------|------------
0                 | SMSC Default
1                 | IA5/ASCII
3                 | Latin 1 (ISO-8859-1)
4                 | Binary (8-bit)
8                 | UCS2 (Unicode)

```json
{
  "outboundBinaryMessageRequest": {
    "address": "9171234567",
    "deliveryInfoList": {
      "deliveryInfo": [],
      "resourceURL": "https://devapi.globelabs.com.ph/binarymessaging/v1/outbound/{senderAddress}/requests?access_token={access_token}",
    "senderAddress": "21581234",
    "userDataHeader": "06050423F423F4",
    "dataCodingScheme": 1,
    "outboundBinaryMessage": {
      "message": "samplebinarymessage"
    },
    "receiptRequest": {
      "notifyURL": "http://example.com/notify",
      "callbackData": null,
      "senderName": null
    },
  "resourceURL": "https://devapi.globelabs.com.ph/binarymessaging/v1/outbound/{senderAddress}/requests?access_token={access_token}",
  }
}
```

###SMS API HTTP Response

|Code|Description|
|----|-----------|
|201|Request has been successful|
|400/401|Request failed. Wrong or missing parameters, invalid subscriber_number format, wrong access_token. |
|502/503|Platform Error. API Service is busy or down|

API requests with a response code of 201, 400 or 401 will be chargeable against your developer wallet. Standard SMS API rates apply, unless otherwise stated.


CALL FLOWS
=====================

**Overview**

The USSD API allows users to access your products or services free of charge by accessing the dial menu through a dedicated number.

__Note__: The USSD API is not readily available upon app creation. To avail, please email your app's use case and company name to api@globe.com.ph

### Sending Network Initiated USSD Message (USSD-NI)

Use <span class="method">POST</span> method on this URI:
```
https://devapi.globelabs.com.ph/ussd/v1/outbound/{shortcode}/send/requests?access_token={access_token}
```

###### Resource Parameters

| Parameter | Usage |
|-----------|----------|
| _string_ **shortcode** This is the last 4 digits of the app's shortcode | Required |
| _string_ **access_token** This contains security information for transacting with a subscriber. The subscriber needs to grant an app first via SMS or Web Form Subscriber Consent Workflow | Required |

###### Representation Format

For the Globe Labs USSD API, the expected format is ```application/json```

###### Sample Post Request (USSD-NI)

```json
curl -X POST
"https://devapi.globelabs.com.ph/ussd/v1/outbound/9996/send/requests?access_token=LsQwuHog3n_OwlYSjj4QFujb451HeqCPyaAdkKgRAkM" -H "Content-type: application/json" -d
{
    "outboundUSSDMessageRequest": {
        "outboundUSSDMessage": {
            "message": "Simple USSD Message\nOption - 1\nOption - 2"
        },
        "address": "639171111111",
        "senderAddress": "21589996",
        "flash": false
    }
}
```

###### Request Body or Payload Parameters

| Parameter | Usage |
|-----------|----------|
| _string_ **outboundUSSDMessage.message** This is the message to be sent and displayed on the subscriber's mobile phone | Required |
| _string_ **address** This is the subscriber's mobile number | Required |
| _string_ **senderAddress** This is the shortcode of the app | Required |
| _boolean_ **flash** This indicates whether this is the final message or not | Required |

###### Sample Successful POST Response (USSD-NI)

```json
{
  "outboundUSSDMessageRequest": {
    "address": "639171111111",
    "deliveryInfoList": {
      "deliveryInfo": [],
      "resourceURL": "https://devapi.globelabs.com.ph/ussd/v1/outbound/21589996/reply/requests?access_token=access_token"
    },
    "senderAddress": "21589996",
    "outboundUSSDMessage": {
      "message": "Simple USSD Message\nOption - 1\nOption - 2"
    },
    "receiptRequest": {
      "ussdNotifyURL": "http://example.com/notify",
      "sessionID": "012345678912"
    },
    "resourceURL": "https://devapi.globelabs.com.ph/ussd/v1/outbound/21589996/reply/requests?access_token=access_token"
  }
}
```
###### Response Parameters

| Parameter | Usage |
|-----------|----------|
| _string_ **address** This is the subscriber's mobile number | |
| _string_ **resourceURL** This is the API endpoint. This specifies the URI that provides network delivery status of the sent message. | |
| _string_ **senderAddress** This is the shortcode of the app | |
| _string_ **outboundUSSDMessage.message** This is the message to be sent and displayed on the subscriber's mobile phone | |
| _string_ **ussdNotifyURL** This is the URI where the USSD responses will be sent | |
| _string_ **sessionID** This is the unique identifier for the ongoing USSD session | |

### Sending USSD Reply Message (USSD-MT)

Use <span class="method">POST</span> method on this URI:

```
https://devapi.globelabs.com.ph/ussd/v1/outbound/{shortcode}/reply/requests?access_token={access_token}
```

###### Resource Parameters

| Parameter | Usage |
|-----------|----------|
| _string_ **shortcode** This is the last 4 digits of the app's shortcode | Required |
| _string_ **access_token** This contains security information for transacting with a subscriber. The subscriber needs to grant an app first via SMS or Web Form Subscriber Consent Workflow | Required |

###### Sample POST Request (USSD-MT)

```json
curl -X POST
'https://devapi.globelabs.com.ph/ussd/v1/outbound/9996/reply/requests?access_token=LsQwuHog3n_OwlYSjj4QFujb451HeqCPyaAdkKgRAkM' -H "Content-type: application/json" -d
{
    "outboundUSSDMessageRequest": {
        "outboundUSSDMessage": {
            "message": "Simple USSD Message\nOption - 1\nOption - 2"
        },
        "address": "639171111111",
        "senderAddress": "21589996",
        "sessionID": "012345678912",
        "flash": false
    }
}
```

| Parameter | Usage |
|-----------|----------|
| _string_ **outboundUSSDMessage.message** This is the message to be sent and displayed on the subscriber's mobile phone | Required |
| _string_ **address** This is the subscriber's mobile number | Required |
| _string_ **senderAddress** This is the shortcode of the app | Required |
| _string_ **sessionID** This is the unique identifier for the ongoing USSD session | Required |
| _boolean_ **flash** This indicates whether this is the final message or not | Required |

###### Successful POST Response (USSD-MT)

```json
{
  "outboundUSSDMessageRequest": {
    "address": "639171111111",
    "deliveryInfoList": {
      "deliveryInfo": [],
      "resourceURL": "https://devapi.globelabs.com.ph/ussd/v1/outbound/21589996/reply/requests?access_token=access_token"
    },
    "senderAddress": "21589996",
    "outboundUSSDMessage": {
      "message": "Simple USSD Message\nOption - 1\nOption - 2"
    },
    "receiptRequest": {
      "ussdNotifyURL": "http://example.com/notify",
      "sessionID": "012345678912",
      "referenceID": "f7b61b82054e4b5e"
    },
    "resourceURL": "https://devapi.globelabs.com.ph/ussd/v1/outbound/21589996/reply/requests?access_token=access_token"
  }
}
```

| Parameter | Usage |
|-----------|----------|
| _string_ **address** This is the subscriber's mobile number | |
| _string_ **resourceURL** This is the API endpoint. This specifies the URI that provides network delivery status of the sent message. | |
| _string_ **outboundUSSDMessage.message** This is the message to be sent and displayed on the subscriber's mobile phone | |
| _string_ **senderAddress** This is the shortcode of the app | |
| _string_ **ussdNotifyURL** This is the URI where the USSD responses will be sent | |
| _string_ **sessionID** This is the unique identifier for the ongoing USSD session | |

###### Sample USSD Callback
This data will be sent to your USSD Notify URL.

```
{
  "inboundUSSDMessageList": {
    "inboundUSSDMessage":[
    {
      "datetime":"Tue May 17 2016 11:10:32 GMT+0000 (UTC)",
      "destinationAddress":"21589996",
      "message":"*120*9996*3#",
      "senderAddress":"639171111111",
      "sessionID": "012345678912"
    }
  ]
  }
}
```

###  User Inititated USSD Message

Dial ``*120*{shortcode}`` and press <span class="method">CALL</span> on your mobile phone. 
This will start the USSD session and send ``{"message":`null`, sessionStart: true}`` to your USSD Notify URL.

Alternatively, you can send a sequence of inputs upon starting the USSD session by dialing ``*120*{shortcode}*{input 1}*{input 2}*{input 3}#``.
For example, dialing ``*120*9996*1*2*3#`` and pressing <span class="method">CALL</span> will start the USSD session and automatically send ``{"message":`['1', '2', '3']`, sessionStart: true}`` to your USSD Notify URL

### Error Codes

| Status Code | Error Message |
|-------------|---------------|
| 200 | N/A |
| 400 | Missing parameters |
| 400 | Excess parameters |
| 400 | Invalid subscriber number |
| 401 | Invalid access token |
| 401 | App is not provisioned for USSD |
| 403 | Your account is currently blocked. Please contact api@globe.com.ph for reactivation. |
| 403 | The subscriber is blacklisted |
| 403 | Insufficient Wallet Balance |
| 404 | App not found |
