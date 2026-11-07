# ITI – Data Analytics & BI Labs

Hands-on labs from the **Data Analytics & BI** program at the **Information Technology Institute (ITI)**.
Two Visual Studio projects: an **SSAS** cube (multidimensional) and **SSRS** reports.

| Folder | Tool | What it is |
|---|---|---|
| [`ssas-cups`](./ssas-cups) | SQL Server Analysis Services | Sales cube built on a star schema (3 progressive tasks) |
| [`ssrs-day3`](./ssrs-day3) | SQL Server Reporting Services | 8 paginated reports (ITI DB + the Sales cube) |

---

## 1. SSAS – Sales Cube (`ssas-cups`)

**Source:** SQL Server database `sales` (star schema)
- Fact: `Fact Sales` (ProductID, CustomerID, SalesmanID, ChannelID, TimeID, Qty, Qty Total price)
- Dimensions: `ProductDim`, `TimeDim`, `CustomerDim`, `SalesmanDim`, `ChannelDim`

Three cubes, each building on the previous one:

| Cube | Dimensions | Highlights |
|---|---|---|
| **Sales task 1** | Time, Product | Measures: `Qty`, `Qty Total Price`, `Fact Sales Count` |
| **Sales task 2** | Time, Product, Customer | + **KPI** (value = Qty, goal = 1000, status: ≥1000 good / ≥800 neutral / else bad)<br>+ **Calculated member** `task2` = `Qty Total Price / Qty` (average price per unit) |
| **Sales task 3** | Time, Product, Customer, Salesman, Channel (Country, City) | + **Distinct Count** measures for Product, Customer, Salesman and Channel |

## 2. SSRS – Reports (`ssrs-day3`)

Shared data source `DataSource1` → `ITI` database, plus a shared dataset (`Student`).

| Report | Data source | What it shows |
|---|---|---|
| Report1 | `Student` table | Students table with full name, **conditional row color** (age > 23) and a total count |
| Report2 | `Course` ⨝ `Topic` | Courses **grouped by topic** with durations |
| Report3 | `sales` table | **Matrix** (Salesman × Product, Sum of Quantity) + **exploded pie chart** |
| Report4 | `Stud_Course` ⨝ `Student` ⨝ `Course` | Student grades per course, filtered by a **student ID parameter** |
| Report5 | Stored procedure `GetAllCourses` | Course list |
| Report6 | Stored procedure `GetStudentsByAgeRange` | Students filtered by **two parameters** (min/max age) |
| Report7 | `Student` + `Department` | Students filtered by a **multi-value department parameter** |
| Report8 | **MDX** query on the SSAS cube | Qty & Total Price by Customer, Product, Salesman, Country, City |

> Report8 reads from the cube in `ssas-cups` — deploy that project first.

## How to run
1. Restore/create the `sales` and `ITI` databases on your SQL Server.
2. Edit the server name in `ssas-cups/cups/Sales.ds` and `ssrs-day3/day3/DataSource1.rds`.
3. Open `ssas-cups/cups.slnx` → deploy the cube.
4. Open `ssrs-day3/day3.slnx` → preview the reports.

**Requirements:** Visual Studio with the Analysis Services and Reporting Services project extensions, SQL Server.
