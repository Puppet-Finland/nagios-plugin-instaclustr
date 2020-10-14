# nagios-plugin-instraclustr

This project contains Nagios plugins that gets metrics from
[Instaclustr](https://www.instaclustr.com) using API calls. Performance data is
provided for graphing purposes. The current plugins:

* check_consumerlag: monitor Kafka consumerlag
* check_instaclustr: monitor performance metrics (e.g. cpuUtilization) of nodes in a Kafka cluster

# Prerequisites

You will need Ruby - version 2.4.x and 2.7.x have been tested. You will also
need the rest-client gem:

    $ gem install rest-client

You will need to have ruby-dev / ruby-devel package installed before installing
the gem as rest-client requires building Ruby native extensions.

# Usage

All parameters are passed on the command-line

    ./check_consumerlag -H api.instaclustr.com -u <instaclustr-username> -p <password> -C <cluster-id> -g <consumer-group> -t <topic> -i <client-id> -w 300 -c 600
    ./check_instaclustr -H api.instaclustr.com -u <instaclustr-username> -p <password> -C <cluster-id> -m <metric> -w 50 -c 80

For further information run the scripts without arguments or with "-h" or "--help".

# Plugin output

Plugin output is based on the format described here:

* https://nagios-plugins.org/doc/guidelines.html#AEN200

When no thresholds are exceeded the output will be similar to this:

    OK - Kafka consumer lag is 6 for topic my_topic | consumer_lag=6;;;;

In case of warnings and/or errors the first part of the output looks different:

    CRITICAL - Kafka consumer lag 1453 for topic my_topic exceeds the threshold of 600! | consumer_lag=1453;;;;

# LICENSE

This plugin is released under the terms of the [GPLv2 license](LICENSE).
