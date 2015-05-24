---
weight: 200
category: authentication
path: /auth/login/passwd
title: 'Login with a username/password'
type: 'POST'

layout: nil
---

This method allows a client (having obtained a nonce) to login with a credentials derived from a username/password pair.

### Request

 * This request requires no special headers.

``` {
	token: 123456789012345678901234567890,
	user-id: 'bobh',
	key: 123131668484872574857485485748,
	desired-ttl: 3600
}
```

**token** MUST be a still-valid token retrieved from /auth/login/nonce

**user-id** MUST be the userid of the logging in user.

**key** MUST be the nonce from before, encrypted with a hash of the password.

**desired-ttl** MAY be a desired length for the returned auth token to live

### Response

```Status: 200 OK```
```{
	token: 348134712387408123740127401234,
	ttl: 3600
}
```
**token** MUST be a access token for later use, encrypted with a different hash of the password

**ttl** MAY be a time, in seconds, until the token is no longer valid

## 2-Factor
```Status: ???? ??```
```{
	token: 34394858723405689234905782304,
	expire: 600,
	methods: 
		[
			'finger',
			'totp',
			'hotp',
			'scard',
			'devicekey'
		]
}
```

**token** MUST be a token, encrypted with a different hash of the password, that will be used later on to send the other factor

**expire** MAY be a time, in seconds, until this token is no longer useable.

**methods** MUST be an array of strings, listhing the acceptable 2-factor methods to use, in order of desire.

Examples:

totp: TOTP-compatble key-generator

hotp: HOTP-compatable key-generator

scard: Smart-Card based authentication

finger: fingerprint based biometrics

devicekey: a key stored in a device-specific manner, for use with systems such as Apple's Touch ID
 
## Error
```Status: 403 Unauthroized```
```{
	reason: "Invalid username/password"
}
```

**reason** MUST be a reason for the failure, may be translated into an apropriate language based on Accept-Language header