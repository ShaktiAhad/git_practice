## I wasn’t able to send email using Gmail username/password. I made following changes to make it work.

## Create password for external [*Check the ScreenShot*]
Got to [Google Account Security](https://myaccount.google.com/security) and Scroll down to **"Signing in to Google"** option. If you don't have 2 step verification on; enable it. Then you will see **"App passwords"** option. Generate a password for your lambda function.\***Make sure to copy it else you will not be able to copy the password later.*** 

[1]: App_Password_1.png "App_Password_1"
[2]: App_Password_2.png "App_Password_2"
[3]: App_Password_3.png "App_Password_3"
[4]: Generated_Password.png "Generated_Password"


## Sample lambda function to verify SMTP server
I have used a this following code to verify my SMTP server is working. 
```
import smtplib, os

def lambda_handler(event=None, context=None):
    sender = os.getenv("SENDER")
    receiver = os.getenv("RECEIVER")
    password = os.getenv("PASSWORD")
    host = os.getenv("SMTPHOST")
    port = os.getenv("SMTPPORT")

    try:
        server = smtplib.SMTP(host, port)
        server.ehlo()
        server.starttls()
        server.login(sender, password)
        server.sendmail(sender, receiver, msg="Subject: Test\n\n This is a test from lambda")
        server.close()
        return True
    except Exception as ex:
        print (ex)
        return False

lambda_handler()
```
