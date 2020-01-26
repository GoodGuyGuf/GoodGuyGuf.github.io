---
layout: post
title:      "The Sinatra Project"
date:       2020-01-15 19:07:53 -0500
permalink:  towards_the_sinatra_project
---


**It feels amazing** finishing this second project. I am getting more attuned to the process of learning here at flatiron. In high school and college I didn't recognize my study patterns at all. I didn't implement real breaks either. The more I notice the patterns the easier it is to work with myself on getting things done. 

***The best advice*** I recieved for this project was *to make something that you would really use*. Immediately I got three grand ideas and two of them ended up being useful for sinatra while the other ends up working well for the next project using rails.

**Hole in the Wall** is a CRUD (Create, Read, Update or Delete), MVC (Models, Views, Controllers) Web application made using Sinatra. Users can favorite select stores they have been to or would like to visit, add a review to the stores they have been to and have access to all of their reviews and favorited stores. It allows users to sign up or log in to their accounts which then prompts them to either an error screen if their credentials aren't authenticated or the home page of hole in the wall. The home page shows all User reviews, gives an option to view all stores currently in the database and an option to view their account info or logout. 

At the end of the project I have: 4 Controllers, 4 Models, 13 views (although I could cut that down), 5 migrations, 27 seeds in my database and 12 css files.

Making my project as big as it is now was no easy task.  But it was definitely easier than the first project in the sense that *I only focused  firstly on the project review requirements and made sure they were 100% functional*. After that I went and styled my project with CSS. Each web page is completely styled (although not totally optimized right now for smaller browser windows) and I loved the process of learning how to manipulate what I want to see happen on the screen. 

Although the HTML and CSS sections are a bonus part of the curriculum I'd say they make your life way easier when you write your code. One of the more difficult sections I went through in sinatra was refactoring my original project submission and adding two forms onto one page.

By creating a button that has an instance variable containing all attributes a particular store has: 
```
<button input class="button" type="submit" value="<%=@store%>">Favorite?</button>
```

The Recieving route is where the user can see all of their favorite stores and now the route has access to all of the store attributes. By allowing access to all of the store info I can provide a list to the user and give individual stores in the list a link to go back to that store's page.

Since I put this bit of code into the Model that connects users and stores:
```
validates :store_id, uniqueness: true
```

Stores cannot be favorited twice. If a user tries to favorite a store again, they will be redirected to their favorite stores list.

The delete button is mapped to the user's favorite store page:

![imgur](https://i.imgur.com/wQRGR2K.png)

When pressed, the browser will refresh the page to show your store is now deleted from your list.

But onto the more confusing section: Connecting the reviews and stores controller.

My stores controller contains the route that contains both create a review form and favoriting the store form on one page.

In my stores controller: 

```
get '/stores/:id' do
        redirect_if_not_logged_in?
        @store = Store.find_by_id(params[:id])
        erb :'/stores/show_individual_store'
    end
```

Which renders the individual store erb:

```
<h2><form action="/my-reviews/<%=@store.id%>" method="POST">
            <input id="title" type="text" name="review[title]" placeholder="Title">
    <br>
    <br>
            <textarea <input id="content" type="text" name="review[content]" placeholder="Content"></textarea>
            <input id="submit" type="submit" value="Submit">
        </form>
    </h2>

```

The way this is set up is that: `erb :'/stores/show_individual_store'` will send the instance variable `@store.id` to the `post  "/my-reviews/:id"` route. It may send it to `"/my-reviews/<%=@store.id%>"`,  but really all of the information being sent is the review with its attributes along with the store id.

In my reviews controller:

```
post "/my-reviews/:id" do
        @user_review = Review.new(params[:review])
        if @user_review.valid?
            @user_review.store_id = params[:id]
            @user_review.user_id = session[:user_id]
            @user_review.save
            redirect to "/my-reviews/#{@user_review.id}" 
        else
            redirect to "/my-reviews"
        end
    end
```


The `params[:id]` matches what was in the  `"/my-reviews/<%=@store.id%>"`. The individual review has its own id, but it is not saved until **after** the `@user_review.store_id` is equal to the `params[:id]`.

The route takes the new review params and if it is valid, it will take the sent store id and match it to the `@store.id`. It will connect the user_id to the user who is in session and then save the review. Finally, they will be redirected to `"/my-reviews/#{@user_review.id}"` Which will have the correct review with its own unique id.
