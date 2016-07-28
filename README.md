# ansible-telegraf [![Build Status](https://travis-ci.org/pbonrad/ansible-telegraf.svg?branch=master)](https://travis-ci.org/pbonrad/ansible-telegraf)

Telegraf is an open source agent written in Go for collecting metrics and data on the system it's running on or from other services. Telegraf writes data it collects to InfluxDB in the correct format.

More information about Telegraf can be found here:
[https://influxdata.com/time-series-platform/telegraf/](https://influxdata.com/time-series-platform/telegraf/)

This role installs Telegraf on the target server using the `apt-get` package manager. It works for Ubuntu and Debian servers and was tested with the help of docker containers. In comparison to other Ansible role tests where Ansible runs inside the container and is connecting to localhost, I decided to use the [Ansible docker connection](http://docs.ansible.com/ansible/intro_inventory.html#non-ssh-connection-types) (`ansible_connection=docker`). The build which run at [Travis CI](https://travis-ci.org/pbonrad/ansible-telegraf) uses this functionality.

See also:
* GitHub project with Dockerfiles:  [https://github.com/pbonrad/ansible-docker-base](https://github.com/pbonrad/ansible-docker-base)
* Role on Ansible Galaxy:  [https://galaxy.ansible.com/pbonrad/telegraf/](https://galaxy.ansible.com/pbonrad/telegraf/)

## Role Variables

Here you can find a complete configuration file together with the explaining comments:
[https://github.com/influxdata/telegraf/blob/master/etc/telegraf.conf](https://github.com/influxdata/telegraf/blob/master/etc/telegraf.conf)

The original documentation could be helpful too:
[https://github.com/influxdata/telegraf/blob/master/docs/CONFIGURATION.md](https://github.com/influxdata/telegraf/blob/master/docs/CONFIGURATION.md)

Specifiying output roles like this:

    telegraf_output_plugins:
      - name: "influxdb"
        config:
          urls: ["http://localhost:8086"]
          database: "telegraf"

And input plugins can be configured that way:

    telegraf_input_plugins:
      - name: cpu
        config:
          percpu: "true"
          totalcpu: "true"
          fielddrop: ["time_*"]
      - name: disk
        config:
          ignore_fs: ["tmpfs", "devtmpfs"]
      - name: diskio
      - name: kernel

## Dependencies

There are no dependencies to other roles. If you want to run the test, you need to install [Docker](https://www.docker.com/).

As a default telegraf writes the collected metrics into a InfluxDB which needs to be accessible in the network. But other datasources can be used too.

## Example Playbook

An example playbook is included in the `test.yml` file. You can use `run.sh` for running a test locally, which starts a docker container as the target.

    - hosts: all
      roles:
         - role: ansible-telegraf

## Contributions and Feedback

Any contributions are welcome. For any bugs or feature requests, please open an issue through [Github](https://github.com/pbonrad/ansible-telegraf/issues).

## License

MIT

## Author Information

Peter Bonrad - [pbonrad](https://github.com/pbonrad) - 2016
