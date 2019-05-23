# HomebrewAPI

#### Keywords:	Homebrew, API

## Application Programming Interface to the Homebrew Tools

**_By Dave Campbell**_

### Abstract

_HomebrewAPI_ defines the application programming interface to the Homebrew family of tools. 

### HomebrewAPI

#### Apache (.htacess) Setup

#### Authentication

_HomebrewCMS_ authentication uses the _Basic HTTP Authentication_ header for requests defined as 

>>**Authentication: Basic base64_encoded(user:password)** <sup>1,2</sup>

Every GET/POST request requires authentication, with the exception of form submissions to blind folders! (See examples for more details on implemting and appropriate REST interface.)

A successful authentication will POST the requested source (unless an error occurs). A failed authentication will have a status code 200, OK, but will return a JSON error object containing a code and message, such as {"code": 401,"msg": "Unauthorized"}. Note, the _fetch_ promise response includes an _jxOK_ value that can test the validity of responses and the response jx field contains the recovered JSON output.

Authentication supports two forms: multi-user (i.e. shared token) and single-user (i.e. user specific credentials), with individual permissions possible in both cases. Verification first trys to match the user specific password. On failure of the user password, it will try to match the _authAs_ password, if specified in the cfg.json file (discussed below).

>>**Authentication: Basic base64_encoded(token:password)** <sup>3</sup>
 
##### Authentication Notes: 
 
  1. <span style="color:red">NOTE: _HomebrewCMS_ uses the _Basic_ authentication method, which passes clear text credentials in the request header. Therefore, it requires use of _https_ access (instead of _http_) for credentials security.</span>
  
  2. <span style="color:red">NOTE: _HomebrewCMS_ assumes and requires client and server support for **Bcrypt** password encryption to authenticate developer and author uploads.</span>

  3. <span style="color:red">NOTE: Single-user mode does not ensure absolute security as it does not prevent a user from "spoofing someone else"! It is intended for easy management of a small group of trusted users, such as a family or small business employees.</span>

#### Cross Origin Requests (CORS)

_HomebrewCMS_ assumes the use of two websites, the actual "live" site and an alternate "preview" site. The tool automatically handles cross-origin requests between the two as defined in the _cfg.json_ file.

### Special Server Operations

The server-side "data emulation script", **$.php** or **cms.js**, exercises a few special operations.

<span style="color:red">TBD</span>

- **Data management**
- **History Management**:
- **Index Management**:

### HomebrewCMS Files and Folders Description

_HomebrewCMS_ involves only a few required files for easy management and learning curve. It assumes all these reside in the same directory on the server, **/cms** by default.

- **cms.html**: A VueJS HTML template for the content management tool, i.e. the user interface. This tempate directly depends on VueJS 2.x and the following modules.<sup>1,2</sup>
- **cmsVueFetchWrapper.js**: A Vue plugin that adds the "fetch" method directly to Vue, optimized for JSON communication with built-in preprocessing of responses.
- **cmsVueModel.js**: Module of JavaScript code that declares the Vue Instance referenced in _cms.html_ and its top-level methods and properties.
- **cmsVueLib.js**: A JavaScript library to modularize the CMS Vue components to cleanup the _cms.html_ and _cmsVueModel.js_ files.
- **schema.js**: Script to define base configuration and root schema definitions. Only used by the cms.html file, loaded by a script reference in the head of that file.
- **cms.css**: Stylesheet for the _cms.html_ file.
- **cmsExtensions2Client.js**: A library of personal JavaScript extensions used throughout the other JavaScript code.
- **$.php** (Apache) or **cms.js** (NodeJS): Server-side database emulation script to upload and manage data, image, and schema files. See _Special Server Operations_ for function details.

Additionally, operation involves site-specific content files, including:

- **cfg.json**: Data file used to override configuration defaults for site specific customization. See _example_cfg.json_ for configuration details. If used, place this file in the _/cms_ folder.
- **credentials.json**: User credentials file in JSON format. <span style="color:red">NOTE: FOR SECURITY, DO NOT place this file under the document root, which includes the _/cms_ folder.</span>
- **HTML templates**: HTML files built with a client-side templating framework such as VueJS or Angular for site pages. The master index.html file should be placed in the document root folder, but others may be organized as appropriate.
- **JSON _schema_ and _data_ files**: Site specific content structure definition and data files in JSON format for the respective templates. HomebrewCMS manages these files in the _/schema_ and _/data_ folders respectively.

_HomebrewCMS_ assumes the following server directory structure for files, which may be overridden using the _cfg.json_ file:<sup>3,4,5</sup>

- **cms**: Recommended folder for content management files, to not clutter root folder. (Blocked from uploads for security.)
- **data**: Folder of JSON data files that define content for individual pages and page sections, referenced by cms.html, $.php or _cms.js_ script, and respective HTML templates.
- **docs**: Folder where uploaded documents get stored, referenced by cms.html, $.php or _cms.js_ script, and respective HTML templates.
- **images**: Folder where uploaded images get stored, referenced by cms.html, $.php or _cms.js_ script, and respective HTML templates.
- **scripts**: Folder of scripts referenced by site pages, if desired. Note: the CMS scripts noted above should always reside in the _/cms_ folder.  (Blocked from uploads for security.)
- **schema**: Folder of "database" schema referenced only by _cms.html_ and _$.php_ or _cms.js_ script.
- **styles**: Folder of site CSS stylesheets for site.

##### Files and Folders Notes

  1. The CMS has external dependencies for VueJS 2.x, referenced by a link in head section of _cms.html_, as well as the **Font Awesome 5 Free** library for icons, also referenced by a link in _cms.html_.
  
  2. For local server development, a static copy of the external dependencites resides in the _cdn_ folder that may be referenced instead of the external paths.
  
  3. All uploads to these folders using _HomebrewCMS_ require authentication, as noted above.
  
  4. For security, _HomebrewCMS_ only uploads files to "configured" folders, not to configured locations.
  
  5. Any of these folders may be renamed or moved as desired, using the _cfg.json_ file. Any hardcoded references in templates and data files must be changed, respectively.

### Changes

- Initial release.

### To Do

- Complete $.php Apache script and cms.js NodeJS middleware
- Example (barebones) site schema and templates.
- User management support
- Image and document upload.
- keywords and indexing (JXV) file definition.
- Publishing.
- Minimization support.