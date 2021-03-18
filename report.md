## Reporting your findings
You'll be submitting a report along with your code that provides commentary on the tasks below.  

1. **(4 points)** Run the program on your local machine to solve cases `k = 2,3,4,5,6`. For each `k`, provide `xS`, its hash value, the total time elapsed, and the number of trials.  

    * k=2
      * Input
      > 
      * Output
      > 
    * k=3
      * Input
      > 
      * Output
      > 
    * k=4
      * Input
      > 
      * Output
      > 
    * k=5
      * Input
      > 
      * Output
      > 
    * k=6
      * Input
      > (base) MacBook-Air:Project_1_Group7 jienli$ spark-submit --class "project_1.main" --master "local[*]" target/scala-2.12/project_1_2.12-1.0.jar this_is_a_bitcoin_block_of_88691938_and_36198718 6 100000000
      * Output
      > found. count:2
        (1480674721this_is_a_bitcoin_block_of_88691938_and_36198718,000000d6da260c3ebc1cb32151fcc66e6bc4b680065d4d8e954f57bd5049b2b9)
        Time elapsed:85s
        
2. **(3 points)** Run the program on GCP to solve the case `k = 7`. Provide `xS`, its hash value, the total time elapsed, and the number of trials. Describe your cluster's configuration (number of machines, number/type of cores, etc.) and your process for estimating the number of trials needed in order to find the nonce.  


3. **(3 points)** Modify **one** line of code in **src/main/scala/project_1/main.scala** so that the program generates the potential nonce from 1 to `n` (the number of trials) instead of randomly. Discuss whether or not this is more efficient than the randomized approach.
