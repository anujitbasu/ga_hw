# Terminal Exercises with Chioptle Data
Anujit Basu

1. The chipotel file consists of item orders with the following fields:
  * **order_id**: An ID representing an order. There are multiple lined with the same order_id becuase each line represents and order item and some orders have multiple items.
  * **quantity**: This filed represnts the number of units of items this line represnts.
  * **item_name**: The name of the item.
  * **choice_description**: Additional details for this item
  * **item_price**: Per-unit proce of this item.

  ```head chipotle.tsv```
  
  ```tail chipotle.tsv```
  
2. There appears to be 1834 orders in the file. I did not rely of looking at the order_id at the bottom of the file. Instead, I extracted all the order_ids, sorted them, extracted the uniques, and did a line count of the result.

    ```cut -f1 chipotle.tsv > chipotle_order_ids.tsv```
    
    ```sort chipotle_order_ids.tsv > chipotle_order_ids_sorted.tsv```
    
    ``` uniq chipotle_order_ids_sorted.tsv > chipotle_order_ids_sorted_uniq.tsv```
    
    ```wc -l chipotle_order_ids_sorted_uniq.tsv```
    
    ```>>> 1835 chipotle_order_ids_sorted_uniq.tsv```
    
One of the lines is from the header, therefore there are 1834 unique orders. This method eliminates the need to check if all orders are sequentially numbered, and even that there are all numbers.

3. Lines in the file: There are 4623 lines in the file. However, one of the lines is the header. There are 4622 lines of data.

 ```wc -l chipotle.tsv```

 ```>>> 4623 chipotle.tsv```


