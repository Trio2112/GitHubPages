# Web Api Design

## Attributes of a Good API
- A good API should protect the server from the user and protect the user from the server. 
- A good API should not surprise the users
- A good API should balance what is good for the user and what is good for the server (eg caching aggressively and etags)
- A good API should be clear how to do different operations on the data. (HTTP verbs)
- A good API should be self-documenting and self-describing.

  Developers who are consuming the APIs should be able to see how to use them in a fairly simple way. As they look at what calls are available, it should be natural to take the next step and get more data or get more services in the application.
  
  For example, getting customers, then orders for those customers, then line items for those orders should be a natural progression through the API. It should be intuitive without having to go back and lookup for each call in the system how to get interrelated data.
  
## RESTful APIs (Representational State Transfer)
  
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

## Resource-based Architectures

- Resources are representations of real-world objects or entities (nouns).
- Relationships between the resources are typically nested down a path of those resources. (ie /Customer/123/Orders/456/OrderItems/
- You should think of these as hierarchies of information, not necessarily as relational models, because the kind of data you are going to be dealing with is going to be hierarchical, not relational models like you would see in database tables.
- URIs are paths to resources.
- Querystrings are used for non-data elements. They don’t represent verbs. They can be used for different purposes like sorting or filtering.

## Principles of URI Design

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
