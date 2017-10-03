# CICO
Cisco Infrastructure Chat Ops

This bot leverages the Spark Bot framework found [here](https://github.com/imapex/ciscosparkbot).

# Contents
[Prerequisites](#prerequisites)
[Installation](#installation)
    [ngrok](#ngrok)
[Meraki Integration](#meraki)
    [Enable API Access](#meraki-api-access)
    [Get API Token](#meraki-api-token)
    [Get Organization ID](#meraki-org-id)
[Spark Call Integration](#sparkcall)
    [Verify Admin Rights](#sparkcall-admin)
    [Get API Token](#sparkcall-token)
[Usage](#usage)


# Prerequisites<a name="prerequisites"/>

If you don't already have a Cisco Spark account, go ahead and register for one.  They are free.
You'll need to start by adding your bot to the Cisco Spark website.

[https://developer.ciscospark.com/add-app.html](https://developer.ciscospark.com/add-app.html)

![add-app](images/newapp.png)

1. Click create bot

![add-bot](images/newbot.png)

2. Fill out all the details about your bot, including a publicly hosted avatar image.  A sample avatar is available at [http://cs.co/devnetlogosq](http://cs.co/devnetlogosq).

![enter-details](images/enterdetails.png)

3. Click "Add Bot", make sure to copy your access token, you will need this in a second

![copy-token](images/copytoken.png)

# Installation<a name="installation"/>

Create a virtualenv and install the module

```
virtualenv venv
source venv/bin/activate
pip install ciscosparkbot
git clone https://github.com/joshand/CICO.git
```

## ngrok - Skip this step if you already have an Internet reachable web-server<a name="ngrok"/>

ngrok will make easy for you to develop your code with a live bot.

If you are running a Mac with Homebrew, you can easily install via "brew cask install ngrok". Additional installation instructions can be found here: https://ngrok.com/download

After you've installed ngrok, in another window start the service


`ngrok http 5000`


You should see a screen that looks like this:

```
ngrok by @inconshreveable                                                                                                                                 (Ctrl+C to quit)

Session Status                online
Version                       2.2.4
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://this.is.the.url.you.need -> localhost:5000
Forwarding                    **https://this.is.the.url.you.need** -> localhost:5000

Connections                   ttl     opn     rt1     rt5     p50     p90
                              2       0       0.00    0.00    0.77    1.16

HTTP Requests
-------------

POST /                         200 OK
```

# Meraki Integration<a name="meraki"/>

---

## Enable your Meraki organization for API Access<a name="meraki-api-access"/>

1. Log in to the Meraki Dashboard. Choose your organization if prompted to do so.

2. On the left Navigation bar, go to Organization and select Settings.

3. Scroll down to the Dashboard API Access section, and turn on API Access.

![meraki-enable-api-access](images/meraki_enable_api_access.png)

## Obtain your Meraki API Token<a name="meraki-api-token"/>

1. Log in to the Meraki Dashboard. Choose your organization if prompted to do so.

2. Under your username, select My profile

![meraki-my-profile](images/meraki_profile.png)

3. Scroll down to the API access section, and copy your API key. You'll need it to get your Organization ID and to set your environment variables.

![meraki-my-key](images/meraki_key.png)

## Obtain your Meraki Organization ID<a name="meraki-org-id"/>

You can use Postman to run this GET:

![postman-meraki-org](images/postman_org.png)

Or you can do it from the command line, with a curl command like this:

`curl --header "X-Cisco-Meraki-API-Key: put-your-meraki-api-token-here" https://dashboard.meraki.com/api/v0/organizations/`

You should see output with one or more networks like this:

```
[{"id":your-meraki-org-id-here,"name":"Your Meraki Network"}]
```

Copy your Meraki organization ID to use for the environment variables below.

# Spark Call Integration<a name="sparkcall"/>

---
Note: The Spark Call APIs being used have not been officially published. As such, they are subject to change at any time without notification.
---

## Verify Spark Call Administrator Privileges<a name="sparkcall-admin"/>

Go to https://admin.ciscospark.com, and log in with a user that has Full Admin rights to your Spark Call organization.

Select Users on the left, then find your Admin user and click on that user. An overview box will slide in.

![spark_call_select_user](images/spark_call_select_user.png)

Click "Roles and Security"

![spark_call_verify_privileges](images/spark_call_verify_privileges.png)

Ensure that your Admin user has "Full administrator privileges" marked

## Obtain your Spark Call Administrator Token<a name="sparkcall-token"/>

Go to https://developer.ciscospark.com, and log in with a user that has Full Admin rights to your Spark Call organization.

In the upper right, click the user portrait, then click the "Copy" button to copy your Token for the environment variables below.

![spark_get_token](images/spark_get_token.png)

# Usage<a name="usage"/>

The easiest way to use this module is to set a few environment variables. On Windows, use "set" instead of "export". See the ngrok section below if you do not have a web server already facing the Internet.

```
export SPARK_BOT_URL=https://mypublicsite.io  *(your public facing webserver, or the forwarding address from ngrok)*
export SPARK_BOT_TOKEN=<your bot token>
export SPARK_BOT_EMAIL=<your bot email>
export SPARK_BOT_APP_NAME=<your bot name>
export MERAKI_API_TOKEN=<Meraki Dashboard API token>
export MERAKI_ORG=<Meraki Dashboard Organization ID>
export SPARK_API_TOKEN=<Spark Call Admin API token>
```

Now launch your bot!!

`python app.py`
