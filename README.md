# Automated survey for Python - Flask
[![Build Status](https://travis-ci.org/TwilioDevEd/automated-survey-flask.svg?branch=master)](https://travis-ci.org/TwilioDevEd/automated-survey-flask)

Learn how to use [Twilio Client](https://www.twilio.com/client) to conduct automated phone surveys.

## Quickstart

### Local development

This project is built using the [Flask](http://flask.pocoo.org/) web framework. It runs on Python 2.7+ and Python 3.4+.

To run the app locally follow these steps:

1. Clone this repository and `cd` into it.

1. Create a new virtual environment:
    - If using vanilla [virtualenv](https://virtualenv.pypa.io/en/latest/):

        ```bash
        virtualenv env
        source env/bin/activate
        ```
    - To deactivate vitural environment:

        ```bash
        deactivate
        ```
        
    - If using [virtualenvwrapper](https://virtualenvwrapper.readthedocs.org/en/latest/):

        ```bash
        mkvirtualenv automated-survey
        ```

1. Install the requirements.

    ```bash
    pip install -r requirements.txt
    ```

1. Copy the `.env.example` file to `.env`, and edit it to match your database.

1. Run `source .env` to apply the environment variables (or even better, use [autoenv](https://github.com/kennethreitz/autoenv))

1. Run the migrations.

    ```bash
    python manage.py db upgrade
    ```
    
    If you want to clear and update the database type the following two commands:
    ```bash
    python manage.py db downgrade
    python manage.py db upgrade
    ```

1. Seed the database.

   ```bash
   python manage.py dbseed
   ```

   Seeding will load `survey.json` into SQLite.

1. Expose your application to the wider internet using ngrok.

    To actually forward incoming calls, your development server will need to be publicly accessible.
    [We recommend using ngrok to solve this problem](https://www.twilio.com/blog/2015/09/6-awesome-reasons-to-use-ngrok-when-testing-webhooks.html).

    ```bash
    $ ngrok http 5000
    ```

    Once you have started ngrok, update your TwiML app's voice URL setting to use your ngrok hostname.
    It will look something like this:

    ```bash
    http://88b37ada.ngrok.io/voice
    ```

1. ngrok is a blocking command, meaning you cannot do anything in the terminal window while it is running.  You will need to open a new tab in the terminal window and activate your virtual environment to run the server.  To start at new tab in the terminal window press Command+T.  To activate your virtual environment type:
        ```bash
        source env/bin/activate
        ```

1. Start the development server.

    ```bash
    python manage.py runserver
    ```

Once ngrok is running, open up your browser and go to your ngrok URL. It will
look like this: `http://88b37ada.ngrok.io`

1. To shut down ngrok and/or the server, press ctrl-c.

That's it!

### Configuring Twilio to call your webhooks

You will also need to configure Twilio to call your application when
calls are received

You will need to provision at least one Twilio number with voice
capabilities so the application's users can take surveys. You can buy
a number
[right here](https://www.twilio.com/user/account/phone-numbers/search). Once
you have a number you need to configure your number to work with your
application. Open
[the number management page](https://www.twilio.com/user/account/phone-numbers/incoming)
and open a number's configuration by clicking on it.

![Open a number configuration](https://raw.github.com/TwilioDevEd/automated-survey-flask/master/images/number-conf.png)

The URL you will place the the *Request URL* field will be as follows. Set
the HTTP method to GET. Be sure to change the ngrok hostname to your own.

```bash
http://20ee7404.ngrok.io/voice
```

Similarly, you must configure the SMS messaging section of your Twilio Phone Number
To call the `/message` webhook.

```bash
http://20ee7404.ngrok.io/voice
```


See the images below for an example:

![Webhook Voice configuration](https://raw.githubusercontent.com/TwilioDevEd/automated-survey-flask/master/images/webhook-conf.png)

You can then visit the application at [http://localhost:5000/](http://localhost:5000/).

Mind the trailing slash.

## Running the tests

You can run the tests locally through [coverage](http://coverage.readthedocs.org/):

1. Run the tests.

    ```bash
    $ coverage run manage.py test
    ```

You can then view the results with `coverage report` or build an HTML report with `coverage html`.

## Meta

* No warranty expressed or implied. Software is as is. Diggity.
* [MIT License](http://www.opensource.org/licenses/mit-license.html)
* Lovingly crafted by Twilio Developer Education.
