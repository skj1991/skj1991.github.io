---
layout: post
title:      "Rails: Almost to the finishline..."
date:       2020-08-09 21:24:03 -0400
permalink:  rails_almost_to_the_finishline
---


I feel like all of my blogs about my struggle with coding and with the current state of the world once again, I really struggled to keep up during the Rails portion. I have been known to be to rough on myself and that was the case this time as well. Completing the Rails module required me to really believe in my skills and knowledge of coding thus far. I was able to finish with a better understanding of nested routes, OmniAuth and forms, just to mention a few of the Rails concept. I really struggled getting my OmniAuth with Facebook initially so here is a breakdown that may help others.

The OmniAuth gem allows users to authorize websites to access their information without handing over a password. There are many gems out there which implement OmniAuth for applications such as  omniauth-facebook, omniauth-github and omniauth-twitter and they are all implemented the same.

**To setup OmniAuth the following structure needs to be followed for proper implementation:**
1. Register the application with the desired provider(s) (i.e. Google, Twitter,  GitHub, etc) and obtain the client_id and client_secret (these are both very IMPORTANT!)
2. Create an omniauth.rb file within config/initializers and add the following block:

```
Rails.application.config.middleware.use OmniAuth::Builder do
   provider :github, ENV[‘FACEBOOK_ID'], ENV[‘FACEBOOK_SECRET']
end
```

Within the above block, we are telling OmniAuth which provider to work with.

Sometimes the provider may want a hash with scoping options, which might look like this:
```
Rails.application.config.middleware.use OmniAuth::Builder do  
   provider :google_oauth2, ENV["GOOGLE_CLIENT_ID"],
                            ENV["GOOGLE_CLIENT_SECRET"],
   {    :name => "google",
        :scope => ['contacts', 'plus.login', 'plus.me', 'email',
                   'profile'],    
        :prompt => "select_account",
        :image_aspect_ratio => "square",
        :image_size => 50,
        :access_type => 'offline'  }
end
```

3. Add the provider to routes.rb as follows:
```
get '/auth/:provider/callback' =>  'sessions#create'
``` 

4. Wherever the login link needs to live the following needs to be added to the view:
```
<=% link_to "Login", provider_name_login_path %>
```

When the user clicks this link they will be redirected to the provider who will ask for permission to send the application the users’ info. Upon a user accepting, the provider sends back a token/code to ‘/auth/:provider/callback’ and provides the following in your ‘sessions#create’
```
def create
   @user = User.find_or_create_by(uid: auth['uid']) do |u|
            u.name = auth['info']['name']
            u.email = auth['info']['email']
            u.image = auth['info']['image']
            u.password = 'password'
        end
        @user.save
        session[:user_id] = @user.id
        redirect_to users_path(@user)
end 

def auth
        request.env['omniauth.auth']
end
``` 

The auth method is a hash of user information sent back from the provider (i.e. profile photo, username, password, etc.). Once I was able to understand how the Omniauth routes work I was able to correct the errors and successfully log into my application with Facebook. Hope this helps anyone else that struggled.

