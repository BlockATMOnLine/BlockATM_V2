# Url Signing

A signed URL helps limit access from unauthorized third parties by providing limited permissions and time to make a request. it must be appended at the end of the URL.

### How to sign URLs

1. Send your widget URL to your backend server.
2. Generate the signature using the secret key found in your BockATM Cashier dashboard.
3. Return either the signature or entire signed URL.
4. Show the widget using the SDK or URL.



### How to generate signatures

1. Create an HMAC (Hash-Based Message Authentication Code) using the SHA-256 hash function

* Use your secret API key as the key
* the original URL's query string as the message.

2. For URL-based integrations, make sure all query parameter values are **URL-encoded** before creating the signature.



### Code examples:





