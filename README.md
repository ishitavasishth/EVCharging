# EVCharging
Project: Analyzing Electric Vehicle Charging Habits

As electronic vehicles (EVs) become more popular, there is an increasing need for access to charging stations, also known as ports. To that end, many modern apartment buildings have begun retrofitting their parking garages to include shared charging stations. A charging station is shared if it is accessible by anyone in the building.


But with increasing demand comes competition for these ports — nothing is more frustrating than coming home to find no charging stations available! In this project, you will use a dataset to help apartment building managers better understand their tenants’ EV charging habits.

The data has been loaded into a PostgreSQL database with a table named charging_sessions with the following columns:

charging_sessions
Column	Definition	Data type
garage_id	Identifier for the garage/building	VARCHAR
user_id	Identifier for the individual user	VARCHAR
user_type	Indicating whether the station is Shared or Private	VARCHAR
start_plugin	The date and time the session started	DATETIME
start_plugin_hour	The hour (in military time) that the session started	NUMERIC
end_plugout	The date and time the session ended	DATETIME
end_plugout_hour	The hour (in military time) that the session ended	NUMERIC
duration_hours	The length of the session, in hours	NUMERIC
el_kwh	Amount of electricity used (in Kilowatt hours)	NUMERIC
month_plugin	The month that the session started	VARCHAR
weekdays_plugin	The day of the week that the session started	VARCHAR



-- unique_users_per_garage
-- Modify the code below
 ```sql
SELECT garage_id, count(distinct user_id) as num_unique_users
FROM charging_sessions
 where user_type = 'Shared'
group by 1
order by 2 desc;
```

-- most_popular_shared_start_times
 ```sql

select *
from 
(select weekdays_plugin, start_plugin_hour, count(weekdays_plugin) as num_charging_sessions
from charging_sessions
 where user_type = 'Shared'
group by 1, 2
order by 3 desc
limit 10) as most_popular_shared_start_times;
```
-- long_duration_shared_users


```sql

(select user_id, avg(duration_hours) as avg_charging_duration
from charging_sessions
 where user_type = 'Shared'
group by 1
having avg(duration_hours) > 10.0
order by avg_charging_duration desc)
```
