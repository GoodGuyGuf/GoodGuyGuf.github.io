---
layout: post
title:      "Rails Back-End && JavaScript Front-End: myBudget"
date:       2020-03-12 01:20:37 +0000
permalink:  rails_back-end_and_and_javascript_front-end_mybudget
---


Javascript month has been the most difficult month of the program so far. I needed more than a few reviews of the curriculum to get a little bit of a handle on it. Knowing Ruby made it helpful as well to learn this language. I knew what things were going to achieve in the lessons using Javascript instead. 

The project this month is a "Single Page Application." It took me a while to find an idea where I could use the app often. I ended up choosing a simple budgeting type of application. A User can create budgets and add expenses. Using `reduce()`, when a user adds an expense the remaining balance gets updated to the site without redirection. Once the balance is zero, the User cannot add anymore entries. The remaining balance will stay stuck at 0.

We split the project into two pieces. The Back-end and the Front-end. The Back-end uses rails as an API only application. This means that our rails app will not use erb files and will not render views. Instead in the controller you will render JSON (JavaScript Object Notation). Your Front-end will then send Fetch requests to the Back-end and update the Front-end with the response. We use Postgresql as the database.

This project requires quite a bit of thought and a lot more planning than the previous projects. Appending Elements to the DOM took more time than I had planned it to. Using fetch requests to create associated data to models was a bit challenging. 

Here is an example of one of my Post Fetch Requests:

```
  newBudget(budgetObject) {
        let fetchObject = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json"
            }, 
            body: JSON.stringify(budgetObject)
        }
    fetch("http://localhost:3000/budgets", fetchObject)
        .then(function(response) { 
        return response.json();
    })
    .then(function(json){
        new Budget(json);
        addToDom(json.data.id, json.data.attributes);
    })
    .catch(function(error) {
        alert("There was a problem in the Fetch Request.");
        console.log(error.message);
      });
    }

```

Fetch takes two arguments. A url and an object. The browser needs to know ahead of time what type of data it is recieving so we set the content type to JSON. The fetch will also recieve data back when it sends its request to the url. We will accept back JSON data. Data in json will only be sent as text so we stringify the object we send in as an argument. When I recieve the response back I create a Javascript Budget Object and append it to the DOM using a separate function. If there was an error, I can catch it as an alert and I can go back to exactly where it messed up and console.log() everything till I see what made an error.
