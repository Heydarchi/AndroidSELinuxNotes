# Android Security Model

### There're 2 type of process classes:
<br/>

#### - System component services
    - declared in the system init scripts
    - Highly priveleged
    - Never share a UID with another process ( either root or the owner of many sensitive kernel objects)
    - It can be a native or non-native code

#### - Applications
    - The UID will be assigned automatically at installed time by the system
    - The package manager is in charge of assigning the new UIDs to the applications
    - These UIDs don't provide any specific capabilities or accessing to ant sensitive system resources
    - Then for any access the process should be added to the appropriate group (GID)
    - The Android permissions will be mapped to supplementary GIDs by the package manager during the installation process (based on the manifest).
        - The mapping is based on /system/etc/permissions/platform.xml ( can be found on the device )
    - The second solution id getting the access through another process on their behalf
    - Most of the requests are handled by the system server. But, it checks if the request is matched with the declared permissions to proceed. Otherwise it throws a security exception.
    - The object owner controls the access rules on the objects via the permission strings


### Binder
    is an Android specific IPC mechanism and remote method invocation system.
    - The binder is a kernel driver
    - It provides a thread pool for dispatching the service methods
    - By the zero copy approach it doesn't copy anything between the processes. But share them across 
    - Binder affords reference counting of the objects and let the service to query clint's UID, GID and PID
    - It let the client and service to know when the other one is terminated via its link to death functionality    
    - It follows a client/server architecture
    - The service publishes and the client consumes
    - The client can bind via the service name or address
    - Each binder interface is known as a binder node and each node has a name
    - The node's addresses are not fixed and after restarting, start-up order or publishing can be vary
    - There's a context manager sit at node address 0. By publishing a service, the name  and address will be sent to it. That's responsible for giving the address when some client looking for a service by the name. Then it provides it to the client.
    - The communication through binder is synchronous


### Zygote
    a special Android process making it possible to share code across processes in Dalvik/Art VM. Then each process will have it's own copy and heap.
    Other than forking itself to start a new app, it especially start the system server.
    Before starting the new app it needs to change UID, PID and GID and also set the app capability to zero. The UID, PID and GIS will be defined by the package manager. There's a exception for the service manager that the capability won't be cleared.


### The property service
    It provides a shared mapping of key-values among all processes. It's read-only for all the processes except the init process which is responsible for updating and all the request will go through that. 
    - The request should be sent through a Unix domain socket. 
    - The request will be parsed and the value will be change if the permissions allow
    - The permissions will be assigned to the  properties at the build time
        - The permissions and the properties can be found at system/core/property_service.c
        - The prop_msg structure can be found at bionic/libc/include/sys/_system_properties.h
    - When a request is received the permission of the peer's socket will be checked


