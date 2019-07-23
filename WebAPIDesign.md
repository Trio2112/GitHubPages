<a id="top_anchor"></a>
# Web Api Design
- [Attributes of a Good API](#attributes-of-a-good-api_anchor)
- [RESTful APIs (Representational State Transfer)](#restful-apis_anchor)
- [Resource-based Architectures](#resource-based-architectures_anchor)
- [Principles of URI Design](#principles-of-uri-design_anchor)
- [Versioning](#versioning_anchor)
<br/><br/>

## Attributes of a Good API<a id="attributes-of-a-good-api_anchor"></a> <sup><sub>[(back to top)](#top_anchor)</sub></sup>
- A good API should protect the server from the user and protect the user from the server. 
- A good API should not surprise the users
- A good API should balance what is good for the user and what is good for the server (eg caching aggressively and etags)
- A good API should be clear how to do different operations on the data. (HTTP verbs)
- A good API should be self-documenting and self-describing.

  Developers who are consuming the APIs should be able to see how to use them in a fairly simple way. As they look at what calls are available, it should be natural to take the next step and get more data or get more services in the application.
  
  For example, getting customers, then orders for those customers, then line items for those orders should be a natural progression through the API. It should be intuitive without having to go back and lookup for each call in the system how to get interrelated data.
<br/><br/>
## RESTful APIs (Representational State Transfer)<a id="restful-apis_anchor"></a> <sup><sub>[(back to top)](#top_anchor)</sub></sup>
  
- A common pattern for doing web APIs.
- Uses URI endpoints, but dictates that the URIs should be resource-based. (ie “customers” instead of “getCustomers”. The verbs are http verbs (put, post, get, delete).
- The server is stateless. The server shouldn’t be holding on to some state that we need to be remembered by.
- Content negotiation with the client determines the format the data comes back in (json, xml, etc).
- Hypermedia As The Engine Of Application State (HATEOAS) includes in the payload links to do other operations. Indicates to the user of the API other operations that can be done.
- Some people believe that REST as a philosophy isn’t super useful because it’s so dogmatic. The constraints it puts on a system to be blessed as “REST-based” becomes overwhelming and not useful. However, REST is important because it helps us dictate how we want to design those APIs.
- We can apply REST in a pragmatic way to help us design good APIs that stand the test of time and won’t have to change often in order to deal with new constraints.
- Concepts include:
  - Separation of Client and Server - The clients are going to call into the server based on URIs, and the server is going to try to meet those URI requests.
  - Server requests are stateless
  - Cacheable requests - caching of data results (get calls into a system)
  - Uniform interface - When someone comes up to our API they can get entities with the same pattern they used to get previous entities. The URIs look like each other.
- Problems
  - To become qualified “REST” tends to be difficult -- requires too much time to follow the letter of the law rather than the spirit of the law
  - Community is split about the dogma of REST
  - There is a lot to learn from REST-style architecture, but being pragmatic is important too.
<br/><br/>

## Resource-based Architectures<a id="resource-based-architectures_anchor"></a> <sup><sub>[(back to top)](#top_anchor)</sub></sup>

- Resources are representations of real-world objects or entities (nouns).
- Relationships between the resources are typically nested down a path of those resources. (ie /Customer/123/Orders/456/OrderItems/
- You should think of these as hierarchies of information, not necessarily as relational models, because the kind of data you are going to be dealing with is going to be hierarchical, not relational models like you would see in database tables.
- URIs are paths to resources.
- Querystrings are used for non-data elements. They don’t represent verbs. They can be used for different purposes like sorting or filtering.
<br/><br/>

## Principles of URI Design<a id="principles-of-uri-design_anchor"></a> <sup><sub>[(back to top)](#top_anchor)</sub></sup>

- Nouns are good, verbs are bad
- URIs should point at nouns
  - Prefer plurals
    - https://.../api/Customers
    - https://.../api/Games
    - https://.../api/Invoices
- Use identifiers to locate individual items in URIs.
  - Does not have to be an internal key. It can be something else but has to be unique for the item.
    - https://.../api/Customers/123
    - https://.../api/Games/halo-3
    - https://.../api/Invoices/2003-01-24
- Use HTTP verbs
  ![useful image](/assets/images/WebAPIDesign/image01.png)
  - **GET**: Retrieve a resource
  - **POST**: Add a new resource
  - **PUT**: Update an existing resource (updates the entire resource)
  - **PATCH**: Update an existing resource with set of changes (only updates certain fields of the resource)
  - **DELETE**: Remove the existing resource
- What should each verb return?
  ![useful image](/assets/images/WebAPIDesign/image02.png)
- Status codes to return
  - Typically a well-designed API returns 8 - 10 different status codes.
  - Use the following, at a minimum:
    ![useful image](/assets/images/WebAPIDesign/image03.png)
  - Other status codes that could be used:<br/>
    ![useful image](/assets/images/WebAPIDesign/image04.png)<br/>
    NOTE: 304 is used with caching to indicate the object you requested has not changed since you last requested it.
  - Here's a complete lising of status codes:
    ![useful image](/assets/images/WebAPIDesign/image05.png)
- Use URI navigation to get associated sub-objects:
  - Examples
    - https://.../api/Customers/123/Invoices
    - https://.../api/Invoices/2003-10-24/Payments
  - The following should return the same shape list of invoices:
    - https://.../api/Customers/123/Invoices
    - https://.../api/Invoices
- Anything more complex should use querystring
  - Examples
    - https://.../api/Customers?state=GA
    - https://.../api/Customers?state=GA&salesperson=144
    - https://.../api/Customers?hasOpenOrders=true
- Formatting Results
  - Content negotiation is a best practice
  - Use the Accept header to determine how to format. The Accept header can have a comma-delimited list of formats the client supports<br/>
    ![useful image](/assets/images/WebAPIDesign/image06.png)<br/>
  - Not necessary to support all
  - Have a sane default
  - Content/Mime Types<br/>
    ![useful image](/assets/images/WebAPIDesign/image07.png)<br/>
- ETags when making a request support caching
  - Within the 200 response is a header named ETag. The value is a version number of the object<br/>
    ![useful image](/assets/images/WebAPIDesign/image08.png)<br/>
  - The client can then send the ETag back to see if a new version is available. This is done using the If-None-Match header and part of the Get.<br/>
    ![useful image](/assets/images/WebAPIDesign/image09.png)<br/>
  - If a new version does not exist, then the server will return a 304 (Not Modified) status with an empty body.
- ETags when updating data support concurrency
  - When doing a put, use the If-Match header with the version number to only update the object if the given version number is the latest.<br/>
    ![useful image](/assets/images/WebAPIDesign/image10.png)<br/>
  - If a Put with the If-Match header fails, then the server returns a 412 status code (Precondition Failed). The updates doesn’t happen if the precondition fails.<br/>
    ![useful image](/assets/images/WebAPIDesign/image11.png)<br/>
- Paging
  - Lists should always support paging. Use query string parameters to request paging information.
  - The returned object wrapper can indicate next/prev links.<br/>
    ![useful image](/assets/images/WebAPIDesign/image12.png)<br/>
  - The returned wrapper can also indicate the page size.<br/>
    https://.../api/games?page=5&pageSize=50
- Getting Partial Items
  - Query string is a common pattern for requesting parts.<br/>
    https://.../api/games/123?fields=id,name,price,imageURL
- Updating Partial Items
  - Use the PATCH HTTP verb to update a partial (with ETag support).
  - The following updates a sub-set of the fields the belong to the object:<br/>
    ![useful image](/assets/images/WebAPIDesign/image13.png)<br/>
- Non-Resource APIs?
  - Be pragmatic and make sure that these parts of your API are documented
  - Be sure that the user can tell it is a different type of operation
  - Should be completely functional
  - Don’t use them as an excuse to build a RPC API
  - Example:
    - https://.../api/calculateTax?state=GA&total=149.99
    - https://.../api/restartServer?isColdBoot=true
    - https://.../api/executeOrder66?includeYounglings=true
<br/><br/>

## Versioning<a id="versioning_anchor"></a> <sup><sub>[(back to top)](#top_anchor)</sub></sup>

- Why version?
  - Publishing an API is not a trivial move
  - Users/customers rely on the API not changing
  - But requirements will change
  - Need a way to evolve the API without breaking existing clients
  - API Versioning isn’t Product Versioning -- try not to tie the two together!
  - API Commandment: **THOU SHALL NOT BREAK EXISTING CLIENTS!**
- There is no one, right way to version your API. What is right depends on your needs and situation.
- URI Path - Using part of your path to version
  - Example: https://.../api/v2/user
  - Allows you to drastically change the API in later versions. Everything below the version is subject to change.
  - One of the most common patterns.
  - Pro(s):
    - Simple to segregate old APIs for backwards compatibility
    - Easier to implement
  - Con(s):
    - Requires lots of client changes as you version (e.g. version # has to change in every client)
    - Increases the size of the URI surface are you have to support -- may be a larger amount of technical debt you have to continue to support
- URI Parameter
  - https://.../api/catalog/titles/series/70023522?v=1.5
  - The version is passed as an optional querystring parameter, with the default being the latest version.
  - Pro(s):
    - Without version, users always get the latest version of API
    - There is little client change as the versions mature, if the client does not pass a specific version number (and relies on the default).
    - Easier to implement
  - Con(s):
    - Can surprise developers with unintended changes
    - Can mean more technical debt to the backend of your project
- Content Negotiation (version is included in the Accept Header)
  - Content Type: application/vnd.mycompany.**1**.param+json
  - Instead of using a standard MIME type in the Accept Header of the request, use a custom type/value.<br/>
    ![useful image](/assets/images/WebAPIDesign/image14.png)<br/>
  - Can include information in Accept Header for formatting too:<br/>
    ![useful image](/assets/images/WebAPIDesign/image15.png)<br/>
  - This type of versioning is becoming increasingly popular because the versioning is separate from the surface area of the API itself.
  - Alternatively, you can create your own MIME type. The standard indicates you can begin the value with “vnd” (meaning vendor).<br/>
    ![useful image](/assets/images/WebAPIDesign/image16.png)<br/>
  - Pro(s):
    - The API and resource versioning are packaged together
    - Removes versioning from API, so clients don’t have to change
  - Con(s):
    - Adds complexity - adding headers isn’t easy on all platforms
    - Can encourage increased versioning which causes more code churning
- Request Header (version is included in header)
  - x-ms-version: 2011-08-18
  - Should be a header value that is only of value to your API
  - Should begin with “x-”. That’s a name that most routers are going to ignore (indicates a user defined header value)<br/>
    ![useful image](/assets/images/WebAPIDesign/image17.png)<br/>
  - Common to use API Date instead of a number. What you use is up to you.<br/>
    ![useful image](/assets/images/WebAPIDesign/image18.png)<br/>
  - Pro(s):
    - Separates versioning from API call signatures
    - Not tied to resource versioning (e.g. Content Type)
  - Con(s):
    - Adds complexity - adding headers isn’t easy on all platforms
- Which to choose?
  - Versioning with Content Negotiation and Custom Headers are popular now
  - Versioning with URI Components are more common
  - Good recommendation: Start with URI Component versioning and see if it leads to too much technical debt. If it does, switch to something like Content Negotiation or Custom Headers.
  - Remember to version your API from the very first release! Make this mandatory.
  - Be sure to pick a versioning option that best matches the maturity level of your team and users.
  - Be pragmatic. Version “just enough”.

  

