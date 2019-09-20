---
title: Using HMAC to authenticate Web service requests
created: '2019-09-05T01:26:10.665Z'
modified: '2019-09-05T01:26:13.338Z'
---

# [Using HMAC to authenticate Web service requests](http://rc3.org/2011/12/02/using-hmac-to-authenticate-web-service-requests/ "Using HMAC to authenticate Web service requests")

[December 2, 2011](http://rc3.org/2011/12/02/using-hmac-to-authenticate-web-service-requests/ "Using HMAC to authenticate Web service requests") / [Rafe](http://rc3.org/author/admin/ "Posts by Rafe") / [6 Comments](http://rc3.org/2011/12/02/using-hmac-to-authenticate-web-service-requests/#comments)

One weakness of many Web services that require authentication, including the ones I’ve built in the past, is that the username and password of the user making the request are simply included as request parameters. Alternatively, some use [basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication), which transmits the username and password in an HTTP header encoded using Base64. Basic authentication obscures the password, but doesn’t encrypt it.

This week I learned that there’s a better way — using a [Hash\-based Message Authentication Code](https://en.wikipedia.org/wiki/HMAC) (or HMAC) to sign service requests with a private key. An HMAC is the product of a hash function applied to the body of a message along with a secret key. So rather than sending the username and password with a Web service request, you send some identifier for the private key and an HMAC. When the server receives the request, it looks up the user’s private key and uses it to create an HMAC for the incoming request. If the HMAC submitted with the request matches the one calculated by the server, then the request is authenticated.

There are two big advantages. The first is that the HMAC allows you to verify the password (or private key) without requiring the user to embed it in the request, and the second is that the HMAC also verifies the basic integrity of the request. If an attacker manipulated the request in any way in transit, the signatures would not match and the request would not be authenticated. This is a huge win, especially if the Web service requests are not being made over a secure HTTP connection.

There’s one catch that complicates things.

For the signatures to match, not only must the private keys used at both ends of the transaction match, but the message body must also match exactly. URL encoding is somewhat flexible. For example, you may choose to encode spaces in a query string as `%20`. I may prefer to use the `+` character. Furthermore, in most cases browsers and Web applications don’t care about the order of HTTP parameters.

```
foo=one&bar=two&baz=three

```

and

```
baz=three&bar=two&foo=one

```

are functionally the same, but the crypto signature of the two will not be.

Another open question is where to store the signature in the request. By the time the request is submitted to the server, the signature derived from the contents of the request will be mixed in with the data that is used to generate the signature. Let’s say I decide to include the HMAC as a request parameter. I start with this request body:

```
foo=one&bar=two&baz=three

```

I wind up with this one:

```
foo=one&bar=two&baz=three&hmac=de7c9b8 ...

```

In order to calculate the HMAC on the server, I have to remove the incoming HMAC parameter from the request body and calculate the HMAC using the remaining parameters. This is where the previous issue comes into play. If the HMAC were not in the request, I could simply calculate the signature based on the raw incoming request. Once I start manipulating the incoming request, the chances of reconstructing it imperfectly rise, possibly introducing cases where the signatures don’t match even though the request is valid.

This is an issue that everyone implementing HMAC\-based authentication for a Web service has to deal with, so I started looking into how other projects handled it. OAuth uses HMAC, with the added wrinkle that the signature must be applied to POST parameters in the request body, query string parameters, and the OAuth HTTP headers included with the request. For OAuth, the signature can be included with the request as an HTTP header or as a request parameter.

This is a case where added flexibility in one respect puts an added burden on the implementor in others. To make sure that the signatures match, OAuth has [very specific rules](https://oauth.net/core/1.0a/#anchor13) for encoding and ordering the request data. It’s up to the implementor to gather all of the parameters from the query string, request body, and headers, get rid of the `oauth_signature` parameter, and then organize them based on rules in the OAuth spec.

Amazon S3’s REST API also uses HMAC signatures for authentication. Amazon embeds the user’s public key and HMAC signature in an HTTP header, eliminating the need to extract it from the request body. In Amazon’s case, the signed message is assembled from the HTTP verb, metadata about the resource being manipulated, and the “Amz” headers in the request. All of this data must be canonicalized and added to the message data to be signed. Any bug in the translation of those canonicalization rules into your own codes means that none of your requests will be authenticated by Amazon.com.

Amazon uses the `Authorization` header to store the public key and HMAC. This is also the approach that [Microsoft recommends](https://msdn.microsoft.com/en-us/library/dd203052.aspx). I think it’s superior to the parameter\-based approach taken by OAuth. It should be noted that the Authorization header is part of the [HTTP specification](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) and if you’re going to use it, you should do so in a way that complies with the standard.

For my service, which is simpler than Amazon S3 or OAuth, I’ll be using the Authorization header and computing the HMAC based on the raw incoming request.

I realize that HMAC may not be new to many people, but it is to me. Now that I understand it, I can’t imagine using any of the older approaches to build an authenticated Web service.

Regardless of which side of the Web service transaction you’re implementing, calculating the actual HMAC is easy. Normally the SHA\-1 or MD5 hashing algorithms are used, and it’s up to the implementor of the service to decide which of those they will support. Here’s how you create HMAC\-SHA1 signatures using a few popular languages.

PHP has a built\-in HMAC function:

```
hash_hmac('sha1', "Message", "Secret Key");

```

In Java, it’s not much more difficult:

```
Mac mac = Mac.getInstance("HmacSHA1");
SecretKeySpec secret =
    new SecretKeySpec("Secret Key".getBytes(), "HmacSHA1");

mac.init(secret);
byte[] digest = mac.doFinal("Message".getBytes());

String hmac = Hex.encodeHexString(digest);

```

In that case, the `Hex` class is the Base64 encoder provided by Apache’s Commons Codec project.

In Ruby, you can use the HMAC method provided with the OpenSSL library:

```
DIGEST  = OpenSSL::Digest::Digest.new('sha1')

Base64.encode64(OpenSSL::HMAC.digest(DIGEST,
  "Secret Key", "Message"))

```

There are also libraries like [crypto\-js](https://code.google.com/p/crypto-js/) that provide HMAC support for JavaScript.

