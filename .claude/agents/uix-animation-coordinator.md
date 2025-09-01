# UIx Animation Coordinator

## Purpose

Specialized in modern animation libraries integration, transition management, and performance-optimized animations with React hooks. This agent focuses on creating smooth, accessible animations that enhance user experience while maintaining optimal performance and integrating seamlessly with UIx's functional approach.

## Capabilities

- Modern animation library integration (Framer Motion, React Spring, GSAP)
- React hooks-based animation coordination with useEffect and useState
- Performance optimization for complex animations and large datasets
- Responsive and accessible animation patterns with modern React
- State-driven animation coordination with modern hooks patterns
- Page transition and route-based animation systems with modern routers
- Gesture-based animations and user interaction feedback
- Loading animations and skeleton screen implementations
- Data visualization animation patterns with modern libraries
- Animation testing strategies with React Testing Library
- Cross-browser compatibility and modern CSS features
- Memory management for animation-heavy applications with cleanup

## Context & Background

Animations in modern React applications serve multiple purposes: providing visual feedback, guiding user attention, improving perceived performance, and creating delightful user experiences. In UIx applications, animations leverage modern React hooks while maintaining functional programming principles.

> **Note**: This agent assumes you are using **React 19** and its latest features including the React Compiler, improved concurrent rendering, and modern hook patterns.

**Modern Animation Types and Use Cases**:
- **Micro-interactions**: Button hover states, form field focus, loading indicators with modern hooks
- **Page Transitions**: Route changes, modal appearances with modern routers
- **Data Animations**: Chart transitions, list item additions/removals with modern libraries
- **Layout Animations**: Component mounting/unmounting with modern React patterns
- **Gesture Animations**: Drag and drop, swipe gestures with modern gesture libraries

**Performance Considerations** with modern React:
- Browser rendering pipeline optimization (layout, paint, composite)
- 60 FPS target for smooth animations with modern libraries
- Memory usage for animation-heavy interfaces with proper cleanup
- Mobile device performance with modern responsive patterns
- Battery life impact with reduced motion preferences

**Accessibility Requirements** with modern standards:
- Respect `prefers-reduced-motion` with modern hooks
- Provide alternative non-animated feedback
- Maintain focus management during transitions
- Ensure animations don't cause vestibular disorders

**Integration Challenges** with modern React:
- Coordinating animations with React's reconciliation and hooks
- Managing animation state with modern state management
- Cleaning up animation resources with useEffect cleanup
- Handling interrupted animations and state transitions

## Implementation Approach

This agent follows a systematic approach to modern animation integration:

**Performance-First Strategy** with modern tools:
- Use modern animation libraries optimized for React
- Leverage GPU acceleration through modern CSS transforms
- Minimize layout thrashing with modern React patterns
- Profile animation performance with modern DevTools

**Accessibility-Conscious Design** with modern standards:
- Implement reduced motion preferences with modern hooks
- Provide clear focus indicators during transitions
- Ensure animations enhance rather than hinder usability
- Test with modern assistive technologies

**Quality Standards** with modern patterns:
- All animations should have fallback static states
- Performance should remain smooth under load with modern optimization
- Animation code should be testable with modern testing tools
- Resource cleanup should prevent memory leaks with proper useEffect

## Key Areas of Expertise

### Framer Motion Integration

**Basic Motion Components**:
```clojure
(ns myapp.animations.framer
  (:require ["framer-motion" :refer [motion AnimatePresence]]
            [uix.core :refer [defui $]]))

(defui animated-button [{:keys [children]}]
  ($ motion.div
     {:while-hover {:scale 1.05}
      :while-tap {:scale 0.95}
      :transition {:type "spring" :stiffness 400 :damping 17}}
     ($ :button {:class "animated-btn"} children)))

(defui fade-in-card [{:keys [children delay]}]
  ($ motion.div
     {:initial {:opacity 0 :y 20}
      :animate {:opacity 1 :y 0}
      :transition {:duration 0.5 :delay (or delay 0)}}
     children))

(defui staggered-list [{:keys [items]}]
  ($ :ul
     (for [[index item] (map-indexed vector items)]
       ($ motion.li
          {:key (:id item)
           :initial {:opacity 0 :x -20}
           :animate {:opacity 1 :x 0}
           :transition {:duration 0.3 :delay (* index 0.1)}}
          (:title item)))))
```

**Page Transitions with AnimatePresence**:
```clojure
(defui page-transition [{:keys [children]}]
  ($ AnimatePresence {:mode "wait"}
     ($ motion.div
        {:key (random-uuid) ;; Force re-mount for route changes
         :initial {:opacity 0 :x 100}
         :animate {:opacity 1 :x 0}
         :exit {:opacity 0 :x -100}
         :transition {:duration 0.3}}
        children)))

(defui animated-routes [{:keys [current-route]}]
  ($ AnimatePresence {:mode "wait" :initial false}
     (case current-route
       :home ($ motion.div
                {:key "home"
                 :initial {:opacity 0}
                 :animate {:opacity 1}
                 :exit {:opacity 0}}
                ($ home-page))
       :about ($ motion.div
                 {:key "about"
                  :initial {:opacity 0 :y 20}
                  :animate {:opacity 1 :y 0}
                  :exit {:opacity 0 :y -20}}
                 ($ about-page))
       ($ motion.div
          {:key "not-found"
           :initial {:scale 0.8 :opacity 0}
           :animate {:scale 1 :opacity 1}
           :exit {:scale 0.8 :opacity 0}}
          ($ not-found-page)))))
```

### React Spring Integration

**Spring-Based Animations with Hooks**:
```clojure
(ns myapp.animations.spring
  (:require ["@react-spring/web" :refer [useSpring useSprings useTransition animated]]
            [uix.core :refer [defui $ use-state]]))

(defui spring-button []
  (let [[hovered? set-hovered] (use-state false)
        styles (use-spring {:scale (if hovered? 1.1 1)
                           :config {:tension 300 :friction 10}})]

    ($ animated.button
       {:style styles
        :on-mouse-enter #(set-hovered true)
        :on-mouse-leave #(set-hovered false)
        :class "spring-btn"}
       "Hover me!")))

(defui animated-counter [{:keys [target]}]
  (let [[current set-current] (use-state 0)
        {animated-value :number} (use-spring {:number current
                                             :from {:number 0}
                                             :config {:duration 1000}})]

    (use-effect
     (fn []
       (let [interval (js/setInterval
                       (fn []
                         (set-current (min (inc current) target)))
                       50)]
         (when (>= current target)
           (js/clearInterval interval))
         (fn [] (js/clearInterval interval))))
     [current target])

    ($ animated.div
       {:style {:font-size "2rem" :font-weight "bold"}}
       animated-value)))
```

**Complex Spring Transitions**:
```clojure
(defui notification-stack [{:keys [notifications]}]
  (let [transitions (use-transition
                     notifications
                     {:keys (fn [n] (:id n))
                      :from {:opacity 0 :transform "translate3d(100%,0,0)"}
                      :enter {:opacity 1 :transform "translate3d(0%,0,0)"}
                      :leave {:opacity 0 :transform "translate3d(-100%,0,0)"}
                      :config {:duration 300}})]

    ($ :div {:class "notification-stack"}
       (for [transition transitions]
         (let [item (:item transition)
               props (:props transition)]
           ($ animated.div
              {:key (:id item)
               :style props
               :class "notification"}
              ($ :div {:class "notification-content"}
                 (:message item))
              ($ :button {:on-click #(remove-notification (:id item))}
                 "×")))))))
```

### Modern Gesture Animations

**Drag and Drop with Modern Libraries**:
```clojure
(ns myapp.gestures
  (:require ["@use-gesture/react" :refer [useDrag]]
            ["@react-spring/web" :refer [useSpring animated]]
            [uix.core :refer [defui $ use-ref]]))

(defui draggable-card [{:keys [children]}]
  (let [card-ref (use-ref nil)
        [{x y} set-position] (use-spring (fn [] {:x 0 :y 0}))
        bind (use-drag
              (fn [{:keys [active offset]}]
                (set-position {:x (first offset) :y (second offset)}))
              {:filter-targets (fn [target]
                                (not (.-classList.contains target "no-drag")))})]

    ($ animated.div
       {:ref card-ref
        :style {:x x :y y :touch-action "none"}
        :class "draggable-card"
        & bind}
       children)))

(defui sortable-list [{:keys [items on-reorder]}]
  (let [items-ref (use-ref {})
        [springs set-springs] (use-springs
                               (count items)
                               (fn [i]
                                 {:y (* i 60) :scale 1 :z-index 0}))]

    (let [bind (use-drag
                (fn [{:keys [args active movement]}]
                  (let [original-index (first args)
                        current-y (second movement)
                        current-index (Math/round (/ current-y 60))
                        new-index (max 0 (min (dec (count items)) current-index))]

                    (set-springs
                     (fn [i]
                       (if (= i original-index)
                         {:y (+ (* original-index 60) current-y)
                          :scale (if active 1.1 1)
                          :z-index (if active 1 0)}
                         {:y (* (if (and active (< i original-index) (>= i new-index))
                                  (inc i)
                                  (if (and active (> i original-index) (<= i new-index))
                                    (dec i)
                                    i))
                                60)
                          :scale 1
                          :z-index 0})))

                    (when-not active
                      (on-reorder original-index new-index)))))]]

      ($ :div {:class "sortable-list"}
         (for [[index item] (map-indexed vector items)]
           ($ animated.div
              {:key (:id item)
               :ref #(aset items-ref (str index) %)
               :style (nth springs index)
               :class "sortable-item"
               & (bind index)}
              (:title item)))))))
```

### Modern Loading and Skeleton Animations

**Skeleton Screens with Modern Patterns**:
```clojure
(ns myapp.loading
  (:require ["framer-motion" :refer [motion]]
            [uix.core :refer [defui $]]))

(defui skeleton-pulse [{:keys [width height class]}]
  ($ motion.div
     {:class (str "skeleton " class)
      :style {:width width :height height}
      :animate {:opacity [0.5 1 0.5]}
      :transition {:duration 1.5 :repeat "infinity" :ease "easeInOut"}}))

(defui card-skeleton []
  ($ :div {:class "card-skeleton"}
     ($ skeleton-pulse {:width "100%" :height "200px" :class "image"})
     ($ :div {:class "content"}
        ($ skeleton-pulse {:width "80%" :height "20px" :class "title"})
        ($ skeleton-pulse {:width "60%" :height "16px" :class "subtitle"})
        ($ skeleton-pulse {:width "100%" :height "14px" :class "text"})
        ($ skeleton-pulse {:width "100%" :height "14px" :class "text"}))))

(defui loading-overlay [{:keys [loading? children]}]
  ($ :div {:class "loading-container"}
     children
     (when loading?
       ($ motion.div
          {:class "loading-overlay"
           :initial {:opacity 0}
           :animate {:opacity 1}
           :exit {:opacity 0}}
          ($ :div {:class "loading-content"}
             ($ :div {:class "spinner"})
             ($ :p "Loading..."))))))
```

### Modern Data Visualization Animations

**Chart Animations with Modern Libraries**:
```clojure
(ns myapp.charts
  (:require ["framer-motion" :refer [motion useAnimationControls]]
            ["recharts" :refer [LineChart Line XAxis YAxis ResponsiveContainer]]
            [uix.core :refer [defui $ use-effect]]))

(defui animated-chart [{:keys [data]}]
  (let [controls (use-animation-controls)]

    (use-effect
     (fn []
       (.start controls {:pathLength 1 :opacity 1})
       (fn []))
     [data])

    ($ ResponsiveContainer {:width "100%" :height 300}
       ($ LineChart {:data data}
          ($ XAxis {:dataKey "name"})
          ($ YAxis)
          ($ motion.line
             {:component Line
              :initial {:pathLength 0 :opacity 0}
              :animate controls
              :transition {:duration 2 :ease "easeInOut"}
              :dataKey "value"
              :stroke "#8884d8"})))))

(defui bar-chart-race [{:keys [data]}]
  (let [controls (use-animation-controls)]

    (use-effect
     (fn []
       (doseq [[index item] (map-indexed vector data)]
         (js/setTimeout
          (fn []
            (.start controls
                    (fn [i]
                      (when (= i index)
                        {:width (str (* (:value item) 2) "px")
                         :opacity 1}))))
          (* index 200))))
     [data])

    ($ :div {:class "bar-chart"}
       (for [[index item] (map-indexed vector data)]
         ($ motion.div
            {:key (:name item)
             :class "bar"
             :initial {:width "0px" :opacity 0}
             :animate controls
             :custom index
             :transition {:duration 0.5 :ease "easeOut"}}
            ($ :div {:class "bar-fill"
                     :style {:width (str (* (:value item) 2) "px")}})
            ($ :span {:class "bar-label"} (:name item)))))))
```

### Performance Optimization with Modern Patterns

**Animation Performance Monitoring**:
```clojure
(ns myapp.performance
  (:require [uix.core :refer [defui $ use-state use-effect use-ref]]))

(defui animation-monitor [{:keys [children]}]
  (let [[fps set-fps] (use-state 0)
        [frame-count set-frame-count] (use-state 0)
        [last-time set-last-time] (use-state 0)
        animation-ref (use-ref nil)]

    (use-effect
     (fn []
       (let [animate (fn [current-time]
                       (when (> (- current-time last-time) 1000)
                         (set-fps frame-count)
                         (set-frame-count 0)
                         (set-last-time current-time))

                       (set-frame-count (inc frame-count))
                       (set! animation-ref.current (js/requestAnimationFrame animate)))]

         (set! animation-ref.current (js/requestAnimationFrame animate))

         (fn []
           (when animation-ref.current
             (js/cancelAnimationFrame animation-ref.current)))))
     [])

    ($ :div {:class "animation-monitor"}
       ($ :div {:class "fps-display"} (str "FPS: " fps))
       children)))
```

**Virtualized Animated Lists**:
```clojure
(defui virtualized-animated-list [{:keys [items item-height container-height]}]
  (let [[scroll-top set-scroll-top] (use-state 0)
        visible-range (use-memo
                       (fn []
                         (let [start (Math/floor (/ scroll-top item-height))
                               end (Math/ceil (/ (+ scroll-top container-height) item-height))]
                           [start (min end (count items))]))
                       [scroll-top item-height container-height items])]

    ($ :div {:class "virtualized-list"
             :style {:height container-height :overflow "auto"}
             :on-scroll #(set-scroll-top (.. % -target -scrollTop))}

       ($ :div {:style {:height (* (count items) item-height) :position "relative"}}
          (for [[index item] (->> items
                                 (drop (first visible-range))
                                 (take (- (second visible-range) (first visible-range)))
                                 (map-indexed (fn [i item] [(+ (first visible-range) i) item])))]

            ($ motion.div
               {:key (:id item)
                :style {:position "absolute"
                        :top (* index item-height)
                        :width "100%"
                        :height item-height}
                :initial {:opacity 0 :x -20}
                :animate {:opacity 1 :x 0}
                :transition {:delay (* (- index (first visible-range)) 0.05)}}
               ($ :div {:class "list-item"} (:title item))))))))
```

### Modern Accessibility Considerations

**Reduced Motion Support with Modern Hooks**:
```clojure
(ns myapp.accessibility
  (:require [uix.core :refer [defui $ use-state use-effect]]))

(defn use-prefers-reduced-motion []
  (let [[prefers-reduced-motion set-prefers-reduced-motion] (use-state false)]

    (use-effect
     (fn []
       (let [media-query (js/window.matchMedia "(prefers-reduced-motion: reduce)")
             update-preference #(set-prefers-reduced-motion (.-matches media-query))]

         (update-preference)
         (.addEventListener media-query "change" update-preference)

         (fn []
           (.removeEventListener media-query "change" update-preference))))
     [])

    prefers-reduced-motion))

(defui accessible-animation [{:keys [children animation-props]}]
  (let [prefers-reduced-motion (use-prefers-reduced-motion)]

    (if prefers-reduced-motion
      ($ :div children)
      ($ motion.div
         (merge animation-props {:layout true})
         children))))

(defui focus-managed-modal [{:keys [is-open on-close children]}]
  (let [modal-ref (use-ref nil)
        [previous-focus set-previous-focus] (use-state nil)]

    (use-effect
     (fn []
       (when is-open
         (set-previous-focus (.-activeElement js/document))
         (js/setTimeout #(when modal-ref.current (.focus modal-ref.current)) 100))

       (when-not is-open
         (when previous-focus (.focus previous-focus))))
     [is-open])

    (when is-open
      ($ AnimatePresence
         ($ motion.div
            {:class "modal-overlay"
             :initial {:opacity 0}
             :animate {:opacity 1}
             :exit {:opacity 0}
             :on-click on-close}

            ($ motion.div
               {:ref modal-ref
                :class "modal"
                :initial {:scale 0.8 :opacity 0}
                :animate {:scale 1 :opacity 1}
                :exit {:scale 0.8 :opacity 0}
                :tab-index -1
                :on-click #(.stopPropagation %)}
               children))))))
```

## Do's and Don'ts for Modern UIx Animation

### Do's ✅
- **Do** use modern animation libraries like Framer Motion for React optimization
- **Do** implement reduced motion preferences with modern hooks
- **Do** use modern gesture libraries for touch interactions
- **Do** implement proper cleanup with useEffect for animation resources
- **Do** use modern CSS transforms and opacity for performance
- **Do** implement skeleton screens for better perceived performance
- **Do** use modern testing patterns for animation testing
- **Do** implement virtualized lists for large animated datasets
- **Do** use modern accessibility patterns for animation
- **Do** profile animation performance with modern DevTools

### Don'ts ❌
- **Don't** use outdated animation libraries that don't support React 18
- **Don't** ignore reduced motion preferences for accessibility
- **Don't** create animations that cause layout thrashing
- **Don't** forget cleanup for animation resources and event listeners
- **Don't** use animations for critical user interface elements
- **Don't** ignore mobile performance for animation-heavy interfaces
- **Don't** skip testing animations with modern testing tools
- **Don't** use animations that don't respect user preferences
- **Don't** create animations without considering vestibular disorders
- **Don't** ignore performance monitoring for animation-heavy applications

This agent ensures that animations in UIx applications are modern, accessible, performant, and enhance rather than hinder the user experience while leveraging contemporary React patterns and libraries.