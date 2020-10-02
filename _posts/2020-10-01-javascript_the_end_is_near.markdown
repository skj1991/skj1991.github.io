---
layout: post
title:      "JavaScript: The End is Near…"
date:       2020-10-02 00:51:16 +0000
permalink:  javascript_the_end_is_near
---


### Project Idea:
For the JavaScript portfolio project, I  decided to create a travel rating and recommendations application. Per requirements this is a  Single Page Application where users are able to create posts about their recent travels that include recommendations for other travelers. It uses a Rails API backend to allow visitors to create trips posts and JavaScript, HTML and CSS on the frontend for styling. Initially I had a grand idea of allowing a person to create a user profile with the ability to favorite other user’s trip posts but I have been struggling to catch up and ran out of time. I plan to revise this app in my own time to really expand my JavaScript knowledge. 

### Backend Setup:
Creating the Rails API was very similar to the Rails project we recently submitted. I created  two models: Trip and Country. After pushing the migrations my schema looked as such:

```
create_table "countries", force: :cascade do |t|
    t.string "name"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
end

create_table "trips", force: :cascade do |t|
    t.string "title"
    t.string "city"
    t.text "description"
    t.integer "rating"
    t.string "hotel"
    t.string "must_visit"
    t.string "top_restaurant"
    t.string "image_url"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.bigint "country_id", null: false
    t.index ["country_id"], name: "index_trips_on_country_id"
end

add_foreign_key "trips", "countries"
```

I was then able to establish a has_many/belongs_to relationship between Country and Trips. I used a branching method to keep my master branch clean while adding code. Once the models were complete, the controller actions and routes were setup. I also used the fast JSON API gem to create serializers. Once everything was working I created a separate repository for the frontend setup.

### Frontend Setup:
I joined the [#js-project-build](https://flatiron-school.slack.com/archives/C015P7VRDA6) slack channel and found a really helpful project build walkthrough that helped to fully connect the dots for me. The instructor advised we organize our Javascript Object Oriented classes into separate files. These were the js files created in the “src” file:

* “index.js” - Responsible for the bulk of the application, including event listeners, fetch requests, etc.
* “trip.js” - Handles creation of Trip model.
* “country.js” - Handles creation of Country model.

She walked through how to build the class and using Bootstrap to assist with styling. I initially only had the Trip and Index js files but quickly ran into an error with my post request not all of the attributes. Since the trip was connected to a country via the id, I had to fetch the country data from the API and pass it into my post fetch request to resolve the error. Though, I have been under an inhumane amount of stress trying to balance my all of my goals, I truly enjoyed learning JavaScript. I really enjoy how tied everything that I have learned thus far together. 

If there is any advice I can give to anyone struggling to keep up, it would be to keep pushing through! **You only fail if you quit!**

