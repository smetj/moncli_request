Moncli Request


moncli_request is a script which allows you to generate and submit Requests to Moncli using templates.
These templates are JSON files with some dynamic data missing such as subject, dates, destination host.
These missing values are completed by moncli_request based upon parameters it receives and the current time.


When executing moncli_request you have to define following parameters:

    user@localhost $ moncli_request --broker host_name --host host_name --subject subject --repository absolute_path

* broker:

    This is the name of the RabbitMQ message broker. When omitted, the JSON document will be printed to STDOUT.

* host

    Since each MonCli instance consumes on the RabbitMQ broker a queue with its own hostname.  If you want to reach the MonCli client of 
    a certain host then you define the name of that host/queue here.  A queue with this name should exist in the RabbitMQ environment.

* subject

    Each report request has a subject.  The subject (besides UUIDs) is a helpful value to differenciate your reports from each other.
    In your monitoring framework such as Nagios the subject could be the service name.

* repository

    The directory containing the json "templates" which will be completed by Moncli.
    The repository has following structure:

        repository/
        ├── Behemoth
        │   ├── Memory
        │   └── Processes
        ├── .default
        │   ├── Cpu
        │   ├── Disk
        │   ├── IOstat
        │   ├── Memory
        │   └── Ping
        └── Sandbox
            └── Disk

    Each repository should have at minimum a .default directory.
    The hostname parameter refers to the first level of directories.
    The subject paramter refers to the second level of directories.

    Whenever repository/hostname/subject does not exist, moncli_request will try to find repository/.default/subject.
    If that doesn't exist, it will give up.

    When moncli_request finds a match either in hostname/subject or .default/subject it will complete that JSON request
    with the necessary fields and submit it to the RabbitMQ broker infrastructure.
