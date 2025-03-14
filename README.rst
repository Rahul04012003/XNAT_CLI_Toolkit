XNAT_CLI_Toolkit
================

Introduction
------------

The **XNAT_CLI_Toolkit** is a command-line interface (CLI) tool designed to interact with XNAT (an imaging informatics platform) seamlessly.
This toolkit offers a variety of functionalities that allow users to:

- List available projects.

- Upload data to XNAT, including prearchiving, archiving, and updating demographics.

- Share data across different users or systems.

- Display a comprehensive list of available commands via the xnat-toolkit command.

The tool is split into different modules, each responsible for specific tasks to ensure a streamlined interaction with XNAT.
The command-line interface allows for flexibility and efficiency in handling large datasets and performing administrative tasks on XNAT.


Core Functionalities:
---------------------

1. **List Projects**: Retrieve and display a list of available XNAT projects or subjects from a specific project.

2. **Upload Data**: Upload data to XNAT in a structured manner:
    - Prearchiving

    - Archiving

    - Demographics update

3. **Sharing Data**: Share project data with collaborators or other systems.

4. **Custom Form Manipulation**: Fetch, Update or Delete Custom Form data with different modular functions.

4. **Help Command**: Display a list of available commands with ``xnat-toolkit``.


Dependencies
------------

1. **Python 3.x**: Required to run the scripts.

::

    python --version

Checks for the python version installed on your machine.

2. **PIP**

::

    py get pip.py

3. **Click**:

::

    pip install click

- **click**: For building command-line interfaces.

After installing click there are some more libraries

4. **XNAT**:

::

    pip install xnat

After Installation of xnat

5. **Libraries and Modules**:
    - **xnat**: A Python library for interacting with XNAT servers. This is essential for connecting to XNAT, handling authentication, and managing the upload, download, and processing of imaging data.

    - **logging**: For logging messages related to script execution, such as successes, errors, and debugging information (part of the Python standard library).

    - **json**: For working with JSON files, such as reading and writing data (part of the Python standard library).

    - **os**: For file system navigation and file handling (part of the Python standard library).

    - **authenticate**: Custom module for managing XNAT credentials and handling expired credentials.
        - This module includes:
            - ``store_credentials()``:A funtion to store the credentials, Using ``xnat-authenticate`` users can store their credentials(Auto-expire in 1 Hour.)

            - ``get_credentials()``: A function to retrieve stored XNAT credentials.

            - ``CredentialExpiredError``: A custom exception for managing credential expiration.

6. **Optional Libraries** (in case not pre-installed):

- **requests**: A common HTTP library for making requests to the XNAT server, often required for interacting with APIs.

**Installation**

::        

    pip install requests 

**Installation Command**

You can install all the necessary libraries using the following command:

::

    pip install click xnat requests


Installation
------------

To install the XNAT_CLI_Toolkit, use the following command:

::

    pip install XNAT_CLI_Toolkit

Once installed, you can start using the command-line tool by typing ``xnat-toolkit`` in your terminal.


XNAT-COMMANDS
-------------

To display the list of available commands in the Toolkit:

::

    xnat-toolkit

This will display all the available commands in XNAT-Toolkit, you can furthermore learn how the commands works using ``<COMMAND-NAME> --help`` to view all the required arguements for the command.
Authentication Module
---------------------

The ``authenticate.py`` module is responsible for securely handling user credentials. It stores credentials for the XNAT server, reuses sessions, and manages credential expiration.
Below is a detailed description of how the authentication process works and the options available.

Authentication Process
~~~~~~~~~~~~~~~~~~~~~~
The ``authenticate.py`` module offers functionality for securely storing and retrieving XNAT server credentials. 
This ensures that you don't need to re-enter your credentials for every command, making the interaction more efficient.

Command for Storing Credentials
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
To store your XNAT credentials (host, username, password), use the following command:

::

    xnat-authenticate --host <XNAT_SERVER_URL> --username <USERNAME> --password <PASSWORD>

This command saves your credentials securely to your ``.netrc`` file and stores a timestamp for session tracking.

Key Functions
~~~~~~~~~~~~~

- ``store_credentials(host, username, password):``
    - Saves the host and user credentials to the .netrc file.

    - Stores a timestamp for the credentials in a JSON file (``~/.last_credentials.json``), allowing the system to verify the session validity.

- ``get_credentials():``
    - Retrieves the last stored credentials from the JSON file.

    - Checks if the credentials have expired. If the session has expired (after 30 minutes), it prompts the user to re-enter their credentials.

Handling Expired Credentials
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
When credentials are older than the expiration time (60 minutes), the system will prompt you to enter new ones. If an attempt is made to access XNAT with expired credentials, the following exception will be raised:

::

    CredentialExpiredError: "Credentials have expired. Please enter new ones through xnat-authenticate."

Project Listing Module
-----------------------

The ``list.py`` module provides functionality to list the available projects or Subjects from a specific project on the XNAT server. 
This command uses the ``click`` package to create a command-line interface that connects to the server, fetches a list of projects or subjects and displays them. 
If credentials (server URL, username, and password) are not provided via the command-line, it retrieves them from previously stored credentials using the ``authenticate.py`` module.

Command for Listing Projects
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The ``xnat-list`` command fetches and displays the list of projects or subjects from a specific project stored on the XNAT server. 
If credentials are not passed in via the command-line options, the stored credentials from ``.netrc`` or the last credentials JSON file are used.

Usage
~~~~~
To list all the projects available on your XNAT server, use the following command:

::

    xnat-list --server <XNAT_SERVER_URL> --username <USERNAME> --password <PASSWORD>

If you have already stored your credentials using the ``xnat-authenticate`` command, you can omit the server, username, and password, and the tool will automatically fetch the saved credentials:

::

    xnat-list

This will securely retrieve your credentials from the stored ``.netrc`` file and connect to the XNAT server.

To list all the subjects from a specific project, use the following command:

::

    xnat-list --server <XNAT_SERVER_URL> --username <USERNAME> --password <PASSWORD> --project-id <PROJECT-ID>

OR, If you have already stored your credentials using the ``xnat-authenticate`` command, you can omit the server, username, and password, and the tool will automatically fetch the saved credentials:

::

    xnat-list --project-id <PROJECT-ID>

Key Features
~~~~~~~~~~~~

- Command-line options: The ``xnat-list`` command supports passing in the server URL, username, and password directly via the command line.
    - ``--server`` or ``-s``: The XNAT server URL (e.g., ``http://localhost``).

    - ``--username`` or ``-u``: The username for the XNAT server.

    - ``--password`` or ``-p``: The password for the XNAT server. If not provided, credentials will be fetched from stored values.

    - ``--project-id`` or ``-pid``: The project id of the project whose subjects you want to be displayed. If not provided, it will give the list of all the available projects on the XNAT Server.

- Logging:
    - Logs are created in the ``logs`` folder in the current working directory.

    - The log filename is generated using the current date and time, ensuring that each session has its own log file. The log captures important events such as connecting to the server, fetching projects, and errors if any occur during the process.

Error Handling
~~~~~~~~~~~~~~
If the connection to the **XNAT** server fails, or if there is an issue with the provided credentials, an appropriate error message is displayed. Errors and exceptions are also logged into the log file for future reference.

Example
~~~~~~~
Here's an example of listing projects from the XNAT server without passing credentials (assuming they have been previously stored):

::

    $ xnat-list
    Using credentials from .netrc...
    server: http://xnat.example.com, Username: myuser
    Project ID: Project1
    Project ID: Project2
    ...

In case of a failure in connecting to the server or retrieving projects, the error will be logged, and an error message will be printed to the console:

::

    $ xnat-list
    Error: Failed to connect to the XNAT server.

Logging Example
~~~~~~~~~~~~~~~
Upon execution, logs are generated with timestamps for every action performed, for example:

::

    2024-10-15 13:45:22 - INFO - Using credentials from .netrc.
    2024-10-15 13:45:24 - INFO - Fetched projects from the XNAT server.
    2024-10-15 13:45:27 - ERROR - Error: Connection refused.

File Structure
~~~~~~~~~~~~~~

- ``xnat-list``: Command to list projects from the XNAT server.

- Logging: Stored in a log file under the ``logs`` directory with a timestamp in the filename.

- Error Handling: Includes error messages for failed connections and logs them.

Upload and Archive Module
-------------------------

The ``upload.py`` module provides a command-line interface to upload DICOM files to an XNAT project, archive the uploaded files, and update demographic information for the subjects. The module uses the ``click`` package for argument parsing and ``xnat`` for connecting and interacting with the XNAT server.

Command for Uploading and Archiving Files
The ``xnat-upload`` command uploads ZIP files containing DICOM data to the XNAT server's prearchive, archives them into the specified project, and updates subject demographic information.

Usage
~~~~~

To upload and archive DICOM files, use the following command:

::

    xnat-upload --project <XNAT_PROJECT> --server <XNAT_SERVER_URL> --username <USERNAME> --source <SOURCE_DIR>

If you have previously stored your credentials (server, username, and password), you can omit the credentials from the command, and they will be fetched from the stored credentials:

::

    xnat-upload --project <XNAT_PROJECT> --source <SOURCE_DIR>

The ``<SOURCE_DIR>`` should contain ZIP files of the DICOM datasets to be uploaded.

Command-line Options
~~~~~~~~~~~~~~~~~~~~

- ``--project`` or ``-d``: The XNAT project where the files will be archived. This option is required.

- ``--username`` or ``-u``: Username for XNAT. If not provided, the stored username is used.

- ``--server`` or ``-s``: The XNAT server URL (e.g., ``http://localhost``). If not provided, the stored server URL is used.

- ``--password`` or ``-p``: Password for XNAT. If not provided, the stored password is used.

- ``--source`` or ``-x``: Directory containing the ZIP files to be uploaded. This option is required.

Workflow
~~~~~~~~

1. **Credential Retrieval**: 
    - If username, server, or password is not provided, the ``get_credentials`` function from the ``authenticate.py`` module retrieves the stored credentials.

2. **File Upload**: 
    - The module traverses the specified source directory and uploads all ZIP files to the XNAT prearchive.

3. **Archiving Files**:
    - Once the files are uploaded, they are archived into the specified XNAT project, and an experiment label is generated based on subject, study date, study time, and modality.

4. **Demographic Update**: 
    - After archiving, the subject demographic variables such as age, date of birth (DOB), and gender are updated using DICOM tags if available.

Logging
~~~~~~~

Logs are created in the logs folder in the current working directory. The log file is named based on the current date and time, e.g., ``2024-10-15_13-45-22_share.log``.

Example log entry:

::

    2024-10-15 13:45:22 - INFO - Connected to XNAT http://xnat.example.com
    2024-10-15 13:45:24 - INFO - Uploading /path/to/file.zip
    2024-10-15 13:45:27 - ERROR - Error uploading /path/to/file.zip: Connection refused.

Example
~~~~~~~
Here's an example of uploading and archiving DICOM files to an XNAT project:

::

    $ xnat-upload --project MyProject --source /path/to/zip/files
    Using credentials from .netrc...
    Connected to XNAT http://xnat.example.com
    Uploading /path/to/file1.zip
    Uploading /path/to/file2.zip
    ...
    Successfully Archived: Subject001_20241015T1330_CT to Project: MyProject
    Archive completed. Now updating Demographic Variables.
    Subject: Subject001; Age: 30; Gender: Male

Error Handling
~~~~~~~~~~~~~~
Errors during upload or archiving are logged and displayed. If any files fail to upload, they are listed at the end of the process.

File Structure
~~~~~~~~~~~~~~

- ``upload_and_archive``: Command to upload and archive DICOM files to XNAT.

- Logging: Stored in a log file under the ``logs`` directory.

- Error Handling: Logs and prints errors during connection, upload, and archiving.

Pre-Archive Module
------------------

Overview
~~~~~~~~
The ``upload_to_prearchive.py`` script is designed to facilitate the uploading of DICOM files (in ZIP format) to the prearchive area of a specified XNAT (eXtensible Neuroimaging Archive Toolkit) project. 
The script connects to the XNAT server using user-provided or stored credentials, scans a given directory for ZIP files, and uploads them to the prearchive.

Usage
~~~~~
To run the script, use the following command in the terminal:

::

    xnat-prearchive --project <project_name> --username <username> --server <server_url> --source <source_directory> --password <password>

Command-Line Options
~~~~~~~~~~~~~~~~~~~~

- ``--project`` or ``-d``: (Required) The name of the destination XNAT project where the DICOM files will be uploaded.

- ``--username`` or ``-u``: (Optional) The username for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--server`` or ``-s``: (Optional) The URL of the XNAT server. If not provided, the script will attempt to fetch stored credentials.

- ``--source`` or ``-x``: (Required) The directory containing the source ZIP files to be uploaded.

- ``--password``or ``-p``: (Optional) The password for XNAT. If not provided, the script will attempt to fetch stored credentials.

Workflow
~~~~~~~~

1. **Credential Management**:
    - The script checks if username, server, and password are provided as command-line arguments.

    - If any are missing, it attempts to retrieve stored credentials using the ``get_credentials()`` function from the ``authenticate`` module. If the credentials are expired, it raises a ``CredentialExpiredError``.

2. **Server Connection**:
    - The script connects to the XNAT server using the provided or retrieved credentials. If the connection fails, an error message is logged.

3. **File Collection**:
    - The script scans the specified source directory for all ZIP files and compiles a list of their paths. It utilizes ``os.walk()`` to traverse subdirectories and ensure that all relevant files are included.

4. **File Uploading**:
    - For each ZIP file in the collected list:
        - The script attempts to upload the file to the prearchive area of the specified XNAT project.

        - It logs the upload process and tracks any errors that occur during the upload.

5. **Subject ID Extraction**:
    - The script extracts the subject ID from the uploaded ZIP file's name (by default, it assumes the subject ID is the filename without the ``.zip`` extension). This may be modified if a different extraction method is needed.

6. **Logging**:
    - The script logs all actions and errors, providing a detailed record of the upload process. Logging is set to the ``INFO`` level by default, and logs are formatted to include timestamps and log levels.

7. **Saving New Subjects**:
    - If any new subjects are added during the upload process, their IDs are saved to a temporary JSON file (``new_subjects.json``). This file can be utilized for further processing or record-keeping.

8. **Error Handling**:
    - The script handles various exceptions, logging errors that occur during the connection to XNAT, file retrieval, and uploading processes.

    - If any uploads fail, the script logs the filenames of the failed uploads.

Example
~~~~~~~
Here's an example command to run the script:

::

    xnat-prearchive --project BrainStudy --username johndoe --server https://xnat.example.com --source /path/to/zip/files --password mypassword

This command will upload all ZIP files located in ``/path/to/zip/files`` to the ``BrainStudy`` project on the specified XNAT server.

Archiving Module
----------------

Overview
~~~~~~~~
The ``archive_to_xnat.py`` script is designed to archive files from the prearchive area to a specified XNAT (eXtensible Neuroimaging Archive Toolkit) project. 
It connects to the XNAT server using user-provided or stored credentials, retrieves files from the prearchive, extracts relevant metadata, and uploads the files to the specified project in XNAT.

Usage
~~~~~
To run the script, use the following command in the terminal:

::

    xnat-archive --project <project_name> --username <username> --server <server_url> --password <password>

Command-Line Options
~~~~~~~~~~~~~~~~~~~~

- ``--project`` or ``-d``: (Required) The name of the destination XNAT project where files will be archived.

- ``--username`` or ``-u``: (Optional) The username for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--server`` or ``-s``: (Optional) The URL of the XNAT server. If not provided, the script will attempt to fetch stored credentials.

- ``--password`` or ``-p``: (Optional) The password for XNAT. If not provided, the script will attempt to fetch stored credentials.

Workflow
~~~~~~~~

1. **Credential Management**:
    - The script checks if username, server, and password are provided as command-line arguments.

    - If any are missing, it attempts to retrieve stored credentials using the ``get_credentials()`` function from the ``authenticate`` module. If the credentials are expired, it raises a ``CredentialExpiredError``.

2. **Server Connection**:
    - The script connects to the XNAT server using the provided or retrieved credentials. If the connection fails, an error message is logged.

3. **Prearchive File Retrieval**:
    - The script retrieves all sessions from the XNAT prearchive. For each session, it extracts the subject and scans.

4. **DICOM Metadata Extraction**:
    - For each scan in the session, the script extracts the following DICOM metadata:
        - Study Date

        - Study Time

        - Modality

    - This information is used to create a unique experiment label.

5. **Archiving Process**:
    - The script archives each session to the specified XNAT project using the generated experiment label. If any errors occur during archiving, they are logged.

6. **Logging**:
    - The script logs all actions and errors, providing a record of the archiving process. Logging is set to the ``INFO`` level by default, and logs are formatted to include timestamps and log levels.

Example
~~~~~~~
Here's an example command to run the script:

::

    xnat-archive --project BrainStudy --username johndoe --server https://xnat.example.com --password mypassword

This command will archive files from the prearchive area into the ``BrainStudy`` project on the specified XNAT server.

Update_Demographics Module
--------------------------

Overview
~~~~~~~~
The ``update_demographics.py`` script is designed to update demographic variables (age, date of birth, and gender) for newly added subjects within a specified XNAT (eXtensible Neuroimaging Archive Toolkit) project. The script connects to the XNAT server, retrieves DICOM data for each new subject, and updates the demographic fields in the project accordingly.
Usage
~~~~~
To run the script, use the following command in the terminal:

::

    xnat-updatedemographics --project <project_name> --username <username> --server <server_url> --password <password> --new_subjects_file <path_to_new_subjects_file>

Command-Line Options
~~~~~~~~~~~~~~~~~~~~

- ``--project`` or ``-d``: (Required) The name of the destination XNAT project where the demographic updates will occur.

- ``--username`` or ``-u``: (Optional) The username for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--server`` or ``-s``: (Optional) The URL of the XNAT server. If not provided, the script will attempt to fetch stored credentials.

- ``--password`` or ``-p``: (Optional) The password for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--new_subjects_file`` or ``-n``: (Optional) The path to the JSON file containing newly added subjects. Defaults to new_subjects.json.

Workflow
~~~~~~~~

1. **Credential Management**:
    - The script checks if username, server, and password are provided as command-line arguments.

    - If any are missing, it attempts to retrieve stored credentials using the ``get_credentials()`` function from the ``authenticate`` module. If the credentials are expired, it raises a ``CredentialExpiredError``.

2. **Server Connection**:
    - The script connects to the specified XNAT server using the provided or retrieved credentials. If the connection fails, an error message is logged.

3. **Loading New Subjects**:
    - The script attempts to load the list of newly added subjects from the specified JSON file. If the file cannot be found or has an invalid format, an error message is displayed.

4. **Demographic Data Retrieval**:
    - For each subject in the loaded list:
        - The script retrieves the subject object from the specified project.

        - It then iterates through the experiments and scans associated with the subject to extract demographic information from the DICOM data.

        - The following DICOM tags are used to obtain demographic variables:
            - Subject Age: Retrieved from the tag ``(0010,1010)``.

            - Subject Date of Birth: Retrieved from the tag ``(0010,0030)``.

            - Subject Gender: Retrieved from the tag ``(0010,0040)``.

5. **Updating Demographics**:
    - The script updates the demographic fields for each subject:
        - Age: If available, the age is converted to an integer (removing the 'Y' suffix) or set to ``0`` if not specified.

        - Date of Birth: Updated with the retrieved value.

        - Gender: Mapped to descriptive strings ('Male', 'Female', 'Other') based on the retrieved value.

6. **Logging**:
    - Throughout the execution, the script logs actions and results, providing a clear record of the updates made to the demographic variables.

Example
~~~~~~~
Here's an example command to run the script:

::

    xnat-updatedemographics --project BrainStudy --username johndoe --server https://xnat.example.com --password mypassword --new_subjects_file new_subjects.json

This command updates the demographic variables for the subjects listed in ``new_subjects.json`` in the ``BrainStudy`` project on the specified XNAT server.

Sharing Module
--------------

Overview
~~~~~~~~
The ``share.py`` script is designed to facilitate the sharing of XNAT projects or data with other users. This script connects to an XNAT server and allows the user to grant access to specified projects, ensuring collaboration among researchers and team members.
Usage
~~~~~
To run the script, use the following command in the terminal:

::

    xnat-share --project <project_name> --username <username> --server <server_url> --password <password> --table <path/to/csv>

Command-Line Options
~~~~~~~~~~~~~~~~~~~~

- ``--project`` or ``-d``: (Required) The name of the XNAT project to be shared.

- ``--username`` or ``-u``: (Optional) The username for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--server`` or ``-s``: (Optional) The URL of the XNAT server. If not provided, the script will attempt to fetch stored credentials.

- ``--password`` or ``-p``: (Optional) The password for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--table`` or ``-t``: (Required) Path to CSV file containing the subject, source, and destination project data.

Workflow
~~~~~~~~

1. **Credential Management**:
    - The script checks if the username, server, and password are provided as command-line arguments.

    - If any are missing, it attempts to retrieve stored credentials using the get_credentials() function from the authenticate module. If the credentials are expired, it raises a CredentialExpiredError.

2. **Server Connection**:
    - The script connects to the specified XNAT server using the provided or retrieved credentials. If the connection fails, an error message is logged.

3. **Project Sharing**:
    - The script accesses the specified project within the XNAT session.

    - It iterates over the list of users provided in the --shared_users option and grants access to each user for the specified project.

4. **Logging**:
    - Throughout the execution, the script logs actions and results, providing a clear record of which users were granted access to the project.

Example
~~~~~~~
Here's an example command to run the script:

::

    xnat-share --project BrainStudy --username johndoe --server https://xnat.example.com --password mypassword --shared_users janedoe,robert

This command shares the BrainStudy project with the users janedoe and robert on the specified XNAT server.

Custom Form Fetch Module
-------------------------

The ``CustomForm Fetch`` and Combine script retrieves and combines custom form data from an XNAT server. 
It allows fetching populated data based on specific identifiers (project, subject, experiment, or form UUID) and merges it with form definitions to create a structured output.
To fetch custom form data, appropriate identifiers such as **Subject Accession ID**, **Experiment Accession ID**, or **Project ID** with corresponding labels must be provided.

Command for Fetching Custom Forms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``xnat-customformget`` command fetches custom form data from the XNAT server based on the provided identifiers. It requires at least one of the following combinations of parameters:

- **Subject Accession ID** (or **Subject Label with Project ID**).
- **Experiment Accession ID** (or **Experiment Label with Project ID**).

Credentials for the server can be passed via command-line options or retrieved from previously stored credentials (e.g., ``.netrc`` or JSON).

Usage
~~~~~

**Fetch data for a subject:**

::

    xnat-customformget --server <XNAT_SERVER_URL> --username <USERNAME> --password <PASSWORD> --subject-id <SUBJECT-ID>

**OR, if using a Subject Label:**

::

    xnat-customformget --project-id <PROJECT-ID> --subject-id <SUBJECT-LABEL>

**Fetch data for an experiment:**

::

    xnat-customformget --experiment-id <EXPERIMENT-ID>

**OR, if using an Experiment Label:**

::

    xnat-customformget --project-id <PROJECT-ID> --experiment-id <EXPERIMENT-LABEL>

Include UUID to target specific custom forms:

::

    xnat-customformget --experiment-id <EXPERIMENT-ID> --uuid <UUID>

Key Features
~~~~~~~~~~~~

1. **Mandatory identifiers**:
    - Requires either **Subject ID** (or Subject Label with Project ID) or **Experiment ID** (or Experiment Label with Project ID).

    - The UUID is optional but helps to narrow down results to a specific custom form.

2. **Command-line options:**
    - ``--server`` or ``-s``: The XNAT server URL (e.g., ``http://localhost``).
    - ``--username`` or ``-u``: The username for the XNAT server.
    - ``--password`` or ``-p``: The password for the XNAT server.
    - ``--uuid``: The UUID of the custom form to delete (mandatory).
    - ``--project-id`` or ``-d``: The project ID to specify the context for deletion.
    - ``--subject-id`` or ``-sid``: The subject ID or label.
    - ``--experiment-id`` or ``-eid``: The experiment ID or label.

3. **Credential management:**
    - Automatically fetches credentials from stored ``.netrc`` or JSON files if not provided on the command line.

4. **Logging:**
    - Logs are created in the ``logs`` folder in the current working directory.
    - Log filenames include a timestamp to ensure uniqueness and to track sessions.

5. **Error handling:** Provides meaningful error messages for issues such as:
    - Missing mandatory options like ``--uuid`` or required subject/experiment identifiers.
    - Failed connections to the XNAT server.

Error Handling
~~~~~~~~~~~~~~
In case of missing mandatory options or invalid identifiers, an appropriate error message is displayed and logged. For example:

::

    $ xnat-customformget --uuid INVALID_UUID
    Error: Insufficient parameters provided. UUID must be combined with valid subject or experiment identifiers.

Example
~~~~~~~
**Fetching custom form data for a subject:**

::

    xnat-customformget --subject-id SUBJECT123
    Using credentials from .netrc...
    Data retrieved successfully.

**Using labels:**

::

    xnat-customformget --project-id PROJECT123 --subject-id SUBJECT_LABEL

**Fetching specific custom form data using UUID:**

::

    xnat-customformget --experiment-id EXP12345 --uuid abc12345-6789
    
In case of errors:

::

    $ xnat-customformget --uuid abc12345-6789
    Error: Missing subject or experiment identifiers. Provide --subject-id or --experiment-id.

Logging Example
~~~~~~~~~~~~~~~
Logs provide detailed timestamps for every action:

::

    2024-11-24 10:30:10 - INFO - Using credentials from .netrc.
    2024-11-24 10:30:12 - INFO - Sending GET request to http://xnat.example.com/...
    2024-11-24 10:30:14 - INFO - Custom form data retrieved successfully.

File Structure
~~~~~~~~~~~~~~

- ``xnat-customformget``: Command for fetching custom form data from XNAT.
- Logs: Created in the ``logs`` folder with timestamps for each session.
- Error handling: Error messages for failed operations are displayed and logged.

Custom Form Update Module
-------------------------

The ``customform_put`` module provides functionality to update specific custom form data on an XNAT server. 
Updating a custom form requires identifiers such as **Subject Accession ID**, **Experiment Accession ID**, or **Project ID** with corresponding labels must be provided.

The custom form data must be provided as a JSON payload that adheres to the schema of the target form.

Command for Updating Custom Forms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``xnat-customformput command`` updates custom form data on the XNAT server based on the provided identifiers and payload. It requires one of the following combinations

- **Subject Accession ID** (or **Subject Label with Project ID**).
- **Experiment Accession ID** (or **Experiment Label with Project ID**).

In addition, the UUID of the custom form must always be provided to specify the target form.

Usage
~~~~~

**Update data for a subject:**

::

    xnat-customformput --server <XNAT_SERVER_URL> --username <USERNAME> --password <PASSWORD> --subject-id <SUBJECT-ID> --uuid <UUID> --json-file <PAYLOAD_FILE>

**OR, if using a Subject Label:**

::

    xnat-customformput --project-id <PROJECT-ID> --subject-id <SUBJECT-LABEL> --uuid <UUID> --json-file <PAYLOAD_FILE>

**Update data for an experiment:**

::

    xnat-customformput --experiment-id <EXPERIMENT-ID> --uuid <UUID> --json-file <PAYLOAD_FILE>

**OR, if using an Experiment Label:**

::

    xnat-customformput --project-id <PROJECT-ID> --experiment-id <EXPERIMENT-LABEL> --uuid <UUID> --json-file <PAYLOAD_FILE>

Include UUID to target specific custom forms:

::

    xnat-customformput --experiment-id <EXPERIMENT-ID> --uuid <UUID>


Key Features
~~~~~~~~~~~~

1. **Mandatory identifiers**:
    - Requires either **Subject ID** (or Subject Label with Project ID) or **Experiment ID** (or Experiment Label with Project ID).

    - The UUID is optional but helps to narrow down results to a specific custom form.

2. **Command-line options:**
    - ``--server`` or ``-s``: The XNAT server URL (e.g., ``http://localhost``).
    - ``--username`` or ``-u``: The username for the XNAT server.
    - ``--password`` or ``-p``: The password for the XNAT server.
    - ``--uuid``: The UUID of the custom form to delete (mandatory).
    - ``--project-id`` or ``-d``: The project ID to specify the context for deletion.
    - ``--subject-id`` or ``-sid``: The subject ID or label.
    - ``--experiment-id`` or ``-eid``: The experiment ID or label.
    - ``--json-file`` or ``-j`` Path to the JSON payload file containing the updated custom form data.

3. **Credential management:**
    - Automatically fetches credentials from stored ``.netrc`` or JSON files if not provided on the command line.

4. **Logging:**
    - Logs are created in the ``logs`` folder in the current working directory.
    - Log filenames include a timestamp to ensure uniqueness and to track sessions.

5. **Error handling:** Provides meaningful error messages for issues such as:
    - Missing mandatory options like ``--uuid`` or required subject/experiment identifiers.
    - Failed connections to the XNAT server.
    - Displays errors for missing parameters or invalid JSON payloads.

Error Handling
~~~~~~~~~~~~~~
In case of missing mandatory options or invalid identifiers, an appropriate error message is displayed and logged. For example:

::

    $ xnat-customformput --uuid INVALID_UUID
    Error: Insufficient parameters provided. UUID must be combined with valid subject or experiment identifiers.

Example
~~~~~~~
**Updating custom form data for a subject:**

::

    xnat-customformput --subject-id SUBJECT123 --uuid abc12345-6789 --payload updated_form.json
    Using credentials from .netrc...
    Custom form data updated successfully.

**Using labels:**

::

    xnat-customformput --project-id PROJECT123 --subject-id SUBJECT_LABEL --uuid abc12345-6789 --payload updated_form.json

**Updating data for an experiment:**

::

    xnat-customformput --experiment-id EXP12345 --uuid abc12345-6789 --payload updated_form.json
    
In case of errors:

::

    $ xnat-customformput --uuid abc12345-6789
    Error: Missing subject or experiment identifiers. Provide --subject-id or --experiment-id.

Logging Example
~~~~~~~~~~~~~~~
Logs provide detailed timestamps for every action:

::

    2024-11-24 10:30:10 - INFO - Using credentials from .netrc.
    2024-11-24 10:30:12 - INFO - Sending GET request to http://xnat.example.com/...
    2024-11-24 10:30:14 - INFO - Custom form data updated successfully.

File Structure
~~~~~~~~~~~~~~

- ``xnat-customformput``: Command for fetching custom form data from XNAT.
- Logs: Created in the ``logs`` folder with timestamps for each session.
- Error handling: Error messages for failed operations are displayed and logged.

Custom Form Delete Module
-------------------------

The ``customform_delete`` module provides functionality to delete specific custom form data from an XNAT server. It supports deletion at various levels, such as subject or experiment,
by utilizing the **custom form's UUID** in combination with identifiers like **Subject Accession ID**, **Experiment Accession ID**, or **Project ID** with appropriate labels. This ensures that data deletion is contextually accurate.
This module is particularly useful for maintaining data integrity and removing outdated or erroneous custom form entries from the XNAT server.

Command for Deleting Custom Forms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``xnat-customformdelete`` command deletes custom form data from the XNAT server. The UUID alone is insufficient for deletion; it must be accompanied by one of the following:

- **Subject Accession ID** (or **Subject Label with Project ID**).
- **Experiment Accession ID** (or **Experiment Label with Project ID**).

Credentials for the server can be passed via command-line options or retrieved from previously stored credentials (e.g., ``.netrc`` or JSON).

Usage
~~~~~

To delete custom form data, you must include the UUID alongside the appropriate subject or experiment identifiers:

**Delete data linked to a subject:**

::

    xnat-customformdelete --server <XNAT_SERVER_URL> --username <USERNAME> --password <PASSWORD> --subject-id <SUBJECT-ID> --uuid <UUID>

**OR using a subject label with a project:**

::

    xnat-customformdelete --project-id <PROJECT-ID> --subject-id <SUBJECT-LABEL> --uuid <UUID>

**Delete data linked to an experiment:**

::

    xnat-customformdelete --experiment-id <EXPERIMENT-ID> --uuid <UUID>

**OR using an experiment label with a project:**

::

    xnat-customformdelete --project-id <PROJECT-ID> --experiment-id <EXPERIMENT-LABEL> --uuid <UUID>

If your credentials are already stored using the ``xnat-authenticate`` command, you can omit the ``--server``, ``--username``, and ``--password`` options:

::

    xnat-customformdelete --subject-id <SUBJECT-ID> --uuid <UUID>

Key Features
~~~~~~~~~~~~

1. **Mandatory identifiers**: The ``xnat-customformdelete`` command requires the UUID in combination with either:
    - **Subject Accession ID** or **Subject Label** with a **Project ID**.

    - **Experiment Accession ID** or **Experiment Label** with a **Project ID**.

Without these combinations, deletion cannot proceed.

2. **Command-line options:**
    - ``--server`` or ``-s``: The XNAT server URL (e.g., ``http://localhost``).
    - ``--username`` or ``-u``: The username for the XNAT server.
    - ``--password`` or ``-p``: The password for the XNAT server.
    - ``--uuid``: The UUID of the custom form to delete (mandatory).
    - ``--project-id`` or ``-d``: The project ID to specify the context for deletion.
    - ``--subject-id`` or ``-sid``: The subject ID or label.
    - ``--experiment-id`` or ``-eid``: The experiment ID or label.
    - ``--uuid`` or ``-uuid``: Custom form UUID. Specify to delete a particular form. Omit to delete all forms.
    - ``--help`` or ``-h``: Displays the help text for usage and options.

3. **Credential management:**
    - Automatically fetches credentials from stored ``.netrc`` or JSON files if not provided on the command line.

4. **Logging:**
    - Logs are created in the ``logs`` folder in the current working directory.
    - Log filenames include a timestamp to ensure uniqueness and to track sessions.

5. **Error handling:** Provides meaningful error messages for issues such as:
    - Missing mandatory options like ``--uuid`` or required subject/experiment identifiers.
    - Failed connections to the XNAT server.
    - Server errors during the delete operation.

Functionality
~~~~~~~~~~~~~

The script allows the user to:
    - **Delete a specific form** by providing its UUID.
    - **Delete all forms** for a subject or an experiment by omitting the UUID.
    - Logs detailed actions and errors in a timestamped log file stored in the ``logs`` directory.

**Form Deletion Scenarios:**
    - Delete a specific form:
        - Requires ``--uuid`` parameter.
        - Automatically constructs the API URL to locate the form and send a DELETE request.
    - Delete all forms:
        - Fetches all form IDs associated with the subject/experiment.
        - Iteratively sends DELETE requests for each form ID.

Error Handling
~~~~~~~~~~~~~~
In case of missing mandatory options or invalid identifiers, an appropriate error message is displayed and logged. For example:

::

    $ xnat-customformdelete --uuid INVALID_UUID
    Error: Insufficient parameters provided. UUID must be combined with valid subject or experiment identifiers.

Example
~~~~~~~
**Deleting custom form data linked to a subject:**

::

    $ xnat-customformdelete --subject-id SUBJECT123 --uuid abc12345-6789
    Using credentials from .netrc...
    Custom form data deleted successfully.

**Deleting custom form data linked to an experiment:**

::

    $ xnat-customformdelete --experiment-id EXP12345 --uuid abc12345-6789
    Using credentials from .netrc...
    Custom form data deleted successfully.

**Using labels with a project context:**

::

    $ xnat-customformdelete --project-id PROJECT123 --subject-id SUBJECT_LABEL --uuid abc12345-6789
    Custom form data deleted successfully.
    
In case of errors:

::

    $ xnat-customformdelete --uuid abc12345-6789
    Error: Missing subject or experiment identifiers. Provide --subject-id or --experiment-id.

Logging Example
~~~~~~~~~~~~~~~
Logs provide detailed timestamps for every action:

::

    2024-11-24 10:30:10 - INFO - Using credentials from .netrc.
    2024-11-24 10:30:12 - INFO - Sending DELETE request to http://xnat.example.com/...
    2024-11-24 10:30:14 - INFO - Custom form data deleted successfully.

File Structure
~~~~~~~~~~~~~~

- ``xnat-customformdelete``: Command for deleting custom form data from XNAT.
- Logs: Created in the ``logs`` folder with timestamps for each session.
- Error handling: Error messages for failed operations are displayed and logged.

Unshare Module
--------------

Overview
~~~~~~~~
The ``unshare.py`` script is designed to facilitate the removal of access to XNAT projects or data from specified users.
This script connects to an XNAT server and allows the user to unshare subjects and their associated experiments from a specified project, streamlining the process of managing project access.
Usage
~~~~~
To run the script, use the following command in the terminal:

::

    xnat-unshare --project-id <project_id> --username <username> --server <server_url> --sublist <path/to/subject_list.txt>

Command-Line Options
~~~~~~~~~~~~~~~~~~~~

- ``--project-id`` or ``-p``: (Required) The ID of the XNAT project from which subjects will be unshared.

- ``--username`` or ``-u``: (Optional) The username for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--server`` or ``-s``: (Optional) The URL of the XNAT server. If not provided, the script will attempt to fetch stored credentials.

- ``--sublist`` or ``-sl``: (Required) Path to a text file containing the list of subjects to be unshared.

Workflow
~~~~~~~~

1. **Credential Management**:
    - The script checks if the username and server are provided as command-line arguments.

    - If any are missing, it attempts to retrieve stored credentials using the `get_credentials()` function from the `authenticate` module. If the credentials are expired, it raises a `CredentialExpiredError`.

2. **Server Connection**:
    - The script connects to the specified XNAT server using the provided or retrieved credentials. If the connection fails, an error message is logged.

3. **Subject and Experiment Unsharing**:
    - The script accesses the specified project within the XNAT session.

    - It iterates over the subjects listed in the `--sublist` file, attempting to unshare each subject and its associated experiments.

    - The unsharing process is performed via API calls to the server, and success or failure messages are logged accordingly.

4. **Logging**:
    - The script logs all actions and results, providing a clear record of which subjects and experiments were successfully unshared.

Example
~~~~~~~
Here's an example command to run the script:

::

    xnat-unshare --project-id BrainStudy --username johndoe --server https://xnat.example.com --sublist /path/to/subjects.txt

This command unshares all subjects listed in the `subjects.txt` file from the `BrainStudy` project on the specified XNAT server.

Move Module
-----------

Overview
~~~~~~~~
The ``move.py`` script is designed to facilitate the movement of subjects and their associated experiments between XNAT projects. 
This script connects to an XNAT server and allows users to transfer data seamlessly, either individually or in bulk using a CSV file.
Usage
~~~~~
To run the script, use the following command in the terminal:

::

    xnat-move --server <server_url> --username <username> --password <password> --source <source_project> --destination <destination_project> --table <path/to/csv>

Command-Line Options
~~~~~~~~~~~~~~~~~~~~

- ``--server`` or ``-s``: (Optional) The URL of the XNAT server. If not provided, the script will attempt to fetch stored credentials.

- ``--username`` or ``-u``: (Optional) The username for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--password`` or ``-p``: (Optional) The password for XNAT. If not provided, the script will attempt to fetch stored credentials.

- ``--source`` or ``-sp``: (Optional) The name of the source XNAT project from which subjects will be moved.

- ``--destination`` or ``-dp``: (Optional) The name of the destination XNAT project to which subjects will be moved.

- ``--table`` or ``-t``: (Optional) Path to a CSV file containing the list of subjects and their source and destination project data. The CSV should have the columns subject, source, and destination.

- ``--subject`` or ``-sj``: (Optional) The ID of a single subject to be moved. If provided, the script will move only this subject.

Workflow
~~~~~~~~

1. **Credential Management**:
    - The script checks if the server, username, and password are provided as command-line arguments.

    - If any are missing, it attempts to retrieve stored credentials using the get_credentials() function.

2. **Mode of Operation**:
    - The script supports multiple modes of operation:

    - Single Subject Mode: Move a single subject using the --subject option.

    - CSV Mode: Read subjects and projects from a CSV file using the --table option.

    - Project Mode: Move all subjects from a source project to a destination project if both are specified.
    
3. **Server Connection**:
    - The script connects to the XNAT server using the provided or retrieved credentials. Connection issues are logged as errors.

4. **Project and Subject Validation**:
    - The script checks the availability of source and destination projects.

    - It fetches subjects from the source project if not provided via a CSV or CLI.

5. **Data Movement**:
    - Temporarily shares subjects with the destination project.

    - Moves each experiment associated with the subject permanently.

    - Moves the subject permanently if all experiments are successfully moved.

6. **Logging**:
    - The script logs each step, providing clear information about the success or failure of moving subjects and experiments.

Example
~~~~~~~
Here's an example command to move subjects using a CSV file:

::

    xnat-move --server https://xnat.example.com --username johndoe --password mypassword --table /path/to/subjects.csv

Alternatively, to move all subjects from one project to another:

::

    xnat-move --server https://xnat.example.com --username johndoe --password mypassword --source OldProject --destination NewProject

And for a single subject:

::

    xnat-move --server https://xnat.example.com --username johndoe --password mypassword --source OldProject --destination NewProject --subject Subject123
