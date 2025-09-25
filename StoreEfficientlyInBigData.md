Lets say we got Harry potter book which is priced at 500. Based on the trends the price increased to 650. Do we store only the new price. If it is a database, we only save the new price. However in data warehouse we want to save old and new price. This allows us to analyze the book sales and other trends at different price of the book.

Slowly changing dimension (SCD)

**SCD Type 0:** The "do nothing" approach. This is the most basic approach. No changes are captured. i.e. if we have SCD Type 0 on book price, even though the price of book is adjusted on the database, we will not change it in data warehouse. However, SCD Type 0 is rarely used. We care about changes made in database and bring it to data warehouse.

**SCD Type 1:** Overwrite the old value in data warehouse. There is no history of change for this type in data warehouse. It acts just like a database. 

No additional storage required. We loose the history of changes.

ex: change of name. A name change can just be overriden in the data warehouse.

**SCD Type 2:** Keeps the full history of changes. 

One of the most popular methods as it lets you keep full history of changes

Even if one value/ property of an entity changes, we still save all the data of that entity in the new row. This will result in a duplicate primary key because the data is repeating itself. Inorder to avoid duplicate primary key, we can use a surrogate key.

surrogate key - system generated unique identifer for each row to differentiate records

**SCD Type 3:** Add a new column instead of a new row. The new column will contain the new value for a particular column.

Drawback of this approach - we don't maintain full history of that column but just the previous and current value.

**SCD Type 4:** Using a historical table. Uses two tables, one for current data and other for historical data.

Historical table will store all history with timestamp. This approach will have clear separation of duties.

Adds complexity of maintaining two tables.. Storage requirement is high. Getting data is pain because it requires a join.

**SCD Type 6:** Hybrid approach. Combines elements of SCD Type 1, 2 and 3.

Allows you to capture every change, keeps full history and also track the most recent change in one table.




