
new database:
`sqlite3 <name>.db`

open existing
`sqlite3`
`>> .open <name>.db`




```sql
CREATE TABLE zoos(
	id INTEGER PRIMARY KEY,
	name TEXT,
	location TEXT
);

//=> Our database has now been created

INSERT INTO Zoos(name, location)
VALUES("Denver Zoo", "Denver, Colorado"),
VALUES("San Diego Zoo", "San Diego, California");
```

****

**zoo table**

| ID (autofilled) | Name          | Location              |
| --------------- | ------------- | --------------------- |
| 1               | Denver Zoo    | Denver, Colorado      |
| 2               | San Diego Zoo | San Diego, California |


To link 2 tables, we need to specify a "foreign" key

```sql
CREATE TABLE animals(
	id INTEGER PRIMARY KEY,
	species, TEXT,
	name TEXT,
	zoo_id INTEGER,
	FOREIGN KEY (zoo_id) REFERENCES zoos(id)
);

INSERT INTO animals(species, name, zoo_id)
VALUES("Bird", "Larry", 1),
("Bird", "Jerry", 1),
("Bird", "Barry", 1),
("Dinosaur", "Rexy", 1),
("Rodent", "Ratatouille", 2),
("Rodent", "Ratatouille JR", 2),
("Criter", "Mia", 2);
```

We can insert into a table with multiple values using VALUES(),(),()...

| ID (autofilled) | species  | name           | zoo_id |
| --------------- | -------- | -------------- | ------ |
| 1               | Bird     | Larry          | 1      |
| 2               | Bird     | Jerry          | 1      |
| 3               | Bird     | Barry          | 1      |
| 4               | Dinosaur | Rexy           | 1      |
| 5               | Rodent   | Ratatouille    | 2      |
| 6               | Rodent   | Ratatouille JR | 2      |
| 7               | Critter  | Mia            | 2      |

****


To add a new column to an existing table
```sql
ALTER TABLE <table name>
ADD COLUMN <attribute name> <attribute type>

//Example adding column to animals

ALTER TABLE animals
ADD COLUMN age INTEGER;
```

| ID (autofilled) | species  | name           | zoo_id | age  |
| --------------- | -------- | -------------- | ------ | ---- |
| 1               | Bird     | Larry          | 1      | NULL |
| 2               | Bird     | Jerry          | 1      | NULL |
| 3               | Bird     | Barry          | 1      | NULL |
| 4               | Dinosaur | Rexy           | 1      | NULL |
| 5               | Rodent   | Ratatouille    | 2      | NULL |
| 6               | Rodent   | Ratatouille JR | 2      | NULL |
| 7               | Critter  | Mia            | 2      | NULL |
Adding a new column means our previous data is untouched, but has `NULL` values for everything in that column.


We're able to update multiple rows at the same time doing this:

```sql
UPDATE animals
SET age = 1;
```

Our new animals table now looks like this

| ID (autofilled) | species  | name           | zoo_id | age |
| --------------- | -------- | -------------- | ------ | --- |
| 1               | Bird     | Larry          | 1      | 1   |
| 2               | Bird     | Jerry          | 1      | 1   |
| 3               | Bird     | Barry          | 1      | 1   |
| 4               | Dinosaur | Rexy           | 1      | 1   |
| 5               | Rodent   | Ratatouille    | 2      | 6   |
| 6               | Rodent   | Ratatouille JR | 2      | 6   |
| 7               | Critter  | Mia            | 2      | 6   |

To only update certain data,

```sql
UPDATE animals
SET age = 6
where zoo_id = 2;
```




```sql
SELECT animals.name, zoos.name
FROM animals
JOIN zoos
ON animals.zoo_id = zoos.id
WHERE zoos.name = "Denver Zoo"

//=> Larry | Denver Zoo
//=> Jerry | Denver Zoo
//=> Barry | Denver Zoo
```


SELECT/FROM/JOIN/ON/WHERE prints the data that matches our WHERE filter

****

We can view our DB using `SQLite DB browser`, or via HTTP using command
(alias)
`viewdb <database.db>`

Full command:
`sqlite_web --host 0.0.0.0 <name>.db`


****


We should place our foreign key on the element where there is more!

For example, a user can have many posts, but a post can only have 1 user, so we place the foreign key in our post table



****



We should use a "seed" file for creating databses.
Its basically just our CLI commands written into a file so it's easily reproducible 

``` SQL
-- seed.sql

CREATE TABLE IF NOT EXISTS animals(
	id INTEGER PRIMARY KEY,
	name TEXT,
	species TEXT,
);
```

when we have sqlite3 command line open, we use `.read <filename>` to "inject" our sql