# Rest-API-Intermediate-Hackerrank-Test
## Question 1
![](Screenshots/photo_2021-02-03_16-52-22.jpg) 
![](Screenshots/photo_2021-02-03_16-52-27.jpg) 
Solution : Only done with 10 GET requests by taking advantage of the constraint of maximum of 10 goals scored by any team.
#### Method 1:
```javascript
const fetch = require("node-fetch");
let goals=[];
for(let i=0;i<=10;i++)
    goals.push(i);
let ans=0;
async function getDrawnMatches(year) {
    
      await Promise.all(
        goals.map(async (goal) => {
            const respone =  await fetch(`https://jsonmock.hackerrank.com/api/football_matches?year=${year}&team1goals=`+goal+`&team2goals=`+goal);
            const data =  await respone.json();
            ans+=data.total;
            console.log(ans,data.total,goal);
        })
      );
      return ans;
}
getDrawnMatches
    (2011).then((answer) => console.log(answer));
```
#### Method 2 [BEST]:
```javascript
const fetch = require("node-fetch");

async function getDrawnMatches(year) {
    let goals=[];
    let ans=0;
    for(let goal=0;goal<=10;goal++)
    {
        const myPromise = fetch(`https://jsonmock.hackerrank.com/api/football_matches?year=${year}&team1goals=`+goal+`&team2goals=`+goal)
                            .then(res => res.json())
                            .then(data => {
                                console.log(data.total,goal);
                                return data.total
                            });

        goals.push(myPromise);
    }
    await Promise.all(goals).then((array)=>{
        array.forEach( item =>{
            ans+=item;
        })
    })
    return ans;
}
getDrawnMatches
    (2011).then((answer) => console.log(answer));
```
#### Method 3:
```javascript
const fetch = require("node-fetch");

async function getDrawnMatches(year) {
    let goals=[];
    let ans=0;
    for(let goal=0;goal<=10;goal++)
    {
        const myPromise = fetch(`https://jsonmock.hackerrank.com/api/football_matches?year=${year}&team1goals=`+goal+`&team2goals=`+goal)
                            .then(res => res.json())
                            .then(data => {
                                console.log(data.total,goal);
                                return data.total
                            });

        goals.push(myPromise);
    }
    return Promise.all(goals);
}
getDrawnMatches
    (2011).then((array)=>{
        let ans=0;
        array.forEach( item =>{
            ans+=item;
        })
        console.log(ans);
    })
```
#### Method 4:
```javascript
const fetch = require("node-fetch");

async function getDrawnMatches(year) {
    let goals=[];
    let ans=0;
    for(let goal=0;goal<=10;goal++)
    {
        const myPromise = fetch(`https://jsonmock.hackerrank.com/api/football_matches?year=${year}&team1goals=`+goal+`&team2goals=`+goal)
                            .then(res => res.json())
                            .then(data => {
                                ans+=data.total;
                                console.log(data.total,goal);
                                return data.total
                            });

        goals.push(myPromise);
    }
    await Promise.all(goals)
    return ans;
}
getDrawnMatches
    (2011).then((ans)=>{
        console.log(ans);
    })
```
___
## Question 2
![](Screenshots/photo_2021-02-03_16-55-141111111111.jpg) 
![](Screenshots/photo_2021-02-03_16-55-161111111111.jpg)
![](Screenshots/photo_2021-02-03_16-55-181111111111.jpg)
![](Screenshots/photo_2021-02-03_16-55-211111111111.jpg)

Solution : Only done with (1+10+10) GET requests by taking advantage of the constraint of maximum of 10 goals scored by any team.  
            1 for finding the winner  
            10 for summing the goals for home matches  
            10 for summing the goals for away mathces  
```javascript
let goals = [];
for (let i = 0; i <= 10; i++)
    goals.push(i);

async function getGoalsScored(competition, year) {
    let ans = 0;
    let response = await fetch("https://jsonmock.hackerrank.com/api/football_competitions?year=${year}&name=" + competition);
    let data = await response.json();
    let winner = data.data[0].winner;
    console.log(winner);

    await Promise.all(
        goals.map(async (goal) => {
            const respone = await fetch("https://jsonmock.hackerrank.com/api/football_matches?competition=" + competition + "&year=" + year + "&team1=" + "winner+&team1goals="+goal);
            const data = await respone.json();
            ans += data.total * goal;
        })
    );
    await Promise.all(
        goals.map(async (goal) => {
            const respone = await fetch("https://jsonmock.hackerrank.com/api/football_matches?competition=" + competition + "&year=" + year + "&team2=" + "winner+&team2goals="+goal);
            const data = await respone.json();
            ans += data.total * goal;
        })
    );
    return ans;
}
getGoalsScored
    ("UEFA Champions League", 2011).then((answer) => console.log(answer));
```
