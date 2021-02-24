## [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self signed certificate in certificate chain...

I encounterred this error when trying to upload some files to Google Storage service using google storage python lib.
As the error suggests, there is a self signed certificate was used to make a connection to the server.
I my case, I was using Charles. Simpley closing Charles fixed the issue.
