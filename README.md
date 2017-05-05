# resolve-projection-draft

# Server-Side
## Declaration Projection
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
		cursor: true,
		all: false,
		'value': true
	}
}
```

## Startup Projections and Query Side
```js
import projectionInMemory from 'resolve-projection-memory'
import projections from './projections/'
import resolveQuery from 'resolve-query';
import express from 'express'

const eventStore = /*...*/
const eventBus = /*...*/

const queries = query({
	store: eventStore,
	bus: eventBus,
	projections,
	driver: projectionInMemory
});

const queries = resolveQuery({projections, store, bus, driver})

console.log(queries.getHandledEvents()) //COUNTER_CREATE, COUNTER_INCREMENT, COUNTER_DECREMENT

const app = express()

const graphQLResolver = queries.getGraphQLResolver()
app.get('/api/graphql/*', (req, res) =>
	const query = gql`${req.params[0]}`

	graphQLResolver(query).then(
		result => res.json(result)
	)
);
//fetch('/api/qraphql/{counters}') // Not work. option "all" disabled 
//fetch(`/api/qraphql/{counters(id: "${id}")}`)
//fetch(`/api/qraphql/{counters(limit: 10)}`)
//fetch(`/api/qraphql/?after=${id}&limit=10`)
//fetch(`/api/counters?limit=10&sortBy=value`)
//fetch(`/api/counters?after=${id}&limit=10&sortBy=value`)


const restAPIResolver = queries.getRestAPIResolver()
app.get('/api/*', (req, res) =>
	const query = req.params[0]

	restAPIResolver(query).then(
		result => res.json(result)
	)
);
//fetch('/api/counters/') // Not work. option "all" disabled 
//fetch(`/api/counters/${id}`)
//fetch(`/api/counters?limit=10`)
//fetch(`/api/counters?after=${id}&limit=10`)
//fetch(`/api/counters?limit=10&sortBy=value`)
//fetch(`/api/counters?after=${id}&limit=10&sortBy=value`)

app.listen(80)
```

# Client-Side
## Reducer
```js
// ./reducers/counters.js

import counters from '../projections/counters'
import { reducer } from 'resolve-projection'

export default reducer(counters)

```





















const projection = {
	initialState: () => Immutable({}),

	eventHandlers: {
		[EventTypes.SOME_EVENT]: (state, event) => state.someOperation(),
		//...
	},

	findBy: {
		id: true,
		cursor: true,
		all: false,
		'myKey1 myKey2': true
	}
}



const driver =  require('resolve-projection-memory')()

const instanceProjection = require('resolve-projection')({ projection, driver, store, bus }) //server-side


const instanceProjection = require('resolve-projection')({ projection }) //client-side



instanceProjection.toReducer() //only client-side

instanceProjection.getHandledEvents() //only server-side

instanceProjection.getGraphQLResolver() //only server-side

instanceProjection.getRestAPIResolver() //only server-side

instanceProjection.find({ after: ID, first: Int, sortBy: String = 'id'  })

instanceProjection.find({ id: ID })

instanceProjection.find()


//See Pagination, Cursor-Based http://dev.apollodata.com/react/pagination.html

//See getQueryResolver
https://github.com/apollographql/graphql-anywhere
