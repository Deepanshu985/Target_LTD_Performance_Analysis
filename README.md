# Target_LTD_Performance_Analysis
🔗 [Open the Performance Analysis Notebook](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/cba537fd5d8f66ea5fd4080b3bbd2c368537cc4a/Target_Ltd_ecommerce_PA.ipynb)

This project combines **SQL** and **Python** to perform a comprehensive analysis for **Target Ltd**, addressing key business questions around sales performance, regional revenue gaps, and customer segmentation. It includes raw SQL queries and visual insights derived through Python.

---

## 🛠️ Tools & Technologies

- **SQL (MySQL)** – for querying and aggregating sales & customer data  
- **Python** – for visualization  
  - pandas, numpy, seaborn, matplotlib  
- **Jupyter Notebook** – for step-by-step execution  
- **GitHub** – Documentation

---


## About Business
Target Ltd is a globally recognized E-Commerce brand in the United States, known for offering exceptional valuable and a unique shopping experience.

## Objective
The primary objective of this project is to conduct a comprehensive performance analysis of Target Ltd’s operations by leveraging structured data and analytical tools. This involves:
- Systematically addressing business questions outlined by the management team
- Utilizing SQL for querying and extracting relevant insights from order, customer, and product data
- Applying Python to process and visualize complex datasets covering order status, pricing, payment behavior, shipping performance, and customer feedback
- Translating raw data into actionable business intelligence to support decision-making across sales, logistics, and customer engagement

## 🧾 SQL Queries & Insights

**1. List all unique cities where customers are located.**

```sql
query = "select distinct customer_city from customers"
cur.execute(query)
data = cur.fetchall()
df=pd.DataFrame(data)
df.head()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/0957202f2ce2b760ceff807578e243329a90701c/outputs/visuals/Screenshot%202025-07-22%20185724.png)

--

**2. Count the number of orders placed in 2017.**

```sql
query = "select count(order_id) from orders where year(order_purchase_timestamp) = 2017 "
cur.execute(query)
data = cur.fetchall()
data[0][0]
```
**📊 Insight:** 
45101

--

**3. Find the total sales per category.**

```sql
query = """select products.product_category category,
round(sum(payments.payment_value)) sales
from products join order_items
on products.product_id= order_items.product_id
join payments
on payments.order_id = order_items.order_id
group by category 
"""

cur.execute(query)
data = cur.fetchall()
df= pd.DataFrame(data, columns = ["Category", "Sales"])  #created data frame for the result
df
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/bf16ac8e095840a7fffc455c71c45b7b70f7d8c7/outputs/visuals/Screenshot%202025-07-22%20183316.png)

--

**4. Calculate the percentage of orders that were paid in installments**

```sql
query = """select sum(case when payment_installments >=1 then 1 else 0 end)/count(*)*100 from payments"""

cur.execute(query)
data = cur.fetchall()
data[0][0]
```
**📊 Insight:** 
Decimal('99.9981')

--

**5. Count the number of customers from each state.**

```sql
query = """select customer_state, count(customer_id)
from customers group by customer_state"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data, columns = ["state","customer_count"])
df= df.sort_values(by = "customer_count", ascending = False) 

plt.figure(figsize = (9,4))
plt.bar(df["state"],df["customer_count"])
plt.xticks(rotation = 90)
plt.xlabel("states")
plt.ylabel("customer_count")
plt.title("Count of Customers by states")
plt.show()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/05d7b2108da452599aab628471988d36d7d58877/outputs/visuals/Screenshot%202025-07-22%20183843.png)

--

**6. Calculate the number of orders per month in 2018.**

```sql
query = """select  monthname(order_purchase_timestamp) months, count(order_id) order_count
from orders where year(order_purchase_timestamp) = 2018
group by months
"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data, columns= ["months","order_count"])
o = ["January","February","March","April","May","June","July","August","September","October","November","December"]

ax=sns.barplot(x = df["months"], y= df["order_count"], order = o)
plt.xticks(rotation = 90)
ax.bar_label(ax.containers[0])
plt.title("Count of Orders by Months in 2018")
plt.show()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/3dcf80376bf6b944f69e3989e4d29e4d5917cd16/outputs/visuals/Screenshot%202025-07-22%20184112.png)

--

**7. Find the average number of products per order, grouped by customer city**

```sql
query = """with count_per_order as 
(select orders.order_id, orders.customer_id, count(order_items.order_id) as oc
from orders join order_items
on orders.order_id = order_items.order_id
group by orders.order_id, orders.customer_id)

select customers.customer_city, round(avg(count_per_order.oc),2) average_orders
from customers join count_per_order
on customers.customer_id = count_per_order.customer_id
group by customers.customer_city
"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data,columns = ["customer city","average products/order"])
df.head(10)
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/c44143f372a03ff4b4a96bccc3ee7711323c7c50/outputs/visuals/Screenshot%202025-07-22%20184258.png)

--

**8. Calculate the percentage of total revenue contributed by each product category**

```sql
query = """select upper(products.product_category) category,
round((sum(payments.payment_value)/(select sum(payment_value) from payments))*100,2) sales_percentage
from products join order_items
on products.product_id = order_items.product_id
join payments
on payments.order_id = order_items.order_id
group by category order by sales_percentage desc
"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data, columns = ["category","sales%"])
df.head(10)
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/4bb86240aea1e0297b36ccef949d22783bae362d/outputs/visuals/Screenshot%202025-07-22%20184434.png)

--

**9. Identify the correlation between product price and the number of times a product has been purchased.**

```sql
query = """select products.product_category category,
count(order_items.product_id) order_count,
round(avg(order_items.price),2) avg_price
from products join order_items
on products.product_id = order_items.product_id
group by category
"""
cur.execute(query)
data = cur.fetchall()

arr1 = df["order_count"]
arr2 = df["price"]

a = np.corrcoef([arr1,arr2])
print("The Correlation between price and number of times a product has been purchased is", a[0][1])
```
**📊 Insight:** 
The Correlation between price and number of times a product has been purchased is -0.10631514167157562

--

**10. Calculate the total revenue generated by each seller, and rank them by revenue.**

```sql
query = """ select *, dense_rank() over(order by revenue desc) as rn from
(select order_items.seller_id seller, sum(payments.payment_value) revenue
from order_items join payments
on order_items.order_id = payments.order_id
group by seller) as a
"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data, columns = ["seller_id","revenue","rank"])
sns.barplot(x= "seller_id", y="revenue", data =df.head())
plt.xticks(rotation=90)
plt.show()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/af345431b9fd5bc583eae9b0c0c9d7d1dbd65d30/outputs/visuals/Screenshot%202025-07-22%20184734.png)

--

**11. Calculate the moving average of order values for each customer over their order history.**

```sql
query = """ select customer_id, order_purchase_timestamp,payment,
avg(payment) over(partition by customer_id order by order_purchase_timestamp
rows between 2 preceding and current row) as mov_avg
from
(select orders.customer_id, orders.order_purchase_timestamp,
payments.payment_value as payment
from payments join orders
on payments.order_id = orders.order_id) as a
"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data, columns=["customer_id","timestamp","price","mov_avg"])
df.head()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/daab14c657fb5b9fdb9efb43f3cdfdff79fcf73e/outputs/visuals/Screenshot%202025-07-22%20184948.png)

--

**12. Calculate the cumulative sales per month for each year.**

```sql
query = """ select years, months, payment,sum(payment)
over(order by years,months) as cum_sales from
(select year(orders.order_purchase_timestamp) as years,
month(orders.order_purchase_timestamp) as months,
round(sum(payments.payment_value),2) as payment
from orders join payments
on orders.order_id = payments.order_id
group by years, months order by years, months) as a
"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data, columns=["year","month","sales","cum_sales"])
df.head()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/9e0d6b4144130dd4dbaa4834d762db64d23156ec/outputs/visuals/Screenshot%202025-07-22%20185138.png)

--

**13. Calculate the year-over-year growth rate of total sales.**

```sql
query = """ with a as(select year(orders.order_purchase_timestamp) as years,
round(sum(payments.payment_value),2) as payment
from orders join payments
on orders.order_id = payments.order_id
group by years order by years)

select years, payment, ((payment-lag(payment,1) over(order by years))/lag(payment,1) over(order by years)) *100  from a
"""

cur.execute(query)

data = cur.fetchall()
df=pd.DataFrame(data, columns=["year","payment","yoy % growth"])
df.head()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/83527d3c73f5b5d63d8e14a30f3275cb3a778794/outputs/visuals/Screenshot%202025-07-22%20185317.png)

--

**14. Calculate the retention rate of customers, defined as the percentage of customers who make another purchase within 6 months of their first purchase.**

```sql
query = """ with a as(select customers.customer_id,
min(orders.order_purchase_timestamp) first_order
from customers join orders
on customers.customer_id = orders.customer_id
group by customers.customer_id),

b as (select a.customer_id, count(distinct orders.order_purchase_timestamp) next_order
from a join orders
on orders.customer_id = a.customer_id
and orders.order_purchase_timestamp > first_order
and orders.order_purchase_timestamp < 
date_add(first_order,interval 6 month)
group by a.customer_id)

select 100 * (count(distinct a.customer_id)/ count(distinct b.customer_id))
from a left join b
on a.customer_id = b.customer_id
"""

cur.execute(query)

data = cur.fetchall()
data
```
**📊 Insight:** 
[(None,)]
since we did not have any repeating customer within 6 months of purchase, the answer is null.

--

**15. Identify the top 3 customers who spent the most money in each year.**

```sql
query="""select years, customer_id, payment,d_rank
from
(select year(orders.order_purchase_timestamp) years , orders.customer_id,
sum(payments.payment_value) payment,
dense_rank() over(partition by year(orders.order_purchase_timestamp) order by sum(payments.payment_value) desc) d_rank
from orders join payments
on orders.order_id = payments.order_id
group by year(orders.order_purchase_timestamp), orders.customer_id) as a 
where d_rank <=3
"""

cur.execute(query)

data=cur.fetchall()
df = pd.DataFrame(data, columns = ["years","id","payment","rank"])

sns.barplot(x= "id", y="payment", data= df, hue="years")
plt.xticks(rotation= 90)
plt.show()
```
**📊 Insight:** 
![](https://github.com/Deepanshu985/Target_LTD_Performance_Analysis/blob/2891c9961d62f32491b36b7b76a2aa0db4eeb2c6/outputs/visuals/Screenshot%202025-07-22%20185614.png)

---
## 🧠Conclusion
This analysis helped uncover key growth challenges and opportunities for Target Ltd, combining the power of SQL queries with Python-based data visualization. The recommendations are data-driven, stakeholder-friendly, and ready to be put into action.




