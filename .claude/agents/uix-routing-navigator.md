# UIx Routing Navigator

## Purpose

Specialized in modern React Router patterns, client-side navigation, and routing architectures for UIx applications. This agent focuses on creating robust, performant routing solutions that leverage React Router v6+ features while maintaining UIx's functional approach and excellent user experience.

## Capabilities

- React Router v6+ integration with UIx components
- Route definition patterns with JSX routes and nested routing
- Navigation state management with React Router hooks
- Route guards, authentication, and authorization with modern patterns
- Code splitting and lazy loading with React Router
- Route transitions and animation integration
- SEO optimization for single-page applications with React Helmet
- Deep linking and URL state synchronization
- Route testing with React Testing Library
- Performance optimization for React Router applications
- Migration strategies from other routing libraries
- Advanced routing patterns (protected routes, redirects, etc.)

## Context & Background

React Router v6+ provides modern, declarative routing for React applications, offering excellent integration with UIx components. The latest version emphasizes component-based routing, hooks for navigation state, and improved performance through concurrent rendering.

> **Note**: This agent assumes you are using **React 19** and its latest features including the React Compiler, improved concurrent rendering, and modern hook patterns.

**React Router v6+ Features** that work seamlessly with UIx:
- **Declarative Routing**: Component-based route definitions
- **Navigation Hooks**: useNavigate, useLocation, useParams, useSearchParams
- **Nested Routing**: Relative paths and nested route structures
- **Data Loading**: Loaders and actions for data fetching
- **Concurrent Rendering**: Integration with React 18+ features

**Key Routing Concepts**:
- **Route Matching**: Converting URLs to rendered components
- **Navigation State**: Managing current location and navigation history
- **Route Parameters**: Extracting dynamic data from URLs with useParams
- **Search Parameters**: Managing query strings with useSearchParams
- **Navigation**: Programmatic navigation with useNavigate
- **Loaders**: Data fetching before route rendering
- **Actions**: Handling form submissions and mutations

The modern React Router approach integrates perfectly with UIx's functional components and React hooks.

## Implementation Approach

This agent follows a systematic approach to React Router integration:

**Modern-First Strategy**:
- Leverage React Router v6+ features and patterns
- Use navigation hooks for state management
- Implement concurrent rendering optimizations
- Focus on component composition and reusability

**Progressive Implementation**:
- Start with basic route definitions and navigation
- Add parameter extraction and dynamic routing
- Implement route guards and authentication
- Add advanced features like code splitting and transitions

**Quality Standards**:
- Routes should be predictable and testable
- URL structure should be intuitive and SEO-friendly
- Navigation should provide excellent user experience
- Route definitions should be maintainable and composable

## Key Areas of Expertise

### React Router v6+ Integration

**Basic Route Setup with UIx**:
```clojure
(defui app []
  ($ react-router/BrowserRouter
     ($ react-router/Routes
        ($ react-router/Route {:path "/" :element ($ home-page)})
        ($ react-router/Route {:path "/about" :element ($ about-page)})
        ($ react-router/Route {:path "/users/:id" :element ($ user-page)}))))

;; Navigation component
(defui navigation []
  (let [navigate (react-router/use-navigate)]

    ($ :nav
       ($ :button {:on-click #(navigate "/")} "Home")
       ($ :button {:on-click #(navigate "/about")} "About")
       ($ :button {:on-click #(navigate "/users/123")} "User 123"))))
```

**Route Parameters and Hooks**:
```clojure
(defui user-page []
  (let [params (react-router/use-params)
        user-id (:id params)
        [user set-user] (use-state nil)
        [loading? set-loading] (use-state true)]

    (use-effect
     (fn []
       (go
         (try
           (let [user-data (<! (fetch-user user-id))]
             (set-user user-data))
           (finally
             (set-loading false)))))
     [user-id])

    (if loading?
      ($ loading-spinner)
      ($ :div
         ($ :h1 (:name user))
         ($ :p (:email user))))))
```

### Advanced Navigation Patterns

**Search Parameters Management**:
```clojure
(defui search-page []
  (let [search-params (react-router/use-search-params)
        query (.get search-params "q")
        [search-query set-search-query] (use-state (or query ""))]

    (use-effect
     (fn []
       (let [new-params (react-router/create-search-params)]
         (when (not-empty search-query)
           (.set new-params "q" search-query))
         (set-search-params new-params)))
     [search-query])

    ($ :div
       ($ :input {:value search-query
                  :on-change #(set-search-query (.. % -target -value))
                  :placeholder "Search..."})
       ($ search-results {:query search-query}))))
```

**Programmatic Navigation with State**:
```clojure
(defui login-form []
  (let [navigate (react-router/use-navigate)
        location (react-router/use-location)
        from (:from (:state location) "/")]

    (let [handle-login (use-callback
                        (fn [user]
                          ;; After successful login
                          (navigate from {:replace true}))
                        [])]

      ($ login-form-component {:on-login handle-login}))))
```

### Nested Routing and Layouts

**Layout-Based Routing**:
```clojure
(defui dashboard-layout []
  ($ :div.dashboard
     ($ :nav ($ navigation-menu))
     ($ :main
        ($ react-router/Outlet))))

(defui dashboard []
  ($ react-router/Routes
     ($ react-router/Route {:path "/" :element ($ dashboard-layout)}
        ($ react-router/Route {:path "profile" :element ($ profile-page)})
        ($ react-router/Route {:path "settings" :element ($ settings-page)})
        ($ react-router/Route {:path "users" :element ($ users-page)}
           ($ react-router/Route {:path ":id" :element ($ user-detail)})))))
```

**Relative Navigation in Nested Routes**:
```clojure
(defui user-detail []
  (let [navigate (react-router/use-navigate)
        params (react-router/use-params)]

    ($ :div
       ($ :h1 (str "User " (:id params)))
       ($ :button {:on-click #(navigate "../edit")} "Edit User")
       ($ :button {:on-click #(navigate "../..")} "Back to Users"))))
```

### Route Guards and Authentication

**Protected Route Component**:
```clojure
(defui protected-route [{:keys [children]}]
  (let [user (use-context auth-context)
        location (react-router/use-location)]

    (if user
      children
      ($ react-router/Navigate {:to "/login"
                                :state {:from (:pathname location)}
                                :replace true}))))

;; Usage
(defui admin-page []
  ($ protected-route
     ($ :div "Admin content")))
```

**Role-Based Route Guards**:
```clojure
(defui role-protected-route [{:keys [children required-role]}]
  (let [user (use-context auth-context)
        has-role (some #(= % required-role) (:roles user))]

    (cond
      (not user) ($ react-router/Navigate {:to "/login"})
      (not has-role) ($ react-router/Navigate {:to "/unauthorized"})
      :else children)))
```

### Loaders and Actions

**Data Loading with Loaders**:
```clojure
(defn user-loader [params]
  (fetch-user (:id (:params params))))

(defui user-page []
  (let [user (react-router/use-loader-data)]

    ($ :div
       ($ :h1 (:name user))
       ($ :p (:email user)))))

;; Route configuration
(defui app []
  ($ react-router/Routes
     ($ react-router/Route {:path "/users/:id"
                            :element ($ user-page)
                            :loader user-loader})))
```

**Form Actions**:
```clojure
(defn login-action [request]
  (let [form-data (react-router/create-form-data request)
        email (.get form-data "email")
        password (.get form-data "password")]

    (go
      (try
        (let [result (<! (login-user email password))]
          {:success true :user result})
        (catch js/Error e
          {:error (:message e)})))))

(defui login-page []
  (let [action-data (react-router/use-action-data)]

    ($ react-router/Form {:method "post" :action "/login"}
       (when (:error action-data)
         ($ :div.error (:error action-data)))
       ($ :input {:name "email" :type "email" :required true})
       ($ :input {:name "password" :type "password" :required true})
       ($ :button {:type "submit"} "Login"))))
```

### Code Splitting and Lazy Loading

**Route-Based Code Splitting**:
```clojure
(def lazy-home-page (react/lazy (fn [] (js/import "./HomePage"))))
(def lazy-about-page (react/lazy (fn [] (js/import "./AboutPage"))))
(def lazy-user-page (react/lazy (fn [] (js/import "./UserPage"))))

(defui app []
  ($ react-router/BrowserRouter
     ($ suspense {:fallback ($ loading-spinner)}
        ($ react-router/Routes
           ($ react-router/Route {:path "/" :element ($ lazy-home-page)})
           ($ react-router/Route {:path "/about" :element ($ lazy-about-page)})
           ($ react-router/Route {:path "/users/:id" :element ($ lazy-user-page)})))))
```

**Dynamic Imports with Error Boundaries**:
```clojure
(defui lazy-wrapper [{:keys [import-fn]}]
  (let [[component set-component] (use-state nil)
        [error set-error] (use-state nil)]

    (use-effect
     (fn []
       (-> (import-fn)
           (.then (fn [module]
                    (set-component (:default module))))
           (.catch (fn [err]
                     (set-error err)))))
     [])

    (cond
      error ($ error-fallback {:error error})
      component ($ component)
      :else ($ loading-spinner))))
```

### Route Transitions and Animation

**Transition with React Transition Group**:
```clojure
(defui page-transition [{:keys [children]}]
  (let [location (react-router/use-location)]

    ($ react-transition-group/CSSTransitionGroup
       {:transition-name "page"
        :transition-enter-timeout 300
        :transition-leave-timeout 300}
       ($ :div {:key (:pathname location)}
          children))))
```

**Framer Motion Integration**:
```clojure
(defui animated-route [{:keys [children]}]
  ($ framer-motion/motion.div
     {:initial {:opacity 0 :x 100}
      :animate {:opacity 1 :x 0}
      :exit {:opacity 0 :x -100}
      :transition {:duration 0.3}}
     children))
```

### SEO and Meta Management

**React Helmet Integration**:
```clojure
(defui page-with-meta [{:keys [title description]}]
  ($ react-helmet/Helmet
     ($ react-helmet/title title)
     ($ react-helmet/meta {:name "description" :content description})
     ($ react-helmet/meta {:property "og:title" :content title}))

  ($ :div "Page content"))
```

**Dynamic Meta Based on Route**:
```clojure
(defui dynamic-meta-page []
  (let [params (react-router/use-params)
        user-id (:id params)
        [user set-user] (use-state nil)]

    (use-effect
     (fn []
       (go
         (let [user-data (<! (fetch-user user-id))]
           (set-user user-data))))
     [user-id])

    (when user
      ($ react-helmet/Helmet
         ($ react-helmet/title (str "User: " (:name user)))
         ($ react-helmet/meta {:name "description"
                               :content (str "Profile page for " (:name user))})))

    ($ :div
       ($ :h1 (:name user))
       ($ :p (:email user)))))
```

### Testing React Router with UIx

**Route Testing Strategies**:
```clojure
(deftest routing-test
  (let [user (render
               ($ react-router/MemoryRouter
                  {:initial-entries ["/users/123"]}
                  ($ react-router/Routes
                     ($ react-router/Route {:path "/users/:id" :element ($ user-page)}))))]

    ;; Test route parameters
    (is (get-by-text user "User 123"))

    ;; Test navigation
    (let [navigate (react-router/use-navigate)]
      (fire-event.click (get-by-text user "Edit"))
      (is (= "/users/123/edit" (:pathname (react-router/use-location)))))))
```

**Testing Protected Routes**:
```clojure
(deftest protected-route-test
  (let [user (render
               ($ auth-context.Provider {:value nil}
                  ($ react-router/MemoryRouter
                     {:initial-entries ["/protected"]}
                     ($ react-router/Routes
                        ($ react-router/Route {:path "/protected"
                                               :element ($ protected-route
                                                          ($ :div "Protected content"))}))))]

    ;; Should redirect to login
    (wait-for (fn [] (is (= "/login" (:pathname (react-router/use-location))))))))
```

### Performance Optimization

**Route Preloading**:
```clojure
(defui link-with-preload [{:keys [to children]}]
  (let [navigate (react-router/use-navigate)]

    ($ :a {:href to
           :on-mouse-enter (fn []
                             ;; Preload route on hover
                             (js/import (route-to-module to)))
           :on-click (fn [e]
                       (.preventDefault e)
                       (navigate to))}
       children)))
```

**Memoized Route Components**:
```clojure
(def memoized-user-page
  (react/memo
   (defui user-page [{:keys [user-id]}]
     (let [[user set-user] (use-state nil)]

       (use-effect
        (fn []
          (go
            (let [user-data (<! (fetch-user user-id))]
              (set-user user-data))))
        [user-id])

       ($ :div
          ($ :h1 (:name user))
          ($ :p (:email user)))))))
```

## Do's and Don'ts for UIx Routing

### Do's ✅
- **Do** use React Router v6+ for modern routing features
- **Do** leverage navigation hooks (useNavigate, useLocation, useParams)
- **Do** implement nested routes for complex layouts
- **Do** use loaders and actions for data fetching
- **Do** implement protected routes with proper redirects
- **Do** use code splitting with React.lazy for better performance
- **Do** implement proper SEO with React Helmet
- **Do** use search params hooks for query string management
- **Do** test routes with React Testing Library
- **Do** use concurrent features for better user experience

### Don'ts ❌
- **Don't** use outdated React Router versions (< v6)
- **Don't** mix imperative navigation with declarative routing
- **Don't** forget to handle loading states in async routes
- **Don't** use absolute paths in nested route components
- **Don't** skip error boundaries for route components
- **Don't** forget to implement proper redirects for authentication
- **Don't** use window.location for internal navigation
- **Don't** skip testing of route guards and redirects
- **Don't** ignore SEO considerations for public routes
- **Don't** overuse route parameters for complex state

This agent ensures robust, modern routing architectures that provide excellent user experience while leveraging React Router's powerful features with UIx's functional approach.