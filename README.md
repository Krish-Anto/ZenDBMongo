Design database for Zen class programme

users

codekata

attendance

topics

tasks

company_drives

mentors

Questions:
1.Find all the topics and tasks which are thought in the month of October ?

db.topics.aggregate([
{
$lookup: {
from: “tasks”,
localField: “topicid”,
foreignField: “topicid”,
as: “taskinfo”
}
},
{
$match: {
$and: [
{ topicDate: { $gte: new Date(“2020–10–01”), $lt: new Date(“2020–11–01”) } },
{
$or: [
{ “taskinfo.task_date”: { $gte: new Date(“2020–10–01”), $lt: new Date(“2020–11–01”) } },
{ “taskinfo.task_date”: { $exists: false } }
]
}
]
}
},
{
$project: {
_id: 0,
topicid: 1,
topic: 1,
topicDate: 1,
task_date: “$taskinfo.task_date”
}
}
])

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

2.Find all the company drives which appeared between 15 oct-2020 and

31-oct-2020?

Answer:
db.companydrives.find({
$or: [
{ drive_date: { $gte: new Date(“2020-oct-15”) ,$lte: new Date(“2022-oct-31”)} }

]
})

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — -

3.Find all the company drives and students who are appeared for the placement.?

Answer:

db.companydrives.aggregate([
{
$lookup: {
from: “users”,
localField: “userid”,
foreignField: “userid”,
as: “info”,
},
},
{
$project: {
_id: 0,
company: 1,
drive_date: 1,
“info.name”: 1,
“info.email”: 1,
“info.userid”: 1,
},
},
])

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — -

4.Find the number of problems solved by the user in codekata.?

Answer:

db.codekata.aggregate([
{
$lookup: {
from: “users”,
localField: “userid”,
foreignField: “userid”,
as: “info”,
},
},
{
$project: {
_id: 0,
userid: 1,
solved_problems: 1,
“info.name”: 1,
},
},
])

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

5. Find all the mentors with who has the mentee’s count more than 15?

Answer:

db.mentors.find({mentee_count:{$gte:15}})

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

6.Find the number of users who are absent and task is not submitted between 15 oct-2020 and 31-oct-2020.?

Answer:

db.attendence.aggregate([
{
$lookup: {
from: “topics”,
localField: “topicid”,
foreignField: “topicid”,
as: “topics”,
},
},
{
$lookup: {
from: “tasks”,
localField: “topicid”,
foreignField: “topicid”,
as: “tasks”,
},
},
{ $match: { $and: [{ staus: false }, { “tasks.submitted”: false }] } },
{
$match: {
$and: [
{
$or: [
{ “topics.topicDate”: { $gte: new Date(“2020–10–15”) , $lte: new Date(“2020–10–31”) } }
],
},
{
$or: [
{ “tasks.task_date”: { $gte: new Date(“2020–10–15”) , $lte: new Date(“2020–10–31”) } },
],
},
],
},
},
{
$count:”Absentees”,
},
])
