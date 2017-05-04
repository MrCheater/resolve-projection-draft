# resolve-projection-draft

# Server-Side
## Declaration Projection
```js
// ./projections/counters.js
import * as EventTypes from '../eventTypes.js'

export default {
	initialState: () => Immutable({}),

	eventHandlers: {
		[EventTypes.COUNTER_INCREMENT]: (state, event) => state.update( event.aggregateId, (counter) => counter.set('value', count) ),
		//...
	},

	findBy: {
		id: true,
		cursor: true,
		all: false,
		'myKey1 myKey2': true
	}
}
```


# Client-Side
## Reducer
```js
// ./reducers/counters.js

import counters from '../projections/counters'
import { reducer } from 'resolve-projection'

export default reducer(counters)

```
