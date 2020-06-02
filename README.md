IVES API
========================

**API Security**

Access Tokens are required in order to navigate and do transactions on IVES.
To get the access token, the subscriber needs to generate one via **POST** request:

### Generate an Access token

Use <span class="method">POST</span> method on this URI:
```
https://ives.ph/api/v1/auth/login
```

###### Representation Formats

For the IVES API, it is implemented using application/x-www-form-urlencoded.

###### Request Parameters

| Parameter | Usage |
| ----------|-------|
| _string_ **email** refers to the email of your account on IVES | Required |
| _string_ **password** refers to the password of your account on IVES | Required |

###### Sample POST Request using Postman

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

###### Response Description

| Parameter 			 | Description    |
| --------------|----------------|
| **token** | Token to be used on your Authorization header.  |
| **exp** | Expiration date of your token. |
| **email** | Refers to the email of your account on IVES. |

###### Error Response

```javascript
{
    "error": "unauthorized"
}
```

###### Response Description

| Parameter 			 | Description    |
| -----------------------|----------------|
| **error** | Invalid account credential. |

Call Flow
========================

**Overview**
Call flows or IVR Trees that are already created and to be used by campaign.

**Note: API calls must include the Authorization header token**

### Call Flows / IVR Trees - listing, filtering and sorting

Use <span class="method">GET</span> method on this URI:
```
/api/v1/ivr_trees
```

###### Representation Formats

For the IVES API, it is implemented
using application/json.

###### Request Parameters

| Parameter | Usage |
| ----------|-------|
| _string_ **with_name** find call flows obtaining with **name** | Optional |
| _string_ **with_caller_id** find call flows obtaining with **caller_id** | Optional |
| _string_ **with_non_globe_caller_id** find call flows obtaining with **non_globe_caller_id** | Optional |
| _string_ **with_sms_masking** find call flows obtaining with **sms_masking** | Optional |
| _string_ **with_variables** find call flows obtaining with **variables** | Optional |
| _string_ **between_by_created_at** find call flows between dates based on their **created_at**. format( **YYYYMMDD,YYYYMMDD** ) sample: **between_by_created_at=20190101,20191230** | Optional |
| _string_ **sorted_by** Sorting all call flows base on values(name_asc, name_desc, created_at_desc, created_at_asc, caller_id_asc, caller_id_desc, sms_masking_asc, sms_masking_desc, variables_asc, variables_desc). sample: **sorted_by=name_desc** | Optional |
| _string_ **per** number of collection you want to list | Optional |
| _string_ **page** page of collections | Optional |


###### Sample GET Request using Postman
**Note: API calls must include the Authorization header token**

```
curl -X GET \
  'https://staging.ives.ph/api/v1/ivr_trees?with_name=C&with_caller_id=9&with_non_globe_caller_id=9&with_sms_masking=IVES&with_variables=mobile&between_by_created_at=20180101,20191230&sorted_by=name_asc&page=1&per=2' \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo2LCJleHAiOjE1NzM2MTM5NTh9.YSjlDJEYvMCVckShzHGYuRr43IQkMUAFBKSS5SXiDj8' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Cookie: _ives_session=V2JCRk9IK1RqbjVmaXBmODU5WlJHUU5Wa1Bzc0tBbStLK3dIZExHWlBlMXY2OGxQb256VkxNMDNPdWx3TVhQR1JYUTltaXZORUxJVzNOWkhENmhteUszSnBLZFUvSzN1QVBaenMvbzlaVjkvZlJvYTYxSXZvZCt1L2xnbkZzRXFFc01JOGRuaVBPYndmb1ZrVjZIbVBmZWRvbUNTZkhLN3VaV1VPelprT1JWYnFxUU9CRXpuN2M2SnpXZTlZWDVhUTFiRDdWQ3l3TFBVVjBSZ1Ztalg5Yk5hR241b2UvKzFNV1BHY2pFMmdPdHdpRU4zZ2FQMlEwTThRRmFockl2bkdKajFkTUVIaGNFaWZqY3BzbkpmWHZhUWZNNVV1Ynd1Y1A1eUFxeFRISHU2Z2txU1J4Q2t1M2NiYStJZFRuVHU3VlV1MFllam1JNnE1RnZ4R1RXQVpBPT0tLWpMaGFaY3gyZnVxenY4K3JhK3c3cWc9PQ%3D%3D--53a4308c655c1b0aef2650d2826f6fd37e8677d9' \
  -H 'Host: staging.ives.ph' \
  -H 'Postman-Token: 3878fcb8-ae1a-469c-b215-f671b099c78f,e4fd5251-1547-4919-91e0-b66e33eecc9a' \
  -H 'User-Agent: PostmanRuntime/7.19.0' \
  -H 'cache-control: no-cache'
```

###### Sample Successful GET Response

```javascript
{
    "status": "success",
    "data": [
        {
            "id": 25,
            "account_id": 6,
            "name": "CF 1",
            "caller_id": "9XXXXXXXXX",
            "non_globe_caller_id": "9XXXXXXXXX",
            "sms_masking": "IVES",
            "variables": "mobile",
            "created_at": "2019-08-13T15:01:13.146+08:00",
            "voice": "female"
        },
        {
            "id": 24,
            "account_id": 6,
            "name": "CF 2",
            "caller_id": "9XXXXXXXXX",
            "non_globe_caller_id": "9XXXXXXXXX",
            "sms_masking": "IVES",
            "variables": "mobile",
            "created_at": "2019-08-13T15:01:13.146+08:00",
            "voice": "female"
        }
    ],
    "page_total": 1,
    "default_per_page": 5,
    "per_page": 5,
    "total_items": 3,
    "current_page": 1
}
```

###### Response Description

| Parameter 			 | Description    |
| --------------|----------------|
| **status** | status or request if success or false. |
| **data** | collections or specific object. |
| **page_total** | total pages of collections.  |
| **default_per_page** | default count of returned collection per page. |
| **per_page** | desired count of returned collection per page. |
| **current_page** | current page you are in. |

**Call flow / Ivr Tree data**

| Parameter 			 | Description    |
| --------------|----------------|
| **id** | Id of call flow / ivr tree. |
| **account_id** | account id that the ivr tree belongs. |
| **name** | call flow / ivr tree name.  |
| **caller_id** | call flow / ivr tree caller_id. |
| **non_globe_caller_id** | call flow / ivr tree non_globe_caller_id. |
| **sms_masking** | call flow / ivr tree sms masking. |
| **variables** | call flow / ivr tree variables used. |
| **created_at** | when call flow / ivr tree is created |
| **voice** | call flow / ivr tree voice **female** or **male**. |

###### Error Response

```javascript
{
    "status": "failed",
    "errors": "Invalid sort option: \"namee\""
}
```

###### Response Description

| Parameter 			 | Description    |
| -----------------------|----------------|
| **status** | Failed. |
| **error** | Error messages |

### Specific Call Flow / Ivr Tree

Use <span class="method">GET</span> method on this URI:
```
/api/v1/ivr_trees/:id
```

###### Representation Formats

For the IVES API, it is implemented using application/json.

###### Request Parameters

| Parameter | Usage |
| ----------|-------|
| **id** refers to call flow / ivr tree id. | Required |

###### Sample GET Request using Postman
**Note: API calls must include the Authorization header token**

```
curl -X GET \
  https://staging.ives.ph/api/v1/ivr_trees/16 \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjo2LCJleHAiOjE1NzM2MTM5NTh9.YSjlDJEYvMCVckShzHGYuRr43IQkMUAFBKSS5SXiDj8' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Cookie: _ives_session=c0ZieVFWc3diMjQvYllLK3pZTFJMMTFmSmh5ZlliYkVpeTZMZTIwS0VHUXFBUlR2cW10RGlzL3M5aVo2eWFrQUdGWkQ5M2JNRGhOMnVnVktvT09CYzZ0NGU2RkJCakZ6bHNScmdNZnJCMnNFVFBQa1VyZ21UazRocTJQT2JUT2hjdjgrald1Y0NRemozS1RrVllqdDhZVWc1NCsydllHcmRNdkNnU0pkN2VHMXZ4Q1F4R1U5M0pxMy95VTJoUmpYT1pHNHNZNk5YMkI1YUZJaWtkTVhJSTZpS0hseCtmU0MrZGp3OE1uODA1dXdpUE9XWjhlR3E4aGxPeDRmbmJqSzhvVW9mSVNaa0duMHkrdEdVdDdWSGozTDNXeDVUempUbjFJZHQzRjVKcFJvOWNZSnM4bElRVi92Z2FxcjZSNTUtLWEzTjhyS2lrNGRQUG9DbTJ3UE5Wamc9PQ%3D%3D--20e35dcad84ef90fcdde9676fdb37b4737fccbd0' \
  -H 'Host: staging.ives.ph' \
  -H 'Postman-Token: b06b4a92-ae94-4ec6-8e5f-c3732092c529,05e40462-1bed-4f33-9d56-e5d3f564d0d8' \
  -H 'User-Agent: PostmanRuntime/7.19.0' \
  -H 'cache-control: no-cache'
```

###### Sample Successful GET Response

```javascript
{
    "status": "success",
    "data": {
        "id": 16,
        "account_id": 6,
        "name": "CallOut - CF 1",
        "caller_id": "9568916933",
        "non_globe_caller_id": "9568916933",
        "sms_masking": "IVES",
        "variables": "mobile",
        "created_at": "2019-06-28T15:01:08.946+08:00",
        "voice": "male"
    }
}
```

###### Response Description

| Parameter 			 | Description    |
| --------------|----------------|
| **status** | status or request if success or false. |
| **data** | collections or specific object. |

**Call flow / Ivr Tree data**

| Parameter 			 | Description    |
| --------------|----------------|
| **id** | Id of call flow / ivr tree. |
| **account_id** | account id that the ivr tree belongs. |
| **name** | call flow / ivr tree name.  |
| **caller_id** | call flow / ivr tree caller_id. |
| **non_globe_caller_id** | call flow / ivr tree non_globe_caller_id. |
| **sms_masking** | call flow / ivr tree sms masking. |
| **variables** | call flow / ivr tree variables used. |
| **created_at** | when call flow / ivr tree is created |
| **voice** | call flow / ivr tree voice **female** or **male**. |

###### Error Response

```javascript
{
    "status": "failed",
    "errors": "Couldn't find IvrTree with 'id'=145"
}
```

###### Response Description

| Parameter 			 | Description    |
| -----------------------|----------------|
| **status** | Failed. |
| **error** | Error messages |
