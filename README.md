
# Third Party API Integrations

We've written a quick step-by-step guide to walk you through how to integrate our Api's, make your first API call inside your system, and interpret the results.


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
If Mobile number or auth key is incorrect then user will get below response.
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
* Users can send Maximum `10` records within a single API hit.
* If more than `10` records are initiated at single hit it will throw error .i.e More than `10` records at once is initiated



* List of field's which needs to be pass in the payload while calling Api

| Field | Datatype | Requirements | Description |
| :--- | :--- | :--- | :--- |
| Name | varchar(50) | Mandatory | Enter Full Name|
| Date of joining | datetime | Mandatory | Enter the Date of Joining of the user|
| Email| varchar(50) | Mandatory | Enter Email Id for the user |
| Mobile No  | varchar(15)  | Mandatory | Mobile number with country code. I.e +91 9898989898 |
| Date of Birth | dateTime() | Optional | Enter Date of birth of the user |
| Gender | varchar(50) | Optional | Male,Female,other |
| Department | varchar(50) | Optional | Name of Department which belongs to the user.|
| Shift Start Time | dateTime() | Optional | Shift Start Time user need to provide shift start time as 24 hrs format in HH:MM |
| Shift End Time | dateTime() | Optional |  Shift End Time user need to provide shift End time as 24 hrs format in HH:MM |
| Designation | varchar(50)  | Optional | Enter Designation of the user|
| Off Day 1 | varchar(50)  | Optional | Pass number of day. i.e. Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday |
| Off Day 2 | varchar(50)  | Optional | Pass number of day. i.e. Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday |
| Off Day 2 Applies To Week | varchar(50) | Optional| Pass weekly off for which off day 2 is applied to be provided in comma separated value i.e. '2nd, 4th' ,if all off day 2 is observed as weekly off then input is to be provided as '1st, 2nd, 3rd, 4th, 5th' |
| Off Day 2 Type | varchar(50) | Optional | Full Day, Half Day (apply for Week_off_day2) |
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
       "user_id":"10000119214",
       "date_of_joining":"23–09-2020",
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
       "name":"Kriti Kumari",
       "user_id":"10000119215",
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
  "error_user_count": 5 "records"
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
| 1 | The {input field} is required .i.e. {name,mobile number,department,user id, email and so on}  | A mandatory field is empty | Ensure all mandatory fields and values are present.|
| 2 | Data Already Exist | An existing user record has been passed .i.e {email,mobile number} | Ensure that a unique user email & mobile number is passed |
| 3 | Please enter valid company id | Invalid Company Id has passed | Ensure that valid company Id is passed |
| 4 | Please do not enter more than 50 characters | User have passed more than 50 characters .i.e {name,email,department,designation,gender,offday1,offday2,offday2 applies to week,offday2 type} | Ensure that less than 50 characters are passed |
| 5 | Please enter valid email | Email format is not valid | Email should be {example@mail.com} |
| 6 | Maximum Limit Exceeded for Mobile Number| Mobile number limit exceeded above 15 | Ensure that 15 digits are passed  |
| 7 | Please enter valid date-format  | {date of joining / date of birth } format is not valid | Date format should be (dd-mm-yyyy) |
| 8 | Please enter valid start/end shift time | {shift start time / shift end time } format is not valid | Start/end shift should be in 24 hrs format in HH:MM|
| 9 | Provided user_role is not available | Only 1 & 3 user role value is allowed | Ensure that only 1(normal user) & 3(supervisior) role is passed |
| 10 | User Limit Exceeded| Users can send Maximum 10 records within a single API hit | Ensure that you don't send more than 10 records |




## 1B) User Synchronization Get Api:

### Instructions 
*  Default user will able to retrieves `10` records per page.
* `page_number` parameter is mandatory in order to consume this api .i.e. `1` or `2` or `3`....n to receive different records per page.
* `number_of_records` parameter is optional .i.e {number_of_records=`100`} depends on how many records user wants, minimum records is `10` & maximum is `100`.


### Description:
* The following endpoint retrieves the details of all the user data from biot account.

#### Default 10 records will retrieve
``` 
 Method: GET
 URL: <base_url>/employee?page_number=1
 Header: 
"token": <jwt token>,
"authentic-key": <client will get it from their email>,
"content-type": application/json

```
#### When Number of records is 100 (Maximum)
``` 
 Method: GET
 URL: <base_url>/employee?page_number=1&number_of_records=100
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
            "email":"john@gmail.com",
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
            "user_role":"3",
         },
        {
          "name":"riya joshi" ,
          "user_id":"10000120",
          "date_of_joining":"23–09-2021",
          "date_of_birth":"23–09-2000",
          "mobile_number":"2309201998",
          "email":"riya@gmail.com",
          "gender":"female",
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
| No Data Available | The {User Id,Date of joining,Date of birth,Mobile number,Email,Department,Designation} does not belong to the requestor, or it doesn't exist. | Ensure that valid endpoint is passed and belongs to the requestor.|



## 1c) User Synchronization Edit Api:
* This Api is used to Edit User records from Biot account.

### Description:
The following endpoint edits the user details such as the name, email, date_of_joining and so on. If user passes same `User Id` with different records then the record for the particular user will be updated.


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
       "name":"johns smith",
       "user_id":"10000119214",
       "date_of_joining":"23–09-2020",
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
       "name":"Kiran Kumari",
       "user_id":"10000119215",
       "date_of_joining":"23–09-2019",
       "date_of_birth":"23–09-1998",
       "mobile_number":"2309201922",
       "email":"Kiran@gmail.com",
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
```json
{
   {
  "status": "success",
  "status_code": 200,
  "message": "User updated successfully." 
   }
}

```


| Error code | Error message| Cause | Solution
| :--- | :--- | :--- | :--- |
| 2 | Data Already Exist | An existing user record has been passed .i.e {email,mobile number} | Ensure that a unique user email & mobile number is passed |







## 2) Transaction (punch detail) :
The following endpoint retrieves the details of all the transaction log from biot account.

### Instructions 
*  Default user will able to retrieves `100` records per page.
* `page_number` parameter is mandatory in order to consume this api .i.e. `1` or `2` or `3`....n to receive different records per page.
* `number_of_records` parameter is optional .i.e {number_of_records=`1000`} depends on how many records user wants, minimum records is `100` & maximum is `1000`.
*  In order to consume this transaction's api transaction_type, transaction_from_date, transaction_to_date, attendance_from_date,attendance_to_date & transaction_id      are the parameters that will be used throughout this transaction api.


### Description: 
* Transaction Type : When transaction_type is passed as `0` then all the transaction logs will be retrieve, when passed as `1` then First punch & Last Punch user wise   records will be retrieved, when passed as `2` then First In Punch & Last Out Punch user wise records will be retrieved & when passed as `3` then latest transaction     log will retrieved based on requested transaction_id .
* Transaction Id : Transaction Id is unique number which is auto generated for each and every transaction logs.

### Example 1 ###
* All Transaction Logs:

| Transaction ID | User Id | Transaction Date | Transaction Time | In Out Flag |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 10001 | 2022-01-01 |10:00:00 | IN |
| 2 | 10001 | 2022-01-01 |11:00:00 | OUT |
| 3 | 10002 | 2022-01-01 |11:00:00 | IN |
| 4 | 10001 | 2022-01-01 |12:00:00 | IN |
| 5 | 10002 | 2022-01-01 |13:00:00 | OUT |
| 6 | 10002 | 2022-01-01 |14:00:00 | IN |
| 7 | 10001 | 2022-01-01 |15:00:00 | OUT |
| 8 | 10001 | 2022-01-01 |16:00:00 | IN |
| 9 | 10002 | 2022-01-01 |17:00:00 | OUT |
| 10 | 10001 | 2022-01-01 |18:00:00 | OUT |
| 11 | 10001 | 2022-01-01 |19:00:00 | IN |
| 12 | 10003 | 2022-01-01 |19:00:00 | IN |

### Example 2 ###
* First Punch & Last Punch Logs (Records based on Example 1):

|  User Id | First Punch Date | First Punch Time |Last Punch Date | Last Punch Time |
| :--- | :--- | :--- | :--- |:--- |
| 10001 | 2022-01-01 | 10:00:00 | 2022-01-01 | 19:00:00 |
| 10002 | 2022-01-01 | 11:00:00 | 2022-01-01 | 17:00:00 |
| 10003 | 2022-01-01 | 19:00:00 | 2022-01-01 | 19:00:00 |


### Example 3 ###
* First IN & Last OUT Punch Logs (Records based on Example 1):

|  User Id | First IN Date | First IN Time |Last OUT Date | Last OUT Time |
| :--- | :--- | :--- | :--- |:--- |
| 10001 | 2022-01-01 | 10:00:00 | 2022-01-01 | 18:00:00 |
| 10002 | 2022-01-01 | 11:00:00 | 2022-01-01 | 17:00:00 |
| 10003 | 2022-01-01 | 19:00:00 | NULL | NUll |


### Example 4 ###
* Latest Transaction Logs (Records based on Example 1):
* If we pass Transaction Id as `6`

| Transaction ID | User Id | Transaction Date | Transaction Time | In Out Flag |
| :--- | :--- | :--- | :--- | :--- |
| 7 | 10001 | 2022-01-01 |15:00:00 | OUT |
| 8 | 10001 | 2022-01-01 |16:00:00 | IN |
| 9 | 10002 | 2022-01-01 |17:00:00 | OUT |
| 10 | 10001 | 2022-01-01 |18:00:00 | OUT |
| 11 | 10001 | 2022-01-01 |19:00:00 | IN |
| 12 | 10003 | 2022-01-01 |19:00:00 | IN |


### Step 1: When User pass `transaction_type` as `0`
*  When user passes `transaction_type` as `0` they need to pass other two parameters `transaction_from_date` & `transaction_to_date` i.e. if user wants transaction        records between `1-01-2022` to `30-01-2022`. This parameters will get users `All` transactions logs of the user between this particular date.


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
	    "transaction_id":"1",
            "name":"john smith",
            "department":"development",
            "designation":"project manager",
            "date":"2022-10-27",
            "time":"10:00:00",	
            "in_out_flag":"IN"
             
 	  },
             
          {
              "user_id":"10000119",
	      "transaction_id":"2",
              "name":"John Doe",
              "department":"development",
              "designation":"project manager",
              "date":"2022-10-27",
              "time":"19:00:00",	
              "in_out_flag":"OUT"
	      
 	  },
	  
	 {
             "user_id":"10000119",
	     "transaction_id":"3",
             "name":"Henry Klaseen",
             "department":"development",
             "designation":"project manager",
             "date":"2022-10-27",
             "time":"19:00:00",	
             "in_out_flag":"OUT"
	      
 	 }
       ]
}
```

### Step 2: When User pass `transaction_type` as `1`

* When user passes `transaction_type` as `1` they need to pass other two parameters ` attendance_from_date` & `attendance_to_date` i.e. if user wants `first` & `last`   punch records between `1-01-2022` to `30-01-2022`. This parameters will get users all first & last punch user wise as well as date wise between this particular date.



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
             "user_id":"10001",
             "name":"john smith",
             "department":"development",
             "designation":"project manager",
             "first_punch_date":"2022-01-01",
             "first_punch_time":"10:00:00",	
             "first_punch_device_id":"5d44",
             "last_punch_date":"2022-01-01",
             "last_punch_time":"19:00:00",	
             "last_punch_device_id":"5d55",
             "shift_start_time": "10:00:00 ",
             "shift_end_time": "19:00:00"
           }
       ]
}

```

### Step 3: When User pass `transaction_type` as `2`
* When user passes `transaction_type` as `2` they need to pass other two parameters ` attendance_from_date` & `attendance_to_date` i.e. if user wants `first in` & `last out` punch records between `1-01-2022` to `30-01-2022`. This parameters will get users all `first in` & `last out` punch user wise as well as date wise between     this particular date.



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
             "user_id":"10001",
             "name":"john smith",
             "department":"development",
             "designation":"project manager",
             "first_in_date":"2022-01-01",
             "first_in_time":"10:00:00",	
             "first_in_device_id":"5d44",
             "last_out_date":"2022-01-01",
             "last_out_time":"18:00:00",	
             "last_out_device_id":"5d55",
          }
	]
}


```

### Step 4: When User pass `transaction_type` as `3`
* When user passes `transaction_type` as `3` they need to pass one parameter `transaction_id` i.e. user wants all latest records after requested Transaction Id like     shown in `Example 4` , user will receive all Transaction logs after Transaction Id `3`.



``` 

 Method: GET
 URL: <base_url>/transactions?transaction_type=3&transaction_id=6&page_number=1
 Header: 
"token": <jwt token>,
"authentic-key": <client will get it from their email>,
"content-type": application/json

```

#### Response for Transaction log (when transaction_type == 3 & transaction_id == 6): 

``` json
{
"data":[
	 {
            "user_id":"10001",
	    "transaction_id":"7",
            "name":"john smith",
            "department":"development",
            "designation":"project manager",
            "date":"2022-01-01",
            "time":"15:00:00",	
            "in_out_flag":"OUT"
             
 	  },
             
          {
              "user_id":"10001",
	      "transaction_id":"8",
              "name":"John Doe",
              "department":"development",
              "designation":"project manager",
              "date":"2022-01-01",
              "time":"16:00:00",	
              "in_out_flag":"IN"
	      
 	  },
	  
	 {
             "user_id":"10002",
	     "transaction_id":"9",
             "name":"Henry Klaseen",
             "department":"development",
             "designation":"project manager",
             "date":"2022-01-01",
             "time":"17:00:00",	
             "in_out_flag":"OUT"
	      
 	 },
	  {
            "user_id":"10001",
	    "transaction_id":"10",
            "name":"john smith",
            "department":"development",
            "designation":"project manager",
            "date":"2022-01-01",
            "time":"18:00:00",	
            "in_out_flag":"OUT"
             
 	  },
             
          {
              "user_id":"10001",
	      "transaction_id":"11",
              "name":"John Doe",
              "department":"development",
              "designation":"project manager",
              "date":"2022-01-01",
              "time":"19:00:00",	
              "in_out_flag":"IN"
	      
 	  },
	  
	 {
             "user_id":"10003",
	     "transaction_id":"12",
             "name":"Henry Klaseen",
             "department":"development",
             "designation":"project manager",
             "date":"2022-01-01",
             "time":"19:00:00",	
             "in_out_flag":"IN"
	      
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
#### Error Response & their failure cases are shown below:
| Error message| Cause | Solution
| :--- | :--- | :--- |
| No Data Available | The {User Id,Device id ,Date ,Department,Designation} does not belong to the requestor, or it doesn't exist. | Ensure that valid endpoint is passed and belongs to the requestor.|		    
			    
			










