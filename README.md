# Proposal page

A single file, `index.html`. No build step.

## 1. Fill in the config
At the top of the `<script>` in `index.html`:

```js
const CONFIG = {
  HER_NAME: "Her Name",
  MY_NAME: "Your Name",
  EMAILJS_PUBLIC_KEY: "YOUR_PUBLIC_KEY",
  EMAILJS_SERVICE_ID: "YOUR_SERVICE_ID",
  EMAILJS_TEMPLATE_ID: "YOUR_TEMPLATE_ID",
};
```

Until the EmailJS keys are filled in, the page still works as a preview. It just doesn't send an email.

## 2. EmailJS template
In the EmailJS dashboard → Email Templates → create a template:

- **To Email:** your own address
- **Subject:** `{{her_name}} answered: {{answer}}`
- **Body:**
  ```
  Answer: {{answer}}
  Her message: {{message}}
  Time: {{time}}
  ```

Public key: Account → General. Service ID: Email Services. Template ID: the template's settings.

Also, under Account → Security, add your hosted domain to the allowed origins, so no one else can use your key.

## 3. Host it
The easiest way is to drag the folder onto https://app.netlify.com/drop. You can also use GitHub Pages. Then send her the link.

## Notes
- Nothing is saved on her device. Every visit starts fresh with the envelope, and every answer she sends arrives as a new email.
- If the EmailJS script is blocked (for example by an ad blocker), the page sends the email straight to the EmailJS API instead. If sending fails, she sees a message asking her to try again. The thank-you screen only appears after the email has gone.
