# 7.1 HW Questions 


1. Using EVregistry, Write a query to select the `ModelYear`, `Make`, and `Model` off all of the vehicles in the registry.
    - SELECT "Model", "Make", "Model Year" FROM "EVRegistry";

2. Using the EVRegistry table, Write a query that lists all of the unique types of EV's. your reult set should have one column, `ElectricVehicleType`. 
    - SELECT DISTINCT "Electric Vehicle Type" FROM "EVRegistry";

3. Using the EVRegistry, Write a query that shows all of the information on Battery Electric Vehicles (BEV) that are in the registry. 
    - SELECT *
FROM EVRegistry
WHERE "Electric Vehicle Type"="Battery Electric Vehicle (BEV)"
    
4. Using the EVRegistry, write a query that returns the `Make` and `Model` of all of the EV's that have a BaseMSRP between 20000 and 35000?  
    - SELECT Make, Model
FROM EVRegistry
WHERE BaseMSRP BETWEEN 20000 AND 35000;


# 7.2 HW Questions 

1. Using EVRegistry, write a query to find a record  where the `City` attribute is NULL. Return all of the available columns. 
    - SELECT * 
FROM EVRegistry
WHERE City IS NULL
2. Write a query to find the `make`, `model`, and `ElectricVehicleType` where the VIN number has  that ends in '3E1EA1J'.
    - SELECT Make, Model, ElectricVehicleType
FROM EVRegistry
WHERE VIN LIKE '%3E1EA1J'
3. Select the `ModelYear`, `make`, `model`, `ElectricVehicleType`, and `range` of the Tesla vehicles or cheverolet vehicles in the registry. Order the result set by Make and Model year in from newest to oldest. 
    - SELECT ModelYear, Make, Model, ElectricVehicleType as 'Type', ElectricRange as 'Range'
FROM EVRegistry
WHERE Make = 'TESLA'
ORDER BY Make, ModelYear DESC;
4. Using EVCharging, Write a query to find out how many many times those stations were used. Order them by the most used to the least used and limit the output to 5 records. 
    - SELECT StationId, COUNT(DISTINCT SessionId) AS NumUses
FROM EVCharging
GROUP BY StationId
ORDER BY NumUses desc
LIMIT 5;

5.  Using EVCharging, For the folks who charged longer than 0.5 hours, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. 
    - SELECT userId, MIN(chargeTimeHrs) as 'minTime', MAX(ChargeTimeHrs) as 'maxTime'
FROM EVCharging
WHERE chargeTimeHrs > 0.5
GROUP BY userId
ORDER BY minTime, maxTime


# Before moving on with the rest of the questions please set up the new database
1. in SQLlite close any open DB
2. file----> Open Database
3. Choose SavvyCoders_SQL_chargeDB.db from the resource repository from this section in the curriculum
4. Make sure that you have 5 tables: 
    - dimDay 
    - dimFacility
    - dimUser
    - factCharge
    - EVCharging


# 7.3 HW Questions

1. Using EVCharging, Which day of the week has the highest average charging time? Round the answer to 2 decimal points.
    - SELECT weekday, ROUND(AVG(chargeTimeHrs), 2) as AvgChargeTimeHrs
FROM EVCharging 
GROUP BY weekday

Wed @ 2.94/Hrs

2. Using, EV charging, Find the total power consumed from charging EV's by each User. Return the `userId` and name the calculated column, `totalPower`. Round the answer to 2 deciaml points and list the out put in highest to lowest order. Limit the order to the top 15 users. 
    - SELECT userId, ROUND(kwhTotal, 2)
FROM EVCharging 
GROUP BY userId
ORDER BY MAX(kwhTotal) DESC
LIMIT 15

3. Using dimfacility and factCharge, write a query to find out which type of facility (GROUP BY) has the most amount of charging stations. Return `type Facility` and name the calculated column `numStation`. Order the result set from highest to lowest number of charging stations.  
    - SELECT dimFacility.typeFacility , COUNT(DISTINCT factCharge.stationId) as numStation
FROM factCharge
INNER JOIN dimFacility ON dimFacility.FacilityKey = factCharge.facilityID 
GROUP BY dimFacility.typeFacility
ORDER BY COUNT(DISTINCT factCharge.stationId) DESC

4. In your own words, Briefly explain Primary Keys and Foreign Keys. 
    - A Primary Key identifies a row of data.  Not my own words but this helps concept make sense: "A foreign key is a column that refers to a primary key [a row of data] in some other table."  

5. Using EV Charging, For the folks who charged longer than one hour, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. HINT: USE `HAVING`
    - SELECT DISTINCT userId, MIN(chargeTimeHrs) as 'minTime', MAX(ChargeTimeHrs) as 'maxTime'
FROM EVCharging
GROUP BY userId
HAVING MIN(chargeTimeHrs) > 1
ORDER BY 2,3