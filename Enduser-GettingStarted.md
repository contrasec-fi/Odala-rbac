# 1. Prerequisites

Basic understanding of Oauth and REST. We suggest to view this [video](https://youtu.be/kbV7GOsELoY) as a demo how to access data before proceeding. [Postman](https://www.postman.com/) is a handy tool, but [Curl](https://curl.se/) works too. Please check the [Chapter 5](#5.-Postman-Collection) - Postman Collection of this page, we have created a ready Postman collection you can use to get an easty start. 

# 2. Getting a keyrock account

Contrasec has created a demo environment where to test the RBAC solution. Here is a step by step how to get a Keyrock Account:

1. Navigate to accounts: https://accounts.odala-rbac.cinfra.fi/sign_up/ sign up for a new account. The email sending has been disabled for security.
2. Send an email to ilari.mikkonen@contrasec.fi and he will activate your account. This may take a working day or two.

# 3. How to query data

1. Once you have received email confirmation that your email has been activated from ilari.mikkonen@contrasec.fi, you can login via https://accounts.odala-rbac.cinfra.fi/ . Here is how successful login should look like:

   ![](pictures/eug1.JPG)

2. Access token is needed to query data. You need to get the following details in order to get the access token: `clientid`,`client secret`,`username` (=email) of your Keyrock account and `password` of your Keyrock account. `clientid` is `4f10e33d-697c-4c5a-95d9-4c6b60a0179f` for Contrasec test application, and you can get those from Keyrock also from the Application details. Clict the "Orion RBAC" application and expand the "Oauth2 Credentials" selector:

   ![](pictures/eug2.jpg) 

4. Send the following request via Postman. Copy the `clientid` and `client secret` to the "username" and "password" fields of Postman:
   ![](pictures/eug3.1.JPG)

    Insert the username and password details. Set `grant_type` as "password":
      
    ![](pictures/eug4.1.JPG)

    `access_token` is in the response, copy that for later use.


    ![](pictures/eug5.JPG)

4. Create the following request with Postman:

    ![](pictures/eug6.1.JPG)

    response that you should get is:

    ![](pictures/eug7.JPG)

    You are done! If you run into trouble, please create an issue to this repo or send email to ilari.mikkonen@contrasec.fi

# 4. How to push data

At this time we do not allow pushing of data. This is strictly to minimize maintenance effort from our side.

# 5. Postman Collection

Is [available](/postman-collections/ODALA-RBAC.postman_collection.json) for download. With the collection you can easily test the system. 

# 6. Common problems and trouble shoot

Nothing yet! When something is wrong, please create an issue.  

# 7. Curl examples

```
curl --location --request POST 'https://accounts.odala-rbac.cinfra.fi/oauth2/token' \
--header 'Authorization: Basic NG....NzU4OWZiNWQ2Ng==' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'username=email@asdsadasc.fi' \
--data-urlencode 'password=pass'
```

```
curl --location --request GET 'https://orion.odala-rbac.cinfra.fi/v2/entities/' \
--header 'Authorization: Bearer 91ff..b732ac47'
```

# 8. References

- [Keyrock read the docs](https://fiware-idm.readthedocs.io/en/latest/)
- [Orion context broker](https://fiware-orion.readthedocs.io/en/master/)
- [Bearer tokens](https://stackoverflow.com/questions/25838183/what-is-the-oauth-2-0-bearer-token-exactly)
- [Role Based Access Control](https://en.wikipedia.org/wiki/Role-based_access_control)
