![image](https://github.com/kamrullab/Activate-Gmail-for-Google-Workspace/assets/128359757/31c73fac-2c9f-4642-be56-b86ed11b5d68)


# Activate Gmail for Google Workspace on Your Domain 🔐

Today I provides step-by-step instructions on how to activate Gmail for your domain using Google Workspace. Gmail activation allows you to use Gmail as your email service provider for your custom domain.<br> Please follow the steps below to set up the necessary DNS records.

## Prerequisites
- You should have administrative access to your domain registrar's dashboard.
- You must have a Google Workspace account.

## Step 1: Access Google Workspace Gmail Setup 🚀
1. Go to [Google Workspace Gmail Setup](https://admin.google.com/u/2/ac/signup/setup/v2/verify/mx) in your Google Workspace admin console.
2. Sign in with your Google Workspace admin credentials.

## Step 2: Configure MX Records ⚙️
1. In the admin console, navigate to **Admin** and search for "users, groups, or settings."
2. Choose **MX** for the type of record.
3. Enter `@` in the Name, Host, or Alias field. Leave this field blank if `@` causes an error.
4. Enter `1` in the Priority field.
5. Enter `1 hour` in the TTL (Time to Live) field.

### Sample MX Record Configuration (If Signed Up After April 2023) 📅:
| Priority | Name/Host/Alias | Value/Answer/Destination |
|----------|-----------------|--------------------------|
| 1        | Blank or @      | SMTP.GOOGLE.COM          |

### Sample MX Record Configuration (If Signed Up Before April 2023) 🕰️:
| Priority | Name/Host/Alias | Value/Answer/Destination |
|----------|-----------------|--------------------------|
| 1        | Blank or @      | ASPMX.L.GOOGLE.COM       |
| 5        | Blank or @      | ALT1.ASPMX.L.GOOGLE.COM  |
| 5        | Blank or @      | ALT2.ASPMX.L.GOOGLE.COM  |
| 10       | Blank or @      | ASPMX2.GOOGLEMAIL.COM    |
| 10       | Blank or @      | ASPMX3.GOOGLEMAIL.COM    |

6. Click on `SMTP.GOOGLE.COM` (if signed up after April 2023) or `ASPMX.L.GOOGLE.COM` (if signed up before April 2023) in the table above to copy the appropriate MX record.
7. Paste the selected MX record in the field labeled **Value/Answer/Destination/Server** at your domain registrar.

### Important Note:
- If you've subscribed to Google Workspace after April 2023, you only need one MX record: `SMTP.GOOGLE.COM`.
- If you subscribed before April 2023, you need five MX records as listed above.

8. Some registrars may require a period at the end of `SMTP.GOOGLE.COM` or `ASPMX.L.GOOGLE.COM`.

For more information on MX records and their setup, you can refer <br> to [Google's MX record setup guide](https://support.google.com/a/answer/174125?hl=en).

## Step 3: Save MX Records 💾
1. Save your new MX record settings.

## Step 4: Set up SPF, DKIM, and DMARC 🔒
To enhance security and authentication for your domain, it's essential to set up SPF, DKIM, and DMARC.

### SPF (Sender Policy Framework) 📩:
To set up SPF, add the following TXT record to your DNS:
```
v=spf1 include:_spf.google.com ~all
```

### DKIM (DomainKeys Identified Mail) 🔐:
1. Go to the Google Workspace Admin Console.
2. Click on **Apps > Google Workspace > Gmail > Authenticate email**.
3. Follow the instructions provided to set up DKIM for your domain. This involves setting up another record on your DNS that's specific to your domain.

### DMARC (Domain-based Message Authentication, Reporting, and Conformance) 📨:
1. Create a DMARC policy record. Most people use the default one provided:
```
v=DMARC1; p=reject; rua=mailto:postmaster@yourdomain.com; pct=100; adkim=s; aspf=s
```
   - Replace `yourdomain.com` with your actual domain name.
   - Explanation of the components:
     - `v=DMARC1`: Indicates the version of DMARC being used.
     - `p=reject`: Specifies the policy for handling messages that fail DMARC authentication. In this example, it rejects such messages.
     - `rua=mailto:postmaster@yourdomain.com`: Specifies the email address where aggregate DMARC reports will be sent. Replace with your desired email address.
     - `pct=100`: Specifies the percentage of messages subjected to filtering. Setting it to 100 means all messages are subjected to filtering.
     - `adkim=s`: Aligns the DKIM authentication domain with the sender's RFC5322.From domain.
     - `aspf=s`: Aligns the SPF authentication domain with the sender's RFC5322.From domain.

2. Add a DNS TXT record or modify an existing one by entering your DMARC record in the TXT record for `_dmarc.yourdomain.com`. 

### Example DMARC TXT Record:
- TXT record name (DNS Hostname): `_dmarc.yourdomain.com`
- TXT record value: `v=DMARC1; p=reject; rua=mailto:dmarc-reports@yourdomain.com`

## Step 5: Activate Gmail ✅
1. Once you've added the MX record(s), set up SPF, DKIM, and DMARC, click on **Activate Gmail** to complete the setup.
2. Your custom domain will now be configured to use Gmail as its email service provider with enhanced security and DMARC authentication.

## Example DNS Configuration 🌐
Here is an example of DNS settings for a domain used with Google Cloud services. Remember to use the `@` symbol to indicate the domain name in your DNS settings.

| Name / Host / Alias | Record Type | Priority | Value / Answer / Destination |
|----------------------|-------------|----------|------------------------------|
| Blank or @           | A           | NA       | 216.239.32.21                |
| Blank or @           | A           | NA       | 216.239.34.21                |
| Blank or @           | A           | NA       | 216.239.36.21                |
| Blank or @           | A           | NA       | 216.239.38.21                |
| Blank or @           | MX          | 1        | ASPMX.L.GOOGLE.COM.          |
| Blank or @           | MX          | 5        | ALT1.ASPMX.L.GOOGLE.COM.     |
| Blank or @           | MX          | 5        | ALT2.ASPMX.L.GOOGLE.COM.     |
| Blank or @           | MX          | 10       | ASPMX2.GOOGLEMAIL.COM.       |
| Blank or @           | MX          | 10       | ASPMX3.GOOGLEMAIL.COM.       |
| mail                 | CNAME       | NA       | ghs.googlehosted.com.         |
| Blank or @           |
| Blank or @           | TXT         | NA       | google-site-verification=6tTalLzrBXBO4Gy9700TAbpg2QTKzGYEuZ_Ls69jle8 |
| Blank or @           | TXT         | NA       | v=spf1 include:_spf.google.com ~all |
| _dmarc.yourdomain.com| TXT         | NA       | v=DMARC1; p=reject; rua=mailto:postmaster@yourdomain.com; pct=100; adkim=s; aspf=s |
| www                  | CNAME       | NA       | ghs.googlehosted.com.         |

These DNS settings are provided as an example and may vary depending on your specific requirements.

## Step 6: Set up VMC (Verified Mark Certificate) 🏆
Google requires your logo to be trademarked and registered with a Verified Mark Certificate authority (VMC). You'll need to use DigiCert or Entrust for this purpose. Once you have obtained a VMC, follow these steps:

- Upload the certificate you acquired to your website and ensure it's publicly accessible.
- Take note of the certificate details as you'll need them in the following steps.

## Step 7: Set up BIMI (Brand Indicators for Message Identification) 🖼️
To display your logo in Gmail next to your emails, follow these steps:

1. Convert your logo to Tiny SVG 1.2 format using a conversion tool.
2. Ensure your SVG file meets the required specifications by referencing the guide and running it through the verification tool.
3. Upload your logo to your website and make sure it's publicly accessible.
4. Create your BIMI record using the generation tool and add it to your DNS as a TXT record. Be sure to include the links to your logo and the PEM certificate issued by your VMC provider.

## Step 8: Success! 🎉
Once your domain and logo have been verified, a blue checkmark should appear next to your emails in Gmail, indicating successful activation, authentication, and DMARC policy enforcement.

Congratulations! You have successfully activated Gmail for your custom domain with enhanced security measures and DMARC authentication.

## About This Repository 📚
This repository is maintained by Kamrul Hossain. We are committed to making educational information in Bangladesh more accessible.

**Explore the World of Premier Business Email Services and Support in Bangladesh**

**Brought to You by Expert Kamrul Hossain**

[Connect with Kamrul Hossain on Facebook](https://www.facebook.com/EliteKamrul) 🌐
