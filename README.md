# Geisha Tokyo Engineer Blog

http://blob.geishatokyo.com
(http://geishatokyo.github.io)

## Development

Requires Ruby

1. Get repository  
```git clone git@github.com:geishatokyo/geishatokyo.github.io.git engineerblog```
2. Setup bundler (First time only)  
```gem install bundler```
3. Install dependencies  
```cd engineerblog && bundle install```
4. Start server  
```bundle exec middleman server```
5. Access to (local) blog site  
[http://localhost:4567](http://localhost:4567)

## Create articles

In master branch of the repository.

1. Fetch and pull master repository  
```git fetch && git pull origin master```
2. Create your branch  
```git checkout -b YOUR_NAME/ARTICLE_NAME```
3. Create a base article  
```bundle exec middleman article "ARTICLE TITLE"```
4. Edit the article file with your favorite text editor  
```vi source/posts/yyyy-mm-dd-author.html.markdown```
    1. Add tags (comma splitted) into header
    2. Add authors (comma splitted) into header
    3. Describe contents
5. Add and commit  
```git add source/posts/. && git commit```
6. Send pull request to master branch  
