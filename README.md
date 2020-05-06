# DMA-Sales-Data-Analysis

# The purpose of this exercise is to show performance by designated market area - commonly referred to as DMA
Overview
The sales_archive.db contains four tables: dmas, sales, transactions, visits. The purpose of this exercise is to show performance by designated market area 

Questions 
1. What’s the total sales amount by designated market area (DMA)? List the name (not the id) and total sales amount for each DMA.

2. Average Order Value (AOV) is defined as sales.amount divided by transactions.transaction_count. List the name (not the id) and average order value for each DMA on January 1st, 2019. Order the result set from highest average order value to lowest.

3. For each DMA, calculate the average, lowest, and highest sales.amount for the month of January 2019. List the name (not the id), average sales amount, minimum sales amount, and maximum sales amount for each DMA.

4. Seems like something may be wrong with the data in our visits table. For the month of February 2019, list the name (not the id) and the frequency (count of occurrences where the condition is true) where a DMA’s visit count is greater than the transaction count. Order by the DMA name.


1. What’s the total sales amount by designated market area (DMA)? List the name (not the id) and total sales amount for each DMA.


select name,sum(amount) as total from dmas inner join sales on dmas.id = sales.dma_id group by name order by sum(amount)
 
2. Average Order Value (AOV) is defined as sales.amount divided by transactions.transaction_count. List the name (not the id) and 
average order value for each DMA on January 1st, 2019. 
Order the result set from highest average order value to lowest.


select b.name, sum_amount/transaction_count as aov
from  (
	select date,dmas.id,name,sum(amount) sum_amount 
	from dmas 
	inner join sales 
		on dmas.id = sales.dma_id  
	group by date,dmas.id,name
) b
join transactions t 
	on b.id = t.dma_id and t.date = b.date
where t.date = '2019-01-01'
group by  b.name,sum_amount/transaction_count
order by sum_amount/transaction_count desc


3. For each DMA, calculate the average, lowest, and highest sales.amount for the month of January 2019. 
List the name (not the id), average sales amount, minimum sales amount, and maximum sales amount for each DMA.


select name, avg(amount),min(amount),max(amount)
from sales s
join dmas d
	on d.id = s.dma_id
where date < '2019-02-01' --month of January 2019. 
group by name



4. Seems like something may be wrong with the data in our visits table. 
For the month of February 2019, list the name (not the id) and the frequency (count of occurrences where the condition is true)
where a DMA’s visit count is greater than the transaction count. Order by the DMA name.


select name, count(*) as frequency
from visits  v
join transactions t
	on v.dma_id = t.dma_id  AND v.date =t.date
join dmas d
	on d.id = t.dma_id
where v.date between  '2019-02-01' and '2019-03-01' and visit_count > transaction_count 
group by name 
order by name 
--where a DMA’s visit count is greater than the transaction count

