# Easily Secure your Spring boot app with Low code No Code

## Pre-requisite:



## Step 1: Sign-up/Login to IBM Cloud

1.1-  If you are an existing user please this link to Login: https://ibm.biz/appsecurity 

And if you are not, don't worry! We have got you covered! There are 3 steps to create your account on [IBM Cloud](<https://ibm.biz/appsecurity>): <br>

1.2-  Put your email and password. <br>

1.3-  You get a verification link with the registered email to verify your account. <br>

1.4-  Fill the personal information fields. <br>

** Please make sure you select the country you are in when asked at any step of the registration process.
  
<img width="688" alt="Screen Shot 2021-05-31 at 11 25 01 AM" src="https://user-images.githubusercontent.com/15332386/120156441-0769d980-c203-11eb-8cb3-29f4a8d5616a.png">


## Step 2: Create an instance of AppID service 

2.1-  In the search bar type "App ID", click the instance from search result, it will take you to a new window. Click create botton to start your App ID instance 

<img width="545" alt="1" src="https://user-images.githubusercontent.com/16270682/134592823-3f5f8eda-253c-422a-b27c-04b72ebc5704.PNG">

2.2-  Click create botton to start your App ID instance 

<img width="699" alt="2" src="https://user-images.githubusercontent.com/16270682/134592832-08d6f08f-b1ad-43dd-9d5f-438e24cbf924.PNG">

## Step 3: Create a Spring Boot application

#### Method 1:

Go to [Spring Initializr](<https://start.spring.io/>) page and generate a Maven project with default specifications. Click on generate to download the project. Unzip the file to a path of your choice.

<img width="650" alt="a" src="https://user-images.githubusercontent.com/16270682/134948639-dd9012c5-d06d-48e0-80fc-a9fd716326c8.PNG">

#### Method 2:

From workshop's [Git Repository](<https://github.com/IBMDeveloperMEA/Easily-Secure-your-Spring-boot-app-with-Low-code-No-Code>) click on code and download zip file. Unzip the file to a path of your choice.

<img width="651" alt="b" src="https://user-images.githubusercontent.com/16270682/134949825-6c2c8d98-23b7-46bb-a365-470b624a91d5.PNG">

## Step 4:  Add the App ID service configuration to the app

4.1 - From App ID Dashboard, select Applications in the left pane, click on add a new application. 

<img width="927" alt="c" src="https://user-images.githubusercontent.com/16270682/134955233-3e8f249a-c77a-4482-bb65-b08985b97609.PNG">

4.2 - Give your application a unique name and click on create.

<img width="623" alt="d" src="https://user-images.githubusercontent.com/16270682/134955253-bfb9bdf0-233a-4de8-962b-bdc16c06a7da.PNG">

4.3 - Click on “View credentials” to display the application credentials.

<img width="735" alt="e" src="https://user-images.githubusercontent.com/16270682/134955381-3bbed03d-5a0d-4d72-b550-a1d76e5cdce2.PNG">

4.4 - Edit the Spring Initializr project you created in step 3, and add the application credentials to configure App ID by going to path -> src/main/resources/application.yml file with the following property names:

          spring:
             security:
               oauth2:
                client:
                 registration:
                  appid:
                  clientId: <<clientId>>
                  clientSecret: <<clientSecret>>
                region: <<region>>
              tenantId: <<tenantId>>


  
## Step 5: Add the web redirect URL in App ID

Go to Manage Authentication > Authentication Settings and Add http://localhost:8080/login/oauth2/code/appid as your web redirect URL.

<img width="890" alt="f" src="https://user-images.githubusercontent.com/16270682/134957525-070053dc-e65a-4ca1-ba89-0ee30aeb077b.PNG">

## Workshop Resources

- Login/Sign Up for IBM Cloud: https://ibm.biz/appsecurity

- Workshop Replay: https://www.crowdcast.io/e/journey-to-low-code-no-code-app-security-2 

## Done with the workshop? Here are some things you can try further 

 

## Links to the next sessions in "Journey to Low Code/ No Code Application Security"

### 30th September- 7PM-9PM (GMT+5) 
- Securely Manage access to your Applications Sensitive Data on IBM Cloud
- https://www.crowdcast.io/e/journey-to-low-code-no-code-app-security-3
