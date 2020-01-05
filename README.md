# EntityFrameworkCore.SqlServer.SimpleBulks
This is a very simple .net core library that can help to work with large number of records using the **SqlBulkCopy** class.
 
## Overview
This library provides extension methods so that you can use with your EntityFrameworkCore **DbContext** instance:
[DbContextExtensions.cs](/src/EntityFrameworkCore.SqlServer.SimpleBulks/Extensions/DbContextExtensions.cs)
or you can use [SqlConnectionExtensions.cs](/src/EntityFrameworkCore.SqlServer.SimpleBulks/Extensions/SqlConnectionExtensions.cs) to work directly with a **SqlConnection** instance.

## EntityFrameworkCore.SqlServer.SimpleBulks supports:
- Bulk Insert
- Bulk Update
- Bulk Delete

## Examples
- Refer [EntityFrameworkCore.SqlServer.SimpleBulks.Demo](/src/EntityFrameworkCore.SqlServer.SimpleBulks.Demo/Program.cs)

- Update the connection string:
```c#
private const string _connectionString = "Server=.;Database=SimpleBulks;User Id=xxx;Password=xxx;MultipleActiveResultSets=true";
```
- Build and run.

### Using Dynamic String:
```c#
dbct.BulkInsert(rows, "Rows", "Column1", "Column2", "Column3");
dbct.BulkInsert(compositeKeyRows, "CompositeKeyRows", "Id1", "Id2", "Column1", "Column2", "Column3");

dbct.BulkUpdate(rows, "Rows", "Id", "Column3", "Column2");
dbct.BulkUpdate(compositeKeyRows, "CompositeKeyRows", "Id1,Id2".Split(',').ToList(), "Column3", "Column2");

dbct.BulkDelete(rows, "Rows", "Id");
dbct.BulkDelete(compositeKeyRows, "CompositeKeyRows", "Id1,Id2".Split(',').ToList());
```

### Using Lambda Expression:
```c#
dbct.BulkInsert(rows, "Rows", row => new { row.Column1, row.Column2, row.Column3 });
dbct.BulkInsert(compositeKeyRows, "CompositeKeyRows", row => new { row.Id1, row.Id2, row.Column1, row.Column2, row.Column3 });

dbct.BulkUpdate(rows, "Rows", row => row.Id, row => new { row.Column3, row.Column2 });
dbct.BulkUpdate(compositeKeyRows, "CompositeKeyRows", row => new { row.Id1, row.Id2 }, row => new { row.Column3, row.Column2 });

dbct.BulkDelete(rows, "Rows", row => row.Id);
dbct.BulkDelete(compositeKeyRows, "CompositeKeyRows", row => new { row.Id1, row.Id2 });
```

## License
**EntityFrameworkCore.SqlServer.SimpleBulks** is licensed under the [MIT](/LICENSE) license.
