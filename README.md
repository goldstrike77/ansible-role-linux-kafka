![](https://img.shields.io/badge/Ansible-kafka-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_kafka.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Kafka Versions](#kafka-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
This Ansible role installs apache Kafka on linux operating system, including establishing a filesystem structure and server configuration with some common operational features.

## Requirements
### Operating systems
This role will work on the following operating systems:

  * CentOS 7

### Kafka versions

The following list of supported the kafka releases:

* Apache Kafka 2.3.1

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `kafka_version`: Specify the Kafka version.
* `kafka_cluster`: Cluster name of servers that implements distribution performance.
* `kafka_path`: Specify the Kafka working directory.
* `kafka_java_home`: Environment variable to point to an installed JDK.

##### JVM memory Variables
* `kafka_jvm_xmx`: Size of the heap in MB.

##### ACL Variables
* `kafka_enable_auth`: Whether enable authentication using SASL.
* `kafka_zoo_client_arg`: Zookeeper authentication information.
* `kafka_user_client_arg`: # Client authentication information.

##### Service Mesh
* `environments`: Define the service environment.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

##### Zookeeper parameters
* `kafka_zoo_servers`: List of Zookeeper servers.
* `kafka_zoo_port`: Zookeeper server listens.

##### Listen port
* `kafka_port.socket`: Server listens.
* `kafka_port.exporter`: Prometheus exporter listens.
* `kafka_port.wrapper`: A socket to communicate with its Java component running inside a JVM.
* `kafka_port.wrapper_jvm`: A socket to communicate with its Java component running inside a JVM.

##### System Variables
* `kafka_arg.num_network_threads` The number of threads that the server uses for receiving requests.
* `kafka_arg.num_io_threads`: The number of threads that the server uses for processing requests.
* `kafka_arg.log_flush_interval_messages`: The number of messages to accept before forcing a flush of data to disk.
* `kafka_arg.log_flush_interval_ms`: The maximum amount of time a message can sit in a log before we force a flush.
* `kafka_arg.socket_send_buffer_bytes`: The send buffer (SO_SNDBUF) used by the socket server.
* `kafka_arg.socket_receive_buffer_bytes`: The receive buffer (SO_RCVBUF) used by the socket server.
* `kafka_arg.socket_request_max_bytes`: The maximum size of a request that the socket server will accept.
* `kafka_arg.lc_ctype`: The language of messages.
* `kafka_arg.user`: Kafka running user.
* `kafka_arg.ulimit_core`: The number of core dump launched by systemd.
* `kafka_arg.ulimit_nofile`: The number of files launched by systemd.
* `kafka_arg.ulimit_nproc`: The number of processes launched by systemd.
* `kafka_arg.wrapper_console_format`: Format to use for output to the console.
* `kafka_arg.wrapper_console_loglevel`: Log level to use for console output.
* `kafka_arg.wrapper_java_command_loglevel`: Log level to use for Java command.
* `kafka_arg.wrapper_logfile`: Sets the path to the Wrapper log file.
* `kafka_arg.wrapper_logfile_format`: Format to use for logging to the log file.
* `kafka_arg.wrapper_logfile_loglevel`: Filters messages sent to the log file according to their log levels.
* `kafka_arg.wrapper_logfile_maxfiles`: Controls the maximum number of rolled files.
* `kafka_arg.wrapper_logfile_maxsize`: Controls the maximum size of rolled files.
* `kafka_arg.wrapper_syslog_loglevel`: Log level to use for logging to the Event Log on syslog on UNIX systems.
* `kafka_arg.wrapper_ulimit_loglevel`: Controls at which log level resource limits will be logged.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
There are no dependencies on other roles.

## Example

### Hosts inventory file
See tests/inventory for an example.

    node01 ansible_host='192.168.1.10' kafka_version='2.3.1'

### Vars in role configuration
Including an example of how to use your role for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: ansible-role-linux-kafka
           kafka_version: '2.3.1'

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

    kafka_version: '2.3.1'
    kafka_cluster: 'cluster'
    kafka_path: '/data'
    kafka_java_home: '/usr/lib/jvm/java'
    kafka_jvm_xmx: '2048'
    kafka_enable_auth: true
    kafka_zoo_client_arg:
      - user: 'kafka'
        pass: 'changeme'
      - user: 'somebody'
        pass: 'changeme'
    kafka_port:
      socket: '9092'
      exporter: '9308'
      wrapper: '35000-35999'
      wrapper_jvm: '36000-36999'
    kafka_arg:
      num_network_threads: '10'
      num_io_threads: '20'
      log_flush_interval_messages: '10000'
      log_flush_interval_ms: '1000'
      socket_send_buffer_bytes: '1048576'
      socket_receive_buffer_bytes: '1048576'
      socket_request_max_bytes: '104857600'
      lc_ctype: 'zh_CN.UTF-8'
      user: 'kafka'
      ulimit_core: 'unlimited'
      ulimit_nofile: '10240'
      ulimit_nproc: '10240'
      wrapper_console_format: 'PM'
      wrapper_console_loglevel: 'INFO'
      wrapper_java_command_loglevel: 'NONE'
      wrapper_logfile: 'kafka.out'
      wrapper_logfile_format: 'ZM'
      wrapper_logfile_loglevel: 'INFO'
      wrapper_logfile_maxfiles: '25'
      wrapper_logfile_maxsize: '50m'
      wrapper_syslog_loglevel: 'NONE'
      wrapper_ulimit_loglevel: 'STATUS'
    kafka_zoo_servers:
      - '127.0.0.1'
    kafka_zoo_port: '2181'
    environments: 'Development'
    tags:
      subscription: 'default'
      owner: 'nobody'
      department: 'Infrastructure'
      organization: 'The Company'
      region: 'IDC01'
    exporter_is_install: false
    consul_public_register: false
    consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
    consul_public_http_port: '8500'
    consul_public_clients:
      - '127.0.0.1'

## License

![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
