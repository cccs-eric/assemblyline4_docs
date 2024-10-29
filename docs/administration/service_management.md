# Service Management

Assemblyline's service management interface allows users to effectively manage services by performing actions such as listing, updating, adding, removing, and backing up services. This guide provides a step-by-step overview of how to manage services in Assemblyline.

## Accessing Service Management

Navigate to the service management interface by selecting the *Administration* topic and choosing the *Services* subtopic.

![Service management](./images/services_bar.png){: .center }

## Service List

Upon loading the service management interface, you are presented with a list of all services in the system.

![Service list](./images/service_list.png)

### Service Management Buttons

The top-right section of the service list page includes several important buttons:

![Service management buttons](./images/service_mgmt_buttons.png){: .center }

1. **Add Service**: Add new services to the system.
2. **Update Services**: Perform updates on existing services.
3. **Install All Services**: Install all available services.
4. **Download Backup**: Download a backup of the current services configurations.
5. **Restore Backup**: Restore services configuration from a previously downloaded backup.

## Adding a Service

To add a new service:
1. Click the green "*(+)*" button in the top-right corner.
2. In the popup window, paste the new YAML-formatted [service manifest](../../developer_manual/services/advanced/service_manifest/) contents.
3. Click "*Add*" to incorporate the service into the system.

![Service add](./images/service_add.png)

!!! tip
    The "service-add" API will automatically replace certain environment variables in your manifest:

    * **$SERVICE_TAG**: Will be replaced by the latest tag for your current deployment type (dev/stable) found in the docker registry where the service container is hosted.
    * **$REGISTRY**: Will be replaced by your local registry.

## Updating Services

As malware evolves, so too must the services that analyze and counteract these threats. Assemblyline's rapid development cycle ensures frequent updates for its supported services, enabling timely responses to new and emerging threats.

The system automatically detects if a newer container version is available for your deployment type (dev/stable). An update button will be displayed on the service card for services that require updating.

![Service update](./images/service_update.png)

Hover over the button to see the new version and click to kick off the update.

## Installing Available Services

The system tracks which services are installed and highlights additional services that are available for installation. To install all available services, click the "Install all services" button at the top right corner. This will prompt the system to install any services that you do not currently have.

Alternatively, if you prefer to install services individually, scroll to the bottom of the service list page. Here, you will find a section listing all services that are available but not yet installed. Click the install button on the service card for each individual service you wish to add.

![Services available section](./images/services_available.png)

## Creating and Restoring Service Config Backups

Backup and restore functionality allows you to save and recover service configurations.

To manage backups:
1. Click the "*arrow pointing down*" button to download the current services configuration as a YAML file, named `<FQDN>_service_backup.yml`.
2. To restore a backup, click the "*clock with counter-clockwise arrow*" button. Paste the backup content into the textbox and click "*Restore*" to reinstate the services configurations to their saved state.

![Service restore](./images/service_restore.png)

## Service Listing Overview

The service listing table provides comprehensive information on each of the services configured in Assemblyline. Below is a detailed explanation of each column and the type of data it represents:

![Service Management page](./images/service_mgmt_page.png)

**Name**

The **Name** column contains the name of the service. This is the identifier that you will use to refer to this service throughout the system.

**Version**

The **Version** column lists the current running version of the service.

**Category**

The **Category** column categorizes the services into predefined groups. This helps in organizing services and understanding the role each service plays. Available categories out-of-the-box include:

- **Antivirus**: Services that scan files for malware.
- **Dynamic Analysis**: Services that execute the file in a sandbox environment to observe behaviors.
- **External**: Services that rely on external data sources or submit data to external systems.
- **Extraction**: Services that extract files from archives or other compound file formats.
- **Filtering**: Services that eliminate certain files from being processed further.
- **Internet Connected**: Services that need to connect to the internet to fetch updates or check data.
- **Networking**: Services that analyze network traffic or related data.
- **Static Analysis**: Services that analyze files without executing them.

**Stage**

The **Stage** column delineates at which point in the analysis pipeline a service will execute. The stages determine the sequence in which services operate, ensuring an organized and efficient workflow. Each service belongs to a single stage, executed in the following order:

- **FILTER**: Initial filtering of files (e.g., Safelist). This stage determines whether subsequent stages will analyze a file.
- **EXTRACT**: File extraction and unpacking (e.g., Extract, Unpacker). Services in this stage focus on decomposing files from archives or other compound formats.
- **CORE**: Core analysis tasks. This stage is where the majority of services perform their analysis.
- **SECONDARY**: Additional analysis performed with context from previous stages (e.g., VirusTotal). These services offer further insights based on the initial core analysis.
- **POST**: Subsequent processing steps (e.g., URLCreator). Services in this stage act upon the results generated by earlier stages.
- **REVIEW**: This final stage involves services that review the entire context of the analysis before performing their own tasks (e.g., Ancestry).

**File Types**

The **File Types** column specifies the Assemblyline file types that the service will accept for processing. This is defined using regular expressions. For instance:

- `.*` will accept all files for analysis.
- Specific regex patterns can be used to target certain Assemblyline file types, such as `executable/windows` for Windows executables.

**External**

The **External** column indicates whether the service sends data outside of Assemblyline's infrastructure. This is an important consideration for privacy and data security. An example is the VirusTotal service, which submits files to the VirusTotal platform for analysis.

**Mode**

The **Mode** column specifies how the service operates, providing key information on its interaction with Assemblyline's core infrastructure. Here are the possible modes:

- **Uses Service Server**: These services utilize the Service Server, which isolates them from the core infrastructure. The Service Server provides APIs for task handling, result publishing, file downloading, and accessing the system safelist, ensuring that services have all necessary functionalities while remaining unaware of the underlying infrastructure.
- **Runs in Privileged Mode**: Services operating in this mode bypass the Service Server, directly pulling tasks from Redis. They can save analysis results into the Datastore and store embedded and supplementary files directly into the Filestore. This results in faster processing due to the direct access to core components.

For more detailed information, please refer to the [full documentation](../overview/architecture.md#service-server).

**Classification**

The **Classification** column lists the classification level at which the service operates, which governs how the service handles and labels data.

**Enabled**

The **Enabled** column indicates if the service is currently enabled or disabled. An enabled service will have one or more instances running and actively process files, whereas a disabled service will not operate until re-enabled.


## Service Details

The service details page allows you to visualize and customize various parameters of a service across multiple tabs.

### Modifying or Removing a Service

Click on any service from the service list to view its details. The service detail page allows you to:

- **Delete the service**: Red "circled minus" button.
- **Toggle Enabled/Disabled state**: Big square button above the tabs.

### General Tab

This tab allows you to view and modify general information about the service.

![Service detail general](./images/service_detail_gen.png)

Here, you can adjust various general settings:

- **Version**: Change the service version.
- **Description**: Edit the service description.
- **Execution Stage**: Specify when the service should run.
- **Category**: Group the service under a specific category.
- **Accepted/Rejected File Types**: Define regular expressions for file types.
- **Execution Timeout**: Modify the time limit for service execution.
- **Maximum Instances**: Set the maximum number of service instances.
- **Location**: Configure the service location to be internal of external.
- **Result Caching**: Enable or disable result caching.

!!! tip
    Refer to the [service manifest](../../developer_manual/services/advanced/service_manifest/) documentation for more detailed information about these fields.

### Container tab

The "*Container*" tab provides information about the containers used by the service.

![Service detail container](./images/service_detail_container.png)

Here, you can:

* Change the update channel (Development/Stable)
* Change the main service container
* Add/modify/remove dependency containers

#### Main service container

The main service container is the container housing and running the service code. By clicking on the main service container, you can modify the parameters used to launch it.

![Service detail container edit](./images/service_detail_container_edit.png)

You can modify the following parameters:

* Container image name
* Type of container registry
* Resources limits (CPU/RAM)
* Container registry credentials (username/password)
* Command executed in the container
* Allow internet access to the container
* Environment variables set before loading the container

!!! tip
    Check the [docker config](../../developer_manual/services/advanced/service_manifest/#docker-config) block in the [service manifest](../../developer_manual/services/advanced/service_manifest/) documentation for more information on the different fields you can modify in the docker container configuration.

#### Dependency containers

Dependency containers are containers use to support the main services in some ways. Either by offering an external place to store data (A database for example) or to perform service updates.

A service can have multiple dependency containers and these containers are shared between the multiple instances of the service that can be loaded in the system i.e., there will only be one instance of each dependency container.

By either click the "*Add Dependency*" button or clicking a dependency container, you will be able to either add or modify container dependencies of the current service.

![Service detail dependency edit](./images/service_detail_dependency_edit.png)

The dependency container configuration window look almost the same and let you modify the same values as the [main service container](#main-service-container) window. There is however an added parameter that you can configure to give the container persistent storage.

!!! tip
    Check the [persistent volume](../../developer_manual/services/advanced/service_manifest/#persistent-volume) block from the [service manifest](../../developer_manual/services/advanced/service_manifest/) documentation to know more about the different fields to configure to get persistent storage in a dependency container.

### Updates tab

The "*Updates*" tab shows information about how the service updates itself or its signatures.

![Service detail updates](./images/service_detail_updates.png)

!!! warning
    This tab is optional and will not be shown for all service. Only services that define and [update config](../../developer_manual/services/advanced/service_manifest/#update-config) block in their [service manifest](../../developer_manual/services/advanced/service_manifest/) will have that tab shown.

In this tab, you will be able to view/modify the following information:

* Interval at which the service updates
* If the service generates signatures in the system or not
* If the service needs to wait for a successful update to start instances of itself
* The various sources where the service pulls its updates from

!!! tip
    Checkout the [Modifying sources](../../administration/source_management/#modifying-sources) documentation to know more about the different values you can change in the signature sources.

### Parameters tab

Finally, the "*Parameters*" tab will let you view and customize the different parameters the service can take in.

![Service detail parameters](./images/service_detail_parameters.png)

Service parameters are split into two categories:

* User specified parameters
* Service variables

#### User specified parameters

User specified parameters are parameters that a user can modify for each specific submission it does in the system.

They are often but not exclusively used for things like:

* Turning on/off features of a service
* Specifying a password used during a submission
* Limit what the service can and cannot do
* Extract more or less files when a service runs

!!! tip
    When these parameters are defined for a service, they will be shown in the [submission options](../../user_manual/submitting_file/#options) available for the user at submission time.

#### Service variables

Service variable are configuration parameters only shared between the service and your deployment. They are used to help the service configure itself to run well in your environment.

Service variables are often but not exclusively things like:

* URLs to connect to external services
* Credentials use to connect to external services
* List of default values used in a service
* Configuration parameter that will limit or increase scanning capabilities of a service

##### OCR Configuration
Some services may perform OCR analysis on images given/generated during analysis. Because of this, you're able to override/customize the default OCR terms from the [service base](https://github.com/CybercentreCanada/assemblyline-v4-service/blob/master/assemblyline_v4_service/common/ocr.py) using the `ocr` key in the `config` block of the service manifest.

###### Simple Term Override (Legacy)
Let's say, I want to use a custom set of terms for `ransomware` detection. Then I can set the following:

```yaml
config:
    ocr:
        ransomware: ['bad1', 'bad2', ...]
```

This will cause the service to **only** use the terms I've specified when looking for `ransomware` terms. This is still subject to the hit threshold defined in the service base.

###### Advanced Term Override
Let's say, I want to use a custom set of terms for `ransomware` detection and I want to set the hit threshold to `1` instead of `2` (default). Then I can set the following:

```yaml
config:
    ocr:
        ransomware:
            terms: ['bad1', 'bad2', ...]
            threshold: 1
```

This will cause the service to **only** use the terms I've specified when looking for `ransomware` terms and is subject to the hit threshold I've defined.

###### Term Inclusion/Exclusion
Let's say, I want to add/remove a set of terms from the default set for `ransomware` detection. Then I can set the following:

```yaml
config:
    ocr:
        ransomware:
            include: ['bad1', 'bad2', ...]
            exclude: ['bank account']
```

This will cause the service to add the terms listed in `include` and remove the terms in `exclude` when looking for `ransomware` terms in OCR detection with the default set.
