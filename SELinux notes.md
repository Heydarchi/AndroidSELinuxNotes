### Some notes

#### Android SELinux user:
Not using SELinux users. 
u   ->  The only user


#### Android SELinux Roles:
r   ->  subject
object_r    ->  object  

#### Android SELinux Sensitivity:

Not using SELinux users. 
s0  ->  The default that always is set




### Some descriptions



#### - Abbreviation

- scontext    ->  The SELinux context of the process that attempted the denied action

- tcontext    ->  The SELinux context of the object (target) the process attempted to access




-----------------------------------------


#### - Files

- file_contexts:
    It's used to associate default contexts to files or file systems that support extended file attributes.
    The file_contexts backend maps from pathname/mode combinations into security contexts.

- seapp_contexts:
    The entries in this file define how security contexts for apps are determined.
    The build process supports additional seapp_contexts files allowing devices to specify their entries as described in the Device Specific Policy section.


-----------------------------------------

### Some path

- Android SELinux path:
    /system/sepolicy/