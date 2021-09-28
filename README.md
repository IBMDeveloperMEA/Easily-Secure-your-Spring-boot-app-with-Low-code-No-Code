# Easily Secure your Spring boot app with Low code No Code

## Pre-requisite:

--Maven installed in your PC (https://maven.apache.org/)
--Java installed in your PC (https://adoptopenjdk.net/releases.html)

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

5.1 - Go to Manage Authentication > Authentication Settings and Add http://localhost:8080/login/oauth2/code/appid as your web redirect URL.

<img width="890" alt="f" src="https://user-images.githubusercontent.com/16270682/134957525-070053dc-e65a-4ca1-ba89-0ee30aeb077b.PNG">

## Step 6:Add necessary dependencies to pom.xml

6.1 - From your project go to your pom.xml file and add the following dependencies 


<dependencies>
    <!-- App ID Starter-->
    <dependency>
        <groupId>com.ibm.cloud.appid</groupId>
        <artifactId>appid-spring-boot-starter</artifactId>
        <version>0.0.5</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- adding jquery as a dependency, this will be used by the front-end UI -->
    <dependency>
        <groupId>org.webjars</groupId>
        <artifactId>jquery</artifactId>
        <version>2.1.1</version>
    </dependency>
    <!-- webjars-locator-core used by Spring to locate static assets in webjars without needing to know the exact versions -->
    <dependency>
        <groupId>org.webjars</groupId>
        <artifactId>webjars-locator-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.webjars</groupId>
        <artifactId>js-cookie</artifactId>
        <version>2.1.0</version>
    </dependency>
    <dependency>
        <groupId>org.webjars</groupId>
        <artifactId>bootstrap</artifactId>
        <version>3.2.0</version>
    </dependency>
</dependencies>


## Step 7: configure security

Create a SecurityConfiguration.java class, and add the following code:

@Configuration
@EnableWebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
        .authorizeRequests()
        .antMatchers("/login**", "/user", "/userInfo").authenticated()
        .and()
        .oauth2Login();
    }
}


## Step 8: Add the protected REST endpoints

8.1 - To add REST endpoints, create a UserController.java class, and add the following code:

@RestController
public class UserController {

    @RequestMapping("/user")
    public Principal user(@AuthenticationPrincipal Principal principal) {
        // Principal holds the logged in user information.
        // Spring automatically populates this principal object after login.
        return principal;
    }

    @RequestMapping("/userInfo")
    public String userInfo(@AuthenticationPrincipal Principal principal) {
        return String.valueOf(principal);
    }
}


## Step 9:  Add a HTML page to call the protected REST endpoints

9.1 - Create index.html file in /appid-spring-boot-example/src/main/resources/static, and add the following code, which shows the logged-in user information:

            <!DOCTYPE html>
              <html lang="en">
           <head>
                <meta charset="UTF-8">
              <meta name="viewport" content="width=device-width, initial-scale=1.0">
              <title>Spring Boot App ID Sample</title>
              <link type="text/css" href="css/style.css" rel="stylesheet" />
              <script type="text/javascript" src="/webjars/jquery/jquery.min.js"></script>
             <script type="text/javascript" src="/webjars/js-cookie/js.cookie.js"></script>
               <script type="text/javascript">
                 $.ajaxSetup({
                beforeSend : function(xhr, settings) {
                    if (settings.type == 'POST' || settings.type == 'PUT' || settings.type == 'DELETE' || settings.type == 'GET') {
                        if (!(/^http:.*/.test(settings.url) || /^https:.*/.test(settings.url))) {
                            // Only send the token to relative URLs i.e. locally.
                            xhr.setRequestHeader("X-XSRF-TOKEN", Cookies.get('XSRF-TOKEN'));
                        }
                        xhr.setRequestHeader("X-XSRF-TOKEN", Cookies.get('XSRF-TOKEN'));
                    }
                }
            });
            $.get("/user", function(data) {
                if (data.principal != null) {
                    $("#user").html(data.principal.attributes.name);
                    $("#userSub").html(data.principal.attributes.sub);
                    $("#userEmail").html(data.principal.attributes.email);
                    $("#provider").html(data.principal.attributes.identities[0].provider);
                    $(".unauthenticated").hide();
                    $(".authenticated").show();
                } else {
                    $(".unauthenticated").show();
                    $(".authenticated").hide();
                }
            }).fail(function() {
                $(".unauthenticated").show();
                $(".authenticated").hide();
            });

            <!-- In this case, we will call GET /userInfo, and this will give us back a string with userinfo details from Principal user -->
            $.get("/userInfo", function(data) {
                if (data.includes("Principal")) {
                    $("#userInfoString").html(data);
                    $(".unauthenticated").hide();
                    $(".authenticated").show();
                } else {
                    $(".unauthenticated").show();
                    $(".authenticated").hide();
                }
            }).fail(function() {
                $(".unauthenticated").show();
                $(".authenticated").hide();
            });
        </script>
    </head>
    <body>
        <div class="container unauthenticated" style="text-align: center;">
            <a href="/login">Login</a>
        </div>
        <div class="container authenticated" style="text-align: center;" >
            <strong>Logged in as: <span id="user"></span></strong>
            <br>
            <br>
            <strong>Sub: </strong><span id="userSub"></span>
            <br>
            <strong>Email: </strong><span id="userEmail"></span>
            <br>
            <strong>Provider: </strong><span id="provider"></span>
            <br>
            <br>
            <strong>User Profile Information: </strong>
            <br>
            <span id="userInfoString"></span>
            <br>
            <br>
       </div>
    </body>
</html>

Your Spring Boot project should now look like:

# PLEASE ADD IMAGE HERE KARIM 🤭

## Step 10: Build and run the app

10.1 - Build and run your app using the following commands:

    mvn clean
    mvn package spring-boot:run

10.2 After the application is running, open a browser, and go to http://localhost:8080. It will take you to a login screen.

## Workshop Resources

- Login/Sign Up for IBM Cloud: https://ibm.biz/appsecurity

- Workshop Replay: https://www.crowdcast.io/e/journey-to-low-code-no-code-app-security-2 

## Done with the workshop? Here are some things you can try further 

 

## Links to the next sessions in "Journey to Low Code/ No Code Application Security"

### 30th September- 7PM-9PM (GMT+5) 
- Securely Manage access to your Applications Sensitive Data on IBM Cloud
- https://www.crowdcast.io/e/journey-to-low-code-no-code-app-security-3
