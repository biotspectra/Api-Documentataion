
# Third Party API Integrations

We've written a quick step-by-step guide to walk you through how to login inside Biot user panel, make your first API call inside your system, and interpret the results.


## Introduction

This Api service will help users to synchronize their data with biot & fetch that data into their own system. For this we have created various Api services where user can purchase services as per their requierment and get acccess towards it. To learn more about this we have created step by step guide where user will get to learn how to integrate this Api with their system. To Know more About this, [Please Contact Our Team](https://biotworld.in/general-inquiry-biot/).


<p align="center">
 <img src="https://biotworld.in/wp-content/uploads/2022/02/biometric-machine.png"  height="400"/>
</p>


# How it works


## Login

### Step 1: Login inside Biot User panel: 
When a user sends a request with payload (mobile_number & password) an authentication token is generated which you will receive in your response as well as you will receive authentication-key via mail.  

**Note** you need to pass both `authentication token` & `authentication-key` inside your header while calling any api



### All Api Request & Response are shown below:

#### we have stored the url in a variable call `base_url`
```
base_url =  https://biotworld.in/api/external

```
### Authentication API:
 ```
  Method: POST
  URL: <base_url>/login
 ```


#### Request
 User will Send a request with Mobile Number & Password
```json
{
   "mobile_number": "9586722584",
   "password"     : "12345"     ,
}

```

#### Success Response
```json
{
   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2NjgzMzM3NjMsImlkIjoxODQxLCJpYXQiOjE2NjU3NDE3NjN9.s6MoReMqc_29hXErb1YuzhZdc5w0plyAyQIjB1k7i3A"
}


```

#### Error Response
```json
{
  "Invalid Credentials"
  
}
```

## 1A) User Synchronization Post Api

### Instructions:
* Users can send Maximum 10 records within a single API hit.
* List of field's which needs to be pass in the payload while calling Api



| Field | Datatype | Requirements | Description |
| :--- | :--- | :--- | :--- |
| Name | varchar(50) | Mandatory | User Name |
| User Id | int | Mandatory | Unique User ID |
| Date of joining | varchar(50) | Mandatory | User Name |
| Email Id | varchar(50) | Mandatory | Email validations |
| Mobile No  | varchar(13)  | Mandatory | Mobile number with country code. I.e +91 9898989898 || Mode | boolean() | Mandatory |  
| Date of Birth | dateTime() | Optional |  |
| Gender | tinyint(1)  | Optional | 1=Male,2=Female,0=other |
| Department | varchar(30 | Optional | Name of Department which belongs to the user. |
| Shift Start Time | dateTime() | Optional | Shift Start Time user need to ptovide shift start time as 24hrs format in HH:MM |
| Shift End Time | dateTime() | Optional |  Shift End Time user need to ptovide shift start time as 24hrs format in HH:MM |
| Designation | varchar(30)  | Optional | User designation |
| Off Day 1 | tinyint(1)  | Optional | Pass number of day. i.e.1=Monday,2=Tuesday,3=Wednesday,4=Thursday,5=Friday,6=Saturday,7=Sunday |
| Off Day 2 | tinyint(1)  | Optional | Pass number of day. i.e.1=Monday,2=Tuesday,3=Wednesday,4=Thursday,5=Friday,6=Saturday,7=Sunday |
| Off Day 2 Applies To Week | varchar(30) | Optional| Pass weekly off for which off day 2 is applied to be provided in comma separated value i.e. '2nd, 4th' ,if all     off day 2 is observed as weekly off then input is to be provided as '1st, 2nd, 3rd, 4th, 5th' |
| Off Day 2 Type | varchar(30) | Optional | 1=Full Day,2=Half Day apply for Week_of_day2.  |
| Role | int[1] | Optional | Pass 0=Normal User, 2=Sub Admin, 3=Supervisior |

#### Note: `Role` parameter & value with description:

| Parameter | Value | Description |
| :--- | :--- | :--- |
| role | 0 | Normal User Data |
| role | Other Than 0,2,3 | Provided role is not available |
| role | 2 | Sub Admin |
| role | 3 | Supervisior |

#### Description

1. This Parameter `role` is used to assign specific role to user, roles can be either normal user, sub admin or supervisior, Default value for parameter role is          considered as 0 .
2. Below is the detail explanation about roles:

  * Normal User can mark & regularize the attendance 
  * Sub Admin is as good as admin but with limited functionality i.e. no access to device management, Can enable or disable successful / Un-sucessfull punch          	   notification & can enable or disable successful / Un-sucessfull punch notification 
  * Supervisor can perform below activity i.e. Full functionality of User Role, Limited Access to “User” Page ( EDIT USER ONLY ), Limited Access to “User” Page ( EDIT     USER ONLY ), Can capture photo of user, Access to “Identity” & “Accessible Devices” tabs, Identity Tab : To Register Fingers & Face and assign BLE card,         	Accessible Devices : To Give access on Biot Devices, Can not assign Supervisor role to existing users
  



### Step 1: Pass Authentication Token & authentic-key in the headers:
Now user will need to pass that authentication token & authentication-key in headers to receive any response without that you will not be able to fetch data.

```
 Method: POST
 URL: <base_url>/employee
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
       "name":"john smith" ,
       "user_id":"10000119",
       "date_of_join":"23–09-2019",
       "date_of_birth":"23–09-2019",
       "mobile_number":"23092019",
       "email_id":"test@gmail.com",
       "gender":"male", 
       "designation":"project manager",
       "shift_start_time":"10:00",
       "shift_end_time":"19:00",
       "week_of" : {
           "offday1": "Sunday",
           "offday2": "Saturday",
           "offday2_applies_to_week": "2nd, 4th",
           "offday2_type": "halfday"
                   },
       "role":"2"
     },
    {
       "name":"john smith" ,
       "user_id":"10000119",
       "date_of_join":"23–09-2019",
       "date_of_birth":"23–09-2019",
       "mobile_number":"23092019",
       "email_id":"test@gmail.com",
       "gender":"male",
       "location":"ahmedabad",
       "designation":"project manager",
       "shift_start_time":"10:00",
       "shift_end_time":"19:00",
       "week_of" : {
           "offday1": "Sunday",
           "offday2": "Saturday",
           "offday2_applies_to_week": "2nd, 4th",
           "offday2_type": "halfday"
                   },
       "role":"0",
    },

  ]
}
```


#### Success Resposnse
success response would be "User data has been synced successfully".
``` json

{
  "status": "success",
  "status_code": 200,
  "message": "User has synced successfully." 
}

```

#### Response when while there is error in saving some data: 
In case any error occurs during sending the request, then the user will be able to see in response for which particular object the error occurred & which object has been synced successfully so next time the user only enters a particular object where there was error and again sends the request.


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
    "status": failed,
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

| Error code | Error message| 
| :--- | :--- |
| 1 | Please enter user name | 
| 2 | Please enter user id | 
| 3 | This user has been already added | 
| 4 | Please enter valid company id | 
| 5 | Please do not enter more than 50 characters | 
| 6 | Please enter mode | 
| 7 | Please enter date_of_joining | 
| 8 | Please enter email_id | 
| 9 | Please enter valid email_id (eg. test@test.com)  | 
| 10 | This email is already exists | 
| 11 | Please enter mobile number | 
| 12 | Please enter 10 digits only | 
| 13 | This mobile number is already exists | 
| 14 | Please enter valid date-format (dd-mm-yyyy) | 
| 15 | Please enter valid date-format (dd-mm-yyyy) | 



## 1B) User Synchronization Get Api:

* This Api is used get user records from biot account

### Discription:
* `page_number` parameter is mandatory in order to consume this api .i.e.  `1` or `2` or `3`....n to receive different records per page.


``` 
 Method: GET
 URL: <base_url>/employee?page_number=1
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
            "date_of_join":"23–09-2019",
            "date_of_birth":"23–09-2019",
            "mobile_number":"23092019",
            "email_id":"test@gmail.com",
            "gender":"male",
            "department":"development",
            "designation":"project manager",
            "from_time":"10:00:00",
            "to_time":"19:00:00",
            "week_of_day1":"7",
            "week_of_day2":"6",
            "no_of_weekday":"2,4",
            "day_type":"1", "[1 = full, 2 = half]"
            "is_admin":"1",
         },
        {
          "name":"john smith" ,
          "user_id":"10000119",
          "date_of_join":"23–09-2019",
          "date_of_birth":"23–09-2019",
          "mobile_number":"23092019",
          "email_id":"test@gmail.com",
          "gender":"male",
          "department":"development",
          "designation":"project manager",
          "from_time":"10:00:00",
          "to_time":"19:00:00",
          "week_of_day1":"7",
          "week_of_day2":"6",
          "no_of_weekday":"2,4",
          "day_type":"1", "[1 = full, 2 = half]"
          "is_admin":"0",
         },

      ]
}


```



### Record set can be filtered through below parameters:

| Parameters | Type | Description |
| :--- | :--- | :--- |
| name  | string | Response will filtered with given name  |
| user_id | string | Response will filtered with given userid  |
| date_of_joining | string | Response will filtered with given date_of_joining  |
| date_of_birth | string | Response will filtered with given date_of_birth |
| mobile_number | integer | Response will filtered with given mobile_number |
| email_id  | string | Response will filtered with given email_id  |
| department  | string | Response will filtered with given department  |
| designation | string | Response will filtered with given designation  |


#### Filter with Single Parameters:
```
1. Name            :  <base_url>/employee?name=rajat&page_number=1
2. UserID          :  <base_url>/employee?user_id=256&page_number=1
3. Date of Joining :  <base_url>/employee?date_of_join=22-11-2020&page_number=1
4. Date of birth   :  <base_url>/employee?date_of_birth=11-05-1994&page_number=1
5. Mobile number   :  <base_url>/employee?mobile_number=8469368525&page_number=1
6. Email id        :  <base_url>/employee?email_id=rajat@gmail.com&page_number=1 
7. Department      :  <base_url>/employee?department=development&page_number=1
8. Designation     :  <base_url>/employee?designation=manager&page_number=1

```

#### Filter with Multiple Parameters:
```
<base_url>/employee?department=IT&designation=manager&page_number=1

```



## 2) Transaction (punch detail) :
####  This Api is used to get Transaction details
####  In order to consume this api `transaction_type`, `transaction_from_date` , `transaction_to_date`, `first_punch_date`, `last_punch_date`, & 'first_in_punch_date`,'first_out_punch_date` are the parameters it will depend on transaction which paremeter need to be used



### Step 1: When User pass `transaction_type` as `0`
* User need to pass `transaction_from_date` & `transaction_to_date` parameters in the url i.e. if user wants transaction data between `1-01-2022` to `30-01-2022`. 
  This parameters will get users `All` transactions logs of the user between this particular date.

### Discription:
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
            "io flag":"i"
             
 	  },
             
          {
              "user_id":"10000119",
              "name":"john smith",
              "department":"development",
              "designation":"project manager",
              "date":"2022-10-27",
              "time":"19:00:00",	
              "io flag":"o"
	      
 	  }
       ]
}
```

### Step 2: When User pass `transaction_type` as `1`
* User need to pass `transaction_from_date` & `transaction_to_date` parameters in the url i.e. if user want first punch log & last punch log between this data `1-01-     2022` to `30-01-2022`. This parameters will get users first punch & last punch logs of the user between this particular date.

### Discription:
* `page_number` parameter is mandatory in order to consume this api .i.e.  `1` or `2` or `3`....n to receive different records per page.

``` 
 Method: GET
 URL: <base_url>/transactions?transaction_type=1&transaction_from_date=1-01-2022&transaction_to_date=30-01-2022&page_number=1
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
             "start_shift": "10:00:00 ",
             "end_shift": "19:00:00"
           }
       ]
}

```

### Step 2: When User pass `transaction_type` as `2`
* User need to pass `transaction_from_date` & `transaction_to_date` parameters in the url i.e. if user want first in punch log & last in punch log between this data     `1-01-2022` to `30-01-2022`. This parameters will get users In/Out punching logs of the user between this particular date.


### Discription:
* `page_number` parameter is mandatory in order to consume this api .i.e.  `1` or `2` or `3`....n to receive different records per page.


``` 

 Method: GET
 URL: <base_url>/transactions?transaction_type=2&transaction_from_date=1-01-2022&transaction_to_date=30-01-2022&page_number=1
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












