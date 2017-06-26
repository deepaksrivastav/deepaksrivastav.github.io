---
layout: post
title: "Embedding an Image in Mail body (HTML) using JavaMail"
date: 2011-08-29 19:02
categories: java javamail
---

Here is the piece of code which can do exactly this:

```java
String emailServer = "mail_server_here";
String fromAddr = "from@email.com";
String toEmail = "to@email.com";
String ccEmail = "cc@email.com";

// Get system properties
Properties props = System.getProperties();

// Setup mail server
props.put("mail.smtp.host", emailServer);

// Get session
Session session = Session.getDefaultInstance(props, null);

// Create the message
Message message = new MimeMessage(session);

// Fill its headers
try {
    message.setSubject("Test mail with message");

    // from address

    message.setFrom(new InternetAddress(fromAddr));

    // to list
    message.addRecipient(Message.RecipientType.TO, new InternetAddress(
            toEmail));

    message.addRecipient(Message.RecipientType.CC, new InternetAddress(
            ccEmail));

    // create the message body part
    BodyPart messageBodyPart = new MimeBodyPart();

    // Set the content of the message body part (html)
    messageBodyPart
            .setContent(
                    "<html><body>test image below<br><img src=\"cid:myImage\"></body></html>",
                    "text/html");

    // Create a related multi-part to combine the parts
    MimeMultipart multipart = new MimeMultipart("related");

    // Add body part to multipart
    multipart.addBodyPart(messageBodyPart);

    // Create part for the image
    messageBodyPart = new MimeBodyPart();

    // Fetch the image and associate to part
    DataSource fds;
    try {
        fds = new InputStreamDataSource("images/someImage.png",
                "image/png");
        messageBodyPart.setDataHandler(new DataHandler(fds));

        // Add a header to connect to the HTML
        messageBodyPart.setHeader("Content-ID", "<myImage>");
    } catch (IOException e) {
        log.error("Unable to send mail. ", e);
        // Re throw exception so that job status will be updated with
        // NOT_RUN state in case of any exception
        throw new RuntimeException(e);
    }

    // Add part to multi-part
    multipart.addBodyPart(messageBodyPart);

    // Associate multi-part with message
    message.setContent(multipart);

    // Send message
    Transport.send(message);

} catch (MessagingException e) {
    log.error("Unable to send mail. ", e);
    // Re throw exception so that job status will be updated with
    // NOT_RUN state in case of any exception
    throw new RuntimeException(e);
}
```

In the above code snippet, notice that the body of the mail is actually as shown below :

```html
<html>
    <body>
        test image below<br>
        <img src="cid:myImage">
    </body>
</html>
```

To embedded images in the body part of the mail (should be html type), you must treat the image as an attachment and reference the image with a special “cid” URL, where the cid is a reference to the Content-ID header of the image attachment. This is exactly what is done in the Line 59 in the above code snippet.
