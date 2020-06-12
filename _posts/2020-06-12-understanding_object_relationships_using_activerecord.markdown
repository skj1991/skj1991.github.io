---
layout: post
title:      "Understanding Object Relationships using ActiveRecord"
date:       2020-06-12 16:26:14 -0400
permalink:  understanding_object_relationships_using_activerecord
---


Object relationships are the idea that instances of a class are able to interact and be associated with each other. This allows us to write programs that reflect real-world, where different objects interact with each other in various ways. When creating object relationships within a Sinatra application, you will need to use ActiveRecord Associations. These associations create a connection between two Active Record models by communicating that there is a connection between the two models. There are a few different types of associations we can implement, however, we will discuss the most used here, and they are as follows:
1. “Belongs to”/“Has Many” 
2. “Has many Through”

We will breakdown the Sinatra application titled “Events Around Town” to further explore these relationships in more detail and how to implement using ActiveRecord. In the Events Around Town application, a user is able to search for many cities and view many events from the searched city.  Therefore, a User is related to Cities, Cities is related to Events, and many Users can be related to many Events. There are two steps to setting up associations within your Sinatra application.

First, write you migrations that create tables within the Database establishing relationships between the tables.
When writing migrations for the database tables, you are creating a Relational Database that will maintain the associations between the records stored. In order to create these relationships between our database tables we use a “Foreign Key” column in table one. The foreign key is the Primary key of the relatable table (table two). The Primary Key is the unique identifier for each record in the database table. Since we are using ActiveRecord, it creates a primary key for the tables and auto-increments the key when new records are added. 

In our, Events Around Town example we need to create the following relationships between our tables:
1. User has many Cities & Cities belongs to User
2. Cities has many Events & Events belongs to Cities 
3. User can have many Events & Events can have many Users

Given the relationships that need to be established between the models the following four tables will be created: Users, Cities, Events, and UserEvents. Executing the rake command rake db:create_migration NAME=create_tablename within terminal will create the table migrations. 
Once all migrations are created the object relationships can be established within the tables. In the “Belongs to”/“Has Many” relationship the table with the foreign key column contains records that belong to the other table. That association looks as follows within our tables: 

`class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :username
      t.string :password_digest
      t.string :email
    end
  end
end`

You will notice, that a column containing the Users table Primary ID has been added to the Cities table as the foreign key column, establishing a “Belongs to”/“Has Many” relationship between the two tables.

`class CreateCities < ActiveRecord::Migration
  def change
    create_table :cities do |t|
      t.string :city
      t.integer :user_id
    end
  end
end`

When creating the the Events table the same will happen with the Primary ID of the Cities table to establish the relationship. So far only 3 of 4 tables have been created. The last migration needed will establish the JOIN table or “Many To Many” relationship between the Users and Events tables. That association looks as follows within our tables: 

`class CreateUserEvents < ActiveRecord::Migration
  def change
    create_table :user_events do |t|
      t.integer :user_id
      t.integer :event_id
    end
  end
end`

The UserEvents table contains two foreign key columns for the primary id’s of the Users and Events table to join them together. This provides access to the users that searched events and the events searched by a specific user. Now that all database table relationships have been established, the next step of implementing the various relationships within the Object Models to finish the associations. Associations are implemented by adding the following ActiveRecord macros to the models:
belongs_to :class_name
has_many :table_name
has_many :table_one, through: :table_two

The ActiveRecord macros will look as follows when properly implemented into our models: 

`class User < ActiveRecord::Base
    has_secure_password
    has_many :cities 
    has_many :user_events
    has_many :events, through: :user_events
    validates :username, :email, uniqueness: true
    validates :username, :email, presence: true
end`

`class City < ActiveRecord::Base
    belongs_to :user
    has_many :events
end`

`class Event < ActiveRecord::Base
    has_many :user_events
    has_many :users, through: :user_events
    belongs_to :city
end`

In the User, City and Event models, we have added the has_may and belongs_to macros. Please note, whenever we use the has_many macro, we will need to add the belongs_to macro in the other model to complete the association.

`class User < ActiveRecord::Base
    has_secure_password
    has_many :cities 
    has_many :user_events
    has_many :events, through: :user_events
    validates :username, :email, uniqueness: true
    validates :username, :email, presence: true
end`

`class UserEvent < ActiveRecord::Base
    belongs_to :user
    belongs_to :event
end`

`class Event < ActiveRecord::Base
    has_many :user_events
    has_many :users, through: :user_events
    belongs_to :city
end`

In the User and Event models, a has_many relationship was established with the UserEvents (JOIN) table so that both classes have many user events. Do not forget that when using the has_many macro we must also use the belongs_to macro, therefore, in the UserEvents table the belongs_to macros was added for the User and Event class to complete the association. The has_many, :through relationships are established to connect the two tables through the UserEvents table. Now, we can search the many events belonging to a specific user or the many users that are associated to one event.


