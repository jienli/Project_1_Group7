### CSCI3390 Large Scale Data Processing  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (Jien Li, Xinyu Yao)
# Reporting your findings

1. **(4 points)** Run the program on your local machine to solve cases `k = 2,3,4,5,6`. For each `k`, provide `xS`, its hash value, the total time elapsed, and the number of trials.  
    * k=2
      * Input
      > spark-submit --class "project_1.main" --master "local[*]" target/scala-2.12/project_1_2.12-1.0.jar this_is_a_bitcoin_block_of_88691938_and_36198718 2 1000
      * Output
      > found. count:2
        (2109811611this_is_a_bitcoin_block_of_88691938_and_36198718,00adb7c0e6944980a73f789dc1a0bc54abf3bef5f2c19a1fb302de16f7d4ea75)
        Time elapsed:3s
    * k=3
      * Input
      > spark-submit --class "project_1.main" --master "local[*]" target/scala-2.12/project_1_2.12-1.0.jar this_is_a_bitcoin_block_of_88691938_and_36198718 3 3000
      * Output
      > found. count:1
        (227708588this_is_a_bitcoin_block_of_88691938_and_36198718,000e767da5343299ae3d5ba456b11936596020fcc85ea676a1c577f4561666a1)
        Time elapsed:4s
    * k=4
      * Input
      > spark-submit --class "project_1.main" --master "local[*]" target/scala-2.12/project_1_2.12-1.0.jar this_is_a_bitcoin_block_of_88691938_and_36198718 4 10000
      * Output
      > found. count:1
        (620086861this_is_a_bitcoin_block_of_88691938_and_36198718,000016d4e3930d358e9475a2c17934236f762d0cd6c99058db6896d269261f37)
        Time elapsed:3s
    * k=5
      * Input
      > spark-submit --class "project_1.main" --master "local[*]" target/scala-2.12/project_1_2.12-1.0.jar this_is_a_bitcoin_block_of_88691938_and_36198718 5 1000000
      * Output
      > found. count:1
        (1568524851this_is_a_bitcoin_block_of_88691938_and_36198718,00000a667463124b4a6cbdd5945b80e238f021649cbb41d9dd4e628ebcff456a)
        Time elapsed:6s
    * k=6
      * Input
      > spark-submit --class "project_1.main" --master "local[*]" target/scala-2.12/project_1_2.12-1.0.jar this_is_a_bitcoin_block_of_88691938_and_36198718 6 100000000
      * Output
      > found. count:2
        (1480674721this_is_a_bitcoin_block_of_88691938_and_36198718,000000d6da260c3ebc1cb32151fcc66e6bc4b680065d4d8e954f57bd5049b2b9)
        Time elapsed:85s
   
   Summary
   |k|#Trials|Time Elapsed(s)|xS|Hash Value|
   |:---:|:---:|:---:|:---:|:---:|
   |2|1000|3|2109811611this_is_a_bitcoin_block_of_88691938_and_36198718|00adb7c0e6944980a73f789dc1a0bc54abf3bef5f2c19a1fb302de16f7d4ea75|
   |3|3000|4|227708588this_is_a_bitcoin_block_of_88691938_and_36198718|000e767da5343299ae3d5ba456b11936596020fcc85ea676a1c577f4561666a1|
   |4|10000|3|620086861this_is_a_bitcoin_block_of_88691938_and_36198718|000016d4e3930d358e9475a2c17934236f762d0cd6c99058db6896d269261f37|
   |5|1000000|6|1568524851this_is_a_bitcoin_block_of_88691938_and_36198718|00000a667463124b4a6cbdd5945b80e238f021649cbb41d9dd4e628ebcff456a|
   |6|100000000|85|1480674721this_is_a_bitcoin_block_of_88691938_and_36198718|000000d6da260c3ebc1cb32151fcc66e6bc4b680065d4d8e954f57bd5049b2b9|
        
2. **(3 points)** Run the program on GCP to solve the case `k = 7`. Provide `xS`, its hash value, the total time elapsed, and the number of trials. Describe your cluster's configuration (number of machines, number/type of cores, etc.) and your process for estimating the number of trials needed in order to find the nonce.  
   
   k|#Trials|Time Elapsed(s)|xS|Hash Value|
   |:---:|:---:|:---:|:---:|:---:|
   |7|1000000000|3890|480888476this_is_a_bitcoin_block_of_88691938_and_36198718|000000028dcd3eecf1a309dcc2f5df73f7503ead72d65f4e400e231f574f5836|
   
   Screenshots of the result and the configuration is attached below:
   
   ![3701616200889_ pic_hd](https://user-images.githubusercontent.com/35572120/111854047-ac0e9780-88f3-11eb-8634-17af83c2e6d7.jpg)
   ![3711616201132_ pic_hd](https://user-images.githubusercontent.com/35572120/111854134-19222d00-88f4-11eb-8ed5-93ea7679d247.jpg)


3. **(3 points)** Modify **one** line of code in **src/main/scala/project_1/main.scala** so that the program generates the potential nonce from 1 to `n` (the number of trials) instead of randomly. Discuss whether or not this is more efficient than the randomized approach.
   -  Original
      ``` 
      val nonce = sc.range(0, trials).mapPartitionsWithIndex((indx, iter) => {  
         val rand = new scala.util.Random(indx + seed)  
         iter.map(x => rand.nextInt(Int.MaxValue - 1) + 1)  
      })
      ```
    
   -  Modified
      ``` 
      val nonce = sc.range(0, trials).mapPartitionsWithIndex((indx, iter) => {  
         val rand = new scala.util.Random(indx + seed)  
         iter.map(x => x)  
      })
      ```
   -  We think for the current situation where only one miner is calculating the nonce, a sequential nonce generation is more efficient becuase it ensures that there is no overlap of nonce generated. However, in a realistic bitcoin mining situation, random generation of nonce is more efficient because there are many miners generating nonce and if everyone starts from 0 and increment sequentially, most of the work will be redundant and wasteful.
