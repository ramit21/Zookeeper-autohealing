# Auto healing using Zookeeper

To run the project, 
follow below steps:

1. Download zookeeper and start the zookeeper server.

2. run `mvn clean install` in both autohealer and falky.worker modules. 

3. To run the autohealer, which in turn would launch and maintain 10 workers:

Run `java -jar target/autohealer-1.0-SNAPSHOT-jar-with-dependencies.jar <number of workers> <path to woker jar>
Example: autohealer/src> `java -jar target/autohealer-1.0-SNAPSHOT-jar-with-dependencies.jar 10 "../flakyworker/target/flaky.worker-1.0-SNAPSHOT-jar-with-dependencies.jar"`


## About the POC:
1. Autohealer launches multiple instances of workers.
2. Each worker creates an ephemeral znode under /workers root znode in zokeeper.
3. Worker has a intentional bug in code which brings it down, and thus removes the ephemeral znode that it had created.
4. The autohealer is implementing the Watcher interface, and on the **NodeChildrenChanged** event, it gets informed by zookeeper if any of the znode created by the worker has been deleted.
5. Auto healer then takes remedial action and launches another instance of the worker, and thus maintaining the specified no. of worker isntances at all times.
