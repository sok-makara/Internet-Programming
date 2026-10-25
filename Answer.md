1. Array index as :key while the list is filtered
   => With the index as the key, Vue knows a card only by its position. After filtering, a different event takes that position, so Vue reuses the old card with its old local state. An open details panel would stay open on the wrong event. Using :key="e.id" fixes this.

2. Why visible is a computed and not a function in the template
   => A computed keeps its result and only recalculates when search, category or events change. A function in the template runs again on every re-render. When I type in the search box, both recalculate, but the computed runs once and reuses the result for the count and the v-for.

3. Why EventCard emits book
   => First, props are read-only, so the card should not change event.booked. Second, the parent should decide what booking means, like refusing a full event. The card only says "the user wants to book event 7".

4. const { booked } = reactive(event); booked++
   => Destructuring copies the number into a plain variable that is no longer connected to the reactive object. Vue cannot see changes to the copy, so the page does not update. I should change the property on the object, like state.booked++.

5. Where myBookings lives
   => It lives in App.vue, because App owns the events and shows the count in the header. EventCard knows only one event, and EventList only filters and shows them, so they cannot share it with the header. State should live in the nearest parent of all the components that need it.
