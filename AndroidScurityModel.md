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
    - It follows a client/server architecture
    - The service publishes and the client consumes
    - The client can bind via the service name or address
    - Each binder interface is known as a binder node and each node has a name
    - The node's addresses are not fixed and after restaring, start-up order or publishing can be vary
    - There's a context manager sit at node address 0. By publishing a service, th ename  and address will be sent to it. That's responsible for giving the address when some client looking for a service by the name. Then it provides it to the client.
    - 




Questions ;

