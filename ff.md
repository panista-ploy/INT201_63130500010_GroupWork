# R-Assignment 4

Created by __CHINCHUTHA THANASUPHAWIMON__ (ID: 63130500010)

## ♨️ Choose Dataset:
1. Top 270 Computer Science / Programing Books (Data from Thomas Konstantin, [Kaggle](https://www.kaggle.com/thomaskonstantin/top-270-rated-computer-science-programing-books)) >> [Using CSV](https://raw.githubusercontent.com/safesit23/INT214-Statistics/main/datasets/prog_book.csv)

### Outlines
1. Explore the dataset
2. Learning function from Tidyverse
3. Transform data with dplyr and finding insight the data
4. Visualization with GGplot2

---
<br>

## 🚪 Part 1: Explore the dataset

```
# Library
library(readr)
library(dplyr)
library(stringr)
library(ggplot2)

# dataset
book <- read_csv("https://raw.githubusercontent.com/safesit23/INT214-Statistics/main/datasets/prog_book.csv")

# viewdataset
glimpse(book)
```

In this dataset has column 
- Rating (numeric) เรตติ้งของหนังสือว่ามีความนิยมมากแค่ไหน
- Reviews (numeric) คะแนนรีวิวของหนังสือว่ามีความนิยมมากแค่ไหน
- Book_title (character) ชื่อของหนังสือ
- Description (character) คำอธิบายของหนังสือ
- Number_Of_Pages (numeric) จำนวนหน้าของหนังสือเล่มนั้นๆ
- Type (character) ประเภทของหนังสือ เช่น หนังสือปกอ่อน หนังสือปกแข็ง
- Price (numeric) ราคาของหนังสือเล่มนั้นๆ
 
```
Rows: 271
Columns: 7
$ Rating          <dbl> 4.17, 4.01, 3.33, 3.97, 4.06, 3.84, 4.09, 4.15, 3.87, 4.62, 4.03, 3.78, 3.7~
$ Reviews         <dbl> 3829, 1406, 0, 1658, 1325, 117, 5938, 1817, 2093, 0, 160, 481, 33, 1255, 59~
$ Book_title      <chr> "The Elements of Style", "The Information: A History, a Theory, a Flood", "~
$ Description     <chr> "This style manual offers practical advice on improving writing skills. Thr~
$ Number_Of_Pages <dbl> 105, 527, 50, 393, 305, 288, 256, 368, 259, 128, 352, 352, 200, 328, 240, 2~
$ Type            <chr> "Hardcover", "Hardcover", "Kindle Edition", "Hardcover", "Kindle Edition", ~
$ Price           <dbl> 9.323529, 11.000000, 11.267647, 12.873529, 13.164706, 14.188235, 14.232353,~
```

---
<br>

## 🚪 Part 2: Learning function from Tidyverse

- Function `arrange()` from package [dplyr](https://dplyr.tidyverse.org/reference/arrange.html)). It using for select columns

```
book %>% arrange(Rating)
```
** You can sepearate this part or combine in part of `Transform data with dplyr and finding insight the data`

---
<br>

## 🚪 Part 3: Transform data with dplyr and finding insight the data

<br>

⚔️ แสดงประเภทของหนังสือว่ามีอะไรบ้างและแต่ละประเภทมีจำนวนเท่าไหร่  

```
book %>% group_by(Type) %>% count()
```

Result

```
 Type                      n
  <chr>                 <int>
1 Boxed Set - Hardcover     1
2 ebook                     7
3 Hardcover                95
4 Kindle Edition           10
5 Paperback               156
6 Unknown Binding           2
```

Explain 

- group_by จัดกลุ่มของประเภทหนังสือว่ามีอะไรบ้าง
- count ใช้นับจำนวนของหนังสือแต่ละประเภทว่ามีทั้งหมดกี่จำนวน

<br>
<br>

⚔️ หนังสือเรื่องอะไรบ้างที่มีราคามากกว่า 100 ขึ้นไป

```
book %>% select(Book_title,Price) %>% filter((Price) > 100)
```

Result

```
  Book_title                                                Price
   <chr>                                                    <dbl>
 1 Computer Architecture: A Quantitative Approach            101.
 2 Computer Systems: A Programmer's Perspective              103.
 3 Modern Operating Systems                                  103.
 4 Modern Operating Systems, 4th Edition                     103.
 5 Structured Computer Organization                          104.
 6 Game Engine Gems 1                                        106.
 7 Concepts, Techniques, and Models of Computer Programming  108.
 8 File Structures: An Object-Oriented Approach with C++     108.
 9 The C++ Standard Library: A Tutorial and Reference        113.
10 Artificial Intelligence: A Modern Approach                116.
# ... with 11 more rows
```

Explain 
- select เลือกข้อมูลเฉพาะ column (Book_title,Price)
- filter ใช้ใส่เงื่อนไขที่ราคาต้องมากกว่า 100
<br>
<br>

⚔️ 3 อันดับหนังสือเล่มไหนที่มีจำนวนหน้าเยอะที่สุด  

```
book %>% select(Book_title,Number_Of_Pages) %>% arrange(desc(Number_Of_Pages)) %>% head(n = 3L)
```

Result

```
Book_title                                                                    Number_Of_Pages
  <chr>                                                                                   <dbl>
1 The Art of Computer Programming, Volumes 1-4a Boxed Set                                  3168
2 The Linux Programming Interface: A Linux and Unix System Programming Handbook            1506
3 Numerical Recipes: The Art of Scientific Computing                                       1235
```

Explain 
- select เลือกข้อมูลเฉพาะ column (Book_title,Number_Of_Pages)
- arrange ในที่นี้ใช้ desc เพื่อใช้เรียงข้อมูลจำนวนหน้าของหนังสือจากมากไปน้อยเพื่อจะได้ง่ายต่อการนำไปคำนวณต่อ
- head ใช้แสดงข้อมูล 3 แถวแรก
<br>
<br>

⚔️ แสดงหนังสือที่ไม่มีคน review เลยและบอกราคาของหนังสือเล่มนั้นๆ

```
book %>% select(Book_title,Reviews,Price) %>% filter(Reviews==0)
```
Result

```
   Book_title                                                                           Reviews Price
   <chr>                                                                                  <dbl> <dbl>
 1 Responsive Web Design Overview For Beginners                                               0  11.3
 2 ZX Spectrum Games Code Club: Twenty fun games to code and learn                            0  14.6
 3 Game Programming Golden Rules                                                              0  19.2
 4 Beginners Guide On PHP Programming: Quick And Easy Guide To Learn PHP With My-SQL          0  23.2
 5 Python Programming Books Set: Python Programming for Beginners & Complete Guide for~       0  23.9
 6 Your First App: Node.js                                                                    0  25.9
 7 Python Programming For Beginners: Quick And Easy Guide For Python Programmers              0  29.0
 8 Building an FPS Game with Unity                                                            0  50.7
 9 Advanced Game Programming: A Gamedev.Net Collection                                        0  59.1
10 Basic Proof Theory                                                                         0  59.5
# ... with 12 more rows
```

Explain 
- select เลือกข้อมูลเฉพาะ column (Book_title,Reviews,Price)
- filter ใช้หาข้อมูล column Reviews ที่เท่ากับ 0
<br>
<br>

⚔️ หนังสือเรื่องไหนที่มีจำนวนรีวิวเยอะที่สุด

```
max_view <- book %>% group_by(Book_title) %>% summarise(Review = max(Reviews))
max_view %>% select(Book_title,Review) %>% filter(Review == max(max_view$Review))
```

Result

```
  Book_title                                                        Review
  <chr>                                                              <dbl>
1 Start with Why: How Great Leaders Inspire Everyone to Take Action   5938
```

Explain 
- group_by จัดกลุ่มของชื่อหนังสือ
- summarise ใช้หาค่า max ของ column reviews และนำข้อมูลทั้ง2อย่างไปเก็บที่ table max_view 
- select เลือกข้อมูลเฉพาะ column (Book_title,Review) จาก table max_view 
- filter ใช้หาค่า review ที่มากที่สุดใน table max_view
<br>
<br>

⚔️ แสดงหนังสือที่ได้เรตติ้งมากที่สุด 5 อันดับแรก

```
book %>% select(Book_title,Rating) %>% arrange(desc(Rating)) %>% head(n = 5L)
```

Result

```
  Book_title                                                                                   Rating
  <chr>                                                                                         <dbl>
1 Your First App: Node.js                                                                        5   
2 The Art of Computer Programming, Volumes 1-4a Boxed Set                                        4.77
3 Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintai~   4.72
4 Build Web Applications with Java: Learn every aspect to build web applications from scratch    4.67
5 Fluent Python: Clear, Concise, and Effective Programming                                       4.67
```

Explain 
- select เลือกข้อมูลเฉพาะ column (Book_title,Rating)
- arrange ในที่นี้ใช้ desc เพื่อใช้เรียงข้อมูลจำนวนเรตติ้งที่มากที่สุด
- head ใช้แสดงข้อมูล 5 แถวแรก

---
<br>

## 🚪 Part 4: Visualization with GGplot2
### 1.) Graph show relation between height and mass
```
scat_plot <- starwars %>% filter(mass<500) %>% ggplot(aes(x=height,y=mass))+
  geom_point(aes(color=gender))

scat_plot+geom_smooth()
```
Result:

![Graph 1](graph1.png)

**Guideline:
Embed Image by using this syntax in markdown file
````
![Name](imageFile)
````
