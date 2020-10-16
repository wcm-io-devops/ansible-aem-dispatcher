# wcm_io_devops.aem_dispatcher

This role installs the [AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html) Apache module on Debian/Ubuntu or RHEL/CentOS servers. It will also ensure that Apache itself is installed (by depending on the [geerlingguy.apache](https://galaxy.ansible.com/geerlingguy/apache/) role).
> This role was developed as part of the
> [wcm.io DevOps Ansible Automation for AEM](http://devops.wcm.io/ansible-aem/)
> to integrate Ansible with
> [CONGA](http://devops.wcm.io/conga/) but can be used independently of
> it.

## Requirements

This role requires Ansible 2.7 or higher and works with Dispatcher 4.2.x or higher. It requires the Dispatcher installation tarball which can be supplied as file or retrieved from a Maven/RPM/APT repository, an HTTP URL or a S3 bucket (see below).

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

	aem_dispatcher_version: 4.3.3
	
The dispatcher version to install.

	aem_dispatcher_ssl_support: true

Whether to install the Dispatcher version which supports SSL for communication with the render instance.

    aem_dispatcher_port: 80

The http port the webserver is listening on

    aem_dispatcher_port_ssl: 443

The https port the webserver is listening on

	aem_dispatcher_download_path: /tmp

Path to download the Dispatcher tarball to.

	aem_dispatcher_tarball_name:
	
Name of the Dispatcher tarball to use for installation. When not specified, the role will automatically build the name from the target environment (Apache version, architecture etc.) and `dispatcher_version`.

	aem_dispatcher_tarball_sha1:

SHA1 checksum of the Dispatcher tarball. Currently this is used to check the integrity of an existing download and files downloaded via URL.
	
	dispatcher_install_source: file
	
The installation source to fetch the installation tarball from. Can either be `file`, `package`, `url`, `s3` or `maven_repository`.

	aem_dispatcher_url: "http://host:port/path/{{ dispatcher_tarball_name }}"
	aem_dispatcher_url_username:
	aem_dispatcher_url_password:

URL, username and password for retrieving the installation file from an URL.
	
	aem_dispatcher_s3_bucket: aem-installation-artifacts
	aem_dispatcher_s3_object: "{{ dispatcher_tarball_name }}"
	aem_dispatcher_s3_access_key:
	aem_dispatcher_s3_secret_key:

Bucket, object (path) and credentials for retrieving the installation file from an S3 bucket.
	
	aem_dispatcher_maven_repository_coordinates:
	- {
	  group_id: group.id,
	  artifact_id: artifact.id,
	  repository_url: 'https://repo.url'
	  version: "{{ dispatcher_version }}",
	  classifier: "{{ dispatcher_apache_version }}-{{ ansible_system | lower }}-{{ ansible_architecture | regex_replace('_', '-') }}",
	  }
	aem_dispatcher_maven_repository_username:
	aem_dispatcher_maven_repository_password:

Maven coordinates for retrieving the installation file from a Maven repository. Version and classifier are build automatically from the target environment and dispatcher_version if not specified (as for the filename).

    aem_dispatcher_dependency_apache: true

 Enables/disables the execution of the apache role dependency.

    # aem_dispatcher_apache_server_root: /etc/apache2

Overwrites the os family specific apache server root.

## Dependencies

This role depends on the [wcm_io_devops.apache](https://github.com/wcm-io-devops/ansible-role-apache) role for installing Apache.

## Example Playbook

Installs AEM in `/opt/adobe/aem-author`: 

    - hosts: webserver
      roles:
         - { role: wcm_io_devops.aem_dispatcher, aem_dispatcher_version: 4.3.2 }

## License

Apache 2.0
