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
In this example when schedule was fired Job1 is going to write the message "HH:MM:SS JOB-1 schedule fired" on console.

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
Here on Startup.cs class the job was registered on ConfigureServices method providing local time zone info and cron expression. 

#### 3 - Finish
After start your WebApi the job will be fired based on cron expression schedule provided. 

## Dependencies
* Cronos 0.7.0 (https://github.com/HangfireIO/Cronos)
