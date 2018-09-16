/* this is table that I intend to pivot from */
SELECT CAST(Invoice.InvoiceDate AS year) AS Period, SUM(Invoice.Total) AS 'Customer Spend',Employee.LastName AS 'Customer Support Rep'
FROM Invoice
JOIN Customer
ON Invoice.CustomerId = Customer.CustomerId
JOIN Employee
ON Customer.SupportRepId = Employee.EmployeeId
GROUP BY 3,1

/* this is the query for the pivot structure */
SELECT Period, [Johnson] AS 'Johnson_Customer Spend',[Park] AS 'Park_Customer Spend', [Peacock] 'Peacock_Customer Spend'
FROM (
SELECT CAST(Invoice.InvoiceDate AS year) AS Period, SUM(Invoice.Total) AS 'Customer Spend',Employee.LastName AS 'Customer Support Rep'
FROM Invoice
JOIN Customer
ON Invoice.CustomerId = Customer.CustomerId
JOIN Employee
ON Customer.SupportRepId = Employee.EmployeeId
GROUP BY 3,1
) AS pivotdata
pivot (customer spend) for 'customer support rep' in (Johnson, Park, Peacock) as Pivoting



(SELECT CAST(Invoice.InvoiceDate AS year) AS Period, SUM(Invoice.Total) AS 'Johnson_Customer Spend'
FROM Invoice
JOIN Customer
ON Invoice.CustomerId = Customer.CustomerId
JOIN Employee
ON Customer.SupportRepId = Employee.EmployeeId
WHERE Employee.EmployeeId = 5
GROUP BY 1)
JOIN (SELECT CAST(Invoice.InvoiceDate AS year) AS Period, SUM(Invoice.Total) AS 'Park_Customer Spend'
FROM Invoice
JOIN Customer
ON Invoice.CustomerId = Customer.CustomerId
JOIN Employee
ON Customer.SupportRepId = Employee.EmployeeId
WHERE Employee.EmployeeId = 4
GROUP BY 1)
ON


==question1 what genre drives sales==

/* query the genre that drives the sales */
SELECT SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS 'Track Sales', Genre.Name AS 'Genre Name'
FROM InvoiceLine
JOIN Track
ON InvoiceLine.TrackId = Track.TrackId
JOIN Genre
ON Genre.GenreId = Track.GenreId
GROUP BY 2
ORDER BY 1 desc


/* query the sales contribution by country (top 10) */
SELECT SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS 'Track Sales', Invoice.BillingCountry
FROM InvoiceLine
JOIN Invoice
ON InvoiceLine.InvoiceId = Invoice.InvoiceId
GROUP BY 2
ORDER BY 1 desc

/* query the type of genre that drives the sales in topmost country */
SELECT ROUND (SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity),0) AS 'Track Sales', Invoice.BillingCountry, Genre.Name AS 'Genre Name'
FROM InvoiceLine
JOIN Invoice
ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track
ON InvoiceLine.TrackId = Track.TrackId
JOIN Genre
ON Genre.GenreId = Track.GenreId
GROUP BY 2
ORDER BY 1 desc
LIMIT 10


