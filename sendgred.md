SendGrid

-     
    
-   [Login](https://app.sendgrid.com/)
-   [Start for Free](https://signup.sendgrid.com/)

![Cookie Preferences](https://consent.trustarc.com/asset/twilio2.png)

[SendGrid Documentation](https://docs.sendgrid.com/) / [For Developers](https://docs.sendgrid.com/for-developers) / [Sending Email](https://docs.sendgrid.com/for-developers/sending-email) / v3 API C# Code Example

## On This Page

-   [Using SendGrid's C# Library](https://docs.sendgrid.com/for-developers/sending-email/v3-csharp-code-example#using-sendgrids-c-library)

Rate this page:

# v3 API C# Code Example

We recommend using SendGrid C#, our client library, [available on GitHub](https://github.com/sendgrid/sendgrid-csharp), with full documentation.

Do you have an [API Key](https://app.sendgrid.com/settings/api_keys) yet? If not, go get one. You're going to need it to integrate!

## [Using SendGrid's C# Library](https://docs.sendgrid.com/for-developers/sending-email/v3-csharp-code-example#using-sendgrids-c-library)

```csharp
// using SendGrid's C# Library
// https://github.com/sendgrid/sendgrid-csharp
using SendGrid;
using SendGrid.Helpers.Mail;
using System;
using System.Threading.Tasks;

namespace Example
{
    internal class Example
    {
        private static void Main()
        {
            Execute().Wait();
        }

        static async Task Execute()
        {
            var apiKey = Environment.GetEnvironmentVariable("NAME_OF_THE_ENVIRONMENT_VARIABLE_FOR_YOUR_SENDGRID_KEY");
            var client = new SendGridClient(apiKey);
            var from = new EmailAddress("test@example.com", "Example User");
            var subject = "Sending with SendGrid is Fun";
            var to = new EmailAddress("test@example.com", "Example User");
            var plainTextContent = "and easy to do anywhere, even with C#";
            var htmlContent = "<strong>and easy to do anywhere, even with C#</strong>";
            var msg = MailHelper.CreateSingleEmail(from, to, subject, plainTextContent, htmlContent);
            var response = await client.SendEmailAsync(msg);
        }
    }
}
```

Rate this page:

#### Need some help?

We all do sometimes; code is hard. Get help now from our [support team](https://support.sendgrid.com/hc/en-us), or lean on the wisdom of the crowd browsing the [SendGrid tag](http://stackoverflow.com/questions/tagged/sendgrid) on Stack Overflow.

-   [Terms of Service](https://docs.sendgrid.com/legal/tos)
-   [Privacy Policy](https://docs.sendgrid.com/legal/privacy)
-   Copyright © 2022 Twilio Inc.