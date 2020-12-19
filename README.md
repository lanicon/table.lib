# table.lib

Simple table library that renders any `List<T>` into a nicely formatted `markdown`, `csv`, `html` or `console` table, allowing for extra formats.

## Markdown format

```c#
Table<TestClass>.Add(list).WriteToConsole();
```

```bash
| Field1 | Field2               | Field3        | Field4 | Field5      | Field6 |
| ------ | -------------------- | ------------- | ------ | ----------- | ------ |
| 321121 | Hi 312321            | 2,121.32      | True   | 01-Jan-1970 | 34.43  |
| 32321  | Hi long text         | 21,111,111.32 | True   | 01-Jan-1970 | 34.43  |
| 321    | Hi longer text       | 2,121.32      | True   | 01-Jan-1970 | 34.43  |
| 13     | Hi very long text    | 21,111,121.32 | True   | 01-Jan-1970 | 34.43  |
| 13     | Hi very, long text   | 21,111,121.32 | True   | 01-Jan-1970 | 34.43  |
| 13     | Hi "very" long  text | 21,111,121.32 | True   | 01-Jan-1970 | 34.43  |
```

| Field1 | Field2               | Field3        | Field4 | Field5      | Field6 |
| ------ | -------------------- | ------------- | ------ | ----------- | ------ |
| 321121 | Hi 312321            | 2,121.32      | True   | 01-Jan-1970 | 34.43  |
| 32321  | Hi long text         | 21,111,111.32 | True   | 01-Jan-1970 | 34.43  |
| 321    | Hi longer text       | 2,121.32      | True   | 01-Jan-1970 | 34.43  |
| 13     | Hi very long text    | 21,111,121.32 | True   | 01-Jan-1970 | 34.43  |
| 13     | Hi very, long text   | 21,111,121.32 | True   | 01-Jan-1970 | 34.43  |
| 13     | Hi "very" long  text | 21,111,121.32 | True   | 01-Jan-1970 | 34.43  |

Considerations:

- Any `CRLF` will be automatically transformed into a space ` ` for easy representation of the output

## Dynamic Fields

If the List contains another collection of <strings>, the library is able to scan those and build the resultant dataset giving them a column name called `DynamicX`:

```c#
var test = new List<IEnumerable<string>>
{
    new List<string>() {"AAA", "BBB", "CCC"},
    new List<string>() {"AAA", "BBB", "CCC"},
    new List<string>() {"AAA", "BBB", "CCC"},
    new List<string>() {"AAA", "BBB", "CCC"}
};

Table<IEnumerable<string>>.Add(test).WriteToConsole();
```

```bash
| Capacity | Count | Dynamic0 | Dynamic1 | Dynamic2 |
| -------- | ----- | -------- | -------- | -------- |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
```

| Capacity | Count | Dynamic0 | Dynamic1 | Dynamic2 |
| -------- | ----- | -------- | -------- | -------- |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |

## Column Name change

If the name of the column is not of your liking, you can change it via `OverrideColumns` and provide your preferred name. Note that this will also alter the column width to allow for more room if the new name is larger than the previous one.

```c#
Table<IEnumerable<string>>.Add(test).
    OverrideColumns(new Dictionary<string, string> {{"Dynamic0","ColumnA"}}).
    WriteToConsole();
```

```bash
| Capacity | Count | ColumnA  | Dynamic1 | Dynamic2 |
| -------- | ----- | -------- | -------- | -------- |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |

```

| Capacity | Count | ColumnA  | Dynamic1 | Dynamic2 |
| -------- | ----- | -------- | -------- | -------- |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |
| 4        | 3     | AAA      | BBB      | CCC      |


## Column filtering

You don't want to show all the columns? Easy, just use the `FilterColumns` property:

```c#
Table<IEnumerable<string>>.Add(test).
    OverrideColumns(new Dictionary<string, string> { { "Dynamic0", "ColumnA" } }).
    FilterColumns(new Dictionary<string, bool> { { "Capacity", false }, { "Count", false } }).
    WriteToConsole();
```

```bash
| ColumnA  | Dynamic1 | Dynamic2 |
| -------- | -------- | -------- |
| AAA      | BBB      | CCC      |
| AAA      | BBB      | CCC      |
| AAA      | BBB      | CCC      |
| AAA      | BBB      | CCC      |
```

| ColumnA  | Dynamic1 | Dynamic2 |
| -------- | -------- | -------- |
| AAA      | BBB      | CCC      |
| AAA      | BBB      | CCC      |
| AAA      | BBB      | CCC      |
| AAA      | BBB      | CCC      |

## HTML Output

Transform your output into a nicely formatted HTML table

```c#
Table<IEnumerable<string>>.Add(test).
    OverrideColumns(new Dictionary<string, string> { { "Dynamic0", "ColumnA" } }).
    FilterColumns(new Dictionary<string, bool> { { "Capacity", false }, { "Count", false } }).
    WriteToHtml("C:\temp\file.html");
```

## CSV Output

Trasform your output into a nicelt formatted CSV file

```c#
Table<TestClass>.Add(list).
    WriteToCsv(@"C:\temp\test-list.csv");
```

The format of the file can be seen here:

```bash
Field1,Field2,Field3,Field4,Field5,Field6
321121,Hi 312321,"2,121.32",True,01-Jan-1970,34.43
32321,Hi long text,"21,111,111.32",True,01-Jan-1970,34.43
321,Hi longer text,"2,121.32",True,01-Jan-1970,34.43
13,Hi very long text,"21,111,121.32",True,01-Jan-1970,34.43
13,"Hi very, long text","21,111,121.32",True,01-Jan-1970,34.43
13,"Hi ""very"" long
 text","21,111,121.32",True,01-Jan-1970,34.43
```

Note that we use the [CSV standard](https://tools.ietf.org/html/rfc4180) when processing `CRLF`, `"` or `,` characters surrouding the value with double quotes. 

![csvoutput](csvoutput.png)
