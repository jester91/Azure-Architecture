## Azure-Architecture
![image](https://github.com/jester91/Azure-Architecture-/assets/50679897/bb0a6370-836f-41ee-ada8-68d60cebf91f)



### High-Level Architecture Overview:
Components and Communication:
- **Azure Front Door**: Serves as the initial entry point for all client requests. It provides global load balancing and SSL offloading, ensuring that requests are handled efficiently and securely.

- **Azure Application Gateway**: Acts as a regional load balancer and is configured for URL-based routing, SSL termination, and session affinity. It helps in distributing traffic within the Azure region to various backend services.

- **Azure API Management**: Manages, secures, and optimizes API calls. It routes authenticated API calls to the correct backend services, and it is capable of handling rate limiting and caching to enhance API responsiveness.

- **Azure Entra ID**: Provides authentication services for both the UI and API layers, ensuring that only authenticated users can access the system's functionalities.

- **Azure Functions**: Used for processing resource-intensive tasks (like processing large transaction files). It interacts with Azure Blob Storage for storing and retrieving large files.
  
- **Azure Queue Storage**: Used for processing small tasks (like email sending). It's serverless, able to scale based on demand and trigger-based execution. 

- **Azure Blob Storage**: Manages the storage of large, unstructured data files that need to be processed by Azure Functions.

- **Azure Key Vault**: Secures application secrets, connection strings, and other sensitive information, ensuring they are not hard-coded or exposed within application code.

### Data Storage:
- **Azure Blob Storage**:  stores large, unstructured files, primarily used by Azure Functions for processing large data sets.
- **Secrets and Keys**: Managed by Azure Key Vault, enhancing security by centrally managing encryption keys and secrets.
  
## Pros and Cons of the Architecture

### Pros:
- **Scalability**: The use of Azure Front Door and Application Gateway allows the system to handle high loads and scale based on demand.
- **Security**: Integrating Azure Active Directory and Azure Key Vault provides robust authentication and secure management of secrets and keys. API Management adds an extra layer of security for API endpoints.
- **Performance**: Azure Functions offer a serverless computing environment that scales automatically, making it cost-effective and efficient for processing varying workloads.
- **Data Integrity and Accessibility**: Azure Blob Storage provides high durability and global accessibility, ensuring data is safe and always accessible.
### Cons:
- **Complexity**: Managing multiple Azure services together increases complexity in configuration and monitoring.
- **Cost**: Utilizing several managed services can increase operational costs, particularly if not managed and scaled appropriately.
- **Cold Start for Azure Functions**: Serverless functions can experience latency due to cold starts, especially if they are not frequently invoked.

## Pros and Cons of the used resources:

- **Azure Front Door**:
  - **PRO**: WAF(Web application firewall) which is a a built-in - **PRO**tection against attacks, Load balacing
  - **CON**: Complex - **CON**figuration for this small application. 
- **Azure Application Gateway**:
  - **PRO**: URL-based routing, session affinity
  - **CON**: Costly for small traffic load.

- **Azure API Management**:
  - **PRO**: Orchestrator, offer throttling, secure access, and OAuth  with Azure Entra. 
  - **CON**: Complex policies, the best practices to handle it in terraform. 
- **Azure Entra ID**:
  - **PRO**: It makes easier to authenticate via APIM. 
  - **CON**: Feature limitation for different tiers. 

- **Azure Functions**:
  - **PRO**: Serverless execution based on call, cost effeciency.
  - **CON**: Cold start which takes some to start the code, same issue for upscaling.
  
- **Azure Queue Storage**:
  - **PRO**: Scalable, cost-effective for smaller tasks.
  - **CON**: Throughput can be limited for extreme high requests. 
- **Azure Blob Storage**:
  - **PRO**: Scalability, easy to scale and store large amount of unstructured data. Cost effective. 
  - **CON**: Access latency based on tier. 

- **Azure Key Vault**:
  - **PRO**: Increase security, help to achieve compliance requirements. 
  - **CON**: Complexity for key rotation. Automation is needed. 

### Alternatives and Considerations:
- **Azure Service Fabric or Kubernetes (AKS)**: Instead of Azure Functions, for applications requiring more control over the environment and persistent services, Azure Kubernetes Service (AKS) or Azure Service Fabric could be used. These services offer better control over containers and microservices, although they come with increased complexity in management.

- **Azure Traffic Manager**: Instead of Azure Front Door if the application does not require advanced WAF or global routing features. Traffic Manager is simpler and may reduce costs but lacks some of the advanced capabilities of Front Door.

- **Direct Use of Azure Blob Storage from the API**: Bypassing Azure Functions for certain types of file manipulations might reduce latency and complexity, though it might increase the load and complexity on the API layer.
