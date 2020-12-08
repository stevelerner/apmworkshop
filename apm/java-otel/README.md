### This lab requires starting from the main [APM Instrumentation Workshop](../workshop-steps/3-workshop-labs.md)

Requirements:  
`java 8 JDK` (`sudo apt install -y openjdk-8-jdk`)  
`maven` (`sudo apt-get install -y maven`)

#### Step #1 Open new terminal to your Linux instance (or open new pane in tmux)

Make sure that you still have the Python Flask server from Workshop Activity #2 running. If you accidentally shut it down follow steps from Workshop #2 to restart the Python Flask server.

Make sure you are in the right directory to start the Java activities:  
`cd ./apmworkshop/apm/java`

#### Step #2 Download current Splunk OpenTelemetry Java Auto-instrumentation

Install Splunk OpenTelemetry Java Auto-instrumentation into `/opt`  

`source install-java-otel.sh`

#### Step #3 Set up environment and run the Java example with OKHTTP requests

```
source setup-otel-apm-env.sh  
source run-client.sh
```

You will see requests printed to the window

#### Step #4 Traces / services will now be viewable in the APM dashboard

A new service takes about 90 seconds to register for the first time, and then all data will be available in real time.
Additionally span IDs will print in the terminal where flask-server.py is running.
You can use `ctrl-c` to stop the requests and server any time.

You should now see a new Java requests service alongside the Python one.

#### Step #5 Check Splunk SignalFx SmartAgent to see that spans are being sent

Open a new terminal window to your Linux instance (or use `tmux` and run in separate pane)

`signalfx-agent status` will show the metrics and spans being sent by the agent like this:

```
ubuntu@primary:~$ signalfx-agent status
SignalFx Agent version:           5.5.1
Agent uptime:                     1h31m32s
Observers active:                 host
Active Monitors:                  10
Configured Monitors:              10
Discovered Endpoint Count:        8
Bad Monitor Config:               None
Global Dimensions:                {host: primary}
GlobalSpanTags:                   map[]
Datapoints sent (last minute):    370
Datapoints failed (last minute):  0
Datapoints overwritten (total):   0
Events Sent (last minute):        6
Trace Spans Sent (last minute):   1083
Trace Spans overwritten (total):  0
```

Notice **Trace Spans Sent (last minute):   1083**  
This means spans are succssfully being sent to Splunk APM.

 
#### Step #6 Where is the auto-instrumentation?

In the `run-client.sh` script is the java command:

```
mvn compile exec:exec \
  -Dexec.executable="java" \
  -Dexec.args="-javaagent:/opt/splunk-otel-javaagent.jar -cp %classpath sf.main.GetExample"
```

The `splunk-otel-javaagent.jar` file is the OpenTelemetry automatic instrumentation that will emit spans from the app. No code changes are necessary.

Splunk's OpenTelemetry autoinstrumentation for java is here: https://github.com/signalfx/splunk-otel-java

You can now go to the next step of [APM Instrumentation Workshop](../workshop-steps/3-workshop-labs.md)