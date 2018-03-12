# aem-dispatcher

This role installs the [AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html) Apache module on Debian/Ubuntu or RHEL/CentOS servers. It will also ensure that Apache itself is installed (by depending on the [geerlingguy.apache](https://galaxy.ansible.com/geerlingguy/apache/) role).
> This role was developed as part of the wcm.io set of roles to integrate Ansible with [CONGA](http://devops.wcm.io/conga/) but can be used independently of it.

## Requirements

This role requires Ansible 2.0 or higher and works with Dispatcher 4.2.x or higher. It requires the Dispatcher installation tarball which can be supplied as file or retrieved from a Nexus/RPM/APT repository, an HTTP URL or a S3 bucket (see below).

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

	dispatcher_version: 4.2.2
	
The dispatcher version to install.

	dispatcher_ssl_support: true

Whether to install the Dispatcher version which supports SSL for communication with the render instance.

	dispatcher_download_path: /tmp

Path to download the Dispatcher tarball to.

	dispatcher_tarball_name:
	
Name of the Dispatcher tarball to use for installation. When not specified, the role will automatically build the name from the target environment (Apache version, architecture etc.) and `dispatcher_version`.

	dispatcher_tarball_sha1:

SHA1 checksum of the Dispatcher tarball. Currently this is used to check the integrity of an existing download and files downloaded via URL.
	
	dispatcher_install_source: file
	
The installation source to fetch the installation tarball from. Can either be `file`, `package`, `url`, `s3` or `nexus`.

	dispatcher_url: "http://host:port/path/{{ dispatcher_tarball_name }}"
	dispatcher_url_username:
	dispatcher_url_password:

URL, username and password for retrieving the installation file from an URL.
	
	dispatcher_s3_bucket: aem-installation-artifacts
	dispatcher_s3_object: "{{ dispatcher_tarball_name }}"
	dispatcher_s3_access_key:
	dispatcher_s3_secret_key:

Bucket, object (path) and credentials for retrieving the installation file from an S3 bucket.
	
	dispatcher_nexus_coordinates:
	- {
	  group_id: group.id,
	  artifact_id: artifact.id,
	  repository_url: 'https://repo.url'
	  version: "{{ dispatcher_version }}",
	  classifier: "{{ dispatcher_apache_version }}-{{ ansible_system | lower }}-{{ ansible_architecture | regex_replace('_', '-') }}",
	  }
	dispatcher_nexus_username:
	dispatcher_nexus_password:

Maven coordinates for retrieving the installation file from Nexus. Version and classifier are build automatically from the target environment and dispatcher_version if not specified (as for the filename).

## Dependencies

This role depends on the [geerlingguy.apache](https://galaxy.ansible.com/geerlingguy/apache/) role for installing Apache.

## Example Playbook

Installs AEM in `/opt/adobe/aem-author`: 

    - hosts: webserver
      roles:
         - { role: aem-dispatcher, dispatcher_version: 4.2.3 }

## License

Apache 2.0