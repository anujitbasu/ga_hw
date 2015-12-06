## Terminal Exercises with Chioptle Data
Anujit Basu

1. The Chipotle file consists of item orders with the following fields:
  * **order_id**: An ID representing an order. There are multiple lines with the same order_id because each line represents an ordered item and some orders have multiple items.
  * **quantity**: This field represnts the number of units of the item being ordered.
  * **item_name**: The name of the item.
  * **choice_description**: Additional details for this item.
  * **item_price**: Price of this item. The price is for the total quantity (not per-unit).

  ```head chipotle.tsv```
  
  ```tail chipotle.tsv```
  
2. There appears to be 1834 orders in the file. I did not rely of looking at the ```order_id``` at the bottom of the file. Instead, I extracted all the ```order_id``` values, sorted them, extracted the uniques, and did a line count of the result.

  ```cut -f1 chipotle.tsv > chipotle_order_ids.tsv```
  
  ```sort chipotle_order_ids.tsv > chipotle_order_ids_sorted.tsv```
  
  ``` uniq chipotle_order_ids_sorted.tsv > chipotle_order_ids_sorted_uniq.tsv```
  
  ```wc -l chipotle_order_ids_sorted_uniq.tsv```
  
  ```>>> 1835 chipotle_order_ids_sorted_uniq.tsv```
  
  One of the lines is from the header, therefore there are 1834 unique orders. This method eliminates the need to check if all orders are sequentially numbered, and even that they are all numerical.

3. Lines in the file: There are 4623 lines in the file. However, one of the lines is the header. There are 4622 lines of data.

 ```wc -l chipotle.tsv```

  ```>>> 4623 chipotle.tsv```

4. To compare the popularity of Chicken Burrito to Steak Burrito, I had to find the number of such burritos in the file. A simple count of the number of lines of each type of Burrito would be misleading as some lines had multiple quantities of each burrito. Therefore, I found the lines with each burrito type, took the quantity column of such lines, sorted the quantity, determined the uniqs with count. Then I manually added up the total number of each burrito type.

 ```cat chipotle.tsv | grep 'Chicken Burrito' | cut -f2 | sort | uniq -c```
 
  ```>>> 521 1```
  
  ```>>>  28 2```
  
  ```>>>   2 3```
  
  ```>>>   2 4```

 This tells me that there are 591 Chicken Burritos ordered in the file.

 ``` cat chipotle.tsv | grep 'Steak Burrito' | cut -f2 | sort | uniq -c```
 
  ```>>> 352 1```
  
  ```>>>  14 2```
  
  ```>>>   2 3```

 This tells me that there are 386 Steak Burritos ordered in the file.

 Therefore **Chicken Burritos** are more popular.

5. To detrmine the Black Beans vs Pinto Beans popularity in Chicken Burritos, I used the same methodology above, but applied an additional grep filter for the beans.

 ```cat chipotle.tsv | grep 'Chicken Burrito' | grep 'Black Beans' | cut -f2 | sort | uniq -c```
 
 ```cat chipotle.tsv | grep 'Chicken Burrito' | grep 'Pinto Beans' | cut -f2 | sort | uniq -c```

 I found 307 Chicken Burritos with Black Beans vs 108 with Pinto Beans. It appears that **Black Beans** are overwhelmingly popular with Chicken Burritos. Again, i would like to emphasize that this is a count of the quantity, not just the lines.

6. List of all CSV and TSV files in the DAT-10 directory: First, ```cd``` to the base of the DAT-10 directory. Then execute the command

 ```ls * | grep .[cCtT][sS][vV]$```

 This recursively lists files and filters then where there is a match for either ```.csv``` or ```.tsv```. Since I am not sure if the file names are uniformly upper case or lower case, I decided to search both upper and lower cases. The ```$``` at the end indicates a match at the end of the string, which ends up showing files with names ending in (case insensitive) ```.csv``` and ```.tsv```. 

7. Occurance of word "dictionary" in Dat-10 directory: First ```cd``` to the base of the Dat-10 directory. Then execute the command

 ```grep -r 'dictionary' | wc -l```
 
 I found 88 lines. Assuming there is only one occurance of the word per line, we can say there are approximately 88 occurances.
 
8. Optional advanced command line tasks: I wanted to see what a high value order looked like. For this, I had to find the sum of ```item_price``` for each ````order_id```, order according to the total order value, and get the ```order_id``` for the highest value. Then I could search for the lines in the Chipotle file that have matching ```order_id```.

 First, display the file without the header line,so that everything can be evaluated as numbers.
 
 ```tail -n +2 chipotle.tsv```
 
 Then extract the ```order_id``` and ```item_price```, which are columns 1 and 5
 
 ```tail -n +2 chipotle.tsv | cut -f1,5```
 
 The price column has the $ character, which makes it difficult to treat them as numbers. Delete the $ sign
 
 ```tail -n +2 chipotle.tsv | cut -f1,5 | tr -d "$"```
 
 Now, we can go though the lines, and sum the ```item_price``` for each ```order_id```. For this I used ```awk```.
 
 ```tail -n +2 chipotle.tsv | cut -f1,5 | tr -d "$" | awk '{arr[$1]+=$2} END {for (key in arr) printf("%s\t%s\n", key, arr[key])}'```
 
 The awk command creates an array, using the first column (```order_id```) as the key, and sums the secod column (```item_price```) as the value. At the end of creating the array, iterate through the array and spit out what is essentially ```order_id``` and ```sum of item_price```.
 
 Finally, sort the output on the 2nd column (order value) in descending order, redirect to a file.
 
 ```tail -n +2 chipotle.tsv | cut -f1,5 | tr -d "$" | awk '{arr[$1]+=$2} END {for (key in arr) printf("%s\t%s\n", key, arr[key])}' | sort -k2 -r >> chipotle_order_values.tsv```
 
 View the top 10 lines of the new file: 
 
 ```head chipotle_order_values.tsv```
 
 ```1449    95.39```
 
 ```759     86.3```
 
 ```1454    85.24```
 
 ```1559    82.44```
 
 ```953     81.14```
 
 The ```order_id``` 1449 has the maximum value ```order_value``` of $95.39. So what's in this order?
 
 ```grep ^1449 chipotle.tsv```
 
 ```1449    2       Chicken Burrito [Fresh Tomato Salsa (Mild), [Black Beans, Rice, Fajita Veggies, Cheese]]        $16.98```
 
 ```1449    2       Steak Burrito   [Fresh Tomato Salsa (Mild), [Black Beans, Rice, Fajita Veggies, Cheese]]        $17.98```
 
 ```1449    2       Veggie Bowl     [Fresh Tomato Salsa (Mild), [Black Beans, Rice, Fajita Veggies, Cheese, Lettuce]]       $16.98```
 
 ```1449    2       Chicken Bowl    [Fresh Tomato Salsa (Mild), [Black Beans, Rice, Fajita Veggies, Cheese]]        $16.98```
 
 ```1449    1       Carnitas Burrito        [Roasted Chili Corn Salsa (Medium), [Black Beans, Lettuce]]     $8.99```
 
 ```1449    1       Carnitas Soft Tacos     [Fresh Tomato Salsa (Mild), [Rice, Cheese, Lettuce]]    $8.99```
 
 ```1449    1       Chicken Soft Tacos      [Tomatillo-Red Chili Salsa (Hot), [Cheese, Sour Cream, Lettuce]]        $8.49```
 
 No Chips and Salsa!! No Guac!! No Drinks!! What kind of a party is this?
 
