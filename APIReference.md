# TweakReviewsDB API v3

**API Base**: https://api.pixelomer.com/tweakDB

This document contains endpoints for TweakReviewsDB API v3. TweakReviewsDB is an app for tweak content and compatibility reviews.

**NOTE:** This API is still in development and it is not yet stable. Breaking changes can and will be made.

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
      "stars": 5,                   // {1...5}
      "username": "john.smith",     // username
      "tags": [ "tweakreviewsdb" ], // list of tags
      "content": "Great tweak!",    // review content
      "version": "1.2.1",           // package version
      "ios": "14.0.1"               // iOS version
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

The `tags` contains zero or more tags about the reviewer and the source of the review.
- **`verified`**: This review comes from a trusted person. This tag is given to developers and other important people in the jailbreak community.
- **`tweakcompatible`**: This review comes from [tweakCompatible](https://github.com/jlippold/tweakCompatible) which is a deprecated platform for tweak reviews.
- **`apptapp`**: This review comes from AppTapp Installer 5.
- **`tweakreviewsdb`**: This review comes from TweakReviewsDB itself.

There also tags that are not used in the API yet. These tags may be added in the future.
- **`moderator`**: This review comes from a moderator of TweakReviewsDB.
- **`postbox`**: This review comes from [PostBox](https://getpostbox.now.sh/).

Additional URL parameters can be specified in `GET` requests.
- **`includeStatus`**: Specify `0` to exclude compatibility reviews. Defaults to `1`.
- **`includeReviews`**: Specify `0` to exclude content reviews. Defaults to `1`.
- **`filterReviews`**: Specify `1` to filter content reviews similar to status reviews. This is not recommended since content reviews are intended to be independent of devices, iOS versions and package versions. Filtering content reviews does not filter by device since a device isn't associated with content reviews. Defaults to `0`.
- **`keys`**: Comma separated list of keys. When this list is specified, only the specified keys will be included in content review data. This value is ignored when `includeReviews` is `0`.
