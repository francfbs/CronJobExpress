# CronJobExpress

This package is a simple way to implements schedule jobs on your Aspnet WebApi based on crontab expressions.

## Installing

```
dotnet add package CronJobExpress
```

## Examples

Create your job class inheriting from CronJobService.
```
public class Job1 : CronJobService
    {
        private readonly ILogger<Job1> _logger;

        public Job1(IScheduleConfig<Job1> config, ILogger<Job1> logger) : base(config.CronExpression, config.TimeZoneInfo)
        {
            _logger = logger;
        }


        public override Task DoJob(CancellationToken cancellationToken)
        {
            _logger.LogInformation($"{DateTime.Now:hh:mm:ss} JOB-1 schedule fired.");
            //
            // job tasks here
            //
            return Task.CompletedTask;
        }
    }
```
In this example when schedule was fired Job1 is going to write the message "HH:MM:SS JOB-1 schedule fired" on console.



