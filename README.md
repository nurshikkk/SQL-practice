# SQL PRACTICE

### SQL Practice №1 | The Warehouse

#### Relational Schema

![Sql_warehouse](https://user-images.githubusercontent.com/69513400/130347774-96d6aab6-cc9e-4813-b312-17d851fe81f8.png)

#### Table creation code 
``` sql
CREATE TABLE Warehouses (
   Code INTEGER PRIMARY KEY NOT NULL,
   Location TEXT NOT NULL ,
   Capacity INTEGER NOT NULL 
 );
 
 CREATE TABLE Boxes (
   Code TEXT PRIMARY KEY NOT NULL,
   Contents TEXT NOT NULL ,
   Value REAL NOT NULL ,
   Warehouse INTEGER NOT NULL, 
   CONSTRAINT fk_Warehouses_Code FOREIGN KEY (Warehouse) REFERENCES Warehouses(Code)
 );
 ```
 
 #### Sample dataset
 
 ``` sql
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (1, 'Chicago', 3);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (2, 'Chicago', 4);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (3, 'New York', 7);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (4, 'Los Angeles', 2);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (5, 'San Francisco', 8);
 
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('0MN7', 'Rocks', 180,3);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('4H8P', 'Rocks', 250,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('4RT3', 'Scissors', 190,4);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('7G3H', 'Rocks', 200,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('8JN6', 'Papers', 75,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('8Y6U', 'Papers', 50,3);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('9J6F', 'Papers', 175,2);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('LL08', 'Rocks', 140,4);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('P0H6', 'Scissors', 125,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('P2T6', 'Scissors', 150,2);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('TU55', 'Papers', 90,5);
 ```


#### Exercises

1. Select all warehouses.

2. Select all boxes with a value larger than $150.

3. Select all distinct contents in all the boxes.

4. Select the average value of all the boxes.

5. Select the warehouse code and the average value of the boxes in each warehouse.

6. Same as previous exercise, but select only those warehouses where the average value of the boxes is greater than 150.

7. Select the code of each box, along with the name of the city the box is located in.

8. Select the warehouse codes, along with the number of boxes in each warehouse. Optionally, take into account that some warehouses are empty (i.e., the box count should show up as zero, instead of omitting the warehouse from the result).

9. Select the codes of all warehouses that are saturated (a warehouse is saturated if the number of boxes in it is larger than the warehouse's capacity).

10. Select the codes of all the boxes located in Chicago.
 
11. Create a new warehouse in New York with a capacity for 3 boxes.

12. Create a new box, with code "H5RT", containing "Papers" with a value of $200, and located in warehouse 2.

13. Reduce the value of all boxes by 15%.

14. Apply a 20% value reduction to boxes with a value larger than the average value of all the boxes.

15. Remove all boxes with a value lower than $100.

16. Remove all boxes from saturated warehouses.

#### Answers

``` sql
1. DELETE FROM Boxes WHERE Warehouse IN (SELECT Code FROM Warehouses WHERE Capacity < (SELECT count(*) FROM Boxes WHERE Warehouse = Warehouses.Code));

2. SELECT * FROM Warehouses;

3. SELECT * FROM Boxes WHERE Value > 150;

4. SELECT DISTINCT Contents FROM Boxes;

5. SELECT avg(Value) FROM Boxes;

6. SELECT Warehouse, avg(Value) FROM boxes GROUP BY Warehouse;

7. SELECT Warehouse, avg(Value) FROM Boxes GROUP BY Warehouse HAVING avg(Value) > 150;

8. SELECT Boxes.Code, Location FROM Boxes INNER JOIN Warehouses W on W.Code = Boxes.Warehouse;

9. SELECT Warehouse, count(*) FROM Boxes GROUP BY Warehouse;
    
10. SELECT Code FROM Warehouses WHERE Capacity < (SELECT count(*) FROM Boxes WHERE Warehouse = Warehouses.Code);
   
11. SELECT Boxes.Code FROM Boxes RIGHT JOIN Warehouses W on W.Code = Boxes.Warehouse AND Location = 'Chicago';

12. INSERT INTO Warehouses (Code, Location, Capacity) VALUES (6, 'New York', 3);

13. INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('H5RT', 'Papers', 200, 2);

14. UPDATE Boxes SET value = value * 0.85;

15. UPDATE Boxes SET value = value * 0.8 WHERE value > (SELECT avg(Value) FROM (SELECT * FROM Boxes) as "B*");

16. DELETE FROM Boxes WHERE Value < 100;
```
