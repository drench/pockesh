# pockesh
A command line interface to Pocket

# Requirements
* `bash`
* `curl`
* [jo](https://github.com/jpmens/jo)
* [jq](https://stedolan.github.io/jq/)

# Getting started

## Register a new Pocket application

* Open https://getpocket.com/developer/apps/new in a browser that's logged in to your Pocket account
* Fill out the form:
  - Give your application a unique name
  - Grant all permissions: Add, Modify, and Retrieve
  - For platform, choose "Desktop (other)"
  - Accept the Terms of Service and click "Create Application"
* Find your application's `consumer_key` listed on https://getpocket.com/developer/apps/

## Authorize your application

Run this:
```bash
curl \
  -d "consumer_key=$consumer_key" \
  -d "redirect_uri=pockesh:auth" \
  https://getpocket.com/v3/oauth/request
```

The output should read something like this:
```
code=abcdef01-2345-6789-abcd-ef0123
```

Take the value of `code` above and substitute it for `$code` in this URL:

```
https://getpocket.com/auth/authorize?request_token=$code&redirect_uri=https://github.com/drench/pockesh
```

Open the URL in a browser that is logged in to your Pocket account.
Click the "Authorize" button and you should get redirected back to this project's
Github page.

Run this command to complete the authorization process, substituting your
`consumer_key` and `code` values:

```bash
curl \
  -d "consumer_key=$consumer_key" \
  -d "code=$code" \
  https://getpocket.com/v3/oauth/authorize
```

The output should read something like this:
```
access_token=abcdef01-2345-6789-abcd-ef0123&username=yourname%40example.com
```

Create a file in your home directory called `.pocketrc` and save your codes in
it using this format:
```
consumer_key=01234-567890abcdef0123456789ab
access_token=abcdef01-2345-6789-abcd-ef0123
```

Put the `pocket-*` files somewhere in your `$PATH`. I like `~/bin`.

# Examples

Add a URL and tag it with `shell`:
```bash
pocket-add --url https://github.com/drench/pockesh --tags shell
```

Delete an item:
```bash
pocket-modify --action delete --item_id 1455241157
```

Get the URLs of all items tagged `longreads`, sorted by most recently added first:
```bash
pocket-get --tag longreads --sort-newest-first | jq '.list | .[] | .resolved_url'
```

Flag an item as a "favorite":
```bash
pocket-modify --action favorite --item_id 145416741
```

# For more information

https://getpocket.com/developer/docs/overview
