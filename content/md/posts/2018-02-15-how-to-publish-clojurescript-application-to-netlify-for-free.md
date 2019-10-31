{:title "How to publish ClojureScript Application to Netlify for free [en]"
 :layout :post
 :author "Mochamad Lucky Pradana"   
 :tags  ["clojurescript" "clojure" "hosting" "reagent" "re-frame"]
 :toc false}

![Netlify and Clojurescript banner](/img/netlify-and-cljs.jpeg)

## Requirements

1. JDK v8 (because the newer version get trouble with Clojure)
2. Leiningen

***

## Create New Project

First of all, let create a new ClojureScript project with Leiningen and Figwheel (default) template

```
lein new figwheel mysite
```

We’re not going to create something incredible there so let the project being plain. Initialize `git` using terminal.

```
git init
git add .
git commit -m "First commit"
```

We create the repository. So open your GitHub or GitLab or BitBucket, but I'll do using BitBucket

1. Go to that site
2. Create a new repository
3. Remote git repository, it looks like this
```
git remote add origin https://ampersanda@bitbucket.org/ampersanda/mysite.git
```
4. Push 'em all
```
git push -u origin master
```
5. Done!

***

## And Yeah, It’s Time to Deploy
1. First of all, you have to register to Netlify site (I think, I don’t need to explain this)
2. After done creating account, Netlify will ask you about creating a new site, choose where is repository of your ClojureScript application (I use BitBucket)

![Choosing git provider](/img/1*fLwL2Do1LL7VQsrrvMAvww.png)

3. For the next step, you have to choose which repository is your application, and click it out

![Choosing repository](/img/1*IOUI7wIJ_ynB_uioZZG90g.png)

***

## The Last Step
In this step you have to define the build command, which Figwheel said in the README, you have to use :

```
lein do clean, cljsbuild once min
```

and the location of where the compiled things is

```
resources/public
```

![Deploy configuration](/img/1*yQ_UumD64Yr1FFUeP6sxVg.png)

and click **Deploy** button in the bottom to launch you application and wait for the site is deploying and boom!

![Deployment status](/img/1*6a0IfHSBS48E2pgWiOjA2g.png)

To prove it, try to open the link of your project

![Site preview](/img/1*lRNTyEMVlsxoZvfKmhRubg.png)

But this is a plain project which need some works to do by you :). Feel free to update your repository.

Happy Coding!!


