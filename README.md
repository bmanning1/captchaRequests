# captchaRequests
This code is meant to be generic however the idea and main purpose of creation was for a challenge on OWASP Juice Shop (see the section at the bottom of this page).

### Purpose
Gets the response from each CAPTCHA retrieval call to http://localhost:3000/rest/captcha/ and sets the extracted captchaId and answer parameters in each subsequent form submission as captchaId and captcha.

### Config
The config file makes this code easily reusable. Just replace each value to make in unique to the requests you want.

```{
    "timesToSend": 10,
    "url": "http://localhost:3000/api/Feedbacks/",
    "payload": {
        "comment": "repeating feeback 10 times",
        "rating": 1,
        "captcha": "5",
        "captchaId": 0
    }
}
```

| Config              | Description                                                              |
|---------------------|--------------------------------------------------------------------------|
| timesToSend         | number of requests to make                                               |
| url                 | post to this url on each request                                         |
| payload             | request body to post with for each request                               | 


### OWASP Juice Shop Challenge - *Submit 10 or more customer feedbacks within 10 seconds*
- Open the Network tab of your browser DevTools and visit http://localhost:3000/#/contact
- You should notice a GET request to http://localhost:3000/rest/captcha/ which retrieves the CAPTCHA for the feedback form. The HTTP response body will look similar to {"captchaId":18,"captcha":"5*8*8","answer":"320"}.
- Regularly fill out the form and submit it while checking the backend interaction in your Developer Tools. The CAPTCHA identifier and solution are transmitted along with the feedback in the request body: {comment: "Hello", rating: 1, captcha: "320", captchaId: 18}
- You will notice that a new CAPTCHA is retrieved from the REST endpoint. It will present a different math challenge, e.g. {"captchaId":19,"captcha":"1*1-1","answer":"0"}
- Write another feedback but before sending it, change the captchaId and captcha parameters to the previous values of captchaId and answer. In this example you would submit captcha: "320", captchaId: 18 instead of captcha: "0", captchaId: 19.
- The server will accept your feedback, telling your that the CAPTCHA can be pinned to any previous one you like.
- Write a script with a 10-iteration loop that submits feedback using your pinned captchaId and captcha parameters. Running this script will solve the challenge.
- Two alternate (but more complex) solutions:
  - Rewrite your script so that it parses the response from each CAPTCHA retrieval call to http://localhost:3000/rest/captcha/ and sets the extracted captchaId and answer parameters in each subsequent form submission as captchaId and captcha.
  - Using an automated browser test tool like Selenium WebDriver you could do the following:
    - Read the CAPTCHA question from the HTML element <code id="captcha" ...>
    - Calculate the result on the fly using JavaScript
    - Let WebDriver write the answer into the <input name="feedbackCaptcha" ...> field.
