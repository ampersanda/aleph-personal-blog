{:title "Session Handler on Compojure"
 :layout :post
 :author "Mochamad Lucky Pradana"   
 :tags  ["clojure" "backend" "backend-session"]
 :toc false}

![Clojure Pink Banner](/img/1*osDhTBzvlT622tNd5MB-Uw.png)

## Requirements
- [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [Leiningen](https://leiningen.org/)

***

## Create New Project
First of all, let create a new Compojure project with Leiningen

```
lein new compojure sessiontest
``` 

***

## Adding JSON Response Dependencies

Open the `project.clj` and add `[ring/ring-json "0.3.1"]` on dependencies part

```clojure
(defproject sessiontest "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :dependencies [[org.clojure/clojure "1.5.1"]
                 [compojure "1.1.8"]
                 [ring/ring-json "0.3.1"]          ;; <= add this 
                 ]
  :plugins [[lein-ring "0.8.10"]]
  :ring {:handler sessiontest.handler/app}
  :profiles
  {:dev {:dependencies [[javax.servlet/servlet-api "2.5"]
                        [ring-mock "0.1.5"]]}})
```

***

## Adding "require"-ments and something that will be "use"-d

Open the `handler.clj` file inside of `/src/sessiontest` folder
*name of the folder depends on the name that you create the project with leiningen, so it can be /src/myweirdproject

The file should look like this

```clojure
;; block 1
(ns sessiontest.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]))
;; block 2
;; this is the route part
(defroutes app-routes
  (GET "/" [] "Hello World")             ;; for /
  (route/not-found "Not Found"))         ;; 404
;; block 3
(def app
  (wrap-defaults app-routes site-defaults))
```

I make the file into 3 parts for clean code adding

```clojure
;; block 1
(ns sessiontest.handler
  (:use [ring.middleware.json :only (wrap-json-response)]
               ;; use json response
        ring.middleware.session
               ;; use session
        [ring.util.response :only (response)]
               ;; use response
        [hiccup.middleware :only (wrap-base-url)])
               ;; use hiccup
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [compojure.handler :as handler]                 
               ;; require handler
            ))
;; block 2
(defroutes app-routes
  ;; catch the :session (keyword) into session variable
  (GET "/" {session :session} []
    ;; catch :counter (keyword) inside the session (variable) to c (variable) if exists, otherwise use 0 (zero) to fill the value
    (let [c (:counter session 0)
          ;; define the session (variable) with the old session which added :counter (keyword) inside and also increment the value
          session (assoc session :counter (inc c))]
      (-> 
          ;; return the result as json 
          (response {:the_counter (:counter session)})
          ;; re-new the session
          (assoc :session session))))
  ;; just handle when no route matched
  (route/not-found "Not Found"))
;; block 3
(def app
  (-> (handler/site
        app-routes {:session 
                      {:cookie-name "counter-session"
                       :cookie-attrs 
                         {:max-age 15 ;; expires in second 
                            }}})
      (wrap-base-url)
      (wrap-json-response)
      (wrap-session))
  ;; delete or comment this default command
  ;;(wrap-defaults app-routes site-defaults)
  )
```

***

## Testing

Run the server by execute this command inside terminal in project folder where `project.clj` is.

```
lein ring server
```  

And try to refresh the page, the counter will increment like you did in code above and try to wait 15 seconds and refresh the page, the session will reset because we set `:max-age` to **15**

<div class="yt-video"><iframe width="560" height="315" src="https://www.youtube.com/embed/vXN35b8zKw4" frameborder="0" allowfullscreen></iframe></div>

Grab the source [here](https://github.com/ampersanda/compojure-session-example)
