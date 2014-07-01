##Sending mails using apache commons email
- [Simple text mail](#simple-text-mail)
- [Sending attachments](#sending-attachments)
- [Bounced emails](#bounced-emails)
- [Other useful classes](#other-useful-classes)
- [Sources of Information](#sources-of-information)
- [Complete Code](#complete-code)

###Simple text mail
Using apache emails is pretty easy. For sending simple text emails, ```SimpleEmail``` class can be used. User id and password for authentication can be set creating a ```DefaultAuthenticator``` object and passing it to SimpleEmail object. Here is a simple example

```java
//Authentication
DefaultAuthenticator getAuthenticator() throws IOException {
    if (auth == null) {
        auth = new DefaultAuthenticator("me@gmail.com", io.ns());
    }
    return auth;
}

//Simple Email
void simpleEmail() throws EmailException, IOException {
    Email email = new SimpleEmail();
    email.setHostName("smtp.googlemail.com");

    email.setSmtpPort(465);
    email.setAuthenticator(getAuthenticator());
    email.setSSLOnConnect(true);

    email.setFrom("me@gmail.com");
    email.setSubject("TestMail");
    email.setMsg("This is a test mail ... :-)");
    email.addTo("12@gmail.com");
    email.send();
}
```
###Sending attachments
The ```SimpleEmail``` class does not support attachments. ```MultiPartEmail``` class can be used for this. Attachments can be inline or normal attachments. A EmailAttachment object should be created and this object should be passed to ```email.attach``` function
```java
//create an attachment object
EmailAttachment getAttachment() {
    EmailAttachment attachment = new EmailAttachment();
    attachment.setPath(System.getProperty("user.home") + "/Desktop/me.png");
    attachment.setDisposition(EmailAttachment.ATTACHMENT); //attachment or inline
    attachment.setDescription("That is me"); //have no idea why an attachment needs description
    attachment.setName("attachment.png"); //the file will be attached with this file name
    return attachment;
}

//attachment example
void emailWithAttachment() throws IOException, EmailException {
    MultiPartEmail email = new MultiPartEmail();
    email.setHostName("smtp.googlemail.com");

    email.setSmtpPort(465);
    email.setAuthenticator(getAuthenticator());
    email.setSSLOnConnect(true);

    email.setFrom("me@gmail.com");
    email.setSubject("TestMail with attachment");
    email.setMsg("This is a test mail");
    email.addTo("me@gmail.com");
    email.attach(getAttachment());
    email.send();
}
```
###Bounced emails
If the email sending fails, by default the mail containing reason for failure is sent back to the sender. From the library documentation, this can be over-rided by setting a bounced address. But this did not work for me. (might be a problem with gmail : [issue #2](https://github.com/dotslash/OneLibADay/issues/2))
###Other useful classes
These are the classes not covered in this document. (description copied from library userguide) 
- ```HtmlEmail``` - This class is used to send HTML formatted emails. It has all of the capabilities as MultiPartEmail allowing attachments to be easily added. It also supports embedded images.
- ```ImageHtmlEmail``` - This class is used to send HTML formatted emails with inline images. It has all of the capabilities as HtmlEmail but transform all image references to inline images.

###Sources of Information
[apache commons email home page](http://commons.apache.org/proper/commons-email/)
###Complete Code
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;
import org.apache.commons.mail.DefaultAuthenticator;
import org.apache.commons.mail.EmailAttachment;
import org.apache.commons.mail.EmailException;
import org.apache.commons.mail.MultiPartEmail;
import org.apache.commons.mail.SimpleEmail;
import org.apache.commons.mail.Email;


public class mail {
    IO io = IO.getInstance();
    private DefaultAuthenticator auth = null;

    public static void main(String[] args) throws EmailException, IOException {
        new mail().simpleEmail();
    }


    //Authentication
    DefaultAuthenticator getAuthenticator() throws IOException {
        if (auth == null) {
            auth = new DefaultAuthenticator("me@gmail.com", io.ns());
        }
        return auth;
    }
    //Simple Email
    void simpleEmail() throws EmailException, IOException {
        Email email = new SimpleEmail();
        email.setHostName("smtp.googlemail.com");

        email.setSmtpPort(465);
        email.setAuthenticator(getAuthenticator());
        email.setSSLOnConnect(true);

        email.setFrom("me@gmail.com");
        email.setSubject("TestMail");
        email.setMsg("This is a test mail ... :-)");
        email.setBounceAddress("someoneelse@gmail.com");
        email.addTo("12@gmail.com");
        email.send();
    }

    //create an attachment object
    EmailAttachment getAttachment() {
        EmailAttachment attachment = new EmailAttachment();
        attachment.setPath(System.getProperty("user.home") + "/Desktop/me.png");
        attachment.setDisposition(EmailAttachment.ATTACHMENT); //attachment or inline
        attachment.setDescription("That is me"); //have no idea why an attachment needs description
        attachment.setName("attachment.png"); //the file will be attached with this file name
        return attachment;
    }

    //attachment example
    void emailWithAttachment() throws IOException, EmailException {
        MultiPartEmail email = new MultiPartEmail();
        email.setHostName("smtp.googlemail.com");

        email.setSmtpPort(465);
        email.setAuthenticator(getAuthenticator());
        email.setSSLOnConnect(true);

        email.setFrom("me@gmail.com");
        email.setSubject("TestMail with attachment");
        email.setMsg("This is a test mail");
        email.addTo("me@gmail.com");
        email.attach(getAttachment());
        email.send();
    }

    //UTIL class for IO
    static class IO {
        //Standard IO
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer tokenizer = null;

        public static IO getInstance() {
            return new IO();
        }

        int ni() throws IOException {
            return Integer.parseInt(ns());
        }

        long nl() throws IOException {
            return Long.parseLong(ns());
        }

        double nd() throws IOException {
            return Double.parseDouble(ns());
        }

        String ns() throws IOException {
            while (tokenizer == null || !tokenizer.hasMoreTokens())
                tokenizer = new StringTokenizer(br.readLine());
            return tokenizer.nextToken();
        }

        String nline() throws IOException {
            tokenizer = null;
            return br.readLine();
        }
    }
}
```


