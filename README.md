# nagios-plugin-instraclustr

This project contains Nagios plugins that gets metrics from
[Instaclustr](https://www.instaclustr.com) using API calls. Performance data is
provided for graphing purposes. The current plugins:

* check_consumergroupclient: monitor Kafka consumer group client metrics
* check_instaclustr: monitor performance metrics (e.g. cpuUtilization) of nodes in a Kafka cluster. Performance data will be created for each node separately, but alerts will be generated based on average value of their metrics.

# Prerequisites

You will need Ruby - version 2.4.x and 2.7.x have been tested. You will also
need the rest-client gem:

    $ gem install rest-client

You will need to have ruby-dev / ruby-devel package installed before installing
the gem as rest-client requires building Ruby native extensions.

# Usage

All parameters are passed on the command-line

    ./check_consumergroupclient -H api.instaclustr.com -u <instaclustr-username> -p <password> -C <cluster-id> -g <consumer-group> -t <topic> -i <client-id> -w 300 -c 600 -m consumerlag
    ./check_instaclustr -H api.instaclustr.com -u <instaclustr-username> -p <password> -C <cluster-id> -m <metric> -w 50 -c 80

If the metric you're tracking is such that low values are bad (e.g. memavailable) then you can use "-r" to reverse alerting logic. For example:

    ./check_instaclustr -H api.instaclustr.com -u <instaclustr-username> -p <password> -C <cluster-id> -m memavailable -w 30000 -c 10000 -r

For further information run the scripts without arguments or with "-h" or "--help".

# Valid metric names

These plugins validate metric names before making the API calls. Valid values are taken from the InstaClustr's
[Monitoring API documentation](https://www.instaclustr.com/support/api-integrations/api-reference/monitoring-api/#),
but for convenience they're listed here:

* check_consumergroupclient
    * consumercount (integer)
    * consumerlag (integer)
    * partitioncount (integer)
* check_instaclustr
    * cpuUtilization (%)
    * osload (%)
    * diskUtilization (%)
    * cpuguestpercent (%)
    * cpuguestnicepercent (%)
    * cpusystempercent (%)
    * cpuiowaitpercent (%)
    * cpuirqpercent (%)
    * cpunicepercent (%)
    * cpusoftirqpercent (%)
    * cpustealpercent (%)
    * cpuuserpercent (%)
    * memavailable (kilobytes?)
    * networkoutdelta (counter)
    * networkindelta (counter)
    * networkouterrorsdelta (counter)
    * networkinerrorsdelta (counter)
    * networkoutdroppeddelta (counter)
    * networkindroppeddelta (counter)
    * tcpall (integer)
    * tcpestablished (integer)
    * tcplistening (integer)
    * tcptimewait (integer)
    * tcpclosewait (integer)
    * filedescriptorlimit (integer)
    * filedescriptoropencount (integer)
    * cpuidlepercent (%)

Please note that the identical metrics can be queried for both Kafka and Cassandra clusters with check_instaclustr. 

# Plugin output

Plugin output is based on the format described here:

* https://nagios-plugins.org/doc/guidelines.html#AEN200

When no thresholds are exceeded with check_consumergroupclient:

    OK - Kafka consumerlag is 6 for topic my_topic | consumerlag=6;;;;

In case of warnings and/or errors the first part of the output looks different:

    CRITICAL - Kafka consumerlag 1453 for topic my_topic exceeds the threshold of 600! | consumerlag=1453;;;;

Output of check_instaclustr will be like this:

    InstaClustr cluster 21cd9dab OK - average cpuUtilization is 2.1% | cpuUtilization_5af74cb2=1.6%;;;; cpuUtilization_7b28ca31=1.9%;;;; cpuUtilization_b521da41=2.8%;;;;

# LICENSE

This plugin is released under the terms of the [GPLv2 license](LICENSE).
