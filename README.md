# CronJobExpress

This package is a simple way to implements schedule jobs on your Aspnet WebApi based on crontab expressions.

## Installing

```
dotnet add package CronJobExpress
```

## How to use

#### 1 - Create your job class inheriting from CronJobService.
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
In this example was created a job class called Job1 that inherits from CronJobService interface.
This interface forces you implement DoJob() method that you use to implement your job tasks. In this example when schedule is fired Job1 just write the message "HH:MM:SS JOB-1 schedule fired" on console.

#### 2 - Register your job on Startup.cs class
```
public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();
            services.AddJob<Job1>(c =>
            {
                c.TimeZoneInfo = TimeZoneInfo.Local;
                c.CronExpression = @"* * * * *";
            });
        }
        .
        .
        .
     }
```
Here on Startup.cs class the job was registered on ConfigureServices method providing local time zone info and cron expression, in this example Job1 will be fired evey minute. 

#### 3 - Finish
After start your WebApi the job will be fired based on cron expression schedule provided. 

### Complete usage example
* https://github.com/francfbs/CronJobExpressUsing

## Dependencies
* Cronos 0.7.0 (https://github.com/HangfireIO/Cronos)

## Learn more about Crontab Expressions
* https://docs.oracle.com/cd/E12058_01/doc/doc.1014/e12030/cron_expressions.htm
* https://en.wikipedia.org/wiki/Cron
