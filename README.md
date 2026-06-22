# ecs-syslog
syslog patterns for linux syslog

small project to create a pipeline pattern to handle linux applications sent by rsyslog.

the pipelines are set up in such a fashion that an event that reaches the input pipeline 
will be branched to reach the correct pipeline with rules specially for that application.

a server is set up to send syslog data to the input logstash filter at the port
specified by the input pipeline.

syslog udp/tcp -> logstash_input_pipeline

the input pipeline sends the data to the syslog pipeline, this is so i can expand later
and add other forms such as beat, or other pipelines.

logstash/input_pipeline -> syslog_pipeline

The syslog pipeline parses the syslog data, and takes care of timestamps, hostnames
facility, etc. It also looks at what application sent the event and populates the
[log][syslog] fields.
Depending on what [log][syslog][appname] the syslog pipeline sends it to a 
pipeline for that application, that inderstands what [event][success] etc are for the 
diffrent events from the app.

right now it has a pipeline for sshd, and some other applications. more to come.

After the application pipeline, it gets sent to output pipeline.
the output pipeline handle some very general rules that are the same for many pipelines.

the idea is to provide a good list of applications that are parsed, and provide them
in a pipeline format so they can be dropped in and placed in your loggflows.
each pipeline can also be placed on its own logstash instance, and communicate through 
IP. input and output filters can be changed to read data/write from kafka, or something else

