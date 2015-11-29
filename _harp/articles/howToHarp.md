# I use harpjs as a simple http static server for this blog. 

Here's how I set it up: 

1. Install npm if you don't have it. 
```
osx:  homebrew install node 
```
2. do a global install of harp: 
```
sudo npm install -g harp 
```
3. Now we can get started with the app. Make a _harp directory to 
keep source files. Harp will not serve anything that beings with an 
underscore so this will keep our source files from getting served. 

4. Harp will begin by rendering index.jade if you have it in the root of the directory. So create a index.jade with just a 
```
h1 Hello World
```
and try serving it by doing 
```
harp server
```
inside the _harp directory 

5. Harp can work by either being the server, as we just invoked it, or by compiling the source files into html and css. We can invoke it to compile by doing: 
```
harp compile _harp . 
```

6. Now go to github pages and follow the steps to set up a new github page and push the 
repository with the compiled harp project. You should be able to see the hello world example 
at the github page url. 

Now that we've got a pretty close to working project, we'd like to have something that's 
a little easier to manage as a blog. So let's write some jade that will help us fetch 
our articles. 

```
h1 Abe's Project Blog

ul
  each article, slug in public.articles._data
    li
      a(href="/articles/#{ slug }") #{ article.title }
      p
        !=article.description
```

Now we make an articles folder with a _data.json file in it and write in our first article: 

```
{
  "example": {
    "title": "example title", 
    "date": "Nov. 29, 2015", 
    "description": "exapmle description"
  }
}
```

Now write an example.md file. The jade file will create a link to this file with the title and the 
description given in the _data.json object. Feel free to look at my own repositiory for 
this blog at: https://github.com/abrahamq/abrahamq.github.io 
