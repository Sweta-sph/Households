# Households
Getting insights for a family members of their income and expenses

select * from household;

---Select the members where avg income is greater than 8000----
select avg(income) from household
where expenses > 8000;


---Select the members whose income is greater than the---
----avg_income based on age_group.----
select count(*) from (
select member_name, income, avg(income) over(partition by 
		case 
		when (year(current_date())-year(birth_date)) between 20 		and 30 then '20-30'
		when (year(current_date())-year(birth_date)) between 30 		and 40 then '30-40'
		when (year(current_date())-year(birth_date)) between 40 		and 50 then '40-50'
		else '50-60'
		end) as avg_income
 from household) as t 
 where income >avg_income;

 
 -----Get the percentile of expeses based on expese type----
 select major_expense_type,
 (sum(expenses) over (partition by major_expense_type)/
  sum(expenses) over ()*100) as percentileOfExpense 
 from household;


 -----Which household member(s) spent more than $1000 on ----
 ------medical expenses?----
select member_name, e.* from household as h 
join (
select member_id,expense_type, sum(amount) over (partition by member_id, expense_type	 
 order by member_id, expense_type) as Total_expense
 from expenses) as e 
 on h.member_id = e.member_id 
 where e.Total_expense > 1000 and expense_type = 'Medical';
 
 
 
 
 SELECT h.member_id, h.member_name, h.income, SUM(e.amount) AS total_expenses,
       (SUM(e.amount) / h.income) * 100 AS expense_percentage
FROM household h
JOIN expenses e ON h.member_id = e.member_id
GROUP BY h.member_id, h.member_name, h.income
HAVING expense_percentage < 20;

