# What is PAM

PAM stands for Pluggable Authentication Modules. It is a framework used
in Unix-like operating systems, including Linux, to manage
authentication, account management, and password policies for various
applications and services. PAM allows system administrators to configure
a consistent authentication mechanism across different applications and
services, making it easier to maintain and enforce security policies.

PAM provides a modular architecture, where authentication-related tasks
are separated into different modules. Each module can handle a specific
aspect of authentication, such as password verification, account
locking, session management, and more. Administrators can configure PAM
to use different modules for different services, allowing for a flexible
and customized authentication process.

PAM allows for centralized authentication policies, supports a wide
range of authentication methods (such as passwords, tokens, smart cards,
biometrics, etc.), and provides the ability to define various rules and
conditions for granting or denying access. It is a powerful tool for
enhancing the security of a Linux system by controlling how users are
authenticated and authorized to access resources.

To interact with PAM, administrators typically configure its behavior
through configuration files located in the `/etc/pam.d/` directory.
These files specify which PAM modules should be used for various
authentication and authorization tasks for each specific service or
application.

# PAM architecture

The architecture of Pluggable Authentication Modules (PAM) in Linux is
based on a modular framework that allows system administrators to
configure authentication, authorization, and other security-related
policies for various services and applications.

1. **Application/Service**: PAM operates as a middle layer between the
	 application or service that requires authentication and the
	 underlying authentication mechanisms. This application could be
	 anything from login prompts, GUI authentication dialogs, or any
	 service that requires user authentication.

2. **PAM Stack**: When an application needs to authenticate a user, it
	 invokes the PAM library. The PAM stack consists of a series of
	 modules that are configured for a specific service. These modules are
	 organized into four distinct categories:

	 a. **Account Management**: Determines if the user is allowed to
			access the service based on account restrictions and policies.

	 b. **Authentication**: Verifies the user's identity using various
			authentication methods such as passwords, tokens, or biometrics.

	 c. **Password Management**: Handles tasks related to password
			changes, expiration, and complexity rules.

	 d. **Session Management**: Manages user sessions, including starting
			and ending sessions, managing resources, and more.

3. **PAM Configuration**: Each service that uses PAM has its own
	 configuration file located in the `/etc/pam.d/` directory. These
	 configuration files define the specific PAM modules that are invoked
	 for different authentication stages (account, authentication,
	 password, session). Administrators can customize the behavior of each
	 service by editing these configuration files.

4. **PAM Modules**: PAM provides a variety of pre-built authentication
	 modules that can be used for different tasks. These modules can be
	 replaced or extended to support custom authentication methods or
	 integrate with external authentication systems.

5. **Authentication Process**: When a user tries to access a service,
	 PAM evaluates the configured modules in the order specified in the
	 PAM stack for that service. Each module can return one of three
	 outcomes: success, failure, or error. PAM continues processing the
	 modules until a failure or error occurs, or until all modules are
	 successfully processed.

6. **Result**: The final result of the PAM stack determines whether the
	 user is granted access to the service. The service itself relies on
	 PAM's decision to proceed or deny access.

# Implementing PAM

Implementing Pluggable Authentication Modules (PAM) on a Linux system
involves configuring PAM modules for specific services or applications.
Here's a general guide on how to implement PAM on a Linux system:

1. **Understand PAM Configuration Files:** Familiarize yourself with PAM
	 configuration files located in the `/etc/pam.d/` directory. Each
	 service/application has its own PAM configuration file with a
	 specific name (e.g., `sshd` for SSH, `login` for console login,
	 `sudo` for sudo access, etc.).

2. **Backup Configuration Files:** Before making any changes, create
	 backups of the PAM configuration files you plan to modify. This
	 ensures you can revert back in case of any issues.

3. **Edit PAM Configuration Files:** Use a text editor to edit the
	 relevant PAM configuration file for the service you want to
	 configure. The configuration files consist of lines that specify PAM
	 modules and options. Each line typically follows the format:
	 `module_type  control_flag  module_path  [module_options]`.

4. **Configure Modules:** Add, modify, or remove PAM modules based on
	 your security requirements. Modules are organized into categories
	 (account, auth, password, session), and you can include them in the
	 desired order within the PAM configuration file.

5. **Module Control Flags:** PAM modules have control flags that define
	 how the result of a module affects the overall authentication
	 process. Common control flags include:
	 - `required`: The module must succeed for authentication to continue.
	 - `requisite`: The module must succeed, and if it fails, no further
		 modules are processed.
	 - `sufficient`: If the module succeeds, no further processing is
		 required, but failure doesn't stop the process.
	 - `optional`: The module's success or failure doesn't affect the
		 overall process.

6. **Module Options:** Some modules may require additional options to
	 configure their behavior. Consult the documentation for specific
	 module options and their meanings.

7. **Testing and Validation:** After making changes to the PAM
	 configuration, it's crucial to test the changes thoroughly to ensure
	 they work as expected. Test with both valid and invalid user
	 credentials to verify the behavior.

8. **Documentation and Resources:** PAM configuration can be complex,
	 and it's important to refer to the official PAM documentation and
	 other resources. You can find more information in the PAM manual
	 pages (`man pam`) and online tutorials.

9. **Restart Services:** Once you are confident that your PAM
	 configuration is correct, you may need to restart the service or
	 application that you've configured PAM for. This ensures that the new
	 PAM settings take effect.

Remember that PAM configuration can have significant security
implications, so it's important to understand the modules you're using
and the impact of your changes. Improperly configured PAM settings can
lead to authentication issues or security vulnerabilities. Always make
changes cautiously and be prepared to revert if necessary. 





Related:
```
* https://borosan.gitbook.io/lpic2-exam-guide/2102-pam-authentication
```
