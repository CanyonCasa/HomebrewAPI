# HomebrewAPI

#### Keywords:	Homebrew, API

## Application Programming Interface to the Homebrew Tools

**_By Dave Campbell**_

### Abstract

_HomebrewAPI_ defines the application programming interface (API) to the Homebrew family of tools, principally HomebrewLite. 

### HomebrewAPI

#### Apache (.htacess) Setup

#### Authentication

_HomebrewAPI_ authentication uses the _Basic HTTP Authentication_ header for requests defined as 

>>**Authentication: Basic base64_encoded(user:password)** <sup>1,2</sup>

Every GET/POST request generally requires authentication, but depends on defined API recipes, 

A successful authentication will POST the requested source (unless an error occurs). A failed authentication will have a status code 200, OK, but will return a JSON error object containing a code and message, such as {"code": 401,"msg": "Unauthorized"}. Note, the _fetch_ promise response (from FetchWrapper.js) includes a _jxOK_ value that can test the validity of responses and the response jx field contains the recovered JSON output.

Authentication supports two forms: multi-user (i.e. shared token) and single-user (i.e. user specific credentials), with individual permissions possible in both cases. Verification first trys to match the user specific password. On failure of the user password, it will try to match the _authAs_ password, if specified in the cfg.json file (discussed below).

>>**Authentication: Basic base64_encoded(token:password)** <sup>3</sup>
 
##### Authentication Notes: 
 
  1. <span style="color:red">_HomebrewAPI_ uses the _Basic_ authentication method, which passes clear text credentials in the request header. Therefore, it requires use of _https_ access (instead of _http_) for credentials security.</span>
  
  2. <span style="color:red">_HomebrewAPI_ assumes server support for **Bcrypt** password encryption to authenticate developer and author uploads.</span>

  3. <span style="color:red">Multi-user mode does not ensure absolute security as it does not prevent a user from "spoofing someone else"! It is intended for easy management of a small group of trusted users, such as a family or small business employees.</span>

  4. <span style="color:red">IoT devices utilize an API key and auth code handled same as user:pw, i.e. key:auth_code</span>

#### Cross Origin Requests (CORS)

_HomebrewAPI_ handles cross-origin requests between multiple sites as defined in the _cfg.json_ file.

### Special Server Operations

The server-side implements handlers for fullfilling client requests. HomebrewLite includes 3 handlers by default outlined below:

- **Data management**
- **Action management**
- **Info management**

### Changes

- Initial release.

### To Do

- 