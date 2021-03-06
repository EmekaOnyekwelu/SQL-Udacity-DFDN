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

/* query1 the genre that drives the sales */
SELECT SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS 'Track Sales', Genre.Name AS 'Genre Name'
FROM InvoiceLine
JOIN Track
ON InvoiceLine.TrackId = Track.TrackId
JOIN Genre
ON Genre.GenreId = Track.GenreId
GROUP BY 2
ORDER BY 1 desc


/* query2 the sales contribution by country (top 10) */
SELECT SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS 'Track Sales Revenue', Invoice.BillingCountry
FROM InvoiceLine
JOIN Invoice
ON InvoiceLine.InvoiceId = Invoice.InvoiceId
GROUP BY 2
ORDER BY 1 desc


/* query3 the type of genre that drives the sales in topmost country */
SELECT ROUND (SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity),0) AS 'Sales_USA Billing Country', Genre.Name AS 'Genre Name'
FROM InvoiceLine
JOIN Invoice
ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track
ON InvoiceLine.TrackId = Track.TrackId
JOIN Genre
ON Genre.GenreId = Track.GenreId
WHERE Invoice.BillingCountry = 'USA'
GROUP BY 2
ORDER BY 1 desc
LIMIT 10

/* query4 the sales evolution of Rock genre from USA customers */
SELECT CAST(Invoice.InvoiceDate AS year) AS 'Sales Period (Year)', ROUND (SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity),0) AS 'USA Customer Spend (Rock Genre)'
FROM Invoice
JOIN InvoiceLine
ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track
ON InvoiceLine.TrackId = Track.TrackId
JOIN Genre
ON Genre.GenreId = Track.GenreId
WHERE Invoice.BillingCountry = 'USA' AND Genre.Name = 'Rock'
GROUP BY 1



