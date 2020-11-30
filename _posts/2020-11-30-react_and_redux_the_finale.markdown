---
layout: post
title:      "React&Redux: The Finale "
date:       2020-11-30 20:56:53 +0000
permalink:  react_and_redux_the_finale
---

# Project Idea:
For the React portfolio project, I decided to create a travel blob application building upon my previous project ideas. Per requirements this is a Single Page Application where users are able to create posts about their recent travels and readers are able to submit comments on the posts. It uses a Rails API backend to allow visitors to create trips posts and React with Redux, HTML and CSS on the frontend for styling. Initially I had a grand idea of allowing a person to create a user profile with the ability to favorite other user’s trip posts but I have been struggling to catch up and ran out of time.
# Backend Setup:
Creating the Rails API was very similar to the past projects we have recently submitted. I created two models: Post and Comment. After pushing the migrations my schema looked as such:

```
ActiveRecord::Schema.define(version: 2020_11_15_230116) do

  create_table "comments", force: :cascade do |t|
    t.text "content"
    t.integer "post_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "posts", force: :cascade do |t|
    t.string "title"
    t.string "image"
    t.text "content"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

end
```

I was then able to establish a has_many/belongs_to relationship between Comments and Posts. Once the models were complete, the controller actions and routes were setup. I also used the Active Model Serializer gem to create serializers for both posts and comments. Once everything was working I created a separate repository for the frontend setup.
# Frontend Setup:
Following a similar pattern of organization I created separate files to store different components. These were the js files and folders were created in the “src” file:

“index.js” - Responsible for the bulk of the application, including event listeners, fetch requests, etc.
“App.js” - Responsible for the bulk of the application, including event listeners, fetch requests, etc.
actions - contains multiple files that control application actions such as fetching and editing posts.
reducers - handles the actions 
containers - layouts for post and comments components
components - components used in containers for layout 

I added React Bootstrap to assist with styling. I was having some trouble with my application refreshing after a change was made. After much research and meeting with my coding instructor I was able to determine that I needed to use a ternary statement to check if the post actually exisited. 
```
<div> 
            {post ? (
                <Card style={{ width: '18rem' }}>
                <Card.Img variant="top" src={post ? post.image : null} />
                <Card.Body>
                    <Card.Title>Title: {post ? post.title : null}</Card.Title>
                    <Card.Text>
                        Content: {post ? post.content : null}
                    </Card.Text>
                    <CommentContainer post={post} />
                    <br></br>
                    <Link to={`/posts/${post.id}/edit`}>
                        <Button variant="primary">Edit Post</Button> 
                    </Link>
                </Card.Body> 
            </Card>
            ) : null}
        </div>
```

I am thankful for finding the strength to believe in my new skills and really looking forward to practicing more!
