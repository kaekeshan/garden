---
title: Netcentric AEM configurations
description: AEM (Adobe Experience Manager) configurations using the Netcentric accesscontroltool — repository setup, dependency installation, and ACL management.
draft: false
tags: [aem, dev]
date: 2025-12-01
---

Reference - [Netcentric/accesscontroltool - Github](https://github.com/Netcentric/accesscontroltool)

### Dependency Installation

#### Inside pom.xml

As a best practice, add this property ==Netcentric.actool== to store the Netcentric version used by the project.
For example,

```xml
<netcentric.actool>3.0.4</netcentric.actool>
```

Refer [Maven Repository](https://mvnrepository.com/artifact/biz.netcentric.cq.tools.accesscontroltool/accesscontroltool-package) to find the latest stable version of Netcentric.

After creating the property, add the following dependency under **<dependencies>**

```xml
<dependencies>
	<dependency>
		<groupId>biz.netcentric.cq.tools.accesscontroltool</groupId>
		<artifactId>accesscontroltool-package</artifactId>
		<classifier>cloud</classifier>
		<type>content-package</type>
		<version>${netcentric.actool}</version>
	</dependency>
</dependencies>
```

#### Inside all/pom.xml

Add Netcentric as a plugin embedding.

```xml

<plugin>
	<embeddeds>
		<!-- Netcentric ACL Tool Embedding -->
		<embedded>
			<groupId>biz.netcentric.cq.tools.accesscontroltool</groupId>
			<artifactId>accesscontroltool-package</artifactId>
			<classifier>cloud</classifier>
			<type>content-package</type>
			<target>/apps/test-packages/application/install</target>
		</embedded>
	</embeddeds>
</plugin>
```
Replace the ==/apps/test-packages/application/install== with corresponding project path

Also add as a dependency under **<dependencies>**

```xml
<dependencies>
	<!--  Netcentric ACL Tool -->
	<dependency>
		<groupId>biz.netcentric.cq.tools.accesscontroltool</groupId>
		<artifactId>accesscontroltool-package</artifactId>
		<type>content-package</type>
		<classifier>cloud</classifier>
		<version>${netcentric.actool}</version>
	</dependency>
</dependencies>
```

#### Configure the JCR Path

Configure the JCR path(s) where the config files reside (usually its just one, can be multiple for multitenant setups)
Create the following file under **ui.config*.

```json
biz.netcentric.cq.tools.actool.impl.AcInstallationServiceImpl~project-identifier.cfg.json
```

File will configure the root path for the Netcentric AC tool. Thess JCR path will contain *yaml* files which will specify the
access control rules.

```json
{
  "configurationRootPaths": [
    "/apps/abc/ac-tool-configs"
  ]
}
```
>[!tip] if configuration file is located under ui.config, make sure to change the package type in ui.config from ‘container’ to ‘mixed’


#### Sample Configuration File

```yaml
- group_config:

  - myeditors:
    - isMemberOf: mystaff
      members: editor

  - system-read:
    - isMemberOf:
      description: system users with read access
      path: fragments/system # if relative, /home/groups is automatically prefixed
      members: system-reader

- user_config:

  - editor:
    - isMemberOf: myeditors
      password: secret

  - replicationReceiver:
    - name: "Replication receiver"
      password: "{someEncryptedValue}"

  - poweruser:
    - name: "Power User"
      isMemberOf: powerusers
      password: secret
      path: myprojusers
      profileContent: <jcr:root jcr:primaryType="nt:unstructured" email="poweruser@example.com"/>

  - system-reader:
    - name: system-reader
      isMemberOf: system-read
      path: system
      isSystemUser: true

```

