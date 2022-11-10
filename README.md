
# Third Party API Integrations

We've written a quick step-by-step guide to walk you through how to login inside the Biot user panel, make your first API call inside your system, and interpret the results.


## Introduction

This Api service will help users to synchronize their data with biot & fetch that data into their own system. For this we have created various Api services where users can purchase services as per their requirement and get access towards it. To learn more about this we have created a step by step guide where users will get to learn how to integrate this Api with their system. To Know more About this, [Please Contact Our Team](https://biotworld.in/general-inquiry-biot/).


<p align="center">
 <img src="https://biotworld.in/wp-content/uploads/2022/02/biometric-machine.png"  height="400"/>
</p>


# How it works


## Login

### Step 1: Login:
when user purchase Api's they will recieve a mail which contains `Mobile Number` & `Authy Key`, which they have to use while calling Login api.
When a user sends a request with payload (mobile_number & auth_key) an authentication token is generated which you will receive in your response.

### All Api Request & Response are shown below:

#### we have stored the url in a variable call `base_url`
```
base_url =  https://biotworld.in/api/external

```


### Login API:
 ```
  Method: POST
  URL: <base_url>/login
 ```


#### Request
 User will Send a request with Mobile Number & Auth Key
```json
{
   "mobile_number": "9586722584",
   "auth_key"     : "gzMzM3NjMsImlkIjoxODQxLCJpYXQiOjE",
}

```

#### Success Response
```json
{
   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2NjgzMzM3NjMsImlkIjoxODQxLCJpYXQiOjE2NjU3NDE3NjN9.s6MoReMqc_29hXErb1YuzhZdc5w0plyAyQIjB1k7i3A"
}


```

#### Error Response
If Mobile number or password is incorrect then user will get below response.
```json
{
  "status": "failed",
  "status_code": 400,
  "message": "Invalid Credentials." 
}
```


If your acccount is not active then user will get below response.
```json
{
  "status": "failed",
  "status_code": 400,
  "message": "Your account is inactive." 
}
```


If your acccount is not registered then user will get below response.
```json
{
  "status": "failed",
  "status_code": 400,
  "message": "Your account is not registered." 
}
```


**Note** you need to pass both `token` & `authentication key` inside your header while calling any of the below api


## 1A) User Synchronization Post Api
### Instructions:
* Users can send Maximum 10 records within a single API hit.
* If more than 10 records are initiated at single hit it will throw error .i.e More than 10 records at once is initiated



* List of field's which needs to be pass in the payload while calling Api

| Field | Datatype | Requirements | Description |
| :--- | :--- | :--- | :--- |
| Name | varchar(50) | Mandatory | Enter Full Name|
| Date of joining | datetime | Mandatory | Enter the Date of Joining of the user|
| Email| varchar(50) | Mandatory | Enter Email Id for the user |
| Mobile No  | varchar(15)  | Mandatory | Mobile number with country code. I.e +91 9898989898 |
| Date of Birth | dateTime() | Optional | Enter Date of birth of the user |
| Gender | varchar(50) | Optional | Male,Female,other |
| Department | varchar(30) | Optional | Name of Department which belongs to the user.|
| Shift Start Time | dateTime() | Optional | Shift Start Time user need to provide shift start time as 24 hrs format in HH:MM |
| Shift End Time | dateTime() | Optional |  Shift End Time user need to provide shift End time as 24 hrs format in HH:MM |
| Designation | varchar(30)  | Optional | Enter Designation of the user|
| Off Day 1 | varchar(30)  | Optional | Pass number of day. i.e. Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday |
| Off Day 2 | varchar(30)  | Optional | Pass number of day. i.e. Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday |
| Off Day 2 Applies To Week | varchar(30) | Optional| Pass weekly off for which off day 2 is applied to be provided in comma separated value i.e. '2nd, 4th' ,if all off day 2 is observed as weekly off then input is to be provided as '1st, 2nd, 3rd, 4th, 5th' |
| Off Day 2 Type | varchar(30) | Optional | Full Day, Half Day (apply for Week_off_day2) |
| User Role | int[1] | Optional | Pass 0=Normal User, 3=Supervisor |

#### Note: `User Role` parameter & value with description:

| Parameter | Value | Description |
| :--- | :--- | :--- |
| user_role | 0 | Normal User Data |
| user_role | 3 | Supervisor |
| user_role | Other Than 0,3 | Provided user_role is not available|

#### Description:
The following endpoint creates a user using basic details such as name, email, mobile_number, department, designation and so on.

1. This Parameter user_role is used to assign specific roles to users, roles can be either normal user, supervisor, Default value for parameter role is considered as 0.
2. Below is the detail explanation about roles :

  * Normal User can mark & regularize the attendance 
  * Supervisor can perform below activity i.e. Full functionality of User Role, Limited Access to “User” Page ( EDIT USER ONLY ), Limited Access to “User” Page (           EDIT USER ONLY ), Can capture photo of user, Access to “Identity” & “Accessible Devices” tabs, Identity Tab : To Register Fingers & Face and assign BLE card,           Accessible Devices : To Give access on Biot Devices, Can not assign Supervisor role to existing users
  

### Step 1: Pass Token & authentic-key in the headers:
Now the user will need to pass that token which you have generated while login & authentication-key which you have recieved via mail in the headers.


```
 Method: POST
 URL: <base_url>/user
 Header: 
"token": <jwt token>,
"authentication-key": <client will get it from their email>,
"content-type": application/json

```

#### Request 
``` json
{
"data":[
     {
       "name":"john smith",
       "date_of_joining":"23–09-2019",
       "date_of_birth":"23–09-1999",
       "mobile_number":"2309201912",
       "email":"john@gmail.com",
       "department":"software",
       "gender":"male", 
       "designation":"project manager",
       "shift_start_time":"10:00",
       "shift_end_time":"19:00",
       "week_off" : {
           "offday1": "Sunday",
           "offday2": "Saturday",
           "offday2_applies_to_week": "2nd, 4th",
           "offday2_type": "half day"
        },
       "user_role":"0"
     },
    {
       "name":"Kriti Kumari" ,
       "date_of_joining":"23–09-2019",
       "date_of_birth":"23–09-1998",
       "mobile_number":"2309201922",
       "email":"kriti@gmail.com",
       "department":"software",
       "gender":"female",
       "designation":"project manager",
       "shift_start_time":"10:00",
       "shift_end_time":"19:00",
       "week_off" : {
           "offday1": "Sunday",
           "offday2": "Saturday",
           "offday2_applies_to_week": "2nd, 4th",
           "offday2_type": "half day"
       },
       "user_role":"3",
    },

  ]
}
```


#### Success Response
success response would be "User data has been synced successfully".
``` json

{
  "status": "success",
  "status_code": 200,
  "message": "User has synced successfully." 
}

```

#### Response when while there is error in saving some data: 
In case any error occurs during sending the request, then the user will be able to see in response for which particular object the error occurred & which object has synced successfully so next time the user only send the particular object in which user receives the error.


``` json
{
  "Total_user_count":10 "records",
  "success_user_count":5 "records",
  "error_user_count": 5 "records",
  "description": "There was an error while adding Emp_id please re enter Emp_id for the last 5 entry"
}


```

``` json
[
   {
     "status":"success",
     "status_code":200,
   },

   {
    "status": "failed",
    "status_code": 400,
    "error_code": 1
   },

  {
    "status": "failed",
    "status_code": 400,
    "error_code": 2
  },
  
  {
    "status": "failed",
    "status_code": 400,
    "error_code": 3
  },
  
  {
    "status": "failed",
    "status_code": 400,
    "error_code": 4
  },
  
  {
   "status": "failed",
   "status_code": 400,
   "error_code": 5
  }
]
```

#### Error Response & their failure cases are shown below:

| Error code | Error message| Cause | Solution
| :--- | :--- | :--- | :--- |
| 1 | The {input field} is required .i.e. {name,mobile number and so on}  | A mandatory field is empty | Ensure all mandatory fields and values are present.|
| 2 | Data Already Exist | An existing user record has been passed .i.e {name,email,mobile number} | Ensure that a unique user record is passed |
| 3 | Please enter valid company id | Invalid Company Id has passed | Ensure that valid company Id is passed |
| 4 | Please do not enter more than 50 characters | User have passed more than 50 characters .i.e {name,email,department,designation} | Ensure that less than 50 characters are passed |
| 5 | Please enter valid email_id | Email Id format is not valid | Email id should be {example@mail.com} |
| 6 | Mobile Number should consist 10 digits only| Email Id format is not valid | Email id should be {+91 9988776655} |
| 7 | Please enter valid date-format  | {date of joining / date of birth } format is not valid | Date format should be (dd-mm-yyyy) |
| 8 | Please enter valid start/end shift time | {shift start time / shift end time } format is not valid | Start/end shift should be in 24 hrs format in HH:MM |




## 1B) User Synchronization Get Api:

### Instructions 
*  Default user will able to retrieves 10 records per page.
* `page_number` parameter is mandatory in order to consume this api .i.e. `1` or `2` or `3`....n to receive different records per page.
* `user_id` will be unique & auto generated.
*  To fetch particular user data user need to pass `user_id` as parameter. 
*  when fetching particular user data no need to pass `page_number` as parameter.


### Description:
* The following endpoint retrieves the details of all the user data from biot account.



``` 
 Method: GET
 URL: <base_url>/employee?page_number=1 (when want all user record) | <base_url>/employee/10000119
 Header: 
"token": <jwt token>,
"authentic-key": <client will get it from their email>,
"content-type": application/json

```

#### Response

``` json
{
"data":[
          {
            "name":"john smith" ,
            "user_id":"10000119",
            "date_of_joining":"23–09-2019",
            "date_of_birth":"23–09-2000",
            "mobile_number":"2309201909",
            "email":"test@gmail.com",
            "gender":"male",
            "department":"development",
            "designation":"project manager",
            "shift_start_time":"10:00:00",
            "shift_end_time":"19:00:00",
            "week_off": {
               "offday1": "Sunday",
               "offday2": "Saturday",
               "offday2_applies_to_week": "2nd, 4th",
               "offday2_type": "half day"
             },
            "user_role":"2",
         },
        {
          "name":"john smith" ,
          "user_id":"10000119",
          "date_of_joining":"23–09-2019",
          "date_of_birth":"23–09-2000",
          "mobile_number":"2309201998",
          "email":"test@gmail.com",
          "gender":"male",
          "department":"development",
          "designation":"project manager",
          "shift_start_time":"10:00:00",
          "shift_end_time":"19:00:00",
          "week_off":  {
               "offday1": "Sunday",
               "offday2": "Saturday",
               "offday2_applies_to_week": "2nd, 4th",
               "offday2_type": "half day"
          },
          "user_role":"0",
         },

      ]
}


```



### Record set can be filtered through below parameters:

| Parameters | Type | Description |
| :--- | :--- | :--- |
| user_id | string | Response will filtered with given user_id  |
| name  | string | Response will filtered with given name  |
| date_of_joining | string | Response will filtered with given date_of_joining  |
| date_of_birth | string | Response will filtered with given date_of_birth |
| mobile_number | integer | Response will filtered with given mobile_number |
| email  | string | Response will filtered with given email  |
| department  | string | Response will filtered with given department  |
| designation | string | Response will filtered with given designation  |


#### Filter with Single Parameters:
```
1. Name            :  <base_url>/employee?name=rajat&page_number=1
2. UserID          :  <base_url>/employee?user_id=256&page_number=1
3. Date of joining :  <base_url>/employee?date_of_joining=22-11-2020&page_number=1
4. Date of birth   :  <base_url>/employee?date_of_birth=11-05-1994&page_number=1
5. Mobile number   :  <base_url>/employee?mobile_number=8469368525&page_number=1
6. Email           :  <base_url>/employee?email=rajat@gmail.com&page_number=1 
7. Department      :  <base_url>/employee?department=development&page_number=1
8. Designation     :  <base_url>/employee?designation=manager&page_number=1

```

#### Filter with Multiple Parameters:
```
<base_url>/employee?department=IT&designation=manager&page_number=1

```
#### Error Response & their failure cases are shown below:
| Error message| Cause | Solution
| :--- | :--- | :--- |
| The user id provided does not exist | The User Id does not belong to the requestor, or it doesn't exist. | Ensure that the User id is valid and belongs to the requestor.|



## 1c) User Synchronization Edit Api:
* This Api is used to Edit User records from Biot account.
* In order to consume this api `user_id` is the parameter
* User id value should be pass in the url 

### Description:
The following endpoint edits the user details such as the name, email, date_of_joining and so on.


```
 Method: POST
 URL: <base_url>/employee/10000119
 Header: 
"token": <jwt token>,
"authentication-key": <client will get it from their email>,
"content-type": application/json

```

#### Request 
``` json
"data":
     {
       "name":"john smiths",
       "date_of_joining":"23–09-2017",
       "date_of_birth":"23–09-1999",
       "mobile_number":"2309201912",
       "email":"john@gmail.com"
     }
     
 ```
 
 #### Success Response
```json
{
   {
  "status": "success",
  "status_code": 200,
  "message": "User updated successfully." 
   }
}


```







## 2) Transaction (punch detail) :
####  This Api is used to get Transaction details
####  In order to consume this api transaction_type, transaction_from_date, transaction_to_date, & attendance_date are the parameters.



### Step 1: When User pass `transaction_type` as `0`
* User need to pass `transaction_from_date` & `transaction_to_date` parameters in the url i.e. if user wants transaction records between `1-01-2022` to   `30-01-2022`. This parameters will get users `All` transactions logs of the user between this particular date.
* `transaction_from_date` & `transaction_to_date` parameters not mandatory. 

### Description:
* `page_number` parameter is mandatory in order to consume this api .i.e.  `1` or `2` or `3`....n to receive different records per page.

``` 

 Method: GET
 URL: <base_url>/transactions?transaction_type=0&transaction_from_date=1-01-2022&transaction_to_date=30-01-2022&page_number=1
 Header: 
"token": <jwt token>,
"authentic-key": <client will get it from their email>,
"content-type": application/json

```

#### Response for Transaction log (when transaction_type == 0): 

``` json
{
"data":[
	 {
            "user_id":"10000119",
            "name":"john smith",
            "department":"development",
            "designation":"project manager",
            "date":"2022-10-27",
            "time":"10:00:00",	
            "punch_flag":"IN"
             
 	  },
             
          {
              "user_id":"10000119",
              "name":"john smith",
              "department":"development",
              "designation":"project manager",
              "date":"2022-10-27",
              "time":"19:00:00",	
              "punch_flag":"OUT"
	      
 	  }
       ]
}
```

### Step 2: When User pass `transaction_type` as `1`
* Users need to pass attendance_date parameter in the url these parameters will get users first punch & last punch logs.
* `attendance_date` parameters are mandatory. 

### Description:
* `page_number` parameter is mandatory in order to consume this api .i.e.  `1` or `2` or `3`....n to receive different records per page.

``` 
 Method: GET
 URL: <base_url>/transactions?transaction_type=1&attendance_date=1-01-2022&page_number=1
 Header: 
"token": <jwt token>,
"authentic-key": <client will get it from their email>,
"content-type": application/json

```

#### Response for Transaction log (when transaction_type == 1): 

``` json
{
"data":[
 	  {
             "user_id":"10000119",
             "name":"john smith",
             "department":"development",
             "designation":"project manager",
             "first_punch_date":"2022-10-27",
             "first_punch_time":"10:00:00",	
             "first_punch_device_id":"5d44",
             "last_punch_date":"2022-10-27",
             "last_punch_time":"19:00:00",	
             "last_punch_device_id":"5d55",
             "shift_start_time": "10:00:00 ",
             "shift_end_time": "19:00:00"
           }
       ]
}

```

### Step 3: When User pass `transaction_type` as `2`
* Users need to pass attendance_date parameter in the url these parameters will get users first in punch & last out punch logs.
* `attendance_date` parameters are mandatory. 


### Description:
* `page_number` parameter is mandatory in order to consume this api .i.e.  `1` or `2` or `3`....n to receive different records per page.


``` 

 Method: GET
 URL: <base_url>/transactions?transaction_type=2attendance_date=1-01-2022&page_number=1
 Header: 
"token": <jwt token>,
"authentic-key": <client will get it from their email>,
"content-type": application/json

```

#### Response for Transaction log (when transaction_type == 2): 

``` json
{
"data":[
	 {
             "user_id":"10000119",
             "name":"john smith",
             "department":"development",
             "designation":"project manager",
             "first_in_date":"2022-10-27",
             "first_in_time":"10:00:00",	
             "first_in_device_id":"5d44",
             "last_out_date":"2022-10-27",
             "last_out_time":"19:00:00",	
             "last_out_device_id":"5d55",
          }
	]
}


```

### Record set can be filtered through below parameters:

| Parameters | Type | Description |
| :--- | :--- | :--- |
| name  | string | Response will filtered with given name  |
| device_id | string | Response will filtered with given device_id  |
| user_id | string | Response will filtered with given user_id |
| date | string | Response will filtered with given date |
| department | integer | Response will filtered with given department  |
| designation  | string | Response will filtered with given designation  |


#### Filter for single Parameter:
```
1. Name            :  <base_url>/employee?name=rajat&page_number=1
2. UserID          :  <base_url>/employee?user_id=256&page_number=1
3. Device id       :  <base_url>/transactions?transaction_type=0&device_id=5d55&page_number=1
4. Date            :  <base_url>/employee?date=22-11-2020&page_number=1
5. Department      :  <base_url>/employee?department=development&page_number=1
6. Designation     :  <base_url>/employee?designation=manager&page_number=1

```


#### Filter for multiple Parameters:
```
<base_url>/transactions?transaction_type=0&department=development&designation=manager&page_number=1

```












