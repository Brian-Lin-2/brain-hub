# JMeter

Java-based multithreading tool that is mainly used for performance testing.

## Execution Order

`Thread Group` - Determines how many concurrent users (threads) to simulate
`Config Elements` - The setup (ie. `HTTP Cookie Manager`)
`Samplers` - The actual work (ie. `HTTP Request`)
`Post-Processors` - Data Extraction
`Assertions` - Pass/Fail Check

## Basic CLI Usage

`jmeter -n -t path/to/script.jmx -l results.jtl -e -o ./dashboard-folder`

| Flag | Meaning                                                     |
| ---- | ----------------------------------------------------------- |
| `-n` | **Non-GUI**: Runs JMeter in headless mode.                  |
| `-t` | **Test Plan**: Path to your `.jmx` file.                    |
| `-l` | **Log**: Saves raw results to a `.jtl` file.                |
| `-e` | **Export**: Generates an HTML dashboard after the test run. |
| `-o` | **Output**: Folder where the HTML dashboard is written.     |

## What Is a `.jmx` File?

A `.jmx` file is your **JMeter test plan**. It is an XML file that stores:

- Thread groups (virtual users, ramp-up, loops)
- Samplers (HTTP requests, JDBC requests, etc.)
- Config elements, assertions, timers, and listeners

Think of it as the source-of-truth definition for how the load test runs. This is essentially the config file JMeter reads to actually run your load test.

**Example:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jmeterTestPlan version="1.2" properties="5.0" jmeter="5.6.3">
  <hashTree>
    <TestPlan guiclass="TestPlanGui" testclass="TestPlan" testname="Minimal Plan">
      <elementProp name="TestPlan.user_defined_variables" elementType="Arguments" guiclass="ArgumentsPanel" testclass="Arguments">
        <collectionProp name="Arguments.arguments"/>
      </elementProp>
    </TestPlan>
    <hashTree>
      <ThreadGroup guiclass="ThreadGroupGui" testclass="ThreadGroup" testname="One User">
        <elementProp name="ThreadGroup.main_controller" elementType="LoopController" guiclass="LoopControlPanel" testclass="LoopController">
          <stringProp name="LoopController.loops">1</stringProp>
        </elementProp>
        <stringProp name="ThreadGroup.num_threads">1</stringProp>
        <stringProp name="ThreadGroup.ramp_time">1</stringProp>
      </ThreadGroup>
      <hashTree>
        <HTTPSamplerProxy guiclass="HttpTestSampleGui" testclass="HTTPSamplerProxy" testname="HTTP Request">
          <stringProp name="HTTPSampler.domain">www.example.com</stringProp>
          <stringProp name="HTTPSampler.protocol">https</stringProp>
          <stringProp name="HTTPSampler.path">/</stringProp>
          <stringProp name="HTTPSampler.method">GET</stringProp>
        </HTTPSamplerProxy>
        <hashTree/>
      </hashTree>
    </hashTree>
  </hashTree>
</jmeterTestPlan>
```

## What Is a `.jtl` File?

A `.jtl` file is a **results/log output file** generated while the test runs.
It records request-level metrics such as:

- Response time
- Success/failure
- Timestamps
- Throughput-related data

You can use this file to:

- Review raw performance data
- Generate HTML dashboards
- Feed results into CI/CD reporting or analysis tools

**Example:**

```txt
timeStamp,elapsed,label,responseCode,responseMessage,threadName,dataType,success,failureMessage,bytes,sentBytes,grpThreads,allThreads,URL,Latency,IdleTime,Connect
1713830400000,245,Home Page,200,OK,Thread Group 1-1,text,true,,1540,320,5,5,https://example.com/index,210,0,45
1713830400500,512,Login API,200,OK,Thread Group 1-2,text,true,,850,410,5,5,https://example.com/api/login,480,0,110
1713830401200,1205,Search,500,Internal Server Error,Thread Group 1-3,text,false,Expected status 200,420,380,5,5,https://example.com/search,1190,0,60
```

Since its just a csv file, you can easily parse this `.jtl` file to get detailed statistics about your JMeter test.

## End-to-End Example

Run a test plan, save raw results, and generate an HTML report:

`jmeter -n -t ./plans/login-load-test.jmx -l ./results/login-test.jtl -e -o ./reports/login-dashboard`
