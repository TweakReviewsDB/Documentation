# TweakReviewsDB

TweakReviewsDB is an API for tweak content and compatibility reviews.

## Glossary

- **Content review:** A review that contains text and stars. A user can only make one content review for a package.
- **(Tweak) status:** "Working", "Partially working" or "Broken".
- **Compatibility review:** A review that contains tweak status information. A user can create a different compatibility review for every version of a package using more than one device.

## Endpoints

### `/v3/package/:package/:version/:ios/:model`

- **`package`:** Package identifier. Must match the regex `/^[0-9a-zA-Z\\-+._]+$/`.
- **`version`:** Package version. Must match the regex `/^[0-9a-zA-Z\\-.+:~]+$/`. Specify `any` in `GET` requests to get compatibility reviews for all package versions.
- **`ios`:** iOS version. Must match the regex `/^[0-9]+\\.[0-9]+(?:\\.[0-9]+)?$/`. Specify `any` in `GET` requests to get compatibility reviews for all iOS versions.
- **`model`:** iOS version. Must match the regex `/^iP(?:od|hone|ad)[0-9]{1,2},[0-9]{1,2}$/`. Specify `any` in `GET` requests to get compatibility reviews for all device models.

#### `GET`

Returns content and compatibility reviews. An example response can be seen below.

```javascript
{
  "reviews": [
    {
      "id": 5382,               // content review ID
      "stars": 5,               // {1...5}
      "username": "john.smith", // unique to each user
      "verified_user": false,   // VIP flag to prevent impersonation
      "content": "This tweak is great!",  // review content
      "version": "1.2.1",       // package version
      "ios": "14.0.1"           // iOS version
    }
  ],
  
  // Always based on filters in URL
  "status": {
    "working": 23, // Fully working
    "partial": 3,  // Partially working
    "broken": 1    // Broken
  }
}
```

Additional URL parameters can be specified in `GET` requests.

- **`includeStatus`:** Specify `0` to exclude compatibility reviews. Defaults to `1`.
- **`includeReviews`:** Specify `0` to exclude content reviews. Defaults to `1`.
- **`filterReviews`:** Specify `1` to filter content reviews similar to status reviews. This is not recommended since content reviews are intended to be independent of devices, iOS versions and package versions. Filtering content reviews does not filter by device since a device isn't associated with content reviews. Defaults to `0`.