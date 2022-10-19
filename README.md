
## Making your first API call

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
  URL: {{base_url}}/login
 ```


#### Request
 User will Send a request with Mobile Number & Password
```json
{
   "mobile_number": "9586722584",
   "password"     : "12345"     ,
}

```

#### Response
```json
{
   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2NjgzMzM3NjMsImlkIjoxODQxLCJpYXQiOjE2NjU3NDE3NjN9.s6MoReMqc_29hXErb1YuzhZdc5w0plyAyQIjB1k7i3A"
}


```

## 1A) User Synchronization Post Api

### Instructions:
####  Users can send Maximum 10 records within the particular api hit.

#### List of field's you need to pass in the payload while calling Api are listed below:

| Field | Datatype | Requirements | Description |
| :--- | :--- | :--- | :--- |
| Name | varchar(50) | Mandatory | User Name |
| user_id | int | Mandatory | Unique User ID |
| Date of joining | varchar(50) | Mandatory | User Name |
| emailId | varchar(50) | Mandatory | Email validations |
| Mobile no  | varchar(13)  | Mandatory | Mobile number with country code. I.e +91 9898989898 |
| mode | boolean() | Mandatory |  |
| Date of birth | dateTime() | Optional |  |
| gender | tinyint(1)  | Optional | 1=Male,2=Female,0=other |
| department | varchar(30 | Optional | Name of Department which belongs to the user. |
| from_time | dateTime() | Optional | Shift Start Time |
| to_time | dateTime() | Optional | Shift End Time |
| designation | varchar(30)  | Optional | User designation |
| week_of_day1 | tinyint(1)  | Optional | Pass number of day. i.e.1=Monday,2=Tuesday,3=Wednesday,4=Thursday,5=Friday,6=Saturday,7=Sunday |
| week_of_day2 | tinyint(1)  | Optional | Pass number of day. i.e.1=Monday,2=Tuesday,3=Wednesday,4=Thursday,5=Friday,6=Saturday,7=Sunday |
| no_of_weekday | int | Optional| Pass number for which week of the month to apply week_of_day2. I.e. if pass 2,4 it’s consider as week_of_Day2 for second and forth week of the month.   |
| day_type | int [1] | Optional | 1=Full Day,2=Half Day apply for Week_of_day2.  |
| is_admin | int[1] | Optional | Pass 0=Normal User, 2=Sub Admin,3=Location Admin |

#### Note: `is_admin` parameter & value with description:

| Parameter | Value | Description |
| :--- | :--- | :--- |
| admin_id | 0 | Normal User Data |
| admin_id | 1 | Error |
| admin_id | 2 | Error |
| admin_id | 3 | User need to pass another Parameter name `location_access` with their values in an array like these `"location_accesss":["ahmedabad","rajkot","mumbai"]` |

#### Description
1. There is Parameter name `admin_id` which user can pass if they want data to be filter.
2. If a user does not pass `is_admin` parameter in the payload then they will receivce all the data.
2. If a user pass  `is_admin` parameter as `1` then they will get an error.
3. If a user pass  `is_admin` parameter as `0` then they will receive normal user data.
4. If a user pass `is_admin` parameter as `3` then they need to pass another parameter `location_access` with field as `xyz` then they can post & receive data as per      the location.


### Step 1: Pass Authentication Token & authentic-key in the headers:
Now user will need to pass that authentication token & authentication-key in headers to receive any response without that you will not be able to fetch data.

```
 Method: POST
 URL: {{base_url}}/employee
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
       "location":"ahmedabad",
       "designation":"project manager",
       "from_time":"10:00:00",
       "to_time":"19:00:00",
       "week_of_day1":"7",
       "week_of_day2":"6",
       "no_of_weekday":"2,4",
       "day_type":"1","[1 = full, 2 = half]"
       "Is_admin":"3",
       "location_accesss":["ahmedabad","rajkot","mumbai"],
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
       "from_time":"10:00:00",
       "to_time":"19:00:00",
       "week_of_day1":"7",
       "week_of_day2":"6",
       "no_of_weekday":"2,4",
       "day_type":"1","[1 = full, 2 = half]"
       "is_admin":"0",
    },

  ]
}
```


#### Success Resposnse
If the user has entered all the data correctly then they will receive a success response that your data has been synced successfully.
``` json

{
  "success": true,
  "status_code": 200,
  "error_code": 0,
  "message": "User has synced successfully." 
}

```

#### Response when some of data saved successfully & there was error while saving other: 
In case any error occurs during sending the request, then the user will be able to see in response for which particular object the error occurred & which object has been successfully saved so next time the user only enters a particular object where there was error and again sends the request.


``` json
{
  "Total_count":10,
  "success_count":5,
  "error_count": 5,
  "description": "There was an error while adding Emp_id please re enter Emp_id for the last 5 entry"
}


```

``` json
[
   {
     "success":true,
     "status_code":200,
   },

   {
    "success": false,
    "status_code": 400,
    "error_code": 1
   },

  {
    "success": false,
    "status_code": 400,
    "error_code": 2
  },
  
  {
    "success": false,
    "status_code": 400,
    "error_code": 3
  },
  
  {
    "success": false,
    "status_code": 400,
    "error_code": 4
  },
  
  {
   "success": false,
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
| 6 |`Please enter mode | 
| 7 | Please enter date_of_joining | 
| 8 | Please enter email_id | 
| 9 | Please enter valid email_id (eg. test@test.com)  | 
| 10 | This email is already exists | 
| 11 | Please enter mobile number | 
| 12 | Please enter 10 digits only | 
| 13 | This mobile number is already exists | 
| 14 | Please enter valid date-format (dd-mm-yyyy) | 
| 15 | Please enter valid date-format (dd-mm-yyyy) | 

### Events:
1. It will store the data in the biot database employee master.
2. It will add a single/multiple user on a single request.
3. It will not store access related data like finger,  BLE card, face. It will only store from the Biot App.


## 1B) User Synchronization Get Api

``` 
 Method: GET
 URL: {{base_url}}/employee
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

### Events:
1. It will provide registered user data as per biot database.
2. It will provide data as per passed filter parameter.

### Filteration will perform by the fields that are listed below:

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


#### For Single Filteration:
```
1. Name            :  {{base_url}}/employee?name=rajat
2. UserID          :  {{base_url}}/employee?user_id=256
3. Date of Joining :  {{base_url}}/employee?date_of_join=22-11-2020
4. Date of birth   :  {{base_url}}/employee?date_of_birth=11-05-1994
5. Mobile number   :  {{base_url}}/employee?mobile_number=8469368525 
6. Email id        :  {{base_url}}/employee?email_id=rajat@gmail.com 
7. Department      :  {{base_url}}/employee?department=development
8. Designation     :  {{base_url}}/employee?designation=manager

```

#### For Multiple Filteration:
```
{{base_url}}/employee?department=IT&designation=manager 

```



## 2) Transaction (punch detail) :
#### To get Transaction details user need to pass these parameter in the url `transaction_type`, `from_date` & `to_date` with their respective values as shown below

### Step 1: When User pass `transaction_type` as `0`
User also need to pass `from_date` & `to_date` parameters in the url with their respective values i.e. if user wants transaction data between `1-01-2022` to `30-01-2022`.This parameters will help users to get `all` transactions log of the employees between this particular date.


``` 

 Method: GET
 URL: {{base_url}}/transactions?transaction_type=0?from_date=1-01-2022?to_date=30-01-2022
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
User also need to pass `from_date` & `to_date` parameters in the url with their respective values i.e. if user wants transaction data between `1-01-2022` to `30-01-2022`. This parameters will help users to get `First Punch` & `Last punch` transactions log of the employees between this particular date.

``` 
 Method: GET
 URL: {{{base_url}}/transactions?transaction_type=1?from_date=1-01-2022?to_date=30-01-2022
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
User also need to pass `from_date` & `to_date` parameters in the url with their respective values i.e. if user wants transaction data between `1-01-2022` to `30-01-2022`. This parameters will help users to get `First In Punch` & `Last out punch` transactions log of the employees between this particular date.


``` 

 Method: GET
 URL: {{{base_url}}/transactions?transaction_type=2?from_date=1-01-2022?to_date=30-01-2022
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

### Events:
1. It will provide data as per data-flag. Single api provides three types of different responses which depend on data-flag. Data flags like 0 = transaction log , 1 =  first and last punch, 2 = First_In and Last_Out punch.
2. It will provide data as per passed filter parameter.


### Filteration will perform by the fields that are listed below:

| Parameters | Type | Description |
| :--- | :--- | :--- |
| name  | string | Response will filtered with given name  |
| device_id | string | Response will filtered with given device_id  |
| user_id | string | Response will filtered with given user_id |
| date | string | Response will filtered with given date |
| department | integer | Response will filtered with given department  |
| designation  | string | Response will filtered with given designation  |


#### For Single Filteration:
```
1. Name            :  {{base_url}}/employee?name=rajat
2. UserID          :  {{base_url}}/employee?user_id=256
3. Device id       :  {{base_url}}/transactions?transaction_type=0&device_id=5d55
4. Date            :  {{base_url}}/employee?date=22-11-2020 
5. Department      :  {{base_url}}/employee?department=development
6. Designation     :  {{base_url}}/employee?designation=manager

```


#### For Multiple Filteration:
```
{{base_url}}/transactions?transaction_type=0&department=development&designation=manager 

```


### Additional implementation in biot :
1. Biot admin panel client management menu export all records at a time as of now it will only export on bases on pagination limit.
2. Add a new report called biot subscription for biot admin panel which will provide data about client subscription plan details.
3. Add functionality to change owner name from biot admin panel.










