1.Знайти всіх людей, зп більше 500. за спаданням.
```js
db.People.find({
"Salary" : { "$gt" : 500 }})
.sort({"Salary" : -1});
```

2.Людей 300 до 500. вивести іх клас.
```js
db.People.find({
"Salary": {$gte: 300, $lte: 500}},
{"Clases_name": 1});
``` 

3.Знайти людей з ЗП 53 и 368.
```js
db.People.find({ Salary: { $in: [100, 500] }})
```

4.Знайти суму всіх зарплат всіх людей.
```js
db.People.aggregate([{
$group: {
_id: null,
sum: { $sum: "$Salary" }}}]);
```

5.Знайти суму зарплат всіх людей класу jun.
```js
db.People.aggregate([{
$match: {
Clases_name: "jun"}},
{$group: {_id: null,sum: { $sum: "$Salary" }},}
])
```

6.Знайти всіх людей в який імя закінчуется на МА.
```js
db.People.find({ Name: { $regex: "ma$" } });
```

7.Знайти людей в яких в імені є буква О.
```js
db.People.find({ People_name: { $regex: /o/ } });
```

8.Знайти всіх людей jun та sen.
```js
db.People.find({ Clases_name: { $in: ["jun", "sen"] } });
```

9.Знайти всіх людей jun та sen старше 1997 року.
```js
db.People.find({
$and: [
{ Clases_name: { $in: ["jun", "sen"] } },
{ Date: { $gt: "1997" } }] });
```

10.Знайти всіх людей в яких ім'я починається на Н або М з ЗП більше 1400.
```js
db.People.find({
$or: [
{ People_name: { $regex: "^D" } },
{ People_name: { $regex: "^I" } }],
Salary: { $gt: 1400 }});
```

11.Знайти всіх людей midd між 1997 иа 1999 роком включно.
```js
db.People.find({
Clases_name: "midd",
Date: {
$gte: "1997.01.01",
$lte: "1999.12.31"}});
```

12.Знайти середню ЗП по всім.
```js
db.People.aggregate([{
$group: {
_id: null,
avg: { $avg: "$Salary" }},
}]);
```

13.Знайти всіх людей які належать до класу jun в якому таблиця міні менше 500.
```js
db.People.find({
$and: [
{ Clases_name: "jun" },
{ Salary: { $lte: 500 } }
]});
```

14.Знайти всіх людей класу, в назві якого є буква "I".
```js
db.People.find({ Clases_name: { $regex: /i/ } })
```

15.Вивести середні ЗП по кожному класу. 
```js
db.People.aggregate([
{
$group: {
_id: "$Clases_name",
avg: { $avg: "$Salary" }},
}]);
```

16.Вивести суми зарплат по кожному класу.
```js
db.People.aggregate([{
$group: {
_id: "$Clases_name",
sum: { $sum: "$Salary" }},
}]);
```

17.Знайти всіх людей в яких зп більше 500 або вони старше 25 років.
```js
db.People.find({
$or: [
{ Salary: { $gte: 500 } },
{ Date: { $lt: "1997" } }
]});
```

18.Вивести всіх людей але щоб колонка Ім'я була унікальною.
```js
db.People.distinct("People_name");
```

19.Уявляємо що зп вказана в таблиці в долларах. Вивести ЗП людей в гривні. 
```js
db.People.aggregate([{
$project: {
People_name: 1,
SalaryUAH: {
$multiply: ["$Salary", 38]}}}
])
```

20.Знайти 5-ть найстаріших замовлень.
```js
1) db.Orders.aggregate([
{ $sort: { Order_date: 1 } },
{ $limit: 5 }
]);

2) db.Orders.find().sort({"Order_date": 1}).limit(5);
```

21.Знайти замовлення, де люди відносяться до класу jun.
```js
db.Orders.aggregate([{
$lookup: {
from: "People",
localField: "Peopleid",
foreignField: "_id",
as: "People"},},
{
$match: {
"People.Clases_name": "jun"},}
]);
```

22.Вивести кількість замовлень, в яких ім'я людини закінчується на А і вони midd клас.
```js
db.Orders.aggregate([{
$lookup: {
from: "People",
localField: "Peopleid",
foreignField: "_id",
as: "People"}},
{
$match: {
"People.People_name": {$regex: "a$" },
Clases_name: "midd"}}
]);
```
