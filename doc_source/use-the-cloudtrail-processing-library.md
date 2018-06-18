# Using the CloudTrail Processing Library<a name="use-the-cloudtrail-processing-library"></a>

The CloudTrail Processing Library is a Java library that provides an easy way to process AWS CloudTrail logs\. You provide configuration details about your CloudTrail SQS queue and write code to process events\. The CloudTrail Processing Library does the rest\. It polls your Amazon SQS queue, reads and parses queue messages, downloads CloudTrail log files, parses events in the log files, and passes the events to your code as Java objects\. 

The CloudTrail Processing Library is highly scalable and fault\-tolerant\. It handles parallel processing of log files so that you can process as many logs as needed\. It handles network failures related to network timeouts and inaccessible resources\.

The following topic shows you how to use the CloudTrail Processing Library to process CloudTrail logs in your Java projects\.

The library is provided as an Apache\-licensed open\-source project, available on GitHub:

+ [https://github.com/aws/aws-cloudtrail-processing-library](https://github.com/aws/aws-cloudtrail-processing-library)

The library source includes sample code that you can use as a base for your own projects\.


+ [Minimum Requirements](#use-the-cloudtrail-processing-library-prerequisites)
+ [Processing CloudTrail Logs](#use-the-cloudtrail-processing-library-how-to)
+ [Advanced Topics](#use-the-cloudtrail-processing-library-advanced)
+ [Additional Resources](#UsingProcessingLib-ar)

## Minimum Requirements<a name="use-the-cloudtrail-processing-library-prerequisites"></a>

To use the CloudTrail Processing Library, you must have the following:

+ [AWS SDK for Java 1\.11\.135](https://github.com/aws/aws-sdk-java)

+ [Java 1\.7](http://docs.oracle.com/javase/7/docs/webnotes/install/)

## Processing CloudTrail Logs<a name="use-the-cloudtrail-processing-library-how-to"></a>

To process CloudTrail logs in your Java application:

1. [Adding the CloudTrail Processing Library to Your Project](#use-the-cloudtrail-processing-library-add-to-project)

1. [Configuring the CloudTrail Processing Library](#use-the-cloudtrail-processing-library-configure)

1. [Implementing the Events Processor](#use-the-cloudtrail-processing-library-implement-events-processor)

1. [Instantiating and Running the Processing Executor](#use-the-cloudtrail-processing-library-instantiate-and-run-executor)

### Adding the CloudTrail Processing Library to Your Project<a name="use-the-cloudtrail-processing-library-add-to-project"></a>

To use the CloudTrail Processing Library, add it to your Java project's classpath\.



#### Adding the Library to an Apache Ant Project<a name="use-the-cloudtrail-processing-library-add-to-project-apache-ant"></a>

**To add the library to an Apache Ant project**

1. Download or clone the CloudTrail Processing Library source code from GitHub:

   + [https://github\.com/aws/aws\-cloudtrail\-processing\-library](https://github.com/aws/aws-cloudtrail-processing-library)

1. Build the \.jar file from source as described in the [README](https://github.com/aws/aws-cloudtrail-processing-library/blob/master/README.rst):

   ```
   mvn clean install -Dgpg.skip=true
   ```

1. Copy the resulting \.jar file into your project and add it to your project's `build.xml` file\. For example:

   ```
   <classpath>
     <pathelement path="${classpath}"/>
     <pathelement location="lib/aws-cloudtrail-processing-library-1.1.2.jar"/>
   </classpath>
   ```

#### Adding the Library to an Apache Maven Project<a name="use-the-cloudtrail-processing-library-add-to-project-apache-maven"></a>

The CloudTrail Processing Library is available for [Apache Maven](http://maven.apache.org/)\. You can add it to your project by writing a single dependency in your project's `pom.xml` file\.

**To add the CloudTrail Processing Library to a Maven project**

+ Open your Maven project's `pom.xml` file and add the following dependency:

  ```
  <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-cloudtrail-processing-library</artifactId>
      <version>1.1.2</version>
  </dependency>
  ```

#### Adding the Library to an Eclipse Project<a name="use-the-cloudtrail-processing-library-add-to-project-eclipse"></a>

**To add the CloudTrail Processing Library to an Eclipse project**

1. Download or clone the CloudTrail Processing Library source code from GitHub:

   + [https://github\.com/aws/aws\-cloudtrail\-processing\-library](https://github.com/aws/aws-cloudtrail-processing-library)

1. Build the \.jar file from source as described in the [README](https://github.com/aws/aws-cloudtrail-processing-library/blob/master/README.rst):

   ```
   mvn clean install -Dgpg.skip=true
   ```

1. Copy the built aws\-cloudtrail\-processing\-library\-1\.1\.2\.jar to a directory in your project \(typically `lib`\)\.

1. Right\-click your project's name in the Eclipse **Project Explorer**, choose **Build Path**, and then choose **Configure**

1. In the **Java Build Path** window, choose the **Libraries** tab\.

1. Choose **Add JARs\.\.\.** and navigate to the path where you copied aws\-cloudtrail\-processing\-library\-1\.1\.2\.jar\.

1. Choose **OK** to complete adding the `.jar` to your project\.

#### Adding the Library to an IntelliJ Project<a name="use-the-cloudtrail-processing-library-add-to-intellij-project"></a>

**To add the CloudTrail Processing Library to an IntelliJ project**

1. Download or clone the CloudTrail Processing Library source code from GitHub:

   + [https://github\.com/aws/aws\-cloudtrail\-processing\-library](https://github.com/aws/aws-cloudtrail-processing-library)

1. Build the \.jar file from source as described in the [README](https://github.com/aws/aws-cloudtrail-processing-library/blob/master/README.rst):

   ```
   mvn clean install -Dgpg.skip=true
   ```

1. From **File**, choose **Project Structure**\.

1. Choose **Modules** and then choose **Dependencies**\.

1. Choose **\+ JARS or Directories** and then go to the path where you built the `aws-cloudtrail-processing-library-1.1.2.jar`\.

1. Choose **Apply** and then choose **OK** to complete adding the `.jar` to your project\.

### Configuring the CloudTrail Processing Library<a name="use-the-cloudtrail-processing-library-configure"></a>

You can configure the CloudTrail Processing Library by creating a classpath properties file that is loaded at runtime, or by creating a `ClientConfiguration` object and setting options manually\.

#### Providing a Properties File<a name="use-the-cloudtrail-processing-library-configure-provide-classpath-properties-file"></a>

You can write a classpath properties file that provides configuration options to your application\. The following example file shows the options you can set:

```
# AWS access key. (Required)
accessKey = your_access_key

# AWS secret key. (Required)
secretKey = your_secret_key

# The SQS URL used to pull CloudTrail notification from. (Required)
sqsUrl = your_sqs_queue_url

# The SQS end point specific to a region.
sqsRegion = us-east-1

# A period of time during which Amazon SQS prevents other consuming components
# from receiving and processing that message.
visibilityTimeout = 60

# The S3 region to use.
s3Region = us-east-1

# Number of threads used to download S3 files in parallel. Callbacks can be
# invoked from any thread.
threadCount = 1

# The time allowed, in seconds, for threads to shut down after
# AWSCloudTrailEventProcessingExecutor.stop() is called. If they are still
# running beyond this time, they will be forcibly terminated.
threadTerminationDelaySeconds = 60

# The maximum number of AWSCloudTrailClientEvents sent to a single invocation
# of processEvents().
maxEventsPerEmit = 10

# Whether to include raw event information in CloudTrailDeliveryInfo.
enableRawEventInfo = false

# Whether to delete SQS message when the CloudTrail Processing Library is unable to process the notification.
deleteMessageUponFailure = false
```

The following parameters are required: 

+ `sqsUrl` – Provides the URL from which to pull your CloudTrail notifications\. If you don't specify this value, the `AWSCloudTrailProcessingExecutor` throws an `IllegalStateException`\.

+ `accessKey` – A unique identifier for your account, such as AKIAIOSFODNN7EXAMPLE\.

+ `secretKey` – A unique identifier for your account, such as wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY\. 

The `accessKey` and `secretKey` parameters provide your AWS credentials to the library so the library can access AWS on your behalf\.

Defaults for the other parameters are set by the library\. For more information, see the [AWS CloudTrail Processing Library Reference](http://docs.aws.amazon.com/awscloudtrail/latest/processinglib)\.

#### Creating a ClientConfiguration<a name="use-the-cloudtrail-processing-library-configure-create-clientconfiguration"></a>

Instead of setting options in the classpath properties, you can provide options to the `AWSCloudTrailProcessingExecutor` by initializing and setting options on a `ClientConfiguration` object, as shown in the following example:

```
ClientConfiguration basicConfig = new ClientConfiguration(
    "http://sqs.us-east-1.amazonaws.com/123456789012/queue2",
    new DefaultAWSCredentialsProviderChain());

basicConfig.setEnableRawEventInfo(true);
basicConfig.setThreadCount(4);
basicConfig.setnEventsPerEmit(20);
```

### Implementing the Events Processor<a name="use-the-cloudtrail-processing-library-implement-events-processor"></a>

To process CloudTrail logs, you must implement an `EventsProcessor` that receives the CloudTrail log data\. The following is an example implementation: 

```
public class SampleEventsProcessor implements EventsProcessor {

    public void process(List<CloudTrailEvent> events) {
        int i = 0;
        for (CloudTrailEvent event : events) {
            System.out.println(String.format("Process event %d : %s", i++, event.getEventData()));
        }
    }
}
```

When implementing an `EventsProcessor`, you implement the `process()` callback that the `AWSCloudTrailProcessingExecutor` uses to send you CloudTrail events\. Events are provided in a list of `CloudTrailClientEvent` objects\.

The `CloudTrailClientEvent` object provides a `CloudTrailEvent` and `CloudTrailEventMetadata` that you can use to read the CloudTrail event and delivery information\.

This simple example prints the event information for each event passed to `SampleEventsProcessor`\. In your own implementation, you can process logs as you see fit\. The `AWSCloudTrailProcessingExecutor` continues to send events to your `EventsProcessor` as long as it has events to send and is still running\.

### Instantiating and Running the Processing Executor<a name="use-the-cloudtrail-processing-library-instantiate-and-run-executor"></a>

After you write an `EventsProcessor` and set configuration values for the CloudTrail Processing Library \(either in a properties file or by using the `ClientConfiguration` class\), you can use these elements to initialize and use an `AWSCloudTrailProcessingExecutor`\.

**To use `AWSCloudTrailProcessingExecutor` to process CloudTrail events**

1. Instantiate an `AWSCloudTrailProcessingExecutor.Builder` object\. `Builder`'s constructor takes an `EventsProcessor` object and a classpath properties file name\.

1. Call the `Builder`'s `build()` factory method to configure and obtain an `AWSCloudTrailProcessingExecutor` object\.

1. Use the `AWSCloudTrailProcessingExecutor`'s `start()` and `stop()` methods to begin and end CloudTrail event processing\.

```
public class SampleApp {
  public static void main(String[] args) throws InterruptedException {
    AWSCloudTrailProcessingExecutor executor = new
      AWSCloudTrailProcessingExecutor.Builder(new SampleEventsProcessor(),
        "/myproject/cloudtrailprocessing.properties").build();

    executor.start();
    Thread.sleep(24 * 60 * 60 * 1000); // let it run for a while (optional)
    executor.stop(); // optional
  }
}
```

## Advanced Topics<a name="use-the-cloudtrail-processing-library-advanced"></a>


+ [Filtering the Events to Process](#use-the-cloudtrail-processing-library-advanced-filter-events)
+ [Reporting Progress](#use-the-cloudtrail-processing-library-advanced-report-progress)
+ [Handling Errors](#use-the-cloudtrail-processing-library-advanced-handle-errors)

### Filtering the Events to Process<a name="use-the-cloudtrail-processing-library-advanced-filter-events"></a>

By default, all logs in your Amazon SQS queue's S3 bucket and all events that they contain are sent to your `EventsProcessor`\. The CloudTrail Processing Library provides optional interfaces that you can implement to filter the sources used to obtain CloudTrail logs and to filter the events that you are interested in processing\.

`SourceFilter`  
You can implement the `SourceFilter` interface to choose whether you want to process logs from a provided source\. `SourceFilter` declares a single callback method, `filterSource()`, that receives a `CloudTrailSource` object\. To keep events from a source from being processed, return `false` from `filterSource()`\.  
The CloudTrail Processing Library calls the `filterSource()` method after the library polls for logs on the Amazon SQS queue\. This occurs before the library starts event filtering or processing for the logs\.  
The following is an example implementation:  

```
public class SampleSourceFilter implements SourceFilter{
  private static final int MAX_RECEIVED_COUNT = 3;

  private static List<String> accountIDs ;
  static {
    accountIDs = new ArrayList<>();
    accountIDs.add("123456789012");
    accountIDs.add("234567890123");
  }

  @Override
  public boolean filterSource(CloudTrailSource source) throws CallbackException {
    source = (SQSBasedSource) source;
    Map<String, String> sourceAttributes = source.getSourceAttributes();

    String accountId = sourceAttributes.get(
      SourceAttributeKeys.ACCOUNT_ID.getAttributeKey());

    String receivedCount = sourceAttributes.get(
      SourceAttributeKeys.APPROXIMATE_RECEIVE_COUNT.getAttributeKey());

    int approximateReceivedCount = Integer.parseInt(receivedCount);

    return approximateReceivedCount <= MAX_RECEIVED_COUNT && accountIDs.contains(accountId);
  }
}
```
If you don't provide your own `SourceFilter`, then `DefaultSourceFilter` is used, which allows all sources to be processed \(it always returns `true`\)\.

`EventFilter`  
You can implement the `EventFilter` interface to choose whether a CloudTrail event is sent to your `EventsProcessor`\. `EventFilter` declares a single callback method, `filterEvent()`, that receives a `CloudTrailEvent` object\. To keep the event from being processed, return `false` from `filterEvent()`\.  
The CloudTrail Processing Library calls the `filterEvent()` method after the library polls for logs on the Amazon SQS queue and after source filtering\. This occurs before the library starts event processing for the logs\.  
See the following example implementation:  

```
public class SampleEventFilter implements EventFilter{

  private static final String EC2_EVENTS = "ec2.amazonaws.com";

  @Override
  public boolean filterEvent(CloudTrailClientEvent clientEvent) throws CallbackException {
    CloudTrailEvent event = clientEvent.getEvent();

    String eventSource = event.getEventSource();
    String eventName = event.getEventName();

    return eventSource.equals(EC2_EVENTS) && eventName.startsWith("Delete");
  }
}
```
If you don't provide your own `EventFilter`, then `DefaultEventFilter` is used, which allows all events to be processed \(it always returns `true`\)\.

### Reporting Progress<a name="use-the-cloudtrail-processing-library-advanced-report-progress"></a>

Implement the `ProgressReporter` interface to customize the reporting of CloudTrail Processing Library progress\. `ProgressReporter` declares two methods: `reportStart()` and `reportEnd()`, which are called at the beginning and end of the following operations:

+ Polling messages from Amazon SQS

+ Parsing messages from Amazon SQS

+ Processing an Amazon SQS source for CloudTrail logs

+ Deleting messages from Amazon SQS

+ Downloading a CloudTrail log file

+ Processing a CloudTrail log file

Both methods receive a `ProgressStatus` object that contains information about the operation that was performed\. The `progressState` member holds a member of the `ProgressState` enumeration that identifies the current operation\. This member can contain additional information in the `progressInfo` member\. Additionally, any object that you return from `reportStart()` is passed to `reportEnd()`, so you can provide contextual information such as the time when the event began processing\.

The following is an example implementation that provides information about how long an operation took to complete:

```
public class SampleProgressReporter implements ProgressReporter {
  private static final Log logger =
    LogFactory.getLog(DefaultProgressReporter.class);

  @Override
  public Object reportStart(ProgressStatus status) {
    return new Date();
  }

  @Override
  public void reportEnd(ProgressStatus status, Object startDate) {
    System.out.println(status.getProgressState().toString() + " is " +
      status.getProgressInfo().isSuccess() + " , and latency is " +
      Math.abs(((Date) startDate).getTime()-new Date().getTime()) + "
      milliseconds.");
  }
}
```

If you don't implement your own `ProgressReporter`, then `DefaultExceptionHandler`, which prints the name of the state being run, is used instead\.

### Handling Errors<a name="use-the-cloudtrail-processing-library-advanced-handle-errors"></a>

The `ExceptionHandler` interface allows you to provide special handling when an exception occurs during log processing\. `ExceptionHandler` declares a single callback method, `handleException()`, which receives a `ProcessingLibraryException` object with context about the exception that occurred\.

You can use the passed\-in `ProcessingLibraryException`'s `getStatus()` method to find out what operation was executed when the exception occurred and get additional information about the status of the operation\. `ProcessingLibraryException` is derived from Java's standard `Exception` class, so you can also retrieve information about the exception by invoking any of the exception methods\.

See the following example implementation:

```
public class SampleExceptionHandler implements ExceptionHandler{
  private static final Log logger =
    LogFactory.getLog(DefaultProgressReporter.class);

  @Override
  public void handleException(ProcessingLibraryException exception) {
    ProgressStatus status = exception.getStatus();
    ProgressState state = status.getProgressState();
    ProgressInfo info = status.getProgressInfo();

    System.err.println(String.format(
      "Exception. Progress State: %s. Progress Information: %s.", state, info));
  }
}
```

If you don't provide your own `ExceptionHandler`, then `DefaultExceptionHandler`, which prints a standard error message, is used instead\.

**Note**  
If the `deleteMessageUponFailure` parameter is `true`, the CloudTrail Processing Library does not distinguish general exceptions from processing errors and may delete queue messages\.  
For example, you use the `SourceFilter` to filter messages by timestamp\.
However, you don't have the required permissions to access the S3 bucket that receives the CloudTrail log files\. Because you don't have the required permissions, an `AmazonServiceException` is thrown\. The CloudTrail Processing Library wraps this in a `CallBackException`\. 
The `DefaultExceptionHandler` logs this as an error, but does not identify the root cause, which is that you don't have the required permissions\. The CloudTrail Processing Library considers this a processing error and deletes the message, even if the message includes a valid CloudTrail log file\.
If you want to filter messages with `SourceFilter`, verify that your `ExceptionHandler` can distinguish service exceptions from processing errors\. 

## Additional Resources<a name="UsingProcessingLib-ar"></a>

For more information about the CloudTrail Processing Library, see the following:

+ [CloudTrail Processing Library](https://github.com/aws/aws-cloudtrail-processing-library) GitHub project, which includes [sample](https://github.com/aws/aws-cloudtrail-processing-library/tree/master/src/sample) code that demonstrates how to implement a CloudTrail Processing Library application\.

+ [CloudTrail Processing Library Java Package Documentation](http://docs.aws.amazon.com/awscloudtrail/latest/processinglib)\.