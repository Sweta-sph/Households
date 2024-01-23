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
 
