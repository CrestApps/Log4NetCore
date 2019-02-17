---------------------------------
CrestApps.Log4NetCore
---------------------------------





# Setup
Add a file called "log4net.config" into the root of you  project with the following context

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>

  <log4net debug="false">
    <appender name="LogFileAppender" type="log4net.Appender.RollingFileAppender,log4net">
      <param name="File" value="app.log" />
      <appendToFile value="true" />
      <rollingStyle value="Size" />
      <maxSizeRollBackups value="10" />
      <maximumFileSize value="10MB" />
      <staticLogFileName value="true" />
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date [%thread] %-5level %logger - %message %newline %location %newline %newline %newline" />
      </layout>
    </appender>
    <root>
      <level value="ERROR" />
      <appender-ref ref="LogFileAppender" />
    </root>
  </log4net>

  <system.diagnostics>
    <trace autoflush="true">
      <listeners>
        <add name="textWriterTraceListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="log4net.txt" />
      </listeners>
    </trace>
  </system.diagnostics>
  
</configuration>
```

Then in Program.cs file or Global.cs you can register your log4net as provider like os

```
ILoggerProvider logProvider = new Log4NetProvider(StorageHelper.MakePath("log4net.config"));
services.AddLogging((builder) =>
{
    builder.ClearProviders();
    builder.AddProvider(logProvider).AddFilter(level => level >= LogLevel.Information);
});
```

Then you can register an instance of you ILogger like this

```
ILogger logger = logProvider.CreateLogger("MyAppName");

services.AddSingleton(logger);

```

Finally, anywhere you want to log into, inject `ILogger` into your class and use it.



------------
Disclaimer: Most of code in this package was copied from https://rajujoseph.com/log4net-provider-for-net-core/
This package make it easy for you to include Log4Net into your code project without having to manually add code into your project

--------