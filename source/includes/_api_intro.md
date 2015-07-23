# REST API

Mautic provides a REST API to manipulate leads and/or obtain information for various entities of Mautic. 

<aside class="notice">
Currently, the only _editable_ entity is leads and is the main purpose for the Mautic API. All other entities are read-only.
</aside>

<aside class="warning">
All Mautic API endpoints require an OAuth1a signtature or OAuth2 access token.
</aside>

## Error Handling

If an OAuth error is encountered, it'll be a JSON encoded array similar to:

<pre class="inline">
{
  "error": "invalid_grant",
  "error_description": "The access token provided has expired."
}
</pre>

If a system error encountered, it'll be a JSON encoded array similar to:

<pre class="inline">
{
    "error": {
        "message": "You do not have access to the requested area/action.",
        "code": 403
    }
}
</pre>
