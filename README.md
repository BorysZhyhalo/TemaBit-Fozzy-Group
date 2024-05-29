# TemaBit-Fozzy-Group

### Опис проекту.
* На підставі історичних даних з продажу товарів сформувати прогноз продажів на період 14 днів у розрізі товар/день.

# EDA
* Кількість записів: 226486
* Колонки: Unnamed: 0, date, category_id, sku_id, sales_price, sales_quantity
* Всі колонки заповнені, пропущені значення відсутні.
* Колонка date є текстовою, її потрібно буде перетворити на формат дати.

* Distribution of sales by day: Visualization shows that the number of sales has a significant variability, 
which may be due to seasonality or other factors. Also , we have a significant drop in sales_quantity

* Distribution of sales by category: Visualization shows that the category "23" has a significant q-ty compared to "7" and "17"

* Distribution of sales by product: SKUs_id 64522 has the highest q-ty of sales

* Average sale price per day: We can notice some growing of sales from 01-2020

### Look like after the growing in sales price from 01-2020 we have a significant sales drop in q-ty

# Feature Engineering

* Create day indicators for week, month, year, day and day off
* Days Indicator (0 - weekday, 1 - day off)
* It's make sence to drop from the data frame 'sku_id', because this data does not give us value. 