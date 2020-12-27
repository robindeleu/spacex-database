# Gamereview database

## Import the Database


Go to the `/sql` directory and import the `spacex.sql` file in your database to get access to the tables for this exercise.

Execute the following command in this project directory with the `mysql` client:

```sql
source spacex.sql
```

Answer every question with:

1. The query that answers the question.
2. The results of that query.

Every query that returns more than 10 row must be reduced to the first 10 rows. (HINT you can do this in you query).

## Example

1. Give all product id's en product descriptions of all products.

```sql
SELECT prod_id, prod_desc FROM products;
```

```
+---------+----------------------------------------------------------------+
| prod_id | prod_desc                                                      |
+---------+----------------------------------------------------------------+
| ANV01   | .5 ton anvil, black, complete with handy hook                  |
| ANV02   | 1 ton anvil, black, complete with handy hook and carrying case |
| ANV03   | 2 ton anvil, black, complete with handy hook and carrying case |
| DTNTR   | Detonator (plunger powered), fuses not included                |
| FB      | Large bag (suitable for road runners)                          |
| FC      | Carrots (rabbit hunting season only)                           |
| FU1     | 1 dozen, extra long                                            |
| JP1000  | JetPack 1000, intended for single use                          |
| JP2000  | JetPack 2000, multi-use                                        |
| OL1     | Oil can, red                                                   |
| SAFE    | Safe with combination lock                                     |
| SLING   | Sling, one size fits all                                       |
| TNT1    | TNT, red, single stick                                         |
| TNT2    | TNT, red, pack of 10 sticks                                    |
+---------+----------------------------------------------------------------+
14 rows in set (0.00 sec)
```

### Get Exercise updates

Exercise instructions will be added. To get the latest instructions you need to pull them into your own project. This can be done with the following command:

```shell
git pull https://github.com/vives-databases-2019/spacex-database master
```

This will add or update the exercise.md file and its dependencies in your own project.

## Exercise

1. Amount of launches

```sql
select count(id) from launches;
```

```text
+-----------+
| count(id) |
+-----------+
|        92 |
+-----------+
```

2. Amount of upcoming launches

```sql
select count(id) from launches where upcoming = 1;

```

```text
+-----------+
| count(id) |
+-----------+
|        16 |
+-----------+
```

3. Amount of past launches

```sql
select count(id) from launches where upcoming = 0;

```

```text
+-----------+
| count(id) |
+-----------+
|        76 |
+-----------+
```

4. Amount of successful launches

```sql
select count(id) from launches where success = 1 ;

```

```text
+-----------+
| count(id) |
+-----------+
|        71 |
+-----------+
```

5. How many launches have details?

```sql
select count(id) from launches where details != 'NULL';
```

```text
+-----------+
| count(id) |
+-----------+
|        71 |
+-----------+
```

6. What are the different core serial numbers that have been used in launches? (no duplicates!)

```sql
select core_serial from launches group by core_serial limit 10;
```

```text
+-------------+
| core_serial |
+-------------+
| NULL        |
| B0003       |
| B0004       |
| B0005       |
| B0006       |
| B0007       |
| B1003       |
| B1004       |
| B1005       |
| B1006       |
+-------------+
```

7. What are the amount of launches per year (for every year)

```sql
select year, count(id) as aantal_lanceringen from launches group by year limit 10;
```

```text
+------+--------------------+
| year | aantal_lanceringen |
+------+--------------------+
| 2006 |                  1 |
| 2007 |                  1 |
| 2008 |                  2 |
| 2009 |                  1 |
| 2010 |                  2 |
| 2012 |                  2 |
| 2013 |                  3 |
| 2014 |                  6 |
| 2015 |                  7 |
| 2016 |                  9 |
+------+--------------------+
```

8. What 'Iridium' missions (name) were launched? (using only launches table)

```sql
select mission_name from launches where mission_name like "%Iridium%";
```

```text
+------------------------+
| mission_name           |
+------------------------+
| Iridium NEXT Mission 1 |
| Iridium NEXT Mission 2 |
| Iridium NEXT Mission 3 |
| Iridium NEXT Mission 4 |
| Iridium NEXT Mission 5 |
| Iridium NEXT Mission 6 |
| Iridium NEXT Mission 7 |
| Iridium NEXT Mission 8 |
+------------------------+
```

9. What mission failed after 150 seconds?

```sql
select mission_name from launches where details like "%150 seconds%";
```

```text
+--------------+
| mission_name |
+--------------+
| CRS-7        |
+--------------+
```

10. What is the amount of water landings for every status value?

```sql
select count(serial) as aantal_waterlandingen from cores where water_landing = 1;
```

```text
+-----------------------+
| aantal_waterlandingen |
+-----------------------+
|                     8 |
+-----------------------+
```

11. What are the core serial number that landed on water?

```sql
select serial from cores where water_landing = 1;
```

```text
+--------+
| serial |
+--------+
| B1003  |
| B1006  |
| B1007  |
| B1010  |
| B1013  |
| B1032  |
| B1036  |
| B1050  |
+--------+
```

12. What are the names of the missions that landed on water?

```sql
select launches.mission_name from launches join cores on launches.core_serial = cores.serial where water_landing = 1 limit 10;
```

```text
+------------------------+
| mission_name           |
+------------------------+
| CASSIOPE               |
| CRS-3                  |
| OG-2 Mission 1         |
| CRS-4                  |
| DSCOVR                 |
| NROL-76                |
| Iridium NEXT Mission 2 |
| Iridium NEXT Mission 4 |
| SES-16 / GovSat-1      |
| CRS-16                 |
+------------------------+
```

13. How many missions landed on land (not water) in 2018?

```sql
select count(launches.mission_name) as aantal_missies_on_water_2018 from launches join cores on launches.core_serial = cores.serial where water_landing = 0 and year = 2018;
```

```text
+------------------------------+
| aantal_missies_on_water_2018 |
+------------------------------+
|                           19 |
+------------------------------+
```

14. What are the mission names (with the mission year) that failed to land on water?

```sql
select launches.mission_name as missies_on_water,year from launches join cores on launches.core_serial = cores.serial where water_landing = 1 and success = 0 limit 10;
```

```text
Empty set 
alles is in succes bewijs:
select launches.mission_name as missies_on_water,year, success from launches join cores on launches.core_serial = cores.serial where water_landing = 1;
+------------------------+------+---------+
| missies_on_water       | year | success |
+------------------------+------+---------+
| CASSIOPE               | 2013 |       1 |
| CRS-3                  | 2014 |       1 |
| OG-2 Mission 1         | 2014 |       1 |
| CRS-4                  | 2014 |       1 |
| DSCOVR                 | 2015 |       1 |
| NROL-76                | 2017 |       1 |
| Iridium NEXT Mission 2 | 2017 |       1 |
| Iridium NEXT Mission 4 | 2017 |       1 |
| SES-16 / GovSat-1      | 2018 |       1 |
| CRS-16                 | 2018 |       1 |
+------------------------+------+---------+
```

15. What is the average payload weight?

```sql
select avg(mass) as gemiddelde_mass_payloads from payloads;
```

```text
+--------------------------+
| gemiddelde_mass_payloads |
+--------------------------+
|                3675.0976 |
+--------------------------+
```

16. What is the average payload weight of 'SES' missions? (note: customers is an string contianing a list...)

```sql
select avg(mass) as gemiddelde_mass_payloads from payloads join missions on payloads.mission_id = missions.id where missions.name like "%SES%";
```

```text
+--------------------------+
| gemiddelde_mass_payloads |
+--------------------------+
|                4781.2500 |
+--------------------------+
```

17. What is the heaviest payload that is launched in a 'LEO' orbit

```sql
select max(mass) from payloads where orbit like "%LEO%";
```

```text
+-----------+
| max(mass) |
+-----------+
|      4990 |
+-----------+
```

18. What are the payload manufacturers that lost there payload due to a failed launch?

```sql
select manufacturer from payloads join launches on payloads.launch_id = launches.id where success = '0';
```

```text
+-----------------------------+
| manufacturer                |
+-----------------------------+
| Israel Aerospace Industries |
| SpaceX                      |
| SpaceX                      |
| SSTL                        |
| NULL                        |
| Space Dev                   |
+-----------------------------+
```

19. What is the amount of payload mass that each nationality has launched?

```sql
select count(mass) as amount_of_payload_mass, nationality from payloads group by nationality limit 10;
```

```text
+------------------------+-------------+
| amount_of_payload_mass | nationality |
+------------------------+-------------+
|                      2 | NULL        |
|                      2 | Argentina   |
|                      1 | Bangladesh  |
|                      1 | Bulgaria    |
|                      4 | Canada      |
|                      2 | France      |
|                      0 | Germany     |
|                      4 | Hong Kong   |
|                      2 | Indonesia   |
|                      3 | Israel      |
+------------------------+-------------+
```

20. What is the amount of payload mass that each nationality has launched in the year 2018?

```sql
 select count(mass) as amount_of_payload_mass, nationality from payloads join launches on launches.id = payloads.launch_id where year = 2018 group by nationality limit 10;
```

```text
+------------------------+---------------+
| amount_of_payload_mass | nationality   |
+------------------------+---------------+
|                      1 | NULL          |
|                      1 | Argentina     |
|                      1 | Bangladesh    |
|                      2 | Canada        |
|                      1 | Indonesia     |
|                      2 | Luxembourg    |
|                      1 | Qatar         |
|                      2 | Spain         |
|                     11 | United States |
+------------------------+---------------+
```

21. What is the average payload mass that each manufacturer has launched in 2017?

```sql
select avg(mass), manufacturer from payloads join launches on launches.id = payloads.launch_id where year = 2017 group by manufacturer;
```

```text
+-----------+--------------------------+
| avg(mass) | manufacturer             |
+-----------+--------------------------+
| 5366.6667 | Airbus Defence and Space |
| 6415.5000 | Boeing                   |
| 4990.0000 | Boeing Defense           |
|  475.0000 | NSPO                     |
| 2578.2500 | SpaceX                   |
| 3669.0000 | SSL                      |
| 8420.0000 | Thales Alenia Space      |
+-----------+--------------------------+
```

22. In how many flights was the 'Falcon 9' rocket used?

```sql
select count(id), rocket_id from launches where rocket_id = "falcon9" group by rocket_id;
```

```text
+-----------+-----------+
| count(id) | rocket_id |
+-----------+-----------+
|        84 | falcon9   |
+-----------+-----------+
```

23. When was the first time that a core landed on water? (exclude empty dates)

```sql
select timestamp,rocket_id from launches join cores on launches.core_serial = cores.serial where water_landing = '1' order by timestamp asc limit 10;
```

```text
+---------------------+-----------+
| timestamp           | rocket_id |
+---------------------+-----------+
| 0000-00-00 00:00:00 | falcon9   |
| 0000-00-00 00:00:00 | falcon9   |
| 0000-00-00 00:00:00 | falcon9   |
| 0000-00-00 00:00:00 | falcon9   |
| 2014-03-05 21:25:00 | falcon9   |
| 2014-06-01 17:15:00 | falcon9   |
| 2015-01-04 00:03:00 | falcon9   |
| 2017-04-01 13:15:00 | falcon9   |
| 2017-11-06 02:27:23 | falcon9   |
| 2018-11-03 19:16:00 | falcon9   |
+---------------------+-----------+
```

24. How many launches have taken place on any launchpad, give the name of those launchpads and their status?

```sql
select count(launches.id), launchpads.name,launchpads.status from launches,launchpads group by launchpads.name;
```

```text
+--------------------+----------------------------------------------------------+--------------------+
| count(launches.id) | name                                                     | status             |
+--------------------+----------------------------------------------------------+--------------------+
|                 92 | Cape Canaveral Air Force Station Space Launch Complex 40 | active             |
|                 92 | Kennedy Space Center Historic Launch Complex 39A         | active             |
|                 92 | Kwajalein Atoll Omelek Island                            | retired            |
|                 92 | SpaceX South Texas Launch Site                           | under construction |
|                 92 | Vandenberg Air Force Base Space Launch Complex 3W        | retired            |
|                 92 | Vandenberg Air Force Base Space Launch Complex 4E        | active             |
+--------------------+----------------------------------------------------------+--------------------+
```

25. What is the mass of the heaviest payload that was (or will be) launched using 'raptor' engines?

```sql
select max(payloads.mass) from payloads join launches on launches.id = payloads.launch_id join rockets on rockets.id = launches.rocket_id where engines like "%raptor%";
```

```text
+--------------------+
| max(payloads.mass) |
+--------------------+
|               NULL |
+--------------------+
```

## Report

When you are ready and submitted the exercise, make sure to fill in the [report](./REPORT.md) file. Don't forget to commit it as well. Answer all questions and check the formatting by viewing the file on GitHub.