![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 创建发送SQL作业警报的作业（测试版）
#### Create The Job SEND SQL JOB ALERTS (beta)
**发布-日期: 2015年7月15日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是创建 SEND SQL JOB ALERTS（beta）作业的sql逻辑（logic）。 像往常一样，以防代码被博客阻挡了，我会继续将分享包含所有逻辑（logic）的pdf。
创建发送SQL JOB ALERTS（beta）的作业



## English
Here’s the sql logic to create the Job SEND SQL JOB ALERTS (beta).  As usual in case the code gets butchered by the blog post; I’ll go ahead and include the pdf that shows you all the logic.
CREATE THE SEND SQL JOB ALERTS (beta) Job


---
## Logic
```SQL

USE [msdb]
GO
/************/ 
BEGIN TRANSACTION

--发送SQL作业警报
DECLARE @ReturnCode INT
SELECT @ReturnCode = 0
/************/
IF NOT EXISTS (SELECT name FROM msdb.dbo.syscategories WHERE name=N'[Uncategorized (Local)]’ AND category_class=1) BEGIN
EXEC @ReturnCode = msdb.dbo.sp_add_category @class=N’JOB’, @type=N’LOCAL’, @name=N'[Uncategorized (Local)]’ IF (@@ERROR <> 0 OR @ReturnCode <> 0) GOTO QuitWithRollback
END
DECLARE @jobId BINARY(16)
EXEC @ReturnCode = msdb.dbo.sp_add_job @job_name=N’SEND SQL JOB ALERTS (beta)’, @enabled=1,
发送SQL作业警报
@notify_level_eventlog=0,
@notify_level_email=0,
@notify_level_netsend=0,
@notify_level_page=0,
@delete_level=0,
@description=N’This is a new SQL Job notification. It”s designed to send the a short error report about step failure within a job.’, @category_name=N'[Uncategorized (Local)]’,

-- 这是一个新的SQL作业通知。 它旨在发送有关作业中步骤失败的简短错误报告
@owner_login_name=N’MyDomain\MyEmailAdmin’, @job_id = @jobId OUTPUT IF (@@ERROR <> 0 OR @ReturnCode <> 0) GOTO QuitWithRollback
/************/
发送有关步骤错误的邮件
EXEC @ReturnCode = msdb.dbo.sp_add_jobstep @job_id=@jobId, @step_name=N’Send email about step failure’, @step_id=1,

-- 发送有关步骤错误的邮件
@cmdexec_success_code=0,
@on_success_action=1,
@on_success_step_id=0,
@on_fail_action=2,
@on_fail_step_id=0,
@retry_attempts=0,
@retry_interval=0,
@os_run_priority=0, @subsystem=N’TSQL’,
@command=N’use msdb;
set nocount on

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

