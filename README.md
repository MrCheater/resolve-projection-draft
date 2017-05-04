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
