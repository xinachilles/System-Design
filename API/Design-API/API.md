
---

# Rest API:

## Key Concepts
## State Transfer

- REST (Representational State Transfer) is a stateless communication method between client and service
- No need to track state over time
- Each request contains all necessary information

## Evolution from Traditional Methods
- Traditional software used procedure calls
- SOAP (Simple Object Access Protocol) is procedure-based
- REST changed the paradigm from procedures to resources

## Resource-Based Approach
- Resources are accessible via URLs
- Resources can be accessed through web browser address bars
- Leverages built-in HTTP protocol methods (GET, POST, PUT, DELETE, etc.)

## Key Differences
**Old Approach**: Procedure-oriented (like SOAP)

 **REST Approach**: Resource-oriented

- Focuses on resources rather than procedures
- Uses standard HTTP methods
- Stateless communication
- URL-based resource access

## Side effect:

We can do the same operation over and over. I am not going to see different result each time. For example, when you get some from rest endpoint, you should expert to be the same except some come to modify behind the screen.

## Data Format

- REST primarily uses JSON instead of XML
- JSON advantages:
  - Designed with JavaScript compatibility in mind
  - Easier for browser/client code to process
  - Smaller data size compared to XML

## Resource Identification
- Resources are represented by URLs
- Resources should be accessible via browser
- Follows noun-based resource naming
- HTTP methods (verbs) define actions on resources
  
## HTTP Methods

- GET: Download/retrieve data
  
- PUT: Modify existing data.The put is replacement that when I am using for update.
  
- POST: Create new data. The idea of post is we posting something new to the collection.  
   - For post, what should be returned. There is a design question behind the API weather you return the product back with the ID populated or just say ok I got the post
  
- Patch: Just modify the part of the object. Maybe you can take partial representation
  
## API Design Considerations
- Focus on business logic and application expertise
- Design for resource accessibility
- Consider how consumers will interact with the API

## Example: Twitter API
- Uses collections (e.g., "status")
- Resources include:
    - User timeline
    - Home timeline

## Key Differences from RPC
- REST is resource-oriented (nouns)
- RPC is procedure-oriented (verbs added to nouns)
- REST uses standard HTTP methods for actions
- Resources are directly accessible via URLs

## How the resource are represent.

- Consider the transaction initiator's perspective
- Balance between:
  - Business logic exposed to API consumers
  - Service-managed business logic
- Focus on your core business expertise
 
## HATEOAS (Hypermedia as the Engine of Application State)
- API provides all necessary information in responses
- Clients can discover available actions
- Example: GitHub repository API
  - Single call provides complete repository information
  - Includes related resources (followers, etc.)
  - Eliminates need for multiple API calls

## Performance Optimization
- Stateless Design
  - No state maintenance required
  - Each request contains complete information
  - Client provides all necessary data per call
- Round Trip Avoidance
  - Minimize HTTP calls
  - Better to send more data in one call than multiple smaller calls
  - Reduces latency and improves performance
  - Prefer comprehensive responses over multiple small requests


# REST API URL Routing and Controller Action Resolution
## URL Template Matching
- URLs must map to specific segments
- Templates must be unambiguous
- Each URL should have a clear, unique match
  
 Let me explain URL templates in REST APIs with clear examples.

# URL Templates in REST APIs

## Definition
A URL template is a pattern that defines how URLs should be structured to access different resources in your API. It's like a blueprint for your API endpoints.

## Basic Structure
```
https://api.example.com/{resource}/{id}
```

## Common URL Template Patterns

### 1. Simple Resource Access
```
GET /users              // Get all users
GET /users/{id}         // Get specific user
POST /users             // Create new user
PUT /users/{id}         // Update specific user
DELETE /users/{id}      // Delete specific user
```

### 2. Nested Resources
```
GET /users/{userId}/posts           // Get all posts by a user
GET /users/{userId}/posts/{postId}  // Get specific post by a user
```

### 3. Query Parameters
```
GET /users?role=admin              // Get users with role=admin
GET /posts?category=tech&limit=10  // Get tech posts with limit
```

### 4. Versioning
```
GET /v1/users
GET /v2/users
```

## Best Practices

1. **Use Nouns, Not Verbs**
   - ✅ `/users` (good)
   - ❌ `/getUsers` (avoid)

2. **Use Plural Nouns**
   - ✅ `/users` (good)
   - ❌ `/user` (avoid)

3. **Use Hyphens for Multi-word Resources**
   - ✅ `/user-profiles` (good)
   - ❌ `/userProfiles` (avoid)

4. **Use Lowercase Letters**
   - ✅ `/users` (good)
   - ❌ `/Users` (avoid)

5. **Avoid File Extensions**
   - ✅ `/users` (good)
   - ❌ `/users.json` (avoid)

## Example URL Template Structure
```
https://api.example.com/v1/users/{userId}/posts/{postId}/comments
```

This template shows:
- API version (`v1`)
- Resource hierarchy (`users` → `posts` → `comments`)
- Resource identifiers (`{userId}`, `{postId}`)

## Common HTTP Methods with URL Templates
```
GET    /users/{id}      // Retrieve
POST   /users           // Create
PUT    /users/{id}      // Update
PATCH  /users/{id}      // Partial update
DELETE /users/{id}      // Delete
```

## Benefits of Good URL Templates
1. **Predictability**: Developers can guess the URL structure
2. **Consistency**: Follows a standard pattern
3. **Scalability**: Easy to add new resources
4. **Maintainability**: Clear and organized structure
5. **Documentation**: Self-documenting API structure

Remember: A good URL template should be:
- Intuitive
- Consistent
- Hierarchical
- Resource-oriented
- Easy to understand and use
 

## Controller Action Resolution Process
1. Controller Selection
  - First identifies the controller segment

I'll explain the controller segment identification with clear examples.

# Controller Segment Identification in REST APIs

## What is a Controller Segment?
The controller segment is the part of the URL that identifies which controller will handle the request. It's typically the first major part of the URL after the domain.

## Examples

### 1. Basic Controller Segments
```
https://api.example.com/users/123
                    ^^^^^
                    Controller Segment (UsersController)

https://api.example.com/products/456
                    ^^^^^^^^^
                    Controller Segment (ProductsController)
```

### 2. Nested Controller Segments
```
https://api.example.com/users/123/orders
                    ^^^^^
                    Controller Segment (UsersController)
                           ^^^^^^
                           Sub-resource (orders)

https://api.example.com/departments/IT/employees
                    ^^^^^^^^^^^^
                    Controller Segment (DepartmentsController)
                               ^^^^^^^^^
                               Sub-resource (employees)
```

### 3. Real-world Examples

#### Blog API
```
https://blog-api.com/posts/123/comments
                    ^^^^^
                    Controller Segment (PostsController)

https://blog-api.com/categories/tech/posts
                    ^^^^^^^^^^
                    Controller Segment (CategoriesController)
```

#### E-commerce API
```
https://shop-api.com/products/789/reviews
                    ^^^^^^^^^
                    Controller Segment (ProductsController)

https://shop-api.com/customers/456/orders
                    ^^^^^^^^^^
                    Controller Segment (CustomersController)
```

## How It Works

1. **URL Parsing**
   ```
   https://api.example.com/users/123
   ```
   - First segment after domain: `users`
   - Maps to: `UsersController`

2. **Controller Resolution**
   ```
   https://api.example.com/products/456/reviews
   ```
   - First segment: `products`
   - Maps to: `ProductsController`
   - Subsequent segments: `456` (ID) and `reviews` (sub-resource)

3. **Default Controller**
   ```
   https://api.example.com/
   ```
   - If no segment specified, uses default controller (often `HomeController`)

## Best Practices

1. **Clear Naming**
   - Use plural nouns for controllers
   - Example: `UsersController` not `UserController`

2. **Consistent Structure**
   ```
   /users/{id}           // UsersController
   /products/{id}        // ProductsController
   /categories/{id}      // CategoriesController
   ```

3. **Hierarchical Organization**
   ```
   /departments/{id}/employees
   /users/{id}/orders
   /products/{id}/reviews
   ```

## Common Controller Patterns

1. **Resource Controllers**
   ```
   /users              // UsersController
   /products           // ProductsController
   /orders             // OrdersController
   ```

2. **Feature Controllers**
   ```
   /auth               // AuthController
   /search             // SearchController
   /reports            // ReportsController
   ```

3. **Versioned Controllers**
   ```
   /v1/users           // V1.UsersController
   /v2/users           // V2.UsersController
   ```

Remember:
- Controller segments should be clear and descriptive
- They should follow a consistent naming convention
- They should reflect the resource hierarchy
- They should be intuitive for API consumers



  - Can be specified explicitly in template
  - Falls back to default controller if not specified
  - Controller name is appended to the end of the URL
   
2. Action Resolution
   - After finding controller, looks for **action**
   - Applies rules in specific order
   - Uses HTTP methods to match actions
3. Action Matching Rules (in order)   
   1. HTTP Method Attributes
     - Explicit attributes like [HttpPut], [HttpGet]
     - Direct mapping to HTTP verbs
   2. Method Name Convention
      - Methods starting with HTTP verbs (e.g., Get, Post)
      - Conventional naming pattern matching
   3. Parameter Matching
      - If no clear match found, looks at method parameters
      - Matches based on parameter compatibility
      - Selects method with most matching parameters

## Key Points
- Controller focuses on parameter matching rather than method names
- Action resolution is based on:
  - HTTP method attributes
  - Method naming conventions
  - Parameter compatibility
- System prioritizes explicit HTTP method attributes over naming conventions

## 4 

1. **200 (OK)**
   - Used for successful operations
   - Can return data or nothing

2. **201 (Created)**
   - Used when a new resource is created
   - Can return the created item or include a Location header
   - Useful for resource creation endpoints

3. **204 (No Content)**
   - Used when operation succeeds but there's nothing to return
   - Common for successful operations that don't need to return data

4. **400 (Bad Request)**
   - Indicates invalid request data
   - Should include validation details in the response
   - Important for client-side error handling

5. **401 (Unauthorized)**
   - Used when the client's identity is unknown
   - Basic authentication failure

6. **403 (Forbidden)**
   - Used when client is authenticated but lacks permission
   - Different from 401 as the server knows who the client is

7. **404 (Not Found)**
   - Used when requested resource doesn't exist
   - Common in DELETE operations when resource isn't found

### Key Design Principles:
1. Use standard HTTP status codes for better client integration
2. Clients expect specific status codes (like 400) to trigger error handling
3. Validation should focus on checking for valid values rather than invalid ones
4. The list of invalid values is infinite, so it's better to validate against a finite set of valid values
5. Status codes help clients handle errors appropriately through their error handling mechanisms

This approach makes APIs more maintainable and easier to integrate with, especially for external clients who are familiar with standard HTTP status codes.


## Security
 
1. **Response Structure**
   - When using GET methods, you can return either:
     - A direct object
     - An HTTPResult wrapper
  
Let me explain what an HTTPResult wrapper is:

An HTTPResult wrapper is a standardized response structure that encapsulates both the data and metadata about the HTTP response. It's a common pattern in API design that provides a consistent way to handle responses across your application.

```typescript
interface HTTPResult<T> {
    status: number;        // HTTP status code
    data?: T;             // The actual response data
    message?: string;     // Optional message
    errors?: any[];       // Any error details
    headers?: {           // Custom headers
        [key: string]: string;
    };
}
```


1. **HTTPResult vs Direct Object**
   - **HTTPResult Advantages**:
     - More flexible for handling different response types
     - Can easily modify status codes
     - Better control over serialization
     - Cleaner error handling
     - More readable code structure

   - **Direct Object Disadvantages**:
     - Less flexible
     - Requires manual response object manipulation
     - More complex error handling
     - Can make code less readable

2. **Security Considerations**
   - When returning objects, you need to consider message signatures
   - Response headers can be used to set status information
  
I'll show you examples of how response headers can be used to set status information in different contexts:

1. **Basic HTTP Response Headers Example**:
```typescript
// Express.js example
app.get('/api/users', (req, res) => {
    res.setHeader('X-Request-Status', 'success');
    res.setHeader('X-Response-Time', '100ms');
    res.status(200).json({ users: [] });
});
```

2. **Pagination Information in Headers**:
```typescript
// Example showing pagination metadata in headers
app.get('/api/posts', (req, res) => {
    const totalItems = 100;
    const pageSize = 10;
    const currentPage = 1;
    
    res.setHeader('X-Total-Count', totalItems.toString());
    res.setHeader('X-Page-Size', pageSize.toString());
    res.setHeader('X-Current-Page', currentPage.toString());
    res.setHeader('X-Total-Pages', Math.ceil(totalItems/pageSize).toString());
    
    res.status(200).json({ posts: [] });
});
```

3. **Rate Limiting Information**:
```typescript
// Example showing rate limit information
app.get('/api/resource', (req, res) => {
    res.setHeader('X-RateLimit-Limit', '100');
    res.setHeader('X-RateLimit-Remaining', '99');
    res.setHeader('X-RateLimit-Reset', '1625097600');
    
    res.status(200).json({ data: {} });
});
```

4. **Cache Control Headers**:
```typescript
// Example showing cache control
app.get('/api/static-data', (req, res) => {
    res.setHeader('Cache-Control', 'max-age=3600');
    res.setHeader('ETag', '"123456789"');
    res.setHeader('Last-Modified', 'Wed, 21 Oct 2023 07:28:00 GMT');
    
    res.status(200).json({ data: {} });
});
```

5. **Custom Status Information**:
```typescript
// Example showing custom status information
app.post('/api/process', (req, res) => {
    const processId = '12345';
    const status = 'processing';
    
    res.setHeader('X-Process-ID', processId);
    res.setHeader('X-Process-Status', status);
    res.setHeader('X-Process-Start-Time', new Date().toISOString());
    
    res.status(202).json({ message: 'Processing started' });
});
```

6. **Error Information in Headers**:
```typescript
// Example showing error information in headers
app.get('/api/resource', (req, res) => {
    res.setHeader('X-Error-Code', 'VALIDATION_ERROR');
    res.setHeader('X-Error-Details', 'Invalid input parameters');
    res.setHeader('X-Error-Timestamp', new Date().toISOString());
    
    res.status(400).json({ error: 'Bad Request' });
});
```

7. **API Version Information**:
```typescript
// Example showing API version information
app.get('/api/data', (req, res) => {
    res.setHeader('X-API-Version', '1.0.0');
    res.setHeader('X-API-Deprecated', 'false');
    res.setHeader('X-API-Sunset-Date', '2024-12-31');
    
    res.status(200).json({ data: {} });
});
```

These headers can be useful for:
- Providing metadata about the response
- Implementing pagination
- Managing rate limiting
- Controlling caching
- Tracking process status
- Handling errors
- Version control
- Performance monitoring

The key advantage of using headers for status information is that it separates metadata from the actual response data, making it easier for clients to process both independently. It's also a standard way to provide additional context about the response without modifying the response body structure.



   - The structure of your response can impact security handling

3. **Best Practice**
   - Using HTTPResult is recommended over direct object returns
   - Provides better control over response handling
   - Makes the code more maintainable and secure
   - Allows for more flexible error handling and status code management

The main takeaway is that using an HTTPResult wrapper provides more flexibility and better control over API responses, making the code more maintainable and secure compared to returning direct objects.



Authentication and authorization

Authentication is who are you ; what is your identify I am know to system or not;

Authorization is who you allow to do . what's my permission.



Why I want Facebook authorization on my APP. There are couple of different reason. The bigest part of it is because it'sgonato be much similar to user. The trends are your usergonnahave F account or T account. Those 4 providers. Igonnamake up of this status on top of my head like 87% or other stats made up on thespot I am willing to bet those4 providers probably cover 99.99% of population going to be using your application. So by allowing them user name and password, they currently have ? you are moving abarrier. Honestly a lot of two. I got to say ?? I would look to say sign up for brand-new user nameand new password . I just go to I am not interesting. You are removing thatbarrier and allow people to log in usercredentials they already have.

The another reason I like to user Fauthorization is because there is less security data for me to have to worry about. The reason way to security datais doesn't store it.

So come to F or come to T will all i get is token. That if turn out my database get hack, all the token are released. It's very easy I just go back to F invalidate those token. Problem solve. I don't need worry aboutexposing someone else password. if you use password cross multiple site. You really kind ofopeningyourself up to that. I don't want one of responsible for exposing out your password which the same password that I have in my luggage . I don't want to worry about it I just want to hold on the token,

Authorized attribution. The action will not run unless we have ??untlfor the individual they areauthorized

[if you got the API token and secrets, you can pass them right into the developer API configuration, and then it just know it can handle that type of login]{.mark}



Overrwirtethesyntry to get the user name out of the header. If it is existed then I'm create anidentifyusing ?? you can create a customeridentifytoo. There are a lot offlexablearound it passing in who is user and I'm set theprinclablethere and Http context



token base Authentication

we you give permission to your app,

your app and it says this app would

like to Tweet on your behalf and post Facebook updates without

you knowing and things like that .



It protects revealing the password to the app, so when it's generated

by a third party, I never get the password. I just get the token, and that's all I'm dealing with.



Token is passed in the header



it's going to be through a secured sockets layer ((SSL) and managed

by your code



encrypted in SSL



cross-site request forgery.



Yes. This is kind of a big thing where somebody could potentially simulate my form and be sending in a request from somewhere else that I'm not expecting.

That's a problem.



attack takes advantage of the fact that you may not be testing where your form posts came from, so I stand up a page





on my site, I take your information and make it look like we're on a good site, but then, under the covers, I might be setting some parameters like delete user

or something that you don't want to happen, and then I'm posting

it to the site that's the





ultimate site we're going to. And

that site is just looking for



00:46:23.880 --> 00:46:26.690

a post. It's not necessarily looking

where it's coming form,











lesson 6



/stand/{name} or / stand/name



I'm gonna create a placeholder for name; so that rather than



you always having the same name equals... name = that you can just simply send it as part of the path.



we can pass the name from the body



but you only can have one parameters was passed from body and multiple from URL



Model Bind





a very explicit way to inform the system how you want to deal with

certain types of complex objects.









































![](../../media/API-Design-API-API-image1.png){width="0.22916666666666666in" height="0.5833333333333334in"}![](../../media/API-Design-API-API-image2.png){width="0.22916666666666666in" height="0.5833333333333334in"}


