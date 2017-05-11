# resolve-projection + resolve-query [draft]

## Server-Side
### Declaration Projection
```js
// ./projections/counters.js
import * as EventTypes from '../eventTypes.js'

export default {
   initialState: () => Immutable({}),

   eventHandlers: {
      [EventTypes.COUNTER_CREATE]: (state, event) => state.set(event.aggregateId, {value: 0}),
      [EventTypes.COUNTER_INCREMENT]: (state, event) => state.update( event.aggregateId, (counter) => counter.set('value', count.value + 1) ),
      [EventTypes.COUNTER_DECREMENT]: (state, event) => state.update( event.aggregateId, (counter) => counter.set('value', count.value - 1) ),
   },

   findBy: {
      id: true,   
      'value': true
   }
}
```

### Startup Projections and Query Side
```js
import projectionInMemory from 'resolve-projection-memory'
import projections from './projections/'
import resolveQuery from 'resolve-query';
import express from 'express'
import gql from 'graphql-tag'

const eventStore = /*...*/
const eventBus = /*...*/

const queries = resolveQuery({
   store: eventStore,
   bus: eventBus,
   projections,
   driver: projectionInMemory
});

const app = express()

const graphQLResolver = queries.getGraphQLResolver() //See getQueryResolver https://github.com/apollographql/graphql-anywhere
app.get('/api/graphql/*', (req, res) =>
   const query = gql`${req.params[0]}`

   graphQLResolver(query).then(
      result => res.json(result)
   )
);
//fetch(`/api/qraphql/{counters}`) // Not work. option "all" disabled 
//fetch(`/api/qraphql/{counters(id:"${id}")}`)
//fetch(`/api/qraphql/{counters(limit:10)}`)
//fetch(`/api/qraphql/{counters(after:"${id}" limit:10)}`)
//fetch(`/api/qraphql/{counters(limit:10 sortBy="value")}`)
//fetch(`/api/qraphql/{counters(after:"${id}" limit:10 sortBy="value")}`) //See Pagination, Cursor-Based http://dev.apollodata.com/react/pagination.html
//fetch(`/api/qraphql/{counters(value:42)}`)


const restAPIResolver = queries.getRestAPIResolver() 
app.get('/api/*', (req, res) =>
   const query = req.params[0]

   restAPIResolver(query).then(
      result => res.json(result)
   )
);
//fetch(`/api/counters/`) // Not work. option "all" disabled 
//fetch(`/api/counters/${id}`)
//fetch(`/api/counters?limit=10`)
//fetch(`/api/counters?after=${id}&limit=10`)
//fetch(`/api/counters?limit=10&sortBy=value`)
//fetch(`/api/counters?after=${id}&limit=10&sortBy=value`) 
//fetch(`/api/counters?value=10`)

app.listen(80)
```

## Client-Side
### Reducer
```js
// ./reducers/counters.js

import counters from '../projections/counters'
import { reducer } from 'resolve-projection'

export default reducer(counters)

```

## API
### resolve-query 
```js
import projectionInMemory from 'resolve-projection-memory'

export default ({ store, bus, projection, driver = projectionInMemory}) => ({/*...*/})
```
### resolve-projection 
```js
   export function reducer() {
      const reducer = (state, action) => {
         const eventHandler = projection.eventHandlers[action.type]
         if(eventHandler) {
            return eventHandler(state, action)
         }
         return state
      }
      return reducer
   }
   
   export function* saga() {
      //...
      //Fetch projections
      //Update reducers
   }
```
