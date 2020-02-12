---
layout: post
title:      "Ruby on Rails - FilmPitch"
date:       2020-01-29 01:06:10 -0500
permalink:  ruby_on_rails
---


The Sinatra Project flew by me fast. I spent the whole project week doing 14 hour programming sessions. Now I am blown away by rails. This month's curriculum for Rails is very big. There are many things that make doing what you did in Sinatra easier, but there are also  many more feautres. They say that the more grounded you are in Sinatra that you will learn Rails easier and I agree. I'm loving these lessons and I'm loving the process. I am 100% a project learner and now that I am in midweek into the project, everything is becoming very clear. 

My Project is called 'FilmPitch': An app similar to KickStarter but only for filmmakers. Users can sign up, login, create their pitch, upload their scripts and let others fund their project. If a project is done they can upload their full finished films.

One of the more difficult things was setting up the migration table for this project as the requirements state that you have to use two many-to-many relationships along with another has_many && belongs_to.

[The ODIN Project was the biggest help in making sense of the associations.](https://www.theodinproject.com/courses/ruby-on-rails/lessons/active-record-associations)

[Along with the documentation for Bi-directional and Polymorphic Associations.](https://guides.rubyonrails.org/association_basics.html#bi-directional-associations)

My models look like this (Other pieces/validations are removed here for focus on associations):

```
class User < ActiveRecord::Base
  has_many :funds
	has_many :funded_pitches, through: :funds, source: :pitch
	has_many :pitches
end
```

Now a user can fund many pitches, through the table funds and its source will be the pitch class. The User can have many pitches themselves.

```
class Fund < ActiveRecord::Base
  belongs_to :user
	belongs_to :pitch
end
```

```
class Pitch < ActiveRecord::Base
  has_many :funds
	has_many :pitch_funders, through: :funds, source: :user
	belongs_to :user
end
```

See how we can name` :pitch_funders` or `:funded_pitches` here even though the Fund table doesn't have these attributes?  By specifying the source of the `user_id` and the `pitch_id`, rails connects the tables together, even though attributes are not labeled in the Fund table. That means you can have many different user types like` :admin || :moderator`. 


My tables look like this:

```
class CreateUsers < ActiveRecord::Migration[6.0]
  def change
    create_table :users do |t|
      t.string :name
      t.string :username
      t.string :email
      t.string :password_digest
    end
  end
end
```

```
class CreatePitches < ActiveRecord::Migration[6.0]
  def change
    create_table :pitches do |t|
      t.string :title
			t.string :genre
      t.string :summary
      t.string :video_link
      t.integer :funding_goal
      t.belongs_to :user
    end
  end
end
```

```
class CreateFunds < ActiveRecord::Migration[6.0]
  def change
    create_table :funds do |t|
      t.belongs_to :user
			t.belongs_to :pitch
    end
  end
end
```
