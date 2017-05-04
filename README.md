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

## Startup Projection
import resolveProjection from 'resolve-projection'
import projections from './projections/'

const startupProjections = {};
for(let projectionName in projections) {
	startupProjections[projectionName] = resolveProjection({
		projection: projections[projectionName],
		
	})
}


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







	query: [
		'id',
		'after first sortBy',
		'myKey1 myKey2',
	],
