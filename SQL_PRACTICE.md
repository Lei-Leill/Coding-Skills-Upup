# Summary:

```
SELECT <Column Names> or <SUM(column) / Count(column)/ AVG(column)> AS "(column name displayed in table"
FROM <Table Name>
(If pull data from multiple tables, do
LEFT/INNER JOIN <Table2>
WHERE <Column Name> = '<Particular Value>'
(If multiple values, do WHERE <Column Name> IN ('Value1', 'Value2',...))
GROUP BY <Column>
ORDER BY <Column> DESC/ ASC
```

Extract and Look at the whole table
```
SELECT * FROM <TABLE>
```

select列的时候可以refer join好的任何column
```
SELECT A2C_COMMUNITY, SUM(FAMILY_COUNT) AS INDIVIDUALS
FROM A2C_ROSTER
LEFT JOIN COMM_MAPPINGS
ON A2C_ROSTER.STATE = COMM_MAPPINGS.STATE
WHERE STATUS IN ("ACTIVE", "STAFF")
GROUP BY COMM_MAPPINGS.A2C_COMMUNITY
ORDER BY INDIVIDUALS DESC
```
Multiple Table Joining
```
SELECT <COLUMN NAME>
FROM <TABLE NAME 1>
LEFT JOIN <TABLE NAME 2>
ON <TABLE 1>.<MATCHING FIELD NAME> = <TABLE 2>.< MATCHING FIELD NAME >
LEFT JOIN <TABLE NAME 3>
ON <TABLE 1>.<MATCHING FIELD NAME> = <TABLE 3>.< MATCHING FIELD NAME >;
```
