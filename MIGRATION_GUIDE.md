# Migration Guide: From Reagent to UIx

This comprehensive guide helps you migrate from Reagent to UIx, leveraging modern React patterns and contemporary ClojureScript development practices.

> **Note**: This guide assumes you are migrating to **React 19** and its latest features including the React Compiler, improved concurrent rendering, and modern hook patterns.

## Overview

UIx is a modern ClojureScript interface to React that embraces contemporary React patterns including hooks, concurrent rendering, and modern JavaScript features. This migration guide covers all major differences and provides practical migration strategies.

## Key Differences

### 1. Component Definition

#### Reagent (Form Components)
```clojure
;; Form-1: Simple function component
(defn user-card [user]
  [:div.user-card
   [:img {:src (:avatar user)}]
   [:h3 (:name user)]])

;; Form-2: State component
(defn counter []
  (let [count (reagent/atom 0)]
    (fn []
      [:div
       [:p "Count: " @count]
       [:button {:on-click #(swap! count inc)} "Increment"]])))

;; Form-3: Lifecycle component
(defn data-component [props]
  (reagent/create-class
   {:component-did-mount #(fetch-data)
    :component-will-unmount #(cleanup)
    :reagent-render (fn [props] [:div "Content"])}))
```

#### UIx (Modern React Components)
```clojure
;; defui with hooks
(defui user-card [{:keys [user]}]
  ($ :div.user-card
     ($ :img {:src (:avatar user)})
     ($ :h3 (:name user))))

;; State with use-state hook
(defui counter []
  (let [[count set-count] (use-state 0)]
    ($ :div
       ($ :p (str "Count: " count))
       ($ :button {:on-click #(set-count inc)} "Increment"))))

;; Effects with use-effect hook
(defui data-component [{:keys [data-id]}]
  (let [[data set-data] (use-state nil)
        [loading? set-loading] (use-state true)]

    (use-effect
     (fn []
       (fetch-data data-id)
       (fn [] (cleanup)))
     [data-id])

    (cond
      loading? ($ loading-spinner)
      :else ($ data-display {:data data}))))
```

### 2. Element Creation

#### Reagent (Hiccup Syntax)
```clojure
;; Vector-based syntax
[:div.container
 [:h1.title "Hello World"]
 [:button.btn {:on-click handle-click} "Click me"]
 [:input {:type "text" :value @input-value :on-change handle-change}]]
```

#### UIx ($ Macro HyperScript)
```clojure
;; Function-based syntax with $
($ :div.container
   ($ :h1.title "Hello World")
   ($ :button.btn {:on-click handle-click} "Click me")
   ($ :input {:type "text" :value input-value :on-change handle-change}))
```

### 3. State Management

#### Reagent (RAtoms)
```clojure
;; Local state
(def counter (reagent/atom 0))

;; Global state
(def app-state (reagent/atom {:user nil :settings {}}))

;; Cursors for nested access
(def user-cursor (reagent/cursor app-state [:user]))

;; Reactions for derived state
(def user-name (reagent/reaction (:name @user-cursor)))
```

#### UIx (React Hooks)
```clojure
;; Local state with use-state
(defui counter-component []
  (let [[count set-count] (use-state 0)]
    ;; Component logic
    ))

;; Global state with context
(def app-context (create-context))

(defui app-provider [{:keys [children]}]
  (let [[user set-user] (use-state nil)
        [settings set-settings] (use-state {})]
    ($ app-context.Provider
       {:value {:user user :set-user set-user
                :settings settings :set-settings set-settings}}
       children)))

;; Derived state with use-memo
(defui user-display []
  (let [user (use-context app-context)
        display-name (use-memo
                      (fn [] (str (:first-name user) " " (:last-name user)))
                      [user])]
    ($ :div (str "Welcome, " display-name))))
```

### 4. Effects and Lifecycle

#### Reagent (Form-3 Components)
```clojure
(defn timer-component []
  (let [time (reagent/atom (js/Date.))]
    (reagent/create-class
     {:component-did-mount
      (fn [this]
        (let [interval (js/setInterval #(reset! time (js/Date.)) 1000)]
          (reagent/set-state this {:interval interval})))

      :component-will-unmount
      (fn [this]
        (when-let [interval (:interval @(reagent/state this))]
          (js/clearInterval interval)))

      :reagent-render
      (fn []
        [:div "Current time: " (.toString @time)])})))
```

#### UIx (useEffect Hook)
```clojure
(defui timer-component []
  (let [[time set-time] (use-state (js/Date.))]

    (use-effect
     (fn []
       (let [interval (js/setInterval #(set-time (js/Date.)) 1000)]
         ;; Return cleanup function
         (fn []
           (js/clearInterval interval))))
     []) ;; Empty dependency array = run once on mount

    ($ :div (str "Current time: " (.toString time)))))
```

### 5. Data Fetching

#### Reagent (Manual with core.async)
```clojure
(defn fetch-user [user-id]
  (go
    (let [response (<! (http/get (str "/api/users/" user-id)))]
      (if (:success response)
        (rf/dispatch [:user/loaded (:body response)])
        (rf/dispatch [:user/error (:error-text response)])))))

(defn user-component [user-id]
  (let [user (rf/subscribe [:current-user])]
    (reagent/create-class
     {:component-did-mount #(fetch-user user-id)
      :reagent-render (fn [user-id]
                       (if @user
                         [:div (:name @user)]
                         [:div "Loading..."]))}))))
```

#### UIx (Modern fetch with hooks)
```clojure
(defn use-user [user-id]
  (let [[user set-user] (use-state nil)
        [loading? set-loading] (use-state false)
        [error set-error] (use-state nil)]

    (use-effect
     (fn []
       (when user-id
         (set-loading true)
         (-> (js/fetch (str "/api/users/" user-id))
             (.then (fn [response]
                      (if (.-ok response)
                        (.json response)
                        (throw (js/Error. "Failed to fetch user")))))
             (.then (fn [data] (set-user data)))
             (.catch (fn [err] (set-error err)))
             (.finally (fn [] (set-loading false)))))
       (fn [])) ;; Cleanup function
     [user-id])

    {:user user :loading? loading? :error error}))

(defui user-component [{:keys [user-id]}]
  (let [{:keys [user loading? error]} (use-user user-id)]

    (cond
      loading? ($ loading-spinner)
      error ($ error-message {:error error})
      user ($ :div (:name user))
      :else ($ :div "No user found"))))
```

## Migration Steps

### Step 1: Update Dependencies

#### Before (Reagent)
```clojure
;; project.clj or deps.edn
[reagent "1.1.0"]
[cljs-http "0.1.46"]
[secretary "1.2.3"]
```

#### After (UIx)
```clojure
;; project.clj or deps.edn
[uix "1.4.4"]
[cljs-http "0.1.46"] ;; Still useful for complex HTTP needs
[react-router-dom "6.8.0"] ;; For routing
```

### Step 2: Update Namespace Imports

#### Before
```clojure
(ns myapp.core
  (:require [reagent.core :as reagent]
            [reagent.dom :as rdom]))
```

#### After
```clojure
(ns myapp.core
  (:require [uix.core :as uix :refer [defui $]]
            [uix.dom :as dom]))
```

### Step 3: Convert Components

#### Simple Component Migration
```clojure
;; Before
(defn greeting [name]
  [:div.greeting
   [:h1 (str "Hello, " name)]
   [:p "Welcome to our app"]])

;; After
(defui greeting [{:keys [name]}]
  ($ :div.greeting
     ($ :h1 (str "Hello, " name))
     ($ :p "Welcome to our app")))
```

#### State Component Migration
```clojure
;; Before
(defn counter []
  (let [count (reagent/atom 0)]
    (fn []
      [:div
       [:p "Count: " @count]
       [:button {:on-click #(swap! count inc)} "Increment"]])))

;; After
(defui counter []
  (let [[count set-count] (use-state 0)]
    ($ :div
       ($ :p (str "Count: " count))
       ($ :button {:on-click #(set-count inc)} "Increment"))))
```

### Step 4: Convert Effects and Lifecycle

#### Data Fetching Migration
```clojure
;; Before
(defn user-profile [user-id]
  (let [user (reagent/atom nil)]
    (reagent/create-class
     {:component-did-mount
      (fn [this]
        (go (let [response (<! (http/get (str "/api/users/" user-id)))]
              (reset! user (:body response)))))

      :reagent-render
      (fn [user-id]
        (if @user
          [:div (:name @user)]
          [:div "Loading..."])))})))

;; After
(defui user-profile [{:keys [user-id]}]
  (let [[user set-user] (use-state nil)
        [loading? set-loading] (use-state true)]

    (use-effect
     (fn []
       (set-loading true)
       (go (let [response (<! (http/get (str "/api/users/" user-id)))]
             (set-user (:body response))
             (set-loading false)))
       (fn [])) ;; Cleanup
     [user-id])

    (if loading?
      ($ loading-spinner)
      ($ :div (:name user)))))
```

### Step 5: Update Routing

#### Before (Secretary/Reitit)
```clojure
;; Secretary
(defroute "/users/:id" [id]
  (rf/dispatch [:set-current-page :user {:id id}]))

;; Reitit
(def routes
  [["/users/:id" {:name :user
                  :view user-page
                  :parameters {:path [:map [:id :int]]}}]])
```

#### After (React Router)
```clojure
;; App routing
(defui app []
  ($ react-router/BrowserRouter
     ($ react-router/Routes
        ($ react-router/Route {:path "/users/:id" :element ($ user-page)}))))

;; Component with params
(defui user-page []
  (let [params (react-router/use-params)
        user-id (:id params)]
    ($ user-profile {:user-id user-id})))
```

### Step 6: Update Styling

#### Before (Traditional CSS)
```clojure
;; CSS
.button {
  padding: 0.5rem 1rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 0.25rem;
}

;; Reagent
[:button.button {:on-click handle-click} "Click me"]
```

#### After (Modern CSS-in-JS)
```clojure
;; styled-components
(def StyledButton
  (styled "button"
    #js {:padding "0.5rem 1rem"
         :backgroundColor "#007bff"
         :color "white"
         :border "none"
         :borderRadius "0.25rem"
         :cursor "pointer"
         :"&:hover" #js {:backgroundColor "#0056b3"}}))

(defui modern-button [{:keys [on-click children]}]
  ($ StyledButton {:on-click on-click} children))
```

### Step 7: Update Forms

#### Before (Manual Form Handling)
```clojure
(defn contact-form []
  (let [form-data (reagent/atom {:name "" :email "" :message ""})]
    (fn []
      [:form {:on-submit (fn [e]
                          (.preventDefault e)
                          (submit-form @form-data))}
       [:input {:type "text"
                :value (:name @form-data)
                :on-change #(swap! form-data assoc :name (.. % -target -value))}]])))
```

#### After (React Hook Form)
```clojure
(defui contact-form []
  (let [form (react-hook-form/useForm)
        {:keys [register handleSubmit formState]} form
        {:keys [errors]} formState

        on-submit (fn [data]
                    (console.log "Form data:" data))]

    ($ :form {:on-submit (handleSubmit on-submit)}
       ($ :input {:& (register "name" #js {:required "Name is required"})})
       (when (:name errors)
         ($ :span {:style {:color "red"}} (:message (:name errors))))
       ($ :button {:type "submit"} "Submit"))))
```

## Advanced Migration Patterns

### Custom Hooks Migration

#### Before (Reagent patterns)
```clojure
(defn use-local-storage [key default-value]
  (let [value (reagent/atom default-value)]
    (reagent/create-class
     {:component-did-mount
      (fn [this]
        (when-let [stored (.getItem js/localStorage key)]
          (reset! value (cljs.reader/read-string stored))))

      :component-did-update
      (fn [this [_ _ prev-value]]
        (when (not= @value prev-value)
          (.setItem js/localStorage key (pr-str @value))))

      :reagent-render
      (fn [key default-value] @value)})))
```

#### After (UIx custom hooks)
```clojure
(defn use-local-storage [key default-value]
  (let [[value set-value] (use-state
                           (or (when-let [stored (.getItem js/localStorage key)]
                                 (cljs.reader/read-string stored))
                               default-value))]

    (use-effect
     (fn []
       (.setItem js/localStorage key (pr-str value)))
     [key value])

    [value set-value]))
```

### Context Migration

#### Before (Reagent context)
```clojure
(def theme-context (reagent/atom :light))

(defn themed-component []
  [:div {:class (case @theme-context
                  :light "light-theme"
                  :dark "dark-theme")}
   "Themed content"])
```

#### After (React Context)
```clojure
(def theme-context (create-context :light))

(defui theme-provider [{:keys [children]}]
  (let [[theme set-theme] (use-state :light)]
    ($ theme-context.Provider {:value {:theme theme :set-theme set-theme}}
       children)))

(defui themed-component []
  (let [{:keys [theme]} (use-context theme-context)]
    ($ :div {:class (case theme
                      :light "light-theme"
                      :dark "dark-theme")}
       "Themed content")))
```

## Performance Considerations

### Bundle Size Optimization
- UIx has smaller bundle size than Reagent
- Better tree shaking with modern build tools
- Lazy loading support for code splitting

### Runtime Performance
- React 18 concurrent features
- Better reconciliation with modern React
- Improved memory management with hooks

### Development Experience
- Better hot reloading
- Improved debugging with React DevTools
- Modern development server integration

## Testing Migration

### Component Testing
```clojure
;; Before (Reagent)
(deftest counter-test
  (let [component (reagent/render [counter] (js/document.createElement "div"))]
    (is (= "Count: 0" (.. component -textContent)))
    ;; Manual DOM manipulation for testing
    ))

;; After (UIx with React Testing Library)
(deftest counter-test
  (let [user (rtl/render ($ counter))
        button (rtl/getByRole user "button")]

    (is (rtl/getByText user "Count: 0"))

    (rtl/fireEvent.click button)
    (is (rtl/getByText user "Count: 1"))))
```

## Common Pitfalls and Solutions

### 1. Dependency Array Issues
```clojure
;; ❌ Wrong - missing dependencies
(use-effect
 (fn []
   (console.log "Value:" value))
 []) ;; Should include [value]

;; ✅ Correct
(use-effect
 (fn []
   (console.log "Value:" value))
 [value])
```

### 2. State Updates in Effects
```clojure
;; ❌ Wrong - direct state mutation
(use-effect
 (fn []
   (set-loading true)
   (fetch-data))
 [])

;; ✅ Correct - proper async handling
(use-effect
 (fn []
   (let [controller (js/AbortController.)]
     (set-loading true)
     (-> (fetch-data controller.signal)
         (.then set-data)
         (.finally #(set-loading false)))
     (fn [] (.abort controller))))
 [deps])
```

### 3. Memory Leaks
```clojure
;; ❌ Wrong - no cleanup
(use-effect
 (fn []
   (let [interval (js/setInterval callback 1000)]
     ;; No cleanup returned
     ))
 [])

;; ✅ Correct - proper cleanup
(use-effect
 (fn []
   (let [interval (js/setInterval callback 1000)]
     (fn [] (js/clearInterval interval))))
 [])
```

## Benefits of Migrating to UIx

1. **Modern React Features**: Full access to React 18+ concurrent rendering, Suspense, and advanced hooks
2. **Better Performance**: Smaller bundle size, improved tree shaking, and optimized rendering
3. **Enhanced Developer Experience**: Better hot reloading, improved debugging, and modern tooling
4. **Future-Proof**: Built for the modern React ecosystem and contemporary web development
5. **Active Maintenance**: Regular updates and active community support
6. **TypeScript Integration**: Better integration with modern type systems
7. **Modern Patterns**: Embraces contemporary React patterns and best practices

## Resources

- [UIx Documentation](https://uix-cljs.dev/)
- [React Documentation](https://react.dev/)
- [React Router Documentation](https://reactrouter.com/)
- [Framer Motion Documentation](https://www.framer.com/motion/)
- [React Hook Form](https://react-hook-form.com/)
- [Modern JavaScript Features](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

## Getting Help

- [UIx GitHub Repository](https://github.com/pitch-io/uix)
- [ClojureScript Community](https://clojurescript.org/community/)
- [React Documentation](https://react.dev/)

This migration guide provides a comprehensive path from Reagent to UIx, ensuring you can leverage modern React patterns while maintaining ClojureScript's functional programming benefits.